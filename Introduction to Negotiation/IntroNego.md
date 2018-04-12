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
: How decisions could be made better. What *can

# Rationality

| Symbol | Desription |
| -------- | -------- |
| $a \in \mathbf{A}$    | Action, set of alternative actions |
| $\omega \in \mathbf{\Omega}$ | Outcome, set of all possible outcomes |
| $u$ | Utility |
| $\lambda$ | Lottery, set of probabilities for the occurence of every $\omega \in \mathbf{\Omega}$ |
| $p(\omega\vert\lambda)$ | Probability of outcome $\omega$ in lottery $\lambda$

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
