# Transaction fees

## Overview of transaction fees
To use the NEO network there are fees for some transactions. The network uses a fee structure with two types of fees. All fees are to be paid in the system utility token (NeoGas).

| Type        | Description                                                         |
|-------------|---------------------------------------------------------------------|
| Network Fee | Fee to pay the validator for including the transaction in the block |
| System Fee  | Fixed fee to pay the network                                        |

### Network fee
The currently optional network fee is calculated by the difference between [inputs](transactions.md#inputs) and [outputs](transactions.md#outputs) for the `GAS` system utility token. The network allows 20 low priority transactions per block without a network fee. Other transactions with a network fee will be prioritized by the amount of network fee. Paying a higher network fee will result in a faster transaction. The network fee can be collected and distributed by the validator to any contract address.

### System fee
The system fee is a fixed fee calculated by [transaction type](types.md) and instructions to be executed by the virtual machine. Generally speaking the more impact a transaction has on the network resources, the more the transaction will cost. There is a system fee discount of `10` GAS for each transaction, so most user interaction with the network and smart contracts will be free.

#### System calls

| System calls                | Fee (GAS)                                                                       |
|-----------------------------|---------------------------------------------------------------------------------|
| *Default*                   | `0.001` for all system calls                                                    |
| `Runtime.CheckWitness`      | `0.2`                                                                           |
| `Blockchain.GetHeader`      | `0.1`                                                                           |
| `Blockchain.GetBlock`       | `0.2`                                                                           |
| `Blockchain.GetTransaction` | `0.1`                                                                           |
| `Blockchain.GetAccount`     | `0.1`                                                                           |
| `Blockchain.GetValidators`  | `0.2`                                                                           |
| `Blockchain.GetAsset`       | `0.1`                                                                           |
| `Blockchain.GetContract`    | `0.1`                                                                           |
| `Transaction.GetReferences` | `0.2`                                                                           |
| `Account.SetVotes`          | `1`                                                                             |
| `Validator.Register`        | `1000`                                                                          |
| `Contract.Create`           | `100` per contract, `400` for enabled storage, `500` for enabled dynamic invoke |
| `Contract.Migrate`          | `100` per contract, `400` for enabled storage, `500` for enabled dynamic invoke |
| `Storage.Get`               | `0.1`                                                                           |
| `Storage.Put`               | `1` per kiB                                                                     |
| `Storage.Delete`            | `0.1`                                                                           |

#### Instructions

| Instruction               | Fee (GAS)                                           |
|---------------------------|-----------------------------------------------------|
| *Default*                 | `0.001` for all instructions in the virtual machine |
| `OpCode.PUSH16` (or less) | `0`                                                 |
| `OpCode.NOP`              | `0`                                                 |
| `OpCode.APPCALL`          | `0.01`                                              |
| `OpCode.TAILCALL`         | `0.01`                                              |
| `OpCode.SHA1`             | `0.01`                                              |
| `OpCode.SHA256`           | `0.01`                                              |
| `OpCode.HASH160`          | `0.02`                                              |
| `OpCode.HASH256`          | `0.02`                                              |
| `OpCode.CHECKSIG`         | `0.1`                                               |
| `OpCode.CHECKMULTISIG`    | `0.1` per signature                                 |

## Utility fee in applications
Any deployed application in the network is able to require an application fee in order to use the smart contract. This is often a NEP-5 compatible utility token, but smart contracts are able to charge GAS as well.
