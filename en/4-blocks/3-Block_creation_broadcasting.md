# Block creation
## Consensus nodes
As discussed in [Part 1](1-Introduction_to_blocks_and_blockchain.md), Neo uses the dBFT mechanism to generate new blocks. In short, this means there are a select amount of consensus nodes, elected by the network.

For each new block that needs to be generated, one of these nodes is elected as the speaker. In the best case scenario, where we have no byzantine nodes, no network failures etc, the speaker node will propose a new block, distribute it and all other consensus nodes will agree to this block. When they get enough agreements from other consensus nodes, they see this block as final. For more details on the dBFT consensus mechanism, have a look at the [whitepaper](https://docs.neo.org/en-us/basic/consensus/whitepaper.html).

## The new Block
When a user wants to send a transaction, typically, his wallet will create and sign the transaction and send that to either an RPC or P2P node. Both RPC and P2P nodes will relay (valid) transactions across the network, to eventually reach one of the consensus nodes. More can be read in the section [5. Network](../5-network/). [This medium post](https://medium.com/neoresearch/understanding-neo-network-in-five-pictures-e51b7c19d6e0) also gives a great summary.

Once the valid transaction has arrived at the consensus nodes they are stored in the mempool. When creating a new block, transactions will be chosen from this mempool to be included. Every block can contain up to 500 transactions, from which the first one always is of type MinerTransaction (**TODO ADD LINK TO CHAPTER 3 MINER TYPE ONCE CHAPTER IS WRITTEN**).

## Transaction fees

The consensus nodes will then select transactions to include in the new block that will be created. When a user wants to make sure his transaction is executed as fast as possible, he can choose to include a transaction fee. There are currently no transaction fees (limited to 21 transactions per block). The user can however choose to pay transaction fee for priority. The larger the fee, the more likely it is this transaction will be included in the next block. This is because the consensus node creating the next block (the speaker) is allowed to keep all transaction fees of the transactions it includes in the block. This makes including transactions with a higher fee more interesting for the node than free transactions. This transaction fee is not a specific field, rather, it is the remaining Gas that was not accounted for when spending the UTXO.

This follows from the fact that a UTXO is not divisible and needs to be spent in a whole. For example, when an UTXO has a value of 500 GAS, spending that UTXO will spend the full 500 GAS. When the transaction is meant to send only 5 GAS, the other 495 GAS need to be send back to yourself, accounting for the 'change'. Compare this to handing the cashier a $100 bill when you only need to pay 1$. You expect 99$ change in return. With an UTXO, this is similar. You need to pay this change back to yourself in the same transaction.

Every unit of an UTXO that is not explicitly sent to an address, is interpreted as transaction fee. So when you spend an UTXO with a next transaction, sending only 5 GAS, if you do not send the remaining 495 GAS back to yourself in the same transaction, that 495 GAS will be seen as the transaction fee. You're in fact telling the consensus node to *keep the change*.

As a normal user however, you do not need to worry about these details. Most advanced wallet software will give you the option to specify the transaction fee, and generate the transaction in such a way that the change will be sent back into your account.

## Other fees
A transaction fee is used to gain priority on the network. There are however fees with specific puroses.
- ***Smart Contract Creation*** The current free to deploy a new contract on the Mainnet is 500 GAS. For structured development, it is advised to start development on a local testnet (link docker). Once the Smart Contract is stable, you can apply for Testnet funds here (link), for final validations. Once you are certain your Smart Contract is implemented correctly, only then should you deploy it onto the Mainnet, as it is irrevokable - meaning you will not get your 500 GAS back even when you destroy the contract.

- ***Smart Contract Execution*** To be able to execute a Smart contract, the nodes need to perform specific computations for you. In order to compensate the nodes for this effort, a system fee should be added to the transaction that is executing the contract. Currently, a fee is not needed to execute contracts that require less than 10 GAS. [This page](https://docs.neo.org/en-us/sc/systemfees.html) gives the complete overview of the system fees required to execute a Smart Contract. For each operation, the required fee for that operation is mentioned.
