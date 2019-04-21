- Create the blocks
	-  The selection of transactions
	-  Calculation of the network fee
		- ***Smart Contract Creation*** The current free to deploy a new contract on the Mainnet is 500 GAS. For structured development, it is advised to start development on a local testnet (link docker). Once the Smart Contract is stable, you can apply for Testnet funds here (link), for final validations. Once you are certain your Smart Contract is implemented correctly, only then should you deploy it onto the Mainnet, as it is irrevokable - meaning you will not get your 500 GAS back even when you destroy the contract.
		- ***Smart Contract Execution*** To be able to execute a Smart contract, the (which? validating? consensus?) nodes need to perform specific computations for you. In order to compensate the nodes for this effort, a transaction fee should be added to the transaction that is executing the contract. Currently, execution of contracts that require less than 10 GAS, the fee is not needed. **TODO CHECK HOW FEE IS CALCED**

	-  The calculation of next consensus
