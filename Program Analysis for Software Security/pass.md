---
title: Program Analysis for System Security and Reliability
desription: Summary of 263-2925-00L Program Analysis for System Security and Reliability at ETH Zürich FS18
tags: ETH, Zusammenfassung
---
PASS Summary
===
Frédéric Vogel `vogelfr@ethz.ch`, ETH Zürich, FS18

[TOC]

<p style="page-break-after:always;"></p>

# Programmable Networks
- Network configuration is hard and misconfigurations are common.
- Local configuration changes have global effects on how traffic is routed.

## Mathematical Background
### Partially ordered sets (posets)

A *partial order* is a binary relation $\sqsubseteq\subseteq L \times L$ with the following properties:

Reflexivity
: $\forall p \in L\ldotp p\sqsubseteq p$

Transitivity
: $\forall p, q, r \in L\ldotp (p\sqsubseteq q \wedge q \sqsubseteq r) \Rightarrow p \sqsubseteq r$

Antisymmetry
: $\forall p, q \in L\ldotp (p \sqsubseteq q \wedge q \sqsubseteq p) \Rightarrow p = q$

A *poset* ($L, \sqsubseteq$) is a set $L$ equipped with a partial ordering $\sqsubseteq$.

#### Bounds
Given a poset ($L, \sqsubseteq$) and a set $Y \subseteq L$:
- $u \in L$ is an *uppper bound* of Y if $\forall p \in Y\ldotp p \sqsubseteq u$
- $\sqcup_Y \in L$ is a *least upper bound* of Y if $\sqcup_Y$ is an upper bound of Y and $sqcup_Y \sqsubseteq u$ whenever $u$ is another upper bound of $Y$
- $l \in L$ is a *lower bound* of Y if $\forall p \in Y\ldotp l \sqsubseteq p$
- $\sqcap_Y \in L$ is a *greatest lower bound* of Y if $\sqcap_Y$ is a lower bound of Y and $l \sqsubseteq \sqcap_Y$ whenever $l$ is another lower bound of $Y$

### Complete lattices
A *complete lattice* ($L, \sqsubseteq, \sqcup, \sqcap$) is a poset ($L, \sqsubseteq$) where $\sqcup_Y$ and $\sqcap_Y$ exist for any $Y \subseteq L$.

### Monotone functions
A function $f: A \to B$ between two posets ($A, \sqsubseteq$) and ($B, \preceq$) is *monotone* if $$\forall a,b \in A: a \sqsubseteq b \Rightarrow f(a) \preceq f(b)$$

For a monotone function $f: A\to A$ we have $$\forall a,b \in A: a \sqsubseteq b \Rightarrow f(a) \sqsubseteq f(b)$$

### Least fixed point
For a poset ($L, \sqsubseteq$) and function $f: L \to L$ element $x\in L$ is a *fixed point* if $f(x) =x$

For a poset ($L, \sqsubseteq$) and function $f: L \to L$ we say that $\textrm{lfp}_f\in L$ is a *least fixed point* of $f$ iff:
- $\textrm{lfp}_f$ is a fixed point
- It is the least fixed point: $\forall a \in L: a = f(a) \Rightarrow \textrm{lfp}_f\sqsubseteq a$

### Tarski's fixed-point theorem
If ($L, \sqsubseteq, \sqcup, \sqcap$) is a complete lattice and $f: L \to L$ is a monotone function, then $\textrm{lfp}_f$ exsits.

## Stratified Datalog
Datalog
: Declarative logic programming language

Datalog Atoms
: - Constants $C = \{alice, bob, carol\}$
  - Variables $V = \{X, Y, Z\}$
  - Predicates $P = \{parent(\_,\_), anc(\_,\_)\}$
  - Ground atoms $G_{P,C} = \{parent(alice, alice), anc(alice, bob), \ldots\}$: only constants
  - Atoms $A_{P,C,V} = \{parent(X,X),anc(alice,X), \ldots\}$: may contain variables

