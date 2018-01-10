---
title: SysSec Summary
desription: Summary of 252-1414-00L System Security at ETH Zürich HS17
tags: ETH, Zusammenfassung
---
SysSec Summary
===
Frédéric Vogel `vogelfr@ethz.ch`, ETH Zürich, HS17
[TOC]

<p style="page-break-after:always;"></p>

# Side Channels
Some implementations of provably secure cryptosystems can be broken by observing the side information leaked by these implementations.

## Timing cryptanalysis
Cryptosystems vulnerable when
1. *execution time* depends on key *for different input data*, and
2. *partial execution time* can be measured

### Protection
Equalizing the time of all operations independently of the input values

But:
- Increases average time of operation
- Difficult to build soft- and hardware that holds property

#### Masking (Kocher):
- Chose *random* $X$, different for each message $M$
- $S = [(m X)^d \mod n] \cdot [(X^{-1})^d \mod n]$
- $= [m^d \mod n] \cdot [(X \cdot X^{-1})^d \mod n] \mod n$
- $= m^d \mod n$

$(X^{-1})^d \mod n$ can be computed in advance

#### Masking (Rivest):
- Chose *random* $Y$
- $S = ([m (Y^{-1})^e]^d \mod n) \cdot Y \mod n$
- $= [m^d \mod n] \cdot [(Y^{-1})^{ed} \mod n] \cdot Y \mod n$
- $= m^d \mod n$

$(Y^{-1})^e \mod n$ can be computed in advance

## Power Analysis Attacks
An attack that enables to retreive a secret by observing power consumptions of a device

### Simple Power Analysis
Measure power consumption of instruction sequences that depend on the key
![](https://i.imgur.com/STK6fwM.png =500x)
If squaring is followed by other squaring that bit is 0, if squaring is followed by multiplication that bit is 1.

### Differential Power Analysis
Power consumption depends not only on the type of executed instruction but also on values of operands:
- Storing data to register or memory (storing 0 *vs* storing 1)
- Shifts and rotations
- Logical and arithmetic operations

E.g. attack on DES: Half of key is in C register and rotated each round in deterministic way.

### Protection
1. Desynchronization
2. Noise generator
3. Filter at the power input + physical shielding
4. Software balancing
5. Hardware balancing

## Cache timing attacks
Table lookups/memory addresses are derived from key+plaintext. Depending on where the data is (cache, memory) access time will vary leaking information.
### Block ciphers
- Data in blocks of $N$ bits
- Each block seen as symbol (Alphabet size: $2^N$)
- Block of plaintext mapped to block of ciphertext ($2^N!$ mappings)
- Secret key indicates which mapping to use

Problem: Key to big for ideal block cipher ($\log_2(2^N!) bits$, $10^{11}GB$ when $N=64$})

Solution: Key of $K$ bits to specify random subset of $2^K$ mappings

#### Shannon's Confusion and Diffusion Principle

Diffusion
: Ciphertext bits should depend on plaintext bits in complex way (if $M_i$ is changed $C_i$ should change with $p=0.5$)

Confusion
: Each bit of $C$ should depend on the whole key (change of one bit in the key changes entire ciphertext)

#### Attack on AES
1. Offline phase:
    For each byte of the key one index value will have slowest lookup time, find value for each byte (e.g. k[13] is slowest when index value is 8)
2. Attack phase:
    - Measure time needed by server to encrypt each sent message
    - Observe that average AES time is maximum when e.g. $n[13] = 147 \Rightarrow k[13] \oplus 147$ takes longest
    - Compare with value from offline phase (k[13] \oplus 147 = 8 \Rightarrow k[13] = 155)

Problem: Difficult to write *constant*, *high-speed* AES Software
Solution: Special AES instruction on chip decouples AES from cache


<p style="page-break-after:always;"></p>

# Readings
## [Remote Timing Attacks are Practical](https://crypto.stanford.edu/~dabo/papers/ssl-timing.pdf)
RSA decryption implementations often have branches depending on the input. Those can be used in a side-channel timing attack, even across a network or VMs.

### Attack
Timing differences in OpenSSL to calculate $g^d \mod q$ come from two sources:
- Extra reductions in a Montgomery reduction
    As $g$ approaches a multiple of the factor $q$ the number of extra reductions increases. When we are just over a multiple of $q$ the number decreases.
    Decryption of $g<q$ is slower than $g>q$.
- Karatsuba *vs* normal multiplication
    When $g$ is just below a multiple of $q$ OpenSSL uses Karatsuba. When $g$ is just over a multiple of $q$ then OpenSSL uses normal multiplication.
    Decryption of $g<q$ is faster than $g>q$.
    
The key can be found by using the timing results with varying parameters:
- Number of samples for particular ciphertext
    From 5 samples on decryption time converges and has low variance.
- Neighbourhood size $n$
    For every bit of $q$ the decryption time for a neighbourhood of values $g, g+1, g+2, \ldots, g+n$ is measured

### Defences
- RSA blinding:
    $x = r^eg \mod N$ is calculated before decryption where $r$ is random. After $x$ is decrypted normally $x^e/r\mod N$ is calculated to get the cleartext. Since $r$ is random the decryption will take random time.
- Make the decryption time independent of input: use single multiplication mehtod and always perform extra reduction in Montgomery's algorithm.

<p style="page-break-after:always;"></p>

## [Cache-timing attacks on AES](http://cr.yp.to/antiforgery/cachetiming-20050414.pdf)
The complete AES key can be recovered from a network server on another computer. The attack works by exploiting a side channel of many AES implementations, namely **S-box lookups**. The index of the lookup is input dependent and since table lookups do not take constant time they leak information.
#### Skipping an operation is faster than doing it
Input-dependent branches result in different execution time depending on the branch taken.


#### Cache is faster than DRAM
If parts of the S-box are stored in the cache but not all of it information will be leaked by the lookup time.

- *L1-table-lookup instruction. Same as below*

#### L1 cache is faster than L2 cache
Similar to previous.

- *L1-table-lookup instruction: (1) ensures entire table is in L1, (2) loads a selected table entry in a constant number of CPU cycles.*

#### Cache associativity is limited
Depending on the layout of the Cache the different AES operations will kick used AES array out of cache.

- *Compress the S-boxes to fit entirely in cache*
- *If AES input and key must be loaded from uncontrolled location assume AES S-box lines have been kicked out of cache -> reload entire S-box.*

#### Code can be interrupted
Multithreading prevents any guarantee that no cache line will be kicked out *during* the execution of AES. 

- *Include cryptographic software into OS-kernel.*
- *If AES computation is interrupted restart from scratch.*

#### Stores can interfere with loads
Even when the data is kept in cache loads from a cache line takes slightly more time if it involves the same cache line mod $x$ as a recent store to the cache.

- *Use compressed S-boxes, shift the stack to a position just below the beginning of the AES S-boxes.*

#### Cache-bank throughput is limited
Loads from cache can bump into each other, taking slightly different amounts of time depending on their addresses.

- *Spread the loads to take place in separate cycles*

