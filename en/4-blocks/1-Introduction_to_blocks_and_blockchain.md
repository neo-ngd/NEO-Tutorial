# Introduction to blocks & blockchain

## Concept
When Alice wants to send a transaction to Bob, she will sign her transaction and broadcast it to the nodes in the network. The consensus nodes will select transactions that they will finalise, and include them into a block. A block is a batch of transactions that get confirmed and stored at the same time. Apart from all transactions that a certain block entails, there are some other fields present in each block which are discussed [here](2-Structure_of_a_block.md).

## Chaining the blocks
One of these fields in each block (b<sub>n</sub>) is a reference to the previous block that was just confirmed before this block (b<sub>n-1</sub>). In more detail, it is the hash of the header of the previous block. It is up to the nodes to calculate the hash to validate the parent. In its turn, this previous block(b<sub>n-1</sub>) has the same behaviour, linking to the block before it(b<sub>n-2</sub>). This chain goes all the way back to the first block ever created, called the Genesis (b<sub>0</sub>) block. This is where the term 'blockchain' comes from, as it is, in fact, a chain of blocks.

## Parents & Children
Every block can only have one parent, referenced in the field for the previous block. Most blockchains can have a temporary situation where a single parent block can have multiple children, that is, multiple blocks can point to the same block as its parent. This happens, on average, once every week for Bitcoin. This happens when 2 miners find a valid nonce for a new block ("solve the puzzle") approximately at the same time, both broadcasting valid blocks to the network. This is what is called a fork, and is automatically solved when new blocks are mined, as they will continue on only 1 of the 2 valid blocks, resulting in the longest (path of most difficulty) path of the fork to survive, abandoning the other path.

Neo, thanks to its dBFT consensus mechanism, does not suffer from this behaviour and will, by algorithmic certainty, never fork, and thus never have a situation in which 2 valid versions of the blockchain exists. For more information on this consensus mechanism, you can read the [whitepaper](https://docs.neo.org/en-us/basic/consensus/whitepaper.html).
