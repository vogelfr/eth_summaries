---
title: Intro to Negitiation Summary
desription: Summary of 363-1039-00L Introduction to Negotiation at ETH Zürich FS18
tags: ETH, Zusammenfassung
---
Intro to Negitiation Summary
===
Frédéric Vogel `vogelfr@ethz.ch`, ETH Zürich, FS18
[TOC]

<p style="page-break-after:always;"></p>

# Definitions
Negotiation
: Process resolving a conflict of needs between 2 or more parties to arrive at a joint decision that entails joint consequences, or payoffs, for each party.

Distributive negotiation
: Competitive negotiation over one issue, a win-lose situation.

Integrative negotiation
: Negotiation that can look for win-win solutions or problem solving in order to have mutual gain.

Mediation
: Any action taken by an actor that is not a direct party to the crisis, that is designed to reduce or remove one or more of the problems of the bargaining relationship, and therefore to facilitate the termination of the crisis itself.

Descriptive orientation
: How decisions are made. How and why individuals act the way they do.

Normative orientation
: How decisions should be made. How *idealized, rational* people *should* act.

Prescriptive orientation
: How decisions could be made better. What *can* a *real* person actually do to make better decisions?

# Rationality
| Symbol | Desription |
| -------- | -------- |
| $a \in \mathbf{A}$    | Action, set of alternative actions |
| $\omega \in \mathbf{\Omega}$ | Outcome, set of all possible outcomes |
| $u$ | Utility |
| $\lambda$ | Lottery, set of probabilities for the occurence of every $\omega \in \mathbf{\Omega}$ |
| $p(\omega\vert\lambda)$ | Probability of outcome $\omega$ in lottery $\lambda$ |

Utility function
: Function $u: \mathbf{\Omega} \rightarrow \mathbb{R}$ such that:
$$u(\omega_1) > u(\omega_2) \Leftrightarrow \omega_1 \succ \omega_2,\\u(\omega_1) = u(\omega_2) \Leftrightarrow \omega_1 \sim \omega_2$$

Payoff function
: Function $\pi: \mathbf{A} \rightarrow \mathbb{R}$

Expected utility
: With $\lambda(a)$ denoting a lottery chosen by selecting action $a$
$$\pi(a) = \sum_{\omega \in \mathbf{\Omega}} p(\omega \vert \lambda(a)) \cdot u(\omega)$$

## Definition of rationality
### Rational under certainty
An individual is *rational under certainty* if his preferences for outcomes $\omega \in \mathbf{\Omega}$ satisfy the following conditions:
1. Completeness
**Either** $\omega_1 \succcurlyeq \omega_2$ **or** $\omega_2 \succcurlyeq \omega_1$
2. Transitivity
**If** $\omega_1 \succcurlyeq \omega_2$ and $\omega_2 \succcurlyeq \omega_3$, **then** $\omega_1 \succcurlyeq \omega_3$

### Rational under uncertainty
An individual is *rational under uncertainty* if his preferences for lotteries satisfy the following conditions:
1. Completeness
**Either** $\lambda_1 \succcurlyeq \lambda_2$ **or** $\lambda_2 \succcurlyeq \lambda_1$
2. Transitivity
**If** $\lambda_1 \succcurlyeq \lambda_2$ and $\lambda_2 \succcurlyeq \lambda_3$, **then** $\lambda_1 \succcurlyeq \lambda_3$
3. Monotonicity
**If** $\lambda_1 \succ \lambda_2$ and $q_1 > q_2$, **then** $q_1 \lambda_1 + (1-q_1)\lambda_2 \succ q_2 \lambda_1 + (1-q_2)\lambda_2$
4. Continuity
**If** $\lambda_1 \succcurlyeq \lambda_2$ and $\lambda_2 \succcurlyeq \lambda_3$, **then** there exists probabilty $q$ such that $\lambda_2 \sim q \lambda_1 + (1-q)\lambda_3$
5. Independence
**If** $\lambda_1 \succ \lambda_2$, **then** $q \lambda_1 + (1-q)\lambda_3 \succ q \lambda_2 + (1-q)\lambda_3$

Not very useful in practice since test not possible.

<p style="page-break-after:always;"></p>

# Basic Concepts
Target Point
: Aspired goal of negotiator

Resistance Point
: Negotiator's bottom line (reservation price)

Zone of Potential Agreement (ZOPA)
: Spread between resistance points of parties

### Steps in integrative solution
1. Sides need to agree on *what the problem is*
2. What are the *interests* behind the positions
3. Generate *alternative* solutions:
    - Redefine the problem (Expand pie, logroll, offer compensation in other area, minimize costs, bridge)
    - Generate solutions for the given problem
4. Make a *list* of solutions
5. Prioritize options and *reduce* the list
6. Select a *solution*

## Getting to Yes
5 principles:
1. People: Separate people from problem
2. Interests: Focus on interest, not positions
3. Options: Invent options for mutual gain
4. Criteria: Insist on using objective criteria
5. Alternatives: Know your best alternative to a negotiated agreement (BATNA)

<p style="page-break-after:always;"></p>

## Case 1: Nuclear Waste
![](https://i.imgur.com/SCO50WD.png)

**Goal** developing a negotiation framework
- Negotiation are voluntary, therefore framework has only a value if all parties accept it

Problems:
- Framework requires concensus with all involved parties
- Negotiation objective was not clear
- Negotiation parties were not clear
- Beginning of negotiation was not clear
- Entry into force of contract
- Commitment to negotiation framework

Result:
- Declaration
- 20 signatures
- Negotiation framework (12 articles)

<p style="page-break-after:always;"></p>

## Examples
### Linear programming
![](https://i.imgur.com/XAfOvCI.png)

### Secretary problem
- Look at $\frac{n}{\mathrm{e}}$
- Take first that is better than all others before

### Casino
1. Probabilities known: choose action to maximize your expected payof
2. Probabilities unknown: assume choises are equally possible, choose action to maximize expected payoff
3. Maxmin: choose maximum of minimum payoff
4. Maxmax: choose maximum of maximum payoff
5. Optimism coefficient $\theta$: choose maximum of $\theta(\max_{row}) + (1-\theta)(\min_{row})$ (Mix between maxmin and maxmax)
6. Maxmin on regret matrix: regret matrix $R_{i,j} = M_{i,j} - \max(M_{:,j})$

<p style="page-break-after:always;"></p>

# Game Theory
## Zero-sum games
- Sum payoffs in each cell is 0
- If strategy forms equilibrium (*saddle point*) then maxmin payoff to A = - maxmin payoff to B

## Nash-Equilibrium
- Given the strategies of the other players, decide what maximizes your payoff
- Nash-Equilibrium: Every player's action is a best response to the other player's actions
    - No one has an incentive to change his strategy **unilaterally**!
- Mixed strategy profile:
    - Player 1 chooses A with probability $p$ and B with $1-p$
    - Player 2 is indifferent between playing A or B if the following holds: $\pi_2(A, A)\cdot p + \pi_2(A, B)\cdot (1-p) = \pi_2(B, A)\cdot p + \pi_2(B, B)\cdot (1-p)$

## Pareto Optimal
- An outcome is pareto optimal if there is no other outcome that increases payoff to one player *without* decreasing payoff to another player