Datalog program
: Set of rules of the form $a \gets l_1, \ldots, l_n$
    - $n\geq 0$, $a$ is an atom
    - $l_1, \ldots, l_n$ literals of the form $a$ (positive literal) or $!a$ (negative literal)

Well-formed
: Program is well-formed if for any rule all variables appearing in the head also appear in the body

### Consequence operator
Interpretations
: $\mathcal{I} = \mathcal{P}(G_{P,C})$

Substitutions
: $\sigma: V \to C$

Consequence operator $T_P:\mathcal{I} \to \mathcal{I}$
: $T_P(I) = \{\sigma(a)|a\gets l_1, \ldots, l_n\in P, \exists\sigma: \forall i\in[1, \ldots, n]: I \vdash\sigma(l_i)\}$
with $I\vdash l_i$ if $l_1 = a$ and $a\in I$
$I\vdash l_i$ if $l_1 = !a$ and $a\notin I$

### Positive Datalog program
A Datalog program is positive if its rules do not contain negative literals.

For any positive Datalog program $P$ the consequence operator $T_P$ is monotone.

The semantics of a positive Datalog program $P$ is $\textrm{lfp}_{T_P}$
$\textrm{lfp}_{T_P} can be computed by iteratively applying the consequence operator until reaching a fixed-point.

### Stratified Datalog
$T_P$ not monotone for Datalog programs with negation

- Same predicate rules in one stratum
- Negated predicates defined in lower stratum
- Positive predicates defined in current or lower stratum

$\to$ For each stratum compute the lfp that contains the lfp of the previous stratum

## Automated Analysis of network configurations
Router configurations
: Datalog input

Distributed Protocols (BGP, OSPF, Statis routes)
: Datalog program

Forwarding plane
: Datalog fixed-point

### Batfish
1. Parse configurations (to derive input facts)
2. Compute forwarding plane (by computing fixed-point)
3. Check for violations (by querying the fixed-point)

$\to$ Is existing configuration correct?

## Automatic Network Configuration Synthesis
### Symbolic Execution
- Runs program with symbolic values $\to$ big constraint formula
- SMT solver used to find satisfying assignments to constraint formula
- Symbolic execution keeps *symbolic store* and *path constraint* $\to$ conjunction gives *symbolic state*
- Challenges:
    - Loops
    - Non-linear constraints
    - Hard-to-solve constraints (e.g. `x=hash(y)`)

## SyNET
- Variables in head of of rule are quantified universally, those in body are quantified existencially
1. Encode Datalog program $P$ into SMT constraints
2. Encode Datalog query $q$ as assertions that must hold on the fixed-point
3. Get a model $M$ that satisfies the conjunction of the above constraints
4. Derive input $I$ from $M$ by checking which atoms are `true` in $M$

### Bounded unrolling of Datalog
Problem: in Datalog `p <- q` is only derived iff `q` is `true`
In logic $p\Leftarrow q$ is also satisfied if $q$ is $false$ and $p$ is $true$.

#### Bounded unrolling:
```
path(X,Y) <- link(X,Y)
path(X,Y) <- link(X,Z), path(Z,Y)
```
leads to the following constraints:
$\forall X, Y\ldotp path_1(X,Y)\Leftrightarrow link(X,Y)$
$\forall X, Y\ldotp path_2(X,Y)\Leftrightarrow (link(X,Y)\vee (\exists Z\ldotp link(X,Z) \wedge path_1(Z,Y)))$

Unrolling works only for *positive* queries.

#### Handling negative queries
No unrolling for negative queries

### Stratified Datalog
Back step
: Backtrack to step Synth $P_i$ if step Synth $P_{i-1}$ returns `unsat`

Synth $P_n$
: Compute input $I_n$ for stratum $P_n$ such that $[P_n]_{I_n}$ satisfies $q$

Synth $P_{n-1}, \ldots, P_1$
: Compute input $I_i$ for stratum $P_i$ such that $[P_i]_{I_i}$ produces the input $I_{i+1}$ produced by previous step

