# C# Smart contract development environment

For NEO C# develpers, it is very fortune for them  because  NEO blockchain is build based on the C# and theferefore, from compiler to toolbox, the C# development environment has been widely supported and it is very easy for those .NET developers begin to learn NEO Smart contract develoment. It is also easy for users who did not get touch with C# to begin his smart contract and Dapp.

## Environment
For smart contract using C#, the best way is to develop with a local development environment with a IDE which supporte the NEO smart contract. Luckily, NEO is prepareing a number of tools that achieve this. The only requirement for that is the operating system of your computer is Windows, prefered Windows 10 64 bit.

For non-windows users, such as MAC and Linux users, the best choice is to use the online editor and compiler which is more convenient for smart contract developing and deopyinng. This will be detailed in this [document](https://medium.com/neweconolab/with-neoray-neo-smart-contract-development-has-never-been-easier-edad41cc3ae6).



### Windows 

In order to set-up a NEO private net and development environment, developer must install some dependencies:

-  [NET FrameWork](https://dotnet.microsoft.com/download/dotnet-framework-runtime/net472)
-  [NET Core](https://dotnet.microsoft.com/download)

In addition, in order to develop the C# based smart contract, also we have to use the IDE and the best choice is Visual Studio:

- [Microsoft Visual Studio](https://visualstudio.microsoft.com/vs/community/) 

### Private Network
NEO blockchain has lauched several years and everyday, a number of users are makeing trascaction and using Dapps on the mainNet. When you develop a smart contract, you have to deploy it to the blockchain and test it on with invocation. Deploying the smart contract pr Dapp on the mainNet is spending the real Gas which is not a economic option for our develoers. When developer want to test there smart contract or Dapp, the best choice is to use TestNet or PrivateNet.

The TestNet is an environment where the user can develop, commission and test programs. Testing programs on the testnet incurs the network fee of testnet GAS (not real GAS!!). Testnet NEO and GAS can be applied free of charge, on the official website.

All the blockchain of the test network are independent of the main network. If you develop a simple smart contract or try to register assets, the use of testnet should suffice. After the testing is complete, the development can be moved to the NEO mainnet online operation.

All the transaction and blocks can be viewed on the [NEO scan](https://neoscan-testnet.io/).

In addition, building a private chain using four nodes and withdraw NEO and GAS from the private chain is a more convinient and fast way of developers like us who want to begin to learn how to develop the smart contract by step to step. By using such a private chain, developers do not have to worry about the GAS and also deploy and test on such a local network is much faster.


#### set up your  private chain
In this tutorial, our development is based on a simplified private Chain which can be downloaded in this [Github Repository](https://github.com/steven1227/NEO-Private-Net).

This repository contains a configured private-chain and you can run it after downloads. The neo-cli in it version is 2.8.0. The gui version is 2.7.6

After clonning or download the repository, you cat start the private-net by run four cmd scripts.

```
enter node1 folder，double click 1Run.cmd

enter node2 folder，double click 1Run.cmd

enter node3 folder，double click 1Run.cmd

enter node4 folder，double click 1Run.cmd

```

 Now you see the privatge chain is running. All of the genesis NEO and GAS are taken into the wallet `1.json`.
 
Open NEO-GUI and click `wallet` ->  `open wallet database`,  Open `1.json` which is located in the *folder* `node1` and type the password is **11111111**.  
 
 The blockchain height is approximately 30. Please open the Neo-GUI and view the account balance.
 
 <p align="center">
  <img src="./imgs/20190219-112142.png" />
 </p>


> [!Note]
> Smart contract deployment and invocation cost GAS. Since GAS is generated with every new block generated, causing limited GAS generated on freshly-built private chains, therefore users are advised not to shut down private chains now in order to generate enough GAS in case of later use.

You can try to create a new wallet and transfert money to it.

1. Open NEO-GUI and click `wallet` ->  `create a wallet database`, then follow the instructions shown on the screen.
2. When the wallet is successfully created, right click the address in the standard account and select `view private key` tod view the account info (address, public key, private key).
3. Copy the address for next step.
4. Open the wallet `1.json` again and  click the `Transaction`, and then click the `+` symble. Now lets add a new transaction. 
The *asset* type is `NEO`, the amount is your prefred amount to transfer, and the *payto* is the address from last step.
 <p align="center">
  <img src="./imgs/20190219-113025.png" />
 </p>
  
 5. After confirmation, you can view the transaction success and there will be a transaction id. Click the `Transaction History ID`. The transaction you just made will be occured here and it may show `unconfirmed`. Wait for seconds and it will diaplay the confirmation number which means the block is confirmed by the consensus nodes. After that, open the new wallet you created just now and you will find the balance changed.
 6. Open the wallet `1.json` again and you can view there is another global asset `GAS`. GAS is the fuel for deploying and running smart contract on the blockchain in NEO. Gas can be claimed by the `NEO` token hodlers. In the GUI, click the `Advanced` ->  `NEO Gas claim`, you will see the gas available for you to claim. Then just click `Claim`, the GAS will show in your account balance.
 
 Now let us prepare the smart contract development environment.
 
### Visual studio setup
 
#### Install and open visual studio. 
Select  `.NET Core cross-platform` development option during installation
 
 <p align="center">
  <img width="80%"  src="./imgs/vs.jpg" />
 </p>
 
#### Install NeoContractPlugin
Open Visual Studio 2017 and click `tool` -> `extensions and Updates`，click `online`on the left column, search NEO and install NeoContractPlugin (the process must be completed online).
 <p align="center">
  <img width="80%" src="./imgs/plugin.jpg" />
 </p>
#### Configure neo-compiler

1. Download [neo-compiler](https://github.com/neo-project/neo-compiler) project to your localhost.

2. Click `file` -> `open` -> `project/solutions` in Visual Studio 2017 and select neo-compiler.sln in the project file.

3. Right click neon project in the list and click `release`.

4. After the release path is configured, click `release`.

	In my setting,  a `neon.exe` file is generated in `xxx\neo-compiler-master\neo-compiler-master\neon\bin\Debug\netcoreapp2.0\publish`

5. Add the neon to the `PATH` in system environment.

	For Windows10, press `Windows+S`, input environment parameter and select edit the account's environment parameters, and add it to `Path`.

#### Create a smart contract project
1. Click `file` -> `create` -> `project`.
2. Select `NeoContract` in the list and change settings where necessary, then click `confirm`.

 <p align="center">
  <img src="./imgs/20190219-120404.png" />
 </p>
 
A C# file will be auto-generated after the project is created with a default class inherited from the SmartContract. As indicated in the screenshot below, now you have a Hello World contract.

 <p align="center">
  <img src="imgs/20190219-120735.png" />
 </p>

Nevertheless, the above only demonstrates a simple data storage method - to store data in private storage area using key-value method.

#### Compiling contract file

Click `generate`->`generate solutions` (hotkeys: Ctrl + Shift + B) in the menu to start compilation.


When the compilation is done, NEO smart contract file named`NEP5.avm` is generated in the `bin/Debug` directory of the project.


`SmartContractDemo.abi.json` is a descriptive file of the smart contract, which contains desciptions of the ScriptHash, entry, parameters and return values of the contract. More information about the smart contract ABI can be found in [NeoContract ABI](https://github.com/neo-project/proposals/blob/master/nep-3.mediawiki).

 <p align="center">
  <img src="imgs/20190219-140640.png" />
 </p>
 
 > [!!!!**Note**]
>
> Given that neon compiles .dll with nep-8 by default, which conflicts with nep-5, thus we need to execute .avm using nep-5 compatible method. 
>
> Open Power Shell or command prompt (CMD), enter bin/Debug directory and input the following command (replace nep5.dll with your own project file):
>
> ```
> neon SmartContractDemo.dll --compatible
> ```

> The new `SmartContractDemo.avm`  file and `SmartContractDemo.abi.json`  file will overwrite the old files.


#### Deploy the contract
 
  <p align="center">
  <img src="imgs/20190219-140958.png" />
 </p>
 
 We may use NEO-GUI to deploy the newly generated contract file.

1. Open 0.json wallet file, click `advance` -> `deploy contracts`.

2. Click `load` to select the compiled contract file `xxx.avm` in the contract deployment dialog.

	*Copy the contract script hash displayed under the code box for late use in contract invocation.*

3. Fill in the params in the information and meta data fields.

   For this contract, the argument is written as 0710 and the return value is 05.

   Detailed rules can be referred to  [Smart Contract Parameters and Return Values](http://docs.neo.org/zh-cn/sc/Parameter.html)。

   Check the box of `required to create a storage area`/

   No need to check the options` require dynamic invocation`.

4. After all the params are defined, click `deploy` -> `test` -> `invoke`.

 
#### Invoking contract

Now you may invoke the smart contract released just recently.

1. Click `advance` -> `contract call` -> `function call`。

2. Paste the contract scripthash copied in the last step to `ScriptHash` and press search button. Relevant contract information will be displayed automatically.

3. Click `...` beside `arguments` to enter the edit interface. Fill in the argument. In this contract, any arguments is ok because the main method does not use that.

4. Click `trial` to test the contract. If no error is spotted, click `invoke`, which may cost several GAS.


If invoke successfully, the gas will be reduced in the account balance.

## Next Step
**Congratulations!**, you set up your private network and invoke your first smart contract successfully. Now let's begin to learn [the basic of NEO smart contract and get your first one.](Smart_Contract_basics.md)
