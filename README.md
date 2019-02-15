<div align="center">  
<h1>NEO-Tutorial</h1>
<img src="neo-rebranding.png" alt="NEO-Tutorial" height="150">
<p>A complete learning tutorial for NEO developers and learners</p>
</div>

## Table of Tutorials
 - Introduction to NEO
    - The introduction of Cryptography and CryptoCurrency 
    - NEO
        - History
        - White Paper
        - Features
        - Documentations

 - Wallet 
    - Keys and address
        - Hash function used in NEO
        - Generation of Private key
        - ECDSA algorithm
        - Base58 check
        - The scripthash and address of NEO
    - Wallet file
        - NEP6 wallet (json based)
        - DB3 wallet (sqlite)
    - The usage of NEO-GUI
    - UTXO model
    - Account model
        - The peronsal account
        - The contract account


 - Transactions
    - The type of transaction 
        -  MinerTransaction
        -  IssueTransaction
        -  ClaimTransaction
        -  StateTransaction
        -  ContractTransaction
        -  InvocationTransaction
    - The process of transcactions
        - Create a transaction
        - Signature
        - Transaction validation
        - Process of transaction
    - Use NEO-GUI to invoke the transactions
    - Use NEO-CLI to invoke the transactions
 
 - Blocks
    - Block header
    - Block body
    - Mekele tree
    - Process of bocks 
        - Create the blocks 
            -  The selection of transactions
            -  Calculation of the network fee
            -  The calculaton of next consensus
        - The broadcast of blocks
        - Validation of blocks
            - The legalness of block
            - The witeness validation
        - proccess of blocks
            - UTXO trnsaction process
            - Contract invocation process

 - Network 
    - The Peer of peer network architecture
    - The node in the network
    - The AKKA protocal
    - The proccess of network process
        - Node start up
        - Node Handshake
        - Message broadcast
 
 - Persistence
    -  Level DB
    -  The table structure in the Level DB
    -  wallet index

 - Consensys
    - Byzantine Fault Tolerance
    - dBFT 
        - The content of algorithm
        - The proccess of message
    - Validators enrollment 
    - Votes for validators

 - NVM
    - What is NEO VM
        - The Neo virtual machine architecture    
    - The NVM Instruction Set
        - Arithmatic operation
        - Stack operations
        - System operations
        - Logic operations
        - Environmental operations
        - Block operations
    - Gas Consumption During Execution

- Smart Contract
    - what is smart contract
    - Write your first NEO contact with C#
    	- Prepare the development environment of your smartcontract
    	- Learn smart contract by demos
    		- ITO(Initial Token Offering)
    			- Introduction to NEP-5 
    			- Smart contract structure 
    			- compile, test and deploy your smart contract
    			- peroperties, constuctor and methods
    			- Data types
    			- Storage usage
    			- Events 
    			- Find your own tokens
    		- NFT (Non-Fungible Tokens)
    			- Transactions and blocks
    			- Minting Tokens
    			- Withdraw global asset
    			- Timestamp in blockchain
    		- CGAS
    			- Gloal asset and NEP-5
    			- UTXO model
    			- Trigger
    			- Signature and Verification
    			- Transaction Invocation
    			- Strorage map
    			- NEP5 Asset <-> Global Asset
    - Write NEO smart contract with Python 
        - what is Neo-python
        - Prepare the Neo-python developmentprocedure environment 
        - NEO python basics
        - Python smart contract example
            - Domain Name Service
        - Dapp demo based on neo python
            - Lucky neo 
    - Write NEO smart contract with JS
        - Introduction of neo-one
        - neo-one smart contract example
            - ICO template
            - Escrow
        - Build a Dapp based on neo-one 
    - Write NEO smart contract with Go.
        - Introduction to neo-storm framework
        - Issue a NEP5 token on using Go.
    - Build game on blockchain
        - The structure of blockchain game
        - Build a game with NEO + Unity
## Smart contract development Learning Resources
- [Neo Smart contract quick start](https://github.com/neo-ngd/NEO-Tutorial/tree/master/neo_docs_SmartContract_QuickStart)
- [Neo-python tutorial](https://github.com/neo-ngd/NEO-Tutorial/tree/master/neo_docs_neopython_tutorial)
