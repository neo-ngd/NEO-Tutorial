

#  NEP-5 contract

## Introduction to NEP-5

The NEP-5 standard is a token standard which represents a tokenized smart contract. This standard can regulated the token which is issued on the NEO blockchain. A standard method for interacting with these tokens relieves the entire ecosystem from maintaining a definition for basic operations that are required by every Smart Contract that employs a token.

In NEP-5 standard, you have some methods and trigger one event


### Methods

#### totalSupply

## Introduction to NEP-5

```csharp
public static BigInteger totalSupply()
```

Returns the total token supply deployed in the system.

#### name

```csharp
public static string name()
```

Returns the name of the token. e.g. <code>"MyToken"</code>.

This method MUST always return the same value every time it is invoked.

#### symbol

```csharp
public static string symbol()
```

Returns a short string symbol of the token managed in this contract. e.g. <code>"MYT"</code>. This symbol SHOULD be short (3-8 characters is recommended), with no whitespace characters or new-lines and SHOULD be limited to the uppercase latin alphabet (i.e. the 26 letters used in English).

This method MUST always return the same value every time it is invoked.

#### decimals

```csharp
public static byte decimals()
```


Returns the number of decimals used by the token - e.g. <code>8</code>, means to divide the token amount by <code>100,000,000</code> to get its user representation.

This method MUST always return the same value every time it is invoked.

#### balanceOf

```csharp
public static BigInteger balanceOf(byte[] account)
```

Returns the token balance of the <code>account</code>.

The parameter <code>account</code> SHOULD be a 20-byte address. If not, this method SHOULD <code>throw</code> an exception.

If the <code>account</code> is an unused address, this method MUST return <code>0</code>.

#### transfer
```csharp
public static bool transfer(byte[] from, byte[] to, BigInteger amount)
```

Transfers an <code>amount</code> of tokens from the <code>from</code> account to the <code>to</code> account.

The parameters <code>from</code> and <code>to</code> SHOULD be 20-byte addresses. If not, this method SHOULD <code>throw</code> an exception.

The parameter <code>amount</code> MUST be greater than or equal to <code>0</code>. If not, this method SHOULD <code>throw</code> an exception.

The function MUST return <code>false</code> if the <code>from</code> account balance does not have enough tokens to spend.

If the method succeeds, it MUST fire the <code>transfer</code> event, and MUST return <code>true</code>, even if the <code>amount</code> is <code>0</code>, or <code>from</code> and <code>to</code> are the same address.

The function SHOULD check whether the <code>from</code> address equals the caller contract hash. If so, the transfer SHOULD be processed; If not, the function SHOULD use the SYSCALL <code>Neo.Runtime.CheckWitness</code> to verify the transfer.

If the <code>to</code> address is a deployed contract, the function SHOULD check the <code>payable</code> flag of this contract to decide whether it should transfer the tokens to this contract.

If the transfer is not processed, the function SHOULD return <code>false</code>.

### Events
#### transfer
```csharp
public static event transfer(byte[] from, byte[] to, BigInteger amount)
```

MUST trigger when tokens are transferred, including zero value transfers.

A token contract which creates new tokens MUST trigger a <code>transfer</code> event with the <code>from</code> address set to <code>null</code> when tokens are created.

A token contract which burns tokens MUST trigger a <code>transfer</code> event with the <code>to</code> address set to <code>null</code> when tokens are burned.

Now let us implement a NEP5-Token!



## Implementation of NEP-5

First of all, we define a readonly property owner to prepresent the owner of the contract. The is the `Owner` and it is a `20` length byte array.
```csharp
// Here string "xxx" stands for the address you assigned as the onwer of address.
private static readonly byte[] Owner = "xxxxxxxxxxxxxxxxxxxxx".ToScriptHash(); //Owner Address
```

Now we begin with the main method and the  trigger:

```csharp
    public static object Main(string method, object[] args)
        {
            if (Runtime.Trigger == TriggerType.Verification)
            {
                return Runtime.CheckWitness(Owner);
            }
            else if (Runtime.Trigger == TriggerType.Application)
            {
	            return true;
            }
        }
}
```
Here the main method accept two arguments. The first one is string `method`,  which is a nep-5 method the user will call to this smart contract. The second one is an array `args`, which represents a list of arguments used in the nep-5 method.

