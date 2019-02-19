# Your first Smart Contract - Initial Token Offering

## 1. Contract structure
Let's have a look at our basic hello world contract
```C#
using Neo.SmartContract.Framework;
using Neo.SmartContract.Framework.Services.Neo;

namespace SmartContractDemo
{
    public class Contract1 : SmartContract
    {
        public static bool Main(string operation, object[] args)
        {
    
            return true;
        }
    }
}
```
Every Smart Contract inherits the `SmartContract` base class which is in the NEO framework and provides some basic methods.

The `NEO` namespace is the API provided by the Neo blockchain, providing a way to access the block-chain data and manipulate the persistent store. These APIs are divided into two categories:

1.  Blockchain ledger. The contract can access all the data on the entire blockchain through interops layer, including complete blocks and transactions, as well as each of their fields.
    
2.  Persistent store. Each application contract deployed on NEO has a storage space that can only be accessed by the contract itself. These methods provided can access the data in the contract.

### 2. Constract property
Inside the contract class, the property defined with `static readonly` or `const` is the contract property which can be used as constants and can not be changed. For instance, when we want to define a Owner of that contract or the factor number which will be used in the later asset transfer, we can define these constants in this way:

```c#
// Represents onwner of this contract, which is a fixed address. Usually should be the contract creator
public static readonly byte[] Owner = "ATrzHaicmhRj15C3Vv6e6gLfLqhSD2PtTr".ToScriptHash();

// A constant number
private const ulong factor = 100000000;
```

These properties defined in contract property are ususally constants that can be used inside the methods of smart contract and everytime the smart contract is running on any instance, these perporties keep the same value.



In addition, developer can define static method  in contract and return a constant, which is exposing the method  out of the contract and let end-user can call the method to get the fixed value when they try to query the smart contract. For instance, when you create you own token, you have to define a name which you may want everyone use you contract can check he name with this method.

```c#
public  static  string  Name() =>  "name of the token";
```

#### 2. Storage property

#### 3 . Data type

#### 4. Main method

#### Trigger
A smart contract trigger is a mechanism that triggers the execution of smart contracts. There are four triggers introduced in the NEO smart contract，`Verification`,  `Application`，  `VerificationR`, and  `ApplicationR`. 

An application trigger is used to invoke the contract as a verification function, which can accept multiple parameters, change the blockchain status, and return values of any type.

Theoretically, smart contracts can have any entry points, but we recommend you use the main function as the entry point of smart contracts for easier invocation.

Unlike the verification trigger which is triggered by a transfer, an application trigger is triggered by a special transaction  `InvocationTransaction`. If the application (Web/App) calls a smart contract, an  `InvocationTransaction`  is constructed, and then signed and broadcast in the program. After the  `InvocationTransaction`  transaction is confirmed, the smart contract is executed by the consensus node. The common node does not execute the smart contract when forwarding the transaction.

Since the application contract is executed after  `InvocationTransaction`  is confirmed, the transaction is recorded in the blockchain no matter the execution of the application contract is successful or not.

The success and failure of InvocationTransaction is not necessarily related to the success or failure of execution of smart contracts.

#### 5. CheckWitness 

#### 6. Events
