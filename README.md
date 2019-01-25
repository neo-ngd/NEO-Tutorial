# NEO-Tutorial

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
            - the legalness of block
            - the witeness validation
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
    - Introduction to smart contract.
        - Transactions
        - Blocks
        - Account
        - I/O
        - Storage
        - Triggers
        - Data types
    - Write your first NEO contact with C#
        - Prepare the development environment of your smartcontract    
        - NEO smart contract basics
        - Smart contract procedure
            - compile the contract 
            - Deploy the contractprocedure
            - Invoke the contract
        - NEO smart contract by example
            - Hello world 
            - NEP5 token
            - cGas
    - Write NEO smart contract with Python 
        - what is Neo-python
        - Prepare the Neo-python developmentprocedure environment 
        - NEO python basics
        - python smart contract example
            - Domain Name Service
    - Write NEO smart contract with Go.
        - Introduction to neo-storm framework
        - Issue a NEP5 token on using Go.
    - Write NEO smart contract with JS
        - Installation of neon-js
        - neon-js structure and API core



