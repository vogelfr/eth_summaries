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


# Blockchain Security

# Attacks and Defenses of Deep Learning

# Probabilistic Security
