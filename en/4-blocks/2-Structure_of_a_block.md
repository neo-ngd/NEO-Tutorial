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
