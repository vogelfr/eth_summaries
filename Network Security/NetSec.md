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


# Domain Name System (DNS) Security
Domain Name System (DNS)
: Mapping of name to resource of several type

### DNS Namespace Hierarchy
<img src="https://i.imgur.com/90S1rFQ.png" style="float:right; width:300px"></img>
- Hierarchical namespace to scale - tree structure down from *root* level `.`
- *Top-level domains (TLD)* below root (`.ch`, `.com`, `.edu`)
- Domains are namespaces
    - Everything below `.com` is the `com` domain
- Fully-qualified-domain name (FQDN)
    - E.g. `mail.ethz.ch` or `svn.netsec.inf.ethz.ch`

### Protocol & Elements
- No encryption, authentication nor integrity built into original protocol
    - DNSSec extensions provide that
- Name server (Servers that map names to objects)
    - Authoritative: server is authoritative for a specific zone
    - Caching/Resolver: server resolves domains recursively, caches results
- Resolver (Client side of DNS resolution)
    - Stub resolver: library that forwards request to name server
    - Recursive resolver: processes DNS resolution iteratively to provide full answer

### Record types
| Record Type | Description               | Usage                                                         |
|-------------|---------------------------|---------------------------------------------------------------|
| A           | Address record            | Maps FQDN into an IP address                                  |
| PTR         | Pointer record            | Maps an IP address into FQDN                                  |
| NS          | Name server record        | Denotes a name server for a zone                              |
| SOA         | Start of Authority record | Specifies many attributes concerning the zone                 |
| CNAME       | Canonical name record     | Defines alias name and maps it to absolute (canonical) name   |
| MX          | Mail Exchanger record     | Used to redirect email for given domain or host to other host |
| TXT         | Text record               | Free form text of any type                                    |
### DNS Resolution process and possible attack vectors
![](https://i.imgur.com/9tSM8Qj.png)


- Cached results expire after a period of time (Time To Live (TTL))

#### Root Zone
- DNS Root Zone is served by 13 server clusters which are authoritative for queries for the top-level domains
- **every name resolution in the Internet either starts with a query to a root server, or, uses information that was once obtained from a root server**
- `a.root-servers.net` to `m.root-servers.net`
- All other name servers use hard coded config file to lookup IP address for root servers

#### Top-Level Name Server
- Manages reservation of second-level domains (SLD) below given TLD
- Must be accredited by TLD registry and/or country code TLD registry (ccTLD)

#### Autoritative Domain Server
- Managed by private entities
- Has all records for a zone configured and can provide final/authoritative information

|                                   | Authoritative Name Server                                                     | Cache/Recursive Resolver                                                    |
|-----------------------------------|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| Availability                      | Should be able to respond to lookup queries from any computer on the Internet | Should only respond to lookup queries that originate from a “local” network |
| Types of queries it should answer | Non-recursive queries                                                         | Recursive queries                                                           |
| Records it should resolve         | Should only respond with data it is authoritative about                       | Should attempt to resolve any legitimate request                            |

## DNS Abuses & Attacks
![](https://i.imgur.com/cjUl5Kh.png)

### DNS Spoofing
![](https://i.imgur.com/KezKbyv.png)

### Distributed Reflection / Amplification DDoS
![](https://i.imgur.com/atm0Xfa.png)

### DNS Cache poisoning
![](https://i.imgur.com/3Els4SY.png)

### DNS/Domain Hijacking - LAN
![](https://i.imgur.com/tWfPGf4.png)

## DNSSEC
| Record Type | Description | Usage |
|-------------|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| RRSIG | Resource record signature | DNSSEC signature for record set. Resolvers verify signature with public key, stored in DNSKEY-record |
| DNSKEY | Public key record | Contains public key that a resolver uses to verify DNSSEC signatures in RRSIG-records |
| DS | Delegation signer record | Holds name of delegated zone. DS record is placed in parent zone along with delegating NS-records. References a DNSKEY-record in sub-delegated zone |
| NSEC | Next secure record | Contains link to next record name in zone and lists record types that exist for record's name. DNS Resolvers use NSEC records to verify non-existence of record name and type as part of DNSSEC validation. |

- Certificate and trust chain based approach for authenticating DNS responses
- SIG verification is done by caching forwarders
- Political and technical worries of how to manager the master keys

# Public Key Infrastructure (PKI)
Trust anchor / Trust root
: Self-signed certificates of public keys that are allowed to sign other certificates