**Key Challenge: Scaling synthesis for OSPF**

## Efficient OSPF Synthesis
$P$
: Set of all simple paths from $src$ to $dst$

$C$
: Set of all cost variables

$\phi(P, C)$
: Encoded requirements

$A$
: Cost assignment

### Direct OSPF Synthesis
Find cost assignment $A$ such that $\phi(P, A)$ holds

Formula for solver: $\exists C\ldotp\phi(P,C)$

$\to$ hard to solve:
    - Constraint quantifies over all simple paths in $P$ $\to$ exponentially many
### Counter Example Guided Inductive Synthesis (CEGIS)
Insight: A small set $E$ of path can be sufficient to constrain the solution:
$$
\exists C \ldotp \phi(E, C), \qquad\textrm{where}\quad E=\{P_1, P_2, \ldots, P_k\} \subseteq P
$$

![](https://i.imgur.com/A5pcfbc.png)

## Probabilistic Network analysis
- State
- Distribution
- Analysis
    - Input:
        - Input distribution $\varphi_i$ over states of program
        - Statement (or program) $s$
    - Output: 
        - Ouput distribution $\varphi_o$ over states of program
- Normalisation:
    - $P_{new}(X) = \frac{P(X)}{\sum P(X)}$
### Bayonet
- Networks exhibit probabilistic behaviours
- Can model in  a *probabilistic programming language*
- Use existing solvers for inference. Benefits:
    - Do not reinvent the wheel
    - Can use to test a specialised solution
    - Provide benchmarks to general tools

# Blockchain Security
## Hash functions
- Function from arbitrary length data to fixed-sized output
- Deterministic
- Uniform
- Efficiently computable
- Collisions exist

### Cryptographic hash functions
#### Properties
Given hash function $h: X \to Y$

Pre-image resistance
: Given $y\in Y$, it is infeasible to find $x\in X$ such that $h(x) = y$

Second pre-image resistance
: Given $x\in X$, it is infeasible to find $x' \in X$ such that $x\neq x'$ and $h(x) = h(x')$

Collision resistance
: It is infeasible to find a pair $(x_1, x_2)$ such that $x_1\neq x_2$ and $h(x_1) = h(x_2)$

#### Applications
Data equality
: If we know that $h(x) = h(y)$ it is safe to assume that $x=y$

Data integrity
: To verify integrity of data $d$ we remember $h_d = h(d)$. When obtaining $d'$ from *untrusted* source we can verify it by checking $h_d\stackrel{?}{=} h(d')$

#### Cryptographic puzzles
Puzzle friendly
: For any output $y$, if $r$ is chosen from probability distribution with high min-entropy, it is infeasible to find $x$ such that $h(r \Vert x) = y$

Search puzzle
: Given puzzle ID $id$, chosen from a probability distribution with high min-entropy, and an output range $T \subseteq Y$ find a solution $x$ such that $h(id \Vert x) \in T$. Because of puzzle-friendliness no strategy is better than trying random values of $x$

### Merkle trees
<img src="https://i.imgur.com/Y5aPXSL.png" style="float:right; height:150px;"></img>
- Hash root $h$ obtained from *trusted* source
- Integrity of data elements verified by reconstructing hash root and comparing

### Digital signatures
Allow only one user to sign but anyone to verify the signature

#### API
- $(sk, pk) = generateKeys(keySize)$
    - $sk$ is the secret key (kept private)
    - $pk$ is the public key (distributed)
- $sig = sign(sk, msg)$
- $verify(pk, msg, sig)$

### Digital identity
- User generates key pair $(sk, pk)$
- $h(pk)$ is public name of user
- $sk$ allows user to endorse statements $stmt$ using digital signature: $sig = sign(sk, stmt)$
- anyone can verify statements endores by user using $verify(pk,stmt,sig)$

### Simple coin creation and transfer
<img src="https://i.imgur.com/E4V3dTj.png" style="float:right; height:150px;"></img>

- Alice creates coin (identified by $coinID$) and endorses it (by signing $coinID$)
- Alice can transfer the coin to Bob (identified by $h(pk_{Bob})$) by signing a transaction

## Bitcoin
### Distributed ledger/consensus
- All nodes must see the same state of the ledger
- The protocol terminates and all correct nodes decide on the same value
- Value must have been proposed by some correct node

To make a transaction:
- Users *broadcast transactions* to the network nodes
- All nodes have a sequence of blocks of *agreed transactions* they have reached consensus on
- Each node has set of *outstanding transactions*

### Blockchain
Chain of blocks of transactions

![](https://i.imgur.com/k7YwqEt.png)

### Concensus algorithm
1. New transactions are broadcast to all nodes
2. Each node collects new transactions in a block
3. In each round a *random block* gets to broadcast its block
4. Other nodes accept the block only if all transactions in it are valid (unspent, valid signatures)
5. Nodes express their acceptance of the block by including its hash in the next block they create

#### Proof of work
To select a random node that broadcast its block: select nodes in proportion to a resource that no one can monopolize

##### Given
- Previous block with hash $h_{prev}$
- Merkle tree consisting of all new transactions with hash $h_{tx}$

##### Find
- Nonce $nonce$ such that
$$
hash(h_{prev} | h_{tx} | nonce) \leq \textrm{difficulty}
$$

##### Properties
- Difficult to compute
- Parametrisable cost
- Easy to verify
- Key security assumption: Attacks infeasible if majority of miners weighted by hash power follow the protocol

Honest nodes extend the longest valid branch

#### Incentives to extend longest chain
##### Block reward
The creator of a block can:
- Include special coin-creation transactions in the block (fixed value)
- Choose recipient address of this transaction
- **Only** paid if block ends up on the long-term concensus branch

##### Transaction fees
- Creator of transaction may choose to make output value less than input value
- Difference is transaction fee that goes to block creator

## Smart contracts
- Computerised transaction protocol that executes terms of contract
- Used to satisfy common contractual conditions
- Used to minimise exceptions (malicious or accidental)
- Used to minimise need for trusted intermediaries

### Ethereum
Decentralised platform designed to run smart contracts
- Similar to a world computer that executes code and maintains state of all smart contracts
- Latest block stores latest local state of all smartest contracts
- Transactions result in executing code (calling a function) in target smart contracts
- Transactions change state in one or more smart contracts
- Turing complete

![](https://i.imgur.com/UhBHDBc.png)

#### Ethereum accounts
Externally owned accounts
- Owned by some external entity
- Contains:
    - Address
    - Ether balance

Contract accounts
- "Owned" by contract
- Contains:
    - Address
    - Ether balance
    - Associated contract code
    - Persistent storage

#### Gas
- Each transaction requires *gas* to fuel contract execution
- Each EVM opcode requires a fixed amount of gas to execute
- Every transactions specifies the maximum ether the sender is willing to spend on the transaction
- Contract successfully executed?
    - Yes: Unspent ether is refunded to sender
    - No: Execution reverts without refunding

#### Authorisation
Any user can call arbitrary functions in contracts. The contract must explicitly restrict access to sensitive information.

#### DAO Bug
```
uint balance = 10;

function withdraw() {
    if (balance > 0):
        msg.sender.call.value(balance)();
    balance = 0;
}
```
Calling `withdraw()` multiple times before balance is set to zero $\to$ profit.
Attacker stole $ 150M of ether from The DAO

#### Partiy bug #1
Fallback function in wallet contract delegating any non-defined function to wallet library contract.

Attacker re-initialized wallet owner

White-hat hackers saved 377k ETH by hacking vulnerable wallets themselves and giving back funds to owners

#### Parity bug #2
User accidentally(?) deleted wallet library contract $\to$ no more withdraws are possible on wallets using the library $\to$ funds freezed in place, no way of recovering.

$ 170M frozen this way

## Security Properties
Solidity, Vyper
: High-level languages

EVM
: Low-level code, stack-based, no types, no functions

### Semantics of smart contracts
#### System state
Storage $S$
: Persistent initial storage is defined by constructor

Memory $M$
: Non-persistent (re-initialised before executing transaction)

Stack $Q$
: Size limited to 1024 elements, each element 256 bit

Block information $B$
: Number, timestamp, etc.
Fixed for a given transaction

State $\sigma$
: $\sigma = (S, M, Q, B)$
Storage, memory and stack may change as contract executes for given transaction

Transaction $T = (caller, data)$
: Transaction sender ($caller$)
Transaction data ($data$)

Trace
: $(\sigma_0, op_0) \to_T (\sigma_1, op_1) \to_T \cdots \to_T (\sigma_{n-1}, op_{n-1}) \to_T (\sigma_n)$
$op_{n-1}$: STOP
$\sigma_n$: final state
Each $op_i$ is next EVM op-code to be executed
Set of all traces for given contract defines the contract's semantics

Unchanged storage after call
: A contract does not change storage after calls iff for any two traces that are identical up to $call$ instruction the final storage $S_n$ and $S_n'$ are identical.

Unrestricted write
: A write to offset $o$ is unrestricted iff for any user address $a$ there is a transaction $T = (a, \_)$ and a trace $(\sigma_0, op_0) \to_{(a, \_)} \cdots \to_{(a, \_)} (\sigma_i, op_i) \to_{(a, \_)}\cdots$ such that $op_i = SStore(o,\_)$

Locked ether
: A contract locks ether iff it *can receive* ether and *can not transfer* ether

Further security properties
: - Unexpexted ether flows
- Insecure coding (e.g. unpriviledged write)
- Use of unsafe input
- Reentrant method calls (e.g. DAO bug)
- Manipulating ether flow via transaction reordering

## Securify
![](https://i.imgur.com/ASsPJoV.png)

<img source="https://i.imgur.com/OwLDpfe.png" style="float:right; height:150px;"></img>

- Security properties get encoded into Compliance and Violation patterns
- Static analysis using fixed-point computation
    - Pointer analysis
    - Data-flow analysis
    - Taint analysis
    - others
- Analysis expressed declaratively in Datalog
    - Declarative (concise specs of analyis)
    - Modular
    - Can leverage existing scalable Datalog solvers

# Attacks and Defenses of Deep Learning
Deep model
: Mathematical model trained from input-output examples
Mainly for tasks that are easy to perform but hard to formally define

## Background
### Classification through Machine Learning
Take a *data-driven* approach and learn a *model* (function) $f$ from data
$f:I\to C$ *approximates* the optimal function $f^*:I\to C$
A model is an *architecture* with real-valued *weights* and *biases*
The architecture defines the space of expressible models


### Perceptron
A classifier parametrized by *weights* $w_0, \ldots, w_{n-1}$ and *bias* $b$
Input $\overline{x} = (x_0, \ldots, x_{n-1})$
$$
f(x) =
\begin{cases}
  1 & \sum w_i x_i + b \geq 0 \\
  0 & \text{otherwise}
\end{cases}
$$

Linearly seperates the space



### Loss functions
Goal: model and optimal classifier are equal: $\forall i\ldotp f^*(i) = f(i)$

Induces a loss function which measures how good a classifier is: $\sum_{i\in I} [f^*(i) \neq f(i)]$
> $[\cdot]$ Iverson brackets $[true] = 1, [false] = 0$

Goal: find weights and biases of the model $f$ which minimises loss

#### Empirical loss
Problem: not all labels of input data $I$ are given
Approach: estimate loss function on some of labeled inputs $D$
Given labeled data $D$ compute *empirical loss* $\sum_{i\in D}[f^*(i)\neq f(i)]$

To avoid overfitting to $D$:
- Split $D$ into training set $D_{Tr}$ and test set $D_{Te}$
- Learn model by minimising loss on $D_{Tr}$, estimate loss on $D_{Te}$

Optimal solution of $\sum_{i\in D}[f^*(i)\neq f(i)]$ is a *global minimum* $\to$ define *differentiable* loss function and find point that nullifies its derivative.


### Training with gradient descent
#### Mean Squared Error

$MSE = \sum_{i \in D_{Tr}} (f^*(i)-f(i))^2$

If model consists of single weight $f(i) = w\cdot i$, then $MSE = \sum_{i\in D_{Tr}} (f^*(i)- w\cdot i )^2$

The minimum nullfies the derivative $\sum_{i\in D_{Tr}} 2(f^*(i)-w\cdot i)\cdot(-i) = 0$ 

Can compute best model (i.e. $w$) *analytically

#### Gradients
For $f(w_0, \ldots, w_{n-1}, b)$ with >1 parameter, derivative of $MSE = \sum_{i \in D_{Tr}} (f^*(i)-f(i))^2$ is generalised to *gradient*

A gradient is a vector defined by partial derivative of the variables
$$
\nabla MSE = \left ( \frac{\partial MSE}{\partial w_0}, \ldots, \frac{\partial MSE}{\partial w_{n-1}}, \frac{\partial MSE}{\partial b} \right )
$$

Minimum nullifies $\nabla MSE$ in all dimensions $\to$ hard to compute analytically

#### Gradient descent
1. Initialise randomly with certain values for weights/bias $a_0$
2. Compute $\nabla MSE$ at $a_i$
3. Next point is the one maximising decrease in $MSE$
$a_{i+1} = a_i - \gamma \nabla MSE(a_i)$, $\gamma$ is learning rate
4. If loss is small enough, complete, otherwise, repeat from 2.

### Deep learning models
- Perceptrons are too simplisti models
- Deep models combine multiple simple models

Deep model
: Directed graph of neurons organised in layers

Neuron
: Simple model followed by activation function

Activation Function
: Determines whether to propagate output of function
$ReLU(a) = \max (0,a)$

![](https://i.imgur.com/lKhscwF.png)

#### Feed forward Neural network (FF NN)
Neurons are connected to all neurons in the next layer

Deep models can be tuned by gradient descent, $\to$ need to compute the partial derivatives of all weights and biases $\to$ *Hard to compute

#### Back propagation
Backprpagate the common parts of the derivatives

#### Multiclass classification
- Output layer has a neuron for each class
- Outputs normalised to a probability distribution with softmax
$$
P(c_i) = \frac{\mathsf{e}^{f_i(x)}}{\sum_j \mathsf{e}^{f_j(x)}}
$$

#### Convolutional Neurol Networks (CNN)
- Neurons are not connected to every part of the input
- Weights are shared between neurons
- Input size reduced by propagating part of it

$\to$ reduces number of weights significantly

Convolutional layer
: Neurons are connected to local regions in the inputs and share weights

Pooling layer
: Neurons compute downsample of the input to reduce dimensionality

Fully connected layer
: Identical to FF NN, used to perform classification

## Adversarial examples
- High accuracy of a network does not imply that the network has learned the underlying concept
- For inputs from new distribution network may behave unexpectedly

### Fast Gradient Sign Method (FGSM)
Goal: from correctly classified $x$ find $x' = x + \eta$ so that $\Vert\eta\Vert_\infty \leq \epsilon$

Use loss function of network to optimise $\eta$ for a target $t$, set every index of $\eta$ to be in the direction of the loss' gradient

1. Compute perturbation
$\eta = \epsilon \cdot \mathrm{sign}(-\nabla_x\mathrm{loss}_t(x))$, where
$$
\nabla_x \mathrm{loss}_t = \left (\frac{\partial \mathrm{loss}_t}{\partial x_1}, \ldots, \frac{\partial \mathrm{loss}_t}{\partial x_n} \right)\quad \mathrm{sign}(x) = 
\begin{cases}
-1 & \text{if } x<0\\
0 & \text{if } x=0\\
1 & \text{if } x >0\\
\end{cases}
$$
2. Perturbe the input
$x' = x + \eta$
3. Check whether $f(x') = t$

### Minimal adversarial examples
Given
- Neural network $f: X \to C$
- Input $x\in X$
- Target label $t\in C$ such that $f(x) \neq t$

Compute a minimal $\eta$ such that $f(x+\eta) = t$
- By computing the salieny map of $f$, indicating where $f$ changes the most

For some inputs small perturbations can significantly change the output

#### Goal:
Given the Jacobian
$$
J_f =
\begin{pmatrix}
    \frac{\partial f_1}{\partial x_1} & \ldots & \frac{\partial f_1}{\partial x_n}\\
    \vdots & \ddots & \vdots\\
    \frac{\partial f_m}{\partial x_1} & \ldots & \frac{\partial f_m}{\partial x_n}\\
\end{pmatrix}
$$
increase $\frac{\partial f_t}{\partial x_i}$, decrease $\frac{\partial f_j}{\partial x_i}$, $\forall j\neq t$

Which $x_i$ should we change?
For a target $t$ increase those for which $\frac{\partial f_t}{\partial x_i}$ increases, while the combined classification of the other labels $f_j$ decreases

#### Saliency Map
Saliency map
: matrix $S$ defining intensity of inputs whose increase helps the most to accomplish the goal
$$
S(x_1, \ldots, x_{n-1}, t)[i] = 
\begin{cases}
    0 & \text{if }\frac{\partial f_t}{\partial x_i} < 0\text{ or }\sum_{j\neq t}\frac{\partial f_j}{\partial x_i} > 0 \\
    \left (\frac{\partial f_t}{\partial x_i}\cdot \left | \sum_{j\neq t}\frac{\partial f_j}{\partial x_i}\right | \right ) & \text{otherwise}\\
\end{cases}
$$

- Given FF NN $f$, an input $x$ and a target $t$:
    1. define $x' = x$
    2. while $f(x') \neq t$:
        1. compute saliency map $S(x_1', \ldots, x_n', t)$
        2. let $i$ be index maximising $S(x_1', \ldots, x_n', t)$
        3. $x_i' += \theta$

### Black-box attacks
- Attacker leverages prior knowledge:
    - Dataset type
    - Common architecture
- Attacker trains *other* model to find advesarial examples

#### Generate input
How to generate input-output examples?
Assume attacker can collect initial set of inputs similar to dataset, then synthesise more inputs and ask for classification to get new input-output examples

1. Define architecture for $\hat{f}$, the function approximating $f$
2. Let $D = \{(i_1, o_1), \ldots, (i_k, o_k)  \}$ be our initial training set
3. Repeat $N$ times:
    1. Train $\hat{f}$ on $D$
    2. Generate new inputs and query $f$ for their output
    3. Extend $D$ with these input-output examples

#### Generating inputs based on gradient
We generate inputs that improve our confidence in $\hat{f}$ by picking inputs whose neighbourhood show a large variance in $\hat{f}$
The gradient of (a single class) $\hat{f}$ points to where it changes most

1. Define architecture for $\hat{f}$, the function approximating $f$
2. Let $D = \{(i_1, o_1), \ldots, (i_k, o_k)  \}$ be our initial training set
3. Repeat $N$ times:
    1. Train $\hat{f}$ on $D$
    2. For every $(i, o) \in D$
        1. Compute $i' = i + \lambda\cdot\mathrm{sign}\left(\frac{\partial f_o}{\partial i_1}, \ldots, \frac{\partial f_o}{\partial i_n}\right)$
        2. Add $(i', f(i'))$ to $D$


### Checking Robustness of Neural Networks
Robustness
: Given a model $f$ and an input $x$, $f$ is robust for $x$ in a neighbourhood $N_x$ if 
$$
\forall x'\in N_x: f(x) = f(x')
$$
Common $N_x$:
$$
N_x^\epsilon = \left\{x' \vert\Vert x' - x \Vert_p < \epsilon \right\} \quad \text{with } p = 0, 1, 2, \infty
$$



# Probabilistic Security