Here we also judge the trigger here. When the triggerType is `Verification`, it means the end user invoke the transaction with the asset transaction. In other words, end user may want to send global asset such as NEO or GAS to or from this contract. In this condition, we should judge if the invoker ( or the Address that signed the contract ) is  owner.

When the triggerType is Application, that means the smart contract is being called by the application (Web/App) with  an InvocationTransaction. In this case, we should call other functions according to the method value. We will fill this part later.

Then we define the functions for name, symbol, decimals, which are fixed value for this contract.

```csharp
[DisplayName("name")]
public static string Name() => "MyToken"; //name of the token
```

```csharp
[DisplayName("decimals")]
public static byte Decimals() => 8;
```

```csharp
[DisplayName("symbol")]
public static string Symbol() => "MYT"; //symbol of the token
```      

We also need  define a Transfer event which is also specified in the `NEP-5` standard.

```csharp
[DisplayName("transfer")]
public static event Action<byte[], byte[], BigInteger> Transferred;
```

Now. Let's define the totalSupply method of the contract. Before that, we should first define a `deploy` method. The deploy method is not specified in the `NEP-5` standard, but should be the first function that called by smart contract owner and called only once. The purpose of deploy function is to set the `totalSupply` value of your `NEP-5` token, and move all the token into the Owner's account balance.   

It is worth noticing that, in tokenized smart contract, the asset is stored in the storage as the key is the address and the value is the balance. Here is tha table which may declare it.

<center>

| Address |   value |
|--|--|
| address1 | 1000 |
| address2 | 200 |
| address3 | 700 |

</center>

```csharp
//Static readonly value of total supply value
private static readonly BigInteger TotalSupplyValue = 10000000000000000;
```

```csharp
[DisplayName("deploy")]
public static bool Deploy()
{
      if (TotalSupply() != 0) return false;
      StorageMap contract = Storage.CurrentContext.CreateMap(nameof(contract));
      contract.Put("totalSupply", TotalSupplyValue);
      StorageMap asset = Storage.CurrentContext.CreateMap(nameof(asset));
      //The contract owner own the total nep-5 token
      asset.Put(Owner, TotalSupplyValue);
      // This is the Event we should fire when NEP-5 asset transferred
      Transferred(null, Owner, TotalSupplyValue);
      return true;
}
```

Now , we have the totalSupply defined in the deployment stage, we can fill our totalSupply method, which get the totalSupply value from the storage.

```csharp
[DisplayName("totalSupply")]
public static BigInteger TotalSupply()
{
    StorageMap contract = Storage.CurrentContext.CreateMap(nameof(contract));
    return contract.Get("totalSupply").AsBigInteger();
}
```
Let's set another mothod `balanceOf`, which get the account `NEP-5` balance of a specified address
```csharp
 [DisplayName("balanceOf")]
public static BigInteger BalanceOf(byte[] account)
{
	  // Do an argument check
      if (account.Length != 20)
          throw new InvalidOperationException("The parameter account SHOULD be 20-byte addresses.");
      StorageMap asset = Storage.CurrentContext.CreateMap(nameof(asset));
      return asset.Get(account).AsBigInteger();
}
```

Now, we have defiend almost all the method required in the `NEP-5` standard except the transfer method, let us fill the main method first.
```csharp
        public static object Main(string method, object[] args)
        {
            if (Runtime.Trigger == TriggerType.Verification)
            {
                return Runtime.CheckWitness(Owner);
            }
            else if (Runtime.Trigger == TriggerType.Application)
            {
                if (method == "balanceOf") return BalanceOf((byte[])args[0]);

                if (method == "decimals") return Decimals();

                if (method == "name") return Name();

                if (method == "symbol") return Symbol();

                if (method == "supportedStandards") return SupportedStandards();

                if (method == "totalSupply") return TotalSupply();

                if (method == "transfer") return Transfer((byte[])args[0], (byte[])args[1], (BigInteger)args[2]);
            }
            return false;
        }
``` 

Now, the only method left is  the transfer method.

```csharp
private static bool Transfer(byte[] from, byte[] to, BigInteger amount, byte[] callscript)
{
      //Check parameters
      if (from.Length != 20 || to.Length != 20)
          throw new InvalidOperationException("The parameters from and to SHOULD be 20-byte addresses.");
      if (amount <= 0)
          throw new InvalidOperationException("The parameter amount MUST be greater than 0.");
      if (!Runtime.CheckWitness(from))
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
```

