# Transactions

## Introduction to transactions
A NEO transaction is a signed data package with an instruction for the network, for example a user indicating that he wants to transfer assets to another address. Each NEO block in the blockchain ledger contains one or more transactions, making each block a transaction batch. To use the NEO blockchain we need to understand how transactions work.

- **[Structure of transactions](transactions.md)**
  - [type](transactions.md#type)
  - [version](transactions.md#version)
  - [attributes](transactions.md#attributes)
  - [outputs](transactions.md#outputs)
  - [inputs](transactions.md#inputs)
  - [scripts](transactions.md#scripts)

### Transaction types
There are several different types of transactions on NEO, each with a different purpose and different properties. Some previously used transaction types are now deprecated or removed from the network, so these should not be used when creating new transactions on the MainNet.

- **[Transaction types](types.md)**
  - [MinerTransaction](types.md#minertransaction)
  - [ClaimTransaction](types.md#claimtransaction)
  - [ContractTransaction](types.md#contracttransaction)
  - [StateTransaction](types.md#statetransaction)
  - [InvocationTransaction](types.md#invocationtransaction)
  - [Registering assets](types.md#registering-assets)

### Transaction fees
Some transactions on the NEO network require fees. The network uses a fee structure with two types of fees; system fees and network fees. All fees are paid in the native utility token GAS (NeoGas).

- **[Transaction fees](fees.md)**
  - [System fees](fees.md#system-fees)
  - [Network fees](fees.md#network-fees)
  - [Utility fee in applications](fees.md#utility-fee-in-applications)

## Broadcasting
Once a transaction has been created it can be sent to a network peer node. If the peer node determines the transaction as being valid, it will be placed in the memory pool and distributed through the rest of the network. Eventually a consensus node (validators on the NEO blockchain) will receive the transaction and process it by including it in a block.

## Tools
There are various tools and libraries available to create and broadcast transactions to the NEO network:

- [neon-js](https://github.com/CityOfZion/neon-js) is a JavaScript library that can be used to build extensive transactions in any JavaScript application
- [neo-python](https://github.com/CityOfZion/neo-python) is a full NEO node that can be used as an SDK to create transactions and interact with the NEO network
- [neo-gui](https://github.com/neo-project/neo-gui/) is the official Windows GUI to interact with the NEO network
- [neo-cli](https://github.com/neo-project/neo-cli/) is the official full node implementation that runs as a command line implementation and can be used to create and broadcast transactions to the NEO network
- Many other wallet or node implementations like [neo-lux](https://github.com/CityOfZion/neo-lux) and [neo-thinsdk-cs](https://github.com/NewEconoLab/neo-thinsdk-cs) in C#
