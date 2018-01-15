---
title: NetSec Summary
desription: Summary of 263-4640-00L Network Security at ETH Zürich HS17
tags: ETH, Zusammenfassung
---
NetSec Summary
===
Frédéric Vogel `vogelfr@ethz.ch`, ETH Zürich, HS17
[TOC]

<p style="page-break-after:always;"></p>

# Crypto Refresher

| Term                  | Definition                                     |
|-----------------------|------------------------------------------------|
| Secrecy               | Keep data hidden from unintended receivers     |
| Confidentiality       | Keep someone else's data secret                |
| Privacy               | Keep data about a person secret                |
| Anonymity             | Keep identity of a protocol participant secret |
| Integrity             | Ensure data is "correct"                       |
| Entity Authentication | Verify identity of other protocol participant  |
| Data Authentication   | Ensure data originates from claimed sender     |


| Primitive  | Definition           | Examples                                                                                                                   |
|------------|----------------------|----------------------------------------------------------------------------------------------------------------------------|
| Symmetric  | Shared-key, same-key | Block cipher (pseudo-random permutation PRP)<br>Stream cipher (pseudo-random generator PRG)<br>Message authentication code (MAC) |
| Asymmetric | Public-private key   | Diffie-Hellman key agreement<br>Public-key encryption<br>Digital signatures                                                      |
| Others     | Unkeyed              | One-way function<br>Cryptographic hash function                                                                               |

Authentication
: Verifies origin, reciever can *not* convince *third party* of origin

Signature
: Implies Authentication, verifies origin, reciever *can* convince third party of origin

Cryptographic Hash Function
: - One-way (given $y=H(x)$ one can not find $x'$ so that $H(x') = y$)
: - Weak collision resitance (given $x$ one can not find $x'\neq x$ so that $H(x) = H(x')$)
: - Strong collision resistance (one can not find $x'\neq x$ so that $H(x) = H(x')$)

One-Way Hash Chain
: 1. Pick random $r_N$ and public one-way function $F$
: 2. $r_i = F(r_{i+1})$
: Secret value: $r_N$, public value: $r_0$
: Use in reverse order of construction: $r_0, r_1, \ldots, r_N$

# Blockchain
## Bitcoin
### Building blocks
1. **"Proof of Work"** puzzles
2. **Public ledger** that prevents double spending

### Transactions
*System state* presented as set of transactions
Transactions have *inputs* and *outputs*

#### Transaction scripts
- Each *output* has small code snippet called `scriptPublicKey`
    - Represents conditions under which output can be used (redeemed)
    - E.g. Pay to Public Key Hash (P2PKH) transaction
        - script specifies public key and signature validation routine
        - redeeming routine must be signed using specified key
- Each *input* must have code snippet called `scriptSig`
    - Typically just complete public key and signature
- Simple stack-based scripting language
    - Script must evaluate to `true` for transaction to be considered valid
    - Scripting language not Turing-complete

#### Ownership
- No explicit user identites or accounts
    - public key hash = pseudonym (or address)
- Knowledge of matching pricate key gives ability to spend (or redeem) outputs
- Representing system state as set of transactions and using scripting language for redemption criteria

### Mining and consensus
- Transanction signing does not prevent using the same output multiple times (double-spending attack)
- All transactions are published in global, immutable log (ledger)
- Transactions are organized in series of *blocks* that containt hash of previous block $\rightarrow$ *blockchain*
- *Consensus* over blockchain needed to prevent double-spending attack. Different views $\rightarrow$ forks
    - Anyone can attempt to *extend* the chain, i.e. append new block
    - Create new valid block: solve computational *puzzle* called *Proof of Work* $\rightarrow$ *mining*
    - Mined blocks are published, participants should reject invalid blocks and continue mining in the longest chain

#### Proof of Work puzzle
- To mine block $B_i$ find nonce $N$ so that $H(H(B_{i-1})| txs | N) < target$
- Best known approach: brute force
- $target$ controls difficult (Bitcoin: new block approximately every 10 minutes)

#### Temporary forks
Two valid blocks mined roughly at the same time will create a temporary fork
- Two chains with *equal length*
- Participants choose *randomly* on which chain to continue mining
- Consensus is reached *gradually* (since one side of fork will become longer *eventually*)

#### Incentives
- *Block reward* for participants that successufully mine block (that is not later rejected)
- Incentivizes miners to work on longest chain
- Mining reward initially 50BTC, halves every 4 years until 2140 (no new coins after that)
- Miners can claim transaction fees (difference between inputs and outputs)

### Peer-to-peer network
- Used to announce new transactions and blocks
- Network delay important for consensus
    - Delay between block discovery and reception by all nodes increases possibility of temporary forks
    - Ability to control significant portion of network denies blocked nodes mining rewards and transaciton inclusion
- Any node can join network
    - Connect to random sample of other nodes (typically 8 *outgoing* connections)
    - Accept up to ~100 incoming connections
    - Well-connected random network of global scale

### Stability
- **Eventual consensus:** All compliant nodes agree on longest chain eventually
- **Exponential convergence:** Probability of fork is exponentially proportional to its depth in chain
- **Liveness:** Valid transactions will be added to the chain
- **Correctness:** All transactions in the longest chain are valid
- **Fairness:** Miner with fraction $f$ of computing power will mine fraction $f$ of all blocks

Bitcoin unstable if single miner controls more than half of all computing power

