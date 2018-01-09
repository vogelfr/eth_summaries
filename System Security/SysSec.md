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

