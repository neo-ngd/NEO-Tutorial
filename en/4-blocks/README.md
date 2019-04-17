- Introduction to blocks & blockchain

 	When Alice wants to send a transaction to Bob, she will sign her transaction and broadcast it to the nodes in the network. The consensus nodes will select transactions that they will finalise, and include them into a block. A block is a batch of transactions that get confirmed and stored at the same time. Apart from all transactions that a certain block entails, there are some other fields present in each block which will be discussed below (link Structure of a block).

	One of these fields in each block (bn) is a reference to the previous block that was just confirmed before this block (bn-1). In more detail, is is the hash of the header of the previous block. It is up to the nodes to calculate the hash to validate the parent. In its turn, this previous block(bn-1) has the same behaviour, linking to the block before it(bn-2). This chain goes all the way back to the first block ever created, called the Genesis (b0) block. This is where the term 'blockchain' comes from, as it is, in fact, a chain of blocks.

	Every block can only have one parent, referenced in the field for the previous block. Most blockchains can have a temporary situation where a single parent block can have multiple children, that is, multiple blocks can point to the same block as its parent. This happens, on average, once every week for Bitcoin. This happens when 2 miners find a valid nonce for a new block ("solve the puzzle") approximately at the same time, both broadcasting valid blocks to the network. This is what is called a fork, and is automatically solved when new blocks are mined, as they will continue on only 1 of the 2 valid blocks, resulting in the longest (path of most difficulty) path of the fork to survive, abandoning the other path.

	Neo, thanks to its dBFT consensus mechanism, does not suffer from this behaviour and will, by algorithmic certainty, never fork, and thus never have a situation in which 2 valid versions of the blockchain exists. For more information on this consensus mechanism, you can read the whitepaper (link to https://docs.neo.org/en-us/basic/consensus/whitepaper.html)

- Structure of a block
	- Block header
		- ***previous block header Hash***: x
		- ***Nonce***: When each block is generated, the consensus node will reach a consensus on a random number, and fill it into the Nonce field of the new block. The contract program can easily obtain the random number of any block, by referencing the Nonce field.

		It should be understood that this is quite different from the nonce in on other blockchain protocols like for example Bitcoin, where miners compete to find the nonce that satisfies a certain difficulty level.  Because of the dBFT consensus mechanism in Neo, there is no Proof of Work (PoW) and nodes are working together to achieve consensus. Since there is no PoW required, the Nonce field is used as a random number to be used by all transactions occurring in that block.
		- ***others***: todo

	- Block body
	- Merkle Tree
		- **INSERT IMAGE**

		 a Merkle tree is (EXPL + LINK).

	- Merkle Tree and Network Security
		- Many blockchains use Merkle trees to efficiently calculate the hash of a specific block, where only the root hash of the Merkle tree is stored. This is sufficient to allow finality of the block and all transactions in it. If any of the transactions would be altered, then that leave would have a totally different hash value (h2'). This different hash value (h2') would (doorsijpelen) to the root, resulting in a totally different root hash for the Merkle tree. In turn, this different Merkle tree (h1') would result in a totally different Block Header Hash value. This is why it is sufficient to store the root hash of the Merkle tree in the block header (validate!) to ensure finality. Since the next block always refers the previous block' header, a change in any of the transactions of the previous block would result in a new Block Header Hash, totally different from what the next block is referring to as it's parent block. This ensures fraudulent blocks can be detected and ignored by all other nodes, securing the network. In a Proof of Work-based blockchain like Bitcoin, getting a malicious block accepted would mean recalculating all Nonces of the malicious block **plus** all blocks thereafter to come up with a longer chain than the current one. This is why the younger the block, the less secure it is, and the general rule is to wait for 6 blocks before assuming a transaction is final. This generally is about 1 hour (6 x 10min block). Neo however is using dBFT, (link) which makes a block final with as little as 1 confirmation, currently within 15 seconds. For a hacker to get a malicious block to replace a valid block, he would need to convince 66% of the consensus nodes to build the next block on top of his malicious blockchain, and use his malicious block header as the 'previous block hash' in the new block.

	- Process of bocks
	- Create the blocks
		-  The selection of transactions
		-  Calculation of the network fee
			- ***Smart Contract Creation*** The current free to deploy a new contract on the Mainnet is 500 GAS. For structured development, it is advised to start development on a local testnet (link docker). Once the Smart Contract is stable, you can apply for Testnet funds here (link), for final validations. Once you are certain your Smart Contract is implemented correctly, only then should you deploy it onto the Mainnet, as it is irrevokable - meaning you will not get your 500 GAS back even when you destroy the contract.
			- ***Smart Contract Execution*** To be able to execute a Smart contract, the (which? validating? consensus?) nodes need to perform specific computations for you. In order to compensate the nodes for this effort, a transaction fee should be added to the transaction that is executing the contract. Currently, execution of contracts that require less than 10 GAS, the fee is not needed. **TODO CHECK HOW FEE IS CALCED**

		-  The calculation of next consensus
	- The broadcast of blocks
	- Validation of blocks
		- The legalness of block
		- The witness validation
	- proccess of blocks
		- UTXO transaction process
		- Contract invocation process
