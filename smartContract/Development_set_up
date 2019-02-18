# C# Smart contract development environment

For NEO C# develpers, it is very fortune for them  because  NEO blockchain is build based on the C# and theferefore, from compiler to toolbox, the C# development environment has been widely supported and it is very easy for those .NET developers begin to learn NEO Smart contract develoment. It is also easy for users who did not get touch with C# to begin his smart contract and Dapp.

## Environment
For smart contract using C#, the best way is to develop with a local development environment with a IDE which supporte the NEO smart contract. Luckily, NEO is prepareing a number of tools that achieve this. The only requirement for that is the operating system of your computer is Windows, prefered Windows 10 64 bit.

For non-windows users, such as MAC and Linux users, the best choicee is to use the online editor and compiler which is more convenient for smart contract developing and deopyinng. This will be detailed in this document.



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

 Now you see the privatge chain is running. All of the genesis NEO and GAS are taken into the wallet 1.json. Open it and type the password is **11111111**.  
 
 The blockchain height is approximately 30. Please open the Neo-GUI and view the account balance.
 
> [!Note]
> Smart contract deployment and invocation cost GAS. Since GAS is generated with every new block generated, causing limited GAS generated on freshly-built private chains, therefore users are advised not to shut down private chains now in order to generate enough GAS in case of later use.



