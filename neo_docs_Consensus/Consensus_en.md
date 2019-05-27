# Introduction

Multi-Agent Systems (MAS) are the core of Internet-of-Things (IoT), in which autonomous devices are able to interact with each other following their specific desired.
Blockchain consensus operates in the same manner, autonomous nodes should reach an agreement throughout a negotiation protocol.
Three pillars of MAS protocols for reaching agreements are: voting, auction and coordination.

We believe that Blockchain protocols are able to safely perform distributed rational decision making during its consensus.

In particular, NEO protocol, so-called Delegated Byzantine Fault Tolerance (dBFT), has its design rooting the works of Practical Byzantine Fault Tolerance, from Miguel Castro and Barbara Liskov around 1999.

This tutorial will introduce the basic steps for understanding the importance of designing and developing such mechanism for our Ecosystem.

## What we expected that you will learn

After reading this tutorial it is expected that you will learn:

- Distinguish Proof-of-Work and consensus based on coordination;
- Learn more about cryptography and multi-sig accounts;
- Learn about Byzantine Fault Tolerant systems;
- Understand the beauty of **one block finality**;


## The roots of proof-of-works

Prof-of-work was mentioned by Satoshi Nakamoto as a mechanism that could cross CPU and voting, namely one-CPU-one-vote.
The basic idea behind this is to create a protocol in which blocks are generating every `X` seconds.
If blocks are generated faster or slower, difficulty would be reduced.

For instance, let's take the word `NEO Ecosystem` and convert it to a Hash256 `4bf65a74b608f6b785286b5da1d39ceb36ed87b62fee6ba97a65ecd4655b7661`.
Now let's take the word `NEO Ecosystem+Nonce`, let's say `NEO Ecosystem+1` and we would get the following Hash256 `0739bcb67c6e934c669b95d65f1c98cdd67bcef0ef8ab22a7c1b4404f0e11450`.
Now, let's see `NEO Ecosystem+12345678` and we would get `011c65a33085565814548bc2860a1a3b1c68b627581381382447147788b0240c`, which has its beginning with `01`, less significative than `07`.
Now, start to play with this nonce until you gets words that should start with `0000000000` and you are going to verify how hard this task is.
Hashes are a kind of cryptographic signature for any data file, which has its basin on the classic SHA-256 algorithm that generates nearly unique fixed-size 32 bytes words.
Hashes are so unidirectional that any known algorithm can revert its information, even with the assist of quantum computers.

At the early times of BTC mining, in 2009, a standard computer could produce around 1 megahash per second.
Since then, mining has evolved from GPUs, FPGAs and ASICs, reaching the impressive dedicated power of generating 13 TH/s per second, around 13000000 faster than in the beginning.

If computers and knowledge dedicated in generating hashes has been evolving so drastically, why would not be the same for digital signatures and communication protocols?
IoT has been a hot topic and focus of dedication of renowned researchers and industries, and its roots are related with communication and agreement between autonomous devices.

The roots of NEO dBFT are digital signatures shared by a group of nodes (autonomous agents), which were selected by the majority of all NEO holders.

## Coordination x Proof-of-Stake x Voting

Notoriously, the so-called Proof-of-Stake (PoS) based algorithms have similarities with what we mentioned about MAS.
The core idea of PoS is to leave decision making for those that have more financial health in the Ecosystem, which would motivates them to keep the network running safely and efficiently.
As can be noticed, if we turn this power to those that are part of the ecosystem we would have a similar PoS in which voting would be the main mechanism for selecting such nodes.
The power of voting can even remove those that are acting as promised.

Summarizing this, we should see that coordination is the core for reaching agreement in decentralized scenarios.
Coordination not in the sense of a centralized coordinator, but in the sense that multiple goals are considered when a decision is being taken.

## pBFT

## dBFT

### A 4-node consensus

As you may already known, an address in the NEO blockchain 2.x is formed by `21`, which means ["Push 34 bytes on the evaluation stack"](https://github.com/neo-project/neo-vm/blob/f81c3039d5fb4417b3c1ad780378c7f92499964a/src/neo-vm/OpCode.cs#L144), the public key and `ac`, which is an opcode that invokes an script for checking witness of the address.

We suggest readers to take a look at the following article:

- [Understanding MultiSig on NEO](https://medium.com/neoresearch/understanding-multisig-on-neo-df9c9c1403b1).

Let's consider nodes with the following pubkeys (21+rootOfPubKey+ac):

- N1: `2102103a7f7dd016558597f7960d27c516a4394fd968b9e65155eb4b013e4040406eac`
- N2: `2102a7bc55fe8684e0119768d104ba30795bdcc86619e864add26156723ed185cd62ac`
- N3: `2102b3622bf4017bdfe317c58aed5f4c753f206b7db896046fa7d774bbc4bf7f8dc2ac`
- N4: `2103d90c07df63e690ce77912e10ab51acc944b66860237b608c4f8f8309e71ee699ac`

Basically, a trivial way to create a multi-signature account can be done using the following script:

`532102103a7f7dd016558597f7960d27c516a4394fd968b9e65155eb4b013e4040406e2102a7bc55fe8684e0119768d104ba30795bdcc86619e864add26156723ed185cd622102b3622bf4017bdfe317c58aed5f4c753f206b7db896046fa7d774bbc4bf7f8dc22103d90c07df63e690ce77912e10ab51acc944b66860237b608c4f8f8309e71ee69954ae`

which is: `53` (number of signers) + `21` + `02...6e` + `21` + `02...62` + `21` + `02...c2` + `21` + `03...99` + `54` (number of owners) + `ae`

We a holders pick those 4 nodes as their desired validators, the following script will be one signing every block, with the following public address: `AZ81H31DMWzbSnFDLFkzh9vHwaDLayV7fU`.
The latter can be achieved by converted that script to "Scripthash big-endian" and then converting to base-58.
We suggest readers to access [NeoCompiler-Eco](https://neocompiler.io/#!/ecolab/conversor) if they want to play with these conversors.

![multisig 3/4](./multisig_3_4.png)

![scripthash to address base58](./scripthash_address.png)

### A simple single-node consensus
