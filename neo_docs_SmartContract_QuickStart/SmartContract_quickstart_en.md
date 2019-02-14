# Introduction

This tutorial acts as a beginer's guide for NEO C# develpers. Advanced learners may refer to [NEO Documentation](http://docs.neo.org/zh-cn/index.html) for more details.

## What you will be taught

This article provides a step-by-step guide on developmet environment set-up and configuration, smart contract compilation, smart contract deployment on private chain and smart contract invocation by demonstrating how to release NEO-5 assets on NEO blockchain.

- Set up a local network (URL)
- Develop and deploy NEP-5 contracts (URL)

## Preparation

This tutorial is based on the usage of the two full-node NEO clients: NEO-GUI and NEO-CLI. NEO-CLI will be used to set up a private chain accessible by nodes and NEO-GUI will be used to release smart contracts. Detailed information about the clients can be found in [NEO Node Introduction](https://docs.neo.org/zh-cn/node/introduction.html).

### System configuration

NEO-GUI runs in the following environments:

Windows 7 SP1 / Windows 8 / Windows 10

 [.NET Framework 4.7.1](https://www.microsoft.com/net/download/framework) must be installed for system versioned prior to Windows 10.

NEO-CLI runs in the following environments: 

- Linux (ubuntu 16.04 and above)
- Windows 10

> [!NOTE]
>
> Windows 10 is a recommended choice since NEO-GUI and NEO-CLI will be running at the same time.
>
> This tutorial only describes the occurences on Windows 10. Readers using other systems may refer to relevant chapters in [NEO Documentation](http://docs.neo.org/zh-cn/index.html) since environment and dependencies may differ in different systems.

### Download clients

- NEO-GUI

  Download the latest Release version at [GitHub](https://github.com/neo-project/neo-gui/releases) and run neo-gui.exe. 

- NEO-CLI

  Take Windows 10 for example:

  Users are not required to install a client. You may get the latest Release version from [GitHub](https://github.com/neo-project/neo-cli/releases) by downloading the source code from [GitHub](https://github.com/neo-project/neo-cli.git) or using the following command:

  ```
  $ git clone https://github.com/neo-project/neo-cli.git
  ```

### Creat wallet files

Users need to create 4 wallet files reserved for private chain set-up, which will be elaborated later in this article. The wallets, which can either be created in NEO-GUI or NEO-CLI, are used to store both NEO account info and the asset info in the accounts. In this artcile, we will create 4 wallets in NEO-GUI for example and name them 1, 2, 3, 4.json respectively.

1. Open NEO-GUI and click `wallet` ->  `create a wallet database`, then follow the instructions shown on the screen.
2. When the wallet is successfully created, right click the address in the standard account and select `view private key` to view the account info (address, public key, private key).
3. Copy the public key of the address for later use.

[![2_gui_4](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/2_gui_4.png)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/2_gui_4.png)

After you have created 4 wallets and saved the public key, close NEO-GUI and proceed to the next step - development environment set-up.

# Set up local network

We will complete the following tasks in this section:

- Set-up a private chain
- Connect the nodes to the private network
- Retrieve NEO and GAS from genesis block
- Create wallet files

## Set up a private chain

NEO Official provides a test net for development, debugging and testing purposes. Besides, users may also choose to set up their own private chain where they can operate more flexibly with plenty pf test tokens. This article only describes a quick method to set up private chain. Please refer to [Build a Private Chain](http://docs.neo.org/zh-cn/network/private-chain/private-chain.html) for standard method.

### Install nodes

At least 4 nodes must reach consensus before NEO private chain is successfully deployed. As such, here we make 4 copies of the neo-cli file folder, which was installed in the early steps, and name them node1, node2, node3 and node4 respectively.

### Install plugins

Install SimplePolicy plugin to activate consensus policy, a mechanism that nodes rely on to reach consensus.

1. Download and unzip [SimplePolicy](https://github.com/neo-project/neo-plugins/releases) plugins.
2. Make 4 copies of Plugins File Folder and place each in a separate node file.

### Place wallet files

Place each of the 4 wallet files created in NEO-GUI in each of the 4 node files.

### Modify config.json

Make following changes to the config.json of each node:

- Change the settings of each port to no-repeat and not occupied by other programs.
- Set the Path and Password under UnlockWallet and define StartConsensus and IsActive as true.

The following configurations may serve as references: 

##### node1/config.json

```
{
  "ApplicationConfiguration": {
    "Paths": {
      "Chain": "Chain_{0}",
      "Index": "Index_{0}"
    },
    "P2P": {
      "Port": 10001,
      "WsPort": 10002
    },
    "RPC": {
      "BindAddress": "127.0.0.1",
      "Port": 10003,
      "SslCert": "",
      "SslCertPassword": ""
    },
    "UnlockWallet": {
      "Path": "1.json",
      "Password": "11111111",
      "StartConsensus": true,
      "IsActive": true
    }
  }
}
```

##### node2/config.json

```
{
  "ApplicationConfiguration": {
    "Paths": {
      "Chain": "Chain_{0}",
      "Index": "Index_{0}"
    },
    "P2P": {
      "Port": 20001,
      "WsPort": 20002
    },
    "RPC": {
      "BindAddress": "127.0.0.1",
      "Port": 20003,
      "SslCert": "",
      "SslCertPassword": ""
    },
    "UnlockWallet": {
      "Path": "2.json",
      "Password": "11111111",
      "StartConsensus": true,
      "IsActive": true
    }
  }
}
```

##### node3/config.json

```
{
  "ApplicationConfiguration": {
    "Paths": {
      "Chain": "Chain_{0}",
      "Index": "Index_{0}"
    },
    "P2P": {
      "Port": 30001,
      "WsPort": 30002
    },
    "RPC": {
      "BindAddress": "127.0.0.1",
      "Port": 30003,
      "SslCert": "",
      "SslCertPassword": ""
    },
    "UnlockWallet": {
      "Path": "3.json",
      "Password": "11111111",
      "StartConsensus": true,
      "IsActive": true
    }
  }
}
```

##### node4/config.json

```
{
  "ApplicationConfiguration": {
    "Paths": {
      "Chain": "Chain_{0}",
      "Index": "Index_{0}"
    },
    "P2P": {
      "Port": 40001,
      "WsPort": 40002
    },
    "RPC": {
      "BindAddress": "127.0.0.1",
      "Port": 40003,
      "SslCert": "",
      "SslCertPassword": ""
    },
    "UnlockWallet": {
      "Path": "4.json",
      "Password": "11111111",
      "StartConsensus": true,
      "IsActive": true
    }
  }
}
```

### Modify protocal.json

Redefine the following parameters of the protocal.json file of each node and keep the consistency of each node settings. 

- Magic: The private chain ID can be defined as any integar ranging from 0 to 4294967295.
- StandbyValidators: Type in the public keys of the 4 wallets in the "standby consensus node public key".
- SeedList: Define the IP address of the seed node as localhost and the port as the 4 P2P Port configured in config.json. 

Below is a recommended configuration for your reference:

```
{
  "ProtocolConfiguration": {
    "Magic": 123456,
    "AddressVersion": 23,
    "SecondsPerBlock": 15,
    "StandbyValidators": [
      "037ebe29fff57d8c177870e9d9eecb046b27fc290ccbac88a0e3da8bac5daa630d",
      "03b34a4be80db4a38f62bb41d63f9b1cb664e5e0416c1ac39db605a8e30ef270cc",
      "03cc384ca982168bf6f08922d27c8acc4357d52a7e8ad8281d4af6683e6f63e94d",
      "03da4ed85a991134bf45592a5b04d6d71399f23a85843f43e6ac1a5d30f5473711"
    ],
    "SeedList": [
      "localhost:10001",
      "localhost:20001",
      "localhost:30001",
      "localhost:40001"
    ],
    "SystemFee": {
      "EnrollmentTransaction": 10,
      "IssueTransaction": 5,
      "PublishTransaction": 5,
      "RegisterTransaction": 100
    }
  }
}
```

> [!Note]
>
> Provide the public keys of the wallets just created or paste the standby public keys you saved when you created the wallet right after "StandbyValidators".

### Create QuickStart

To enable a quickstart path of the private chain, you may create a .txt file, input  `dotnet neo-cli.dll /rpc` , rename it to 1Run.cmd and copy it to the directory of the 4 nodes.

Thus far, we have successfully built a private chain. The amended file structure is presented below.

```
├─node1
│      1.json
│      1Run.cmd
│      config.json
│      protocol.json
│
├─node2
│      1Run.cmd
│      2.json
│      config.json
│      protocol.json
│
├─node3
│      1Run.cmd
│      3.json
│      config.json
│      protocol.json
│
└─node4
        1Run.cmd
        4.json
        config.json
        protocol.json
```

### Start private chain

Enter the directory of each node and double click `1Run.cmd`, as shown in the screenshot below:：

[![2_privatechain_demo](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/2_privatechain_demo.png)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/2_privatechain_demo.png)

Right click the `command prompt` and click `close all windows` to disable the private chain.

## Connect to private chain

After the private chain is successfully built, you may proceed to connect NEO nodes to the private chain.

1. Copy the protocal.json file from the directory of whichever node you choose from node1 ~ node4 to overwrite the protocal.json file in NEO-GUI directory. 

2. Replace all the "localhost" in the "SeedList" field of the protocal.json with the IPv4 address of the local host.

3. Examine to ensure that the ports defined in config.json do not conflict with the ports of the four consensus nodes.

   Otherwise, NEO-GUI and NEO-CLI won't be able to run at the same time.

Run neo-gui.exe and open 1.json wallet file. You will see that the connection counts shown on the left corner of the interface is more than 0 and the three values start to increase, which represents wallet height/block height.block head height each. 

[![2_wallet_height](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/2_wallet_height.png)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/2_wallet_height.png)

As of now, we have successfully built a private chain and connected the nodes to the chain.

> [!Note]
>
> If the connection counts of NEO-GUI is 0, you may delete the "Chain_xx" and "Index_xx" files in the root directory and reconnect.

## Retrieve NEO and GAS

100mn NEO are stored in NEO genesis block and Gas is generated with every new block generated after the private chain is set up. Our next step is to retrieve these NEO and GAS from multi-signature smart contract for test use. 

### Create multi-signature address

Open the four wallets in sequence in NEO-GUI and follow the instructions below:

1. Right click the margins of the account page and select `create a contract address` -> `multi-signature` to add multi-signature address to each wallet.

2. Input in sequence the saved public keys of the 4 wallets, define the minimum signature required as 3 (consensus node counts/2+1) and click `Confirm`.

   [![2_privatechain_12](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/2_privatechain_12.jpg)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/2_privatechain_12.jpg)

3. Click `wallet` -> `reconstruct wallet index`.

> [!Note]
>
> Users are required to add multi-signature addresses to all four wallets, otherwise the signature may be deemed invalid.

100mn NEO will show up in the contract address as indicated in the screenshot below.

[![2_privatechain_14](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/2_privatechain_14.jpg)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/2_privatechain_14.jpg)

### Transfer NEO to standard address 

Take the following steps to transfer NEO from a contract address to a standard address:

1. Randomly open a wallet out of the four wallets and click  `transaction`-> `asset transfer`.

2. Input the standard recipient address and transfer the 100mn NEO to this address.

3. The system warns "Transactions are constructed, but with insufficient signatures." Copy the code line.

4. Open the second wallet and click `transaction` ->`signature`.

5. Paste the code line copied in step 3, click `signature` and copy the code line generated. 

6. Open the third wallet and click `transaction`-> `signature`, paste the code line copied in step 5 and click `signature`.

   The window will display a  `broadcast` button, which indicates that the transaction is signed by the minimum required number of parties and is ready for broadcasting.

7. Click `broadcast` to conplete asset transfer.

   Wait a moment and 100mn NEO will show up in the standard address.

   [![2_privatechain_20](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/2_privatechain_20.jpg)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/2_privatechain_20.jpg)

### Transfer GAS to standard address

Open the recipient wallet and click `advance` -> `retrieve NeoGas` -> `retrieve all`.

[![2_privatechain_21](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/2_privatechain_21.jpg)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/2_privatechain_21.jpg)

The following operation is quite similar to NEO transfer operation. Copy the code line indicating insufficient signatures, open the second and third wallet in sequence, sign the transactions and broadcast. If the transaction is successful, the amount will show up in the account as indicated in the screenshot below.

[![2_privatechain_26](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/2_privatechain_26.jpg)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/2_privatechain_26.jpg)

## Prepare node wallet file

Now we will create a new wallet file named 0.json and place it in the root directory of the nodes for smart contract release.

1. Create a new wallet file named 0.json and copy the default address in case of later use.
2. Open the recipient wallets of NEO and GAS, transfer all the wallet assets to 0.json and wait for the transaction to be confirmed.
3. Open 0.json and assets will be displayed in the account.

> [!Note]
>
> Smart contract deployment and invocation cost GAS. Since GAS is generated with every new block generated, causing limited GAS generated on freshly-built private chains, therefore users are advised not to shut down private chains now in order to generate enough GAS in case of later use.

# Develop and deploy NEP-5 contract

So far, we have learned how to build a private chain and connect nodes to the chain. The following part will proceed to environment configuration, NEO smart contract coding and compilation and NEO smart contract deployment and invocation on private chain using C# and Windows 10.

We will complete the following tasks in this section:

- Install contract development environment
- Create a NEP-5 contract project
- Compile a contract
- Deploy a contracts
- Invoke a contract
- View contract assets

## Install development environment

### Install Visual Studio 2017

Download [Visual Studio 2017](https://www.visualstudio.com/products/visual-studio-community-vs)  and get it installed. Select `.NET Core cross-platform development` option during installation.

[![3_install_core_cross_platform_development_toolset](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/3_install_core_cross_platform_development_toolset.jpg)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/3_install_core_cross_platform_development_toolset.jpg)

### Install NeoContractPlugin

Open Visual Studio 2017 and click `tool` -> `extensions and update`，click `online`on the left column, search NEO and install NeoContractPlugin (the process must be completed online).

[![3_download_and_install_smart_contract_plugin](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/3_download_and_install_smart_contract_plugin.jpg)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/3_download_and_install_smart_contract_plugin.jpg)

### Configure neo-compiler

1. Download [neo-compiler](https://github.com/neo-project/neo-compiler) project to your localhost.

2. Click `file` -> `open` -> `project/solutions` in Visual Studio 2017 and select neo-compiler.sln in the project file.

3. Right click neon project in the list and click `release`.

   [![3_publish_neo_compiler_msil_project](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/3_publish_neo_compiler_msil_project.jpg)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/3_publish_neo_compiler_msil_project.jpg)

4. After the release path is configured, click `release`.

   Upon successful release, a neon.exe file will be generated in bin\Release\PublishOutput.

   [![3_publish_and_profile_settings](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/3_publish_and_profile_settings.png)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/3_publish_and_profile_settings.png)

> [!Note]
>
> In case of error warning in the process of release: unable to copy file "obj\Release\netcoreapp1.0\win10-x64\neon.dll" as the file cannot be located, which may be be traced to a bug in VS 2017 (e.g. v15.4, 15.5), what you need to do is manually copy `\obj\Release\netcoreapp1.0\neon.dll` to `\obj\Release\netcoreapp1.0\win10-x64\` folder and release it again.

### Change environment parameter settings

Next we need to add path using the following method to allow neon.exe to be accessible from any point:

1. For Windows10, press Windows+S, input environment parameter and select `edit the account's environment parameters`.

   [![3_2017-06-07_12-07-03](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/3_2017-06-07_12-07-03.png)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/3_2017-06-07_12-07-03.png)

2. Select Path and click `edit`:

   [![3_environment_variable](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/3_environment_variable.png)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/3_environment_variable.png)

3. Click `create` in the popped up window and input your file filer directory that contains neon.exe, then press `confirm`.

> [!Note]
>
> Do not add a path that contains“…… neon.exe” in the environment parameter field. Remember to input the path of the **file folder directory** that contains neon.exe instead of the path of neon.exe file.

[![3_edit_environment_variable](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/3_edit_environment_variable.png)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/3_edit_environment_variable.png)

After the path is added, run CMD or PowerShell for testing purpose (if CMD starts working before the path is added, remember to restart it after adding the path).  If no error is reported after inputting neon and  the system sends the prompt message containing version number as follows, it means that the environment parameter configuration is successful.

[![3_1545037391347](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/3_1545037391347.png)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/3_1545037391347.png)

## Create a NEO contract project

Upon completition of the previous steps, you may start to create NEO smart contract project in Visual Studio 2017 (no specific requirement for .NET Framework version): 

1. Click `file` -> `create` -> `project`.
2. Select `NeoContract` in the list and change settings where necessary, then click `confirm`.

[![3_new_smart_contract_project](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/3_new_smart_contract_project.png)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/3_new_smart_contract_project.png)

A C# file will be auto-generated after the project is created with a default class inherited from the SmartContract. As indicated in the screenshot below, now you have a Hello World stored in the contract.

[![3_smart_contract_function_code](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/3_smart_contract_function_code.png)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/3_smart_contract_function_code.png)

Nevertheless, the above only demonstrates a simple data storage method - to store data in private storage area using key-value method.

## Edit NEP-5 code

Many developers are curious about how to release their own contract assets on NEO public chain. Now let's walk through the process on private chain. 

1. Download NEP-5 examples from [Github](https://github.com/neo-project/examples).

2. Create a NEO smart contract project in Visual Studio 2017 and name it NEP5.

3. Open NEP5.cs example

   The code contains basic information of the assets and the methods available to be invoked. You can make changes when needed.

   > [!NOTE]
   >
   > If there are red underlines under the code warning that the system is unable to find NEO name space and there is "!" in project references, you may take the following steps:
   >
   > Right click the solution file in VS, click `manage NuGet package` and update the Neo.SmartContract.Framework to the latest official version in a new page.  If the red underlines still exist when program update is completed, you may try double clicking the "!". If the problem remains unsolved, you may resort to the solutions below:：
   >
   > 1. Download nuget.exe [here](https://www.nuget.org/downloads)  and copy it to the root directory of NeoContract project.
   > 2. Open Power Shell or command prompt (CMD).
   > 3. Redirect to the root directory of NeoContract project and run `nuget restore`.

4. Here we make certain modifications to the example files as follows:

   - Define total assets and `deploy` method.
   - Replace "Owner" with the address in 0.json (otherwise the wallet assets won't be accessible).

   The code is as follows:

```
using Neo.SmartContract.Framework;
using Neo.SmartContract.Framework.Services.Neo;
using Neo.SmartContract.Framework.Services.System;
using System;
using System.ComponentModel;
using System.Numerics;

namespace NEP5
{
    public class NEP5 : SmartContract
    {
        [DisplayName("transfer")]
        public static event Action<byte[], byte[], BigInteger> Transferred;

        private static readonly byte[] Owner = "Ad1HKAATNmFT5buNgSxspbW68f4XVSssSw".ToScriptHash(); //Owner Address
        private static readonly BigInteger TotalSupplyValue = 10000000000000000;

        public static object Main(string method, object[] args)
        {
            if (Runtime.Trigger == TriggerType.Verification)
            {
                return Runtime.CheckWitness(Owner);
            }
            else if (Runtime.Trigger == TriggerType.Application)
            {
                var callscript = ExecutionEngine.CallingScriptHash;

                if (method == "balanceOf") return BalanceOf((byte[])args[0]);

                if (method == "decimals") return Decimals();

                if (method == "deploy") return Deploy();

                if (method == "name") return Name();

                if (method == "symbol") return Symbol();

                if (method == "supportedStandards") return SupportedStandards();

                if (method == "totalSupply") return TotalSupply();

                if (method == "transfer") return Transfer((byte[])args[0], (byte[])args[1], (BigInteger)args[2], callscript);
            }
            return false;
        }

        [DisplayName("balanceOf")]
        public static BigInteger BalanceOf(byte[] account)
        {
            if (account.Length != 20)
                throw new InvalidOperationException("The parameter account SHOULD be 20-byte addresses.");
            StorageMap asset = Storage.CurrentContext.CreateMap(nameof(asset));
            return asset.Get(account).AsBigInteger();
        }
        [DisplayName("decimals")]
        public static byte Decimals() => 8;

        private static bool IsPayable(byte[] to)
        {
            var c = Blockchain.GetContract(to);
            return c == null || c.IsPayable;
        }

        [DisplayName("deploy")]
        public static bool Deploy()
        {
            if (TotalSupply() != 0) return false;
            StorageMap contract = Storage.CurrentContext.CreateMap(nameof(contract));
            contract.Put("totalSupply", TotalSupplyValue);
            StorageMap asset = Storage.CurrentContext.CreateMap(nameof(asset));
            asset.Put(Owner, TotalSupplyValue);
            Transferred(null, Owner, TotalSupplyValue);
            return true;
        }

        [DisplayName("name")]
        public static string Name() => "GinoMo"; //name of the token

        [DisplayName("symbol")]
        public static string Symbol() => "GM"; //symbol of the token

        [DisplayName("supportedStandards")]
        public static string[] SupportedStandards() => new string[] { "NEP-5", "NEP-7", "NEP-10" };

        [DisplayName("totalSupply")]
        public static BigInteger TotalSupply()
        {
            StorageMap contract = Storage.CurrentContext.CreateMap(nameof(contract));
            return contract.Get("totalSupply").AsBigInteger();
        }
#if DEBUG
        [DisplayName("transfer")] //Only for ABI file
        public static bool Transfer(byte[] from, byte[] to, BigInteger amount) => true;
#endif
        //Methods of actual execution
        private static bool Transfer(byte[] from, byte[] to, BigInteger amount, byte[] callscript)
        {
            //Check parameters
            if (from.Length != 20 || to.Length != 20)
                throw new InvalidOperationException("The parameters from and to SHOULD be 20-byte addresses.");
            if (amount <= 0)
                throw new InvalidOperationException("The parameter amount MUST be greater than 0.");
            if (!IsPayable(to))
                return false;
            if (!Runtime.CheckWitness(from) && from.AsBigInteger() != callscript.AsBigInteger())
                return false;
            StorageMap asset = Storage.CurrentContext.CreateMap(nameof(asset));
            var fromAmount = asset.Get(from).AsBigInteger();
            if (fromAmount < amount)
                return false;
            if (from == to)
                return true;

            //Reduce payer balances
            if (fromAmount == amount)
                asset.Delete(from);
            else
                asset.Put(from, fromAmount - amount);

            //Increase the payee balance
            var toAmount = asset.Get(to).AsBigInteger();
            asset.Put(to, toAmount + amount);

            Transferred(from, to, amount);
            return true;
        }
    }
}
```

When the editing is done, the coding part of the smart contract is done.

## Compile contract file

Click `generate`->`generate solutions` (hotkeys: Ctrl + Shift + B) in the menu to start compilation.

[![img](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/compile.png)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/compile.png)

When the compilation is done, NEO smart contract file named`NEP5.avm`  will be generated in the `bin/Debug` directory of the project.

[![img](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/contractfile.png)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/contractfile.png)

`NEP5.abi.json` is a descriptive file of the smart contract, which contains desciptions of the ScriptHash, entry, parameters and return values of the contract. More information about the smart contract ABI can be found in [NeoContract ABI](https://github.com/neo-project/proposals/blob/master/nep-3.mediawiki).

> [!Note]
>
> Given that neon compiles .dll with nep-8 support by default, which conflicts with nep-5, thus we need to execute .avm using nep-5 compatible method. 
>
> Open Power Shell or command prompt (CMD), enter bin/Debug directory and input the following command (replace nep5.dll with your own project file):
>
> ```
> neon nep5.dll --compatible
> ```
>
> The new `nep5.avm`  file and `nep5.abi.json`  file will overwrite the old files.

## Deploy contract

We may use NEO-GUI to deploy the newly generated contract file.

1. Open 0.json wallet file, click `advance` -> `deploy contracts`.

2. Click `load` to select the compiled contract file in the contract deployment dialog.

   Copy the contract scripthash displayed under the code box for late use in contract invocation.

3. Fill in the params in the information and meta data fields. Do not leave any parameter undefined, otherwise the `deploy` button won't function properly.

   For NEP-5 asset contract, the argument is written as 0710 and the return value is 05.

   Detailed rules can be referred to  [Smart Contract Parameters and Return Values](http://docs.neo.org/zh-cn/sc/Parameter.html)。

   Check the box of `required to create a storage area` as according to NEP-5 standard, storage areas are used to maintain accounts. 

   No need to check the options` require dynamic invocation` and `Payable`.

4. After all the params are defined, click `deploy`.

5. Click `trial run` in the popped up contract invocation interface. Double check and click `invoke`.

   Contract deployment costs about 100-1000 GAS, which is further explained in [system fees](http://docs.neo.org/zh-cn/sc/systemfees.html).

Upon successful deployment, your smart contract is now released to the blockchain.

## Invoke contract

Now you may invoke the smart contract released just recently.

1. Click `advance` -> `contract call` -> `function call`。

2. Paste the contract scripthash copied in the early step to `ScriptHash` and press search button. Relevant contract information will be displayed automatically.

3. Click `...` beside `arguments` to enter the edit interface.

   [![3_1546846629992](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/3_1546846629992.png)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/3_1546846629992.png)

4. Concerning the smart contract you wrote, [0] represents the function name while [1] the input param of the function (ignore if not exist). If you need to invoke deploy function and release the assets onto the chain, please take the following steps: click [0], fill in "deploy" (all in lowercase letters) in the new value, click`update `and close the window.

   [![3_1545633970239](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/3_1545633970239.png)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/3_1545633970239.png)

5. Click `trial run` to test the contract. If no error is spotted, click `invoke`, which may cost several GAS.

## View contract assets

Click `advance`-> `options` in NEO-GUI and fill in the scripthash of the recently deployed assets. The NEP-5 assets will show up in your asset page.

[![3_check_nep5](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/raw/master/3_check_nep5.png)](https://github.com/nicolegys/neo_docs_SmartContract_QuickStart/blob/master/3_check_nep5.png)

You have successfully released a smart contract on NEO private chain. Congratulations!
