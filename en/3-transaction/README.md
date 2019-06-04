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
There are several different types of transactions with each a different purpose and different properties. Some previously used transaction types are now deprecated or even removed from the network. They should not be used anymore in new transactions on mainnet.

- **[Transaction types](types.md)**
  - [MinerTransaction](types.md#minertransaction)
  - [ClaimTransaction](types.md#claimtransaction)
  - [ContractTransaction](types.md#contracttransaction)
  - [StateTransaction](types.md#statetransaction)
  - [InvocationTransaction](types.md#invocationtransaction)
  - [Registering assets](types.md#registering-assets)

### Transaction fees
To use the NEO network there are fees for some transactions. The network uses a fee structure with two types of fees. All fees are to be paid in the system utility token (NeoGas).

- **[Transaction fees](fees.md)**
  - [System fees](fees.md#system-fees)
  - [Network fees](fees.md#network-fees)
  - [Utility fee in applications](fees.md#utility-fee-in-applications)

## Broadcasting
Once the transaction is created it can be sent to a network node. If the network node validates the transaction it will be placed in the memory pool and distributed through the rest of the network. Eventually a validator will receive the transaction and process it in the next block.

## Tools
There are various tools and libraries available to create and broadcast transactions to the NEO network:

- [neon-js](https://github.com/CityOfZion/neon-js) is a JavaScript library to build extensive transactions in any JavaScript application
- [neo-python](https://github.com/CityOfZion/neo-python) is a full NEO node that can be used as SDK to create transactions and interact with the NEO network
- [neo-gui](https://github.com/neo-project/neo-gui/) is the official Windows GUI to interact with the NEO network
- [neo-cli](https://github.com/neo-project/neo-cli/) is the official full node implementation that runs as a command line implementation and can be used to create and broadcast transactions to the NEO network
- Many other wallet or node implementations like [neo-lux](https://github.com/CityOfZion/neo-lux) and [neo-thinsdk-cs](https://github.com/NewEconoLab/neo-thinsdk-cs) in C#
