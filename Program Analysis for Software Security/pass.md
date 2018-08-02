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
: For any output $y$, if $r$ is chosen from probability distribution with high min-entropy, it is infeasible to find $x$ such that $h(r || x) = y$

Search puzzle
: Given puzzle ID $id$, chosen from a probability distribution with high min-entropy, and an output range $T \subseteq Y$ find a solution $x$ such that $h(id || x) \in T$. Because of puzzle-friendliness no strategy is better than trying random values of $x$

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





# Attacks and Defenses of Deep Learning

# Probabilistic Security
