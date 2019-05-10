- Wallet 
	- Keys and address
		- Hash function used in NEO
		- Generation of Private key
		- ECDSA algorithm
		- Base58 check
		- Mnemonic words
		- The scripthash and address of NEO
	- Wallet file
		- NEP6 wallet (json based)
		- DB3 wallet (sqlite)
	- The usage of NEO-GUI
	- UTXO model
	- Account model
		- The peronsal account
		- The contract account


# Understanding Wallets
When a user typically begins with NEO or other blockchains they are given a user "wallet". This is different from modern centralized applications where user is given a user account which is hosted on some centralized server. Although your wallet is used to access your NEO, GAS, and other tokens, the name is actually a bit of a misnomer. A wallet at its core is a cryptographic public/private key pair which is used to sign and authenticate transactions that occur on the NEO network. 

Let's consider how a user would perform a write operation on a traditional centralized database vs how a user would perform a write operation to the NEO blockchain.

## Centralized Database
The user would first create an account using some sort of credentials like email/password. These credentials are stored on the services database. When the user logs in to the service they receive some form of session token in their local environment which allows them to perform write operations on the services database.

### Advantages:
-> If user loses their credential information, it can be recovered by service
-> This is a standardized flow that user is used to

### Disadvantages
-> Storing all user credentials in a centralized error makes this attractive for hacking
-> A seperate set of credentials needs to be generated for every single service


## NEO Blockchain
A user would generate a public/private key pair. These key pairs are only stored locally on the user device, in a dedicated hardware module, or somewhere else in the client. These key pairs NEVER touch a remote server. When a user wishes to perform a write an operation on the NEO blockchain (database), they generate a transaction locally with their intended operation, for example sending 1 NEO to a friend. They then sign this transaction with their cryptographic signature, which is generated via their wallet or public key pair.

This transaction is then verified, and distributed amongst the network which finalizes the write operation

### Advantages
-> No central point of attack for a hacker. This removes a lot of responsibility from the service provider
-> User credentials can be shared, amongst various service providers

### Disadvantages
-> No recovery mechanism if user lose their credentials
-> New UX pattern for users who are not used to this kind of system


So in summary we can summarize a wallet as a public/private key pair which used to perform write operations on a distributed database (blockchain). It has advantages and disadvatnages compared to typical client/server authentication architecture, but we believe that the security and user control that this system provides, allows for an overall more robust system. 

We'll now go into some of the specifics about NEO key architecture.

## Keys and Addresses
So how do we actually generate a wallet?  First we generate a private key, which is simply a 64 character hexadecimal string. This represents a number between the range 0 and 2^256 (1.15792089e77). From this number, the rest of your “Account” information is derived. For our purposes an account will consist of your Private Key, WIF (Wallet Import Format), public key, and address.

The first challenge of any wallet software is deriving the account from the private key. We’ll go briefly go into more details of the 3 other keys. If you’re already familiar with address generation from another blockchain feel free to skip ahead, as Neo basically follows the same method as all others.

### WIF
The WIF is relatively to understand. In practice a private key can end up looking something like this…

0C28FCA386C7A227600B2FE50B7CAEEC86D3BF1FBE471BE89827E19D72AA1D 

It would be nice to have something that is at least a little bit more human readable, so we can convert the private key into the WIF otherwise known as the wallet import format

5HueCGU8rMjxEXxiPuD5BDku4MkFqeZyd4dZ1jvhTVqvbTLvyTJ

which although still not entirely readable is certainly better than the original string. WIF also has some basic error checking so that when you send to an address denominated by WIF format you are more likely to catch an error. The conversion from the raw private key to the wif format was done via a BASE 58 check encoding algorithm.

## Base 58 check encoding
Base 58 is similar to the common base 64 encoding scheme exept that it removes non alphanumeric characters as well as characters that might look similar to each other to the human eye. For example 0 (zero), O (capital o), I (capital i) and l (lower case L) are all omitted from the base 58 encoding scheme. The full list of available characters in NEO's base58 encoding is 123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz.

A full implementation of NEO's check encoding written in go can be seen below
```
func b58checkencode(ver uint8, b []byte) (s string) {
	/* Prepend version */
	bcpy := append([]byte{ver}, b...)

	/* Create a new SHA256 context */
	sha256_h := sha256.New()

	/* SHA256 Hash #1 */
	sha256_h.Reset()
	sha256_h.Write(bcpy)
	hash1 := sha256_h.Sum(nil)

	/* SHA256 Hash #2 */
	sha256_h.Reset()
	sha256_h.Write(hash1)
	hash2 := sha256_h.Sum(nil)

	/* Append first four bytes of hash */
	bcpy = append(bcpy, hash2[0:4]...)

	/* Encode base58 string */
	s = b58encode(bcpy)

	/* For number of leading 0's in bytes, prepend 1 */
	for _, v := range bcpy {
		if v != 0 {
			break
		}
		s = "1" + s
	}

	return s
}
```
The steps to perform the check encoding can be broken down as follows
1. Prepend the version byte
2. Double Hash the resulting hex using SHA256
3. Append the first bytes of the hash to the prepended version
4. Convert the hex with prepended version and appended checksum to base58.
5. If any leading zeros in the bytes attach 1

So to go from the original private key described above  to the wif format we can use this simple function

```
// ToWIF converts a Bitcoin private key to a Wallet Import Format string.
func (priv *PrivateKey) ToWIF() (wif string) {
	/* See https://en.bitcoin.it/wiki/Wallet_import_format */

	/* Convert the private key to bytes */
	priv_bytes := priv.ToBytes()

	/* Convert bytes to base-58 check encoded string with version 0x80 */
	wif = b58checkencode(0x80, priv_bytes)

	return wif
}
``` 

## Public key derivation
The public key is derived used cryptography which follows standard elliptic curve derivation. NEO keys use the SECP256R1 elliptic curve in contrast to bitcoin which uses SECP256K1.

Both Bitcoin and Ethereum have excellent explanations about Elliptic Curve Cryptography and public key derivation so I will not delve into them here. The important thing to know for us is that the public key is derived FROM the private key in a way function. It is impossible to go from the private key back to the public key.

If you would like to learn more about the mathematical properties of key derivation, I would reccomend the relevant sections in mastering bitcoin and mastering ethereum

https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch04.asciidoc#public-keys

https://github.com/ethereumbook/ethereumbook/blob/develop/04keys-addresses.asciidoc#elliptic-curve-cryptography-explained

Just remember NEO uses the curve SECP256R1 NOT SECP256K1 like bitcoin or ethereum

For a reference NEO implementation of key generation I reccommend looking at 

https://github.com/O3Labs/neo-utils/tree/master/neoutils

You'll notice that the portion that performs key derivation is actually a modified version of btckey a key generation library for bitcoin.


## NEO Address
Similar to the wif format the NEO address allows for a simpler, more human readable format for the public key. 

```
// ToNeoAddress converts a NEO public key to a NEO address string.
func (pub *PublicKey) ToNeoAddress() (address string) {
	/* Convert the public key to bytes */
	pub_bytes := pub.ToBytes()

	pub_bytes = append([]byte{0x21}, pub_bytes...)
	pub_bytes = append(pub_bytes, 0xAC)

	/* SHA256 Hash */
	sha256_h := sha256.New()
	sha256_h.Reset()
	sha256_h.Write(pub_bytes)
	pub_hash_1 := sha256_h.Sum(nil)

	/* RIPEMD-160 Hash */
	ripemd160_h := ripemd160.New()
	ripemd160_h.Reset()
	ripemd160_h.Write(pub_hash_1)
	pub_hash_2 := ripemd160_h.Sum(nil)

	program_hash := pub_hash_2
	
	/* Convert hash bytes to base58 check encoded sequence */
	address = B58checkencodeNEO(0x17, program_hash)

	return address
}
```

The unencoded base58 version of the address is know as the ScriptHash. This hex string is typically what is used in smart contracts as the public identifier as opposed to the address. Since the use of byte arrays is common, it makes a lot more sense at the base58 encoded versions is meant to be read by humans, not computers!

## Mnemonic Words
The use of Mnemonic words is not common in the NEO ecosystem, this section should be omitted as there is no proposed NEP for the use of mnemonic phrases for seed derivation

## Nep6 File
Storing raw private keys on disk is a security liability. Anyone that that has access to a raw private key can drain these funds. It would be more secure if these keys were encrypted via a password. For this reason we have the NEP-2 standard format (https://github.com/neo-project/proposals/blob/master/nep-2.mediawiki)

This encrypted key provides an additional layer of security to the raw private so that the attacker would require would both the encrypted key and the password in order to access that funds. This is good, but it is often that one might need to have multiple wallets, which means that they have multiple keys. Storing each NEP2 encrypted key would be very cumbersome, so instead we can create a file structure that would allow for all of these encrypted keys to be stored in the same place. 

This standards allows for a standard way of importing wallets into various blockchain clients and contains the additional secruity guarantees of the NEP2 format.

A full specification of the file format can be found here. (https://github.com/neo-project/proposals/blob/master/nep-6.mediawiki). It follows a JSON structure that contains information about the private/public key pairs as well as metadata about each account. The metadata details contains info like which wallet should be used as default, the encryption parameters, and any other relevant metadata. 

The NEP6 file also supports watch only addresses. Watch only addresses do not contain any information related to the private key, which maybe useful if the account is stored seperately in a more secure location.

## Contract Account
NEO also supports more sophisticated Account Types. In these cases the funds are not associated with a single user but stored in a smart contract. The contract would create special rules on what is required in order for funds to to be withdrawn from this account. The most common case of this type of account is a multi signature account. A multi signature account requires that N of X people provides signatures for the transaction in order to transfer funds. For example 2 of 3 of the account owners must sign in order for the funds to be withdrawn. 

We can generate a simple contract for this account using NEO op codes

Suppose we want to create a multisig for THREE different persons (public keys):

036245f426b4522e8a2901be6ccc1f71e37dc376726cc6665d80c5997e240568fb
0303897394935bb5418b1c1c4cf35513e276c6bd313ddd1330f113ec3dc34fbd0d
02e2baf21e36df2007189d05b9e682f4192a101dcdf07eed7d6313625a930874b4
We want to have at least TWO of them to sign the transactions, so we proceed this way:

push on stack value 2 (minimum number of signatures). opcode 52
push each of the three public keys (remember this order is important). opcode 21 (corresponds to 33 bytes)
push value 3 (total number of people). opcode 53
check that multisig is correct (opcode ae CHECKMULTISIG)
This is the result: 52 + 21 + 036…fb + 21 + 030…0d + 21+02e…b4 +53+ae

5221036245f426b4522e8a2901be6ccc1f71e37dc376726cc6665d80c5997e240568fb210303897394935bb5418b1c1c4cf35513e276c6bd313ddd1330f113ec3dc34fbd0d2102e2baf21e36df2007189d05b9e682f4192a101dcdf07eed7d6313625a930874b453ae

We then calculate the script hash and address of this account with the methods that we have already described previously

Calculate the scripthash (and Address): 4d0c0932fa032debdceaaf5cd8086cf3f882961f / AJetuB7TxUkSmRNjot1G7FL5dDpNHE6QLZ

This contract information can also be stored in the NEP6 file, which allows you to keep track of accounts that are not necessarily associated with a single private key. More complex account types can be created using NEO's scripting capabilities. 

Multi signatures are currently supported in the NEO-GUI wallet.

### NEO DB3
NEO db3 is a legacy file format that was previously supported in NEO GUI prior to the introduction of the NEP-6 file format. It is highly reccommended to upgrade to NEP6 file format which can be done in the NEO-GUI

https://docs.neo.org/en-us/node/gui/wallet.html


## UTXO Model
One of the main purposes of wallets is facilitatinf asset transfer. Assets are divided into categories. One's which are based on UTXO's (Unspent transaction outputs) and ones which are based on the account model. In NEO, NEO and GAS follow the UTXO model while NEP-5 tokens follow the account model. Lets go into moth in more detail. 

Let's consider a simple example where a user has 10 NEO. This 10 NEO actually consists of multiple UTXO's. The sum of all the UTXO's must equal 10. For example this 10 NEO may be consist of 3 UTXOs. UTXO_1 is worth 2 NEO, UTXO_2 is worth 3 NEO, and UTXO_3 is worth 5 NEO, which sums up to the total balance of 10 NEO. So if we need to send someone 3 NEO then we can simply use UTXO_2 3 as the input of the transaction and the the recipient receives as an output UTXO also worth 3 NEO. 

If we try to send 5 NEO, then we can combine the UTXO_1 and UTXO_2 together as the inputs, and the recipient recieves 5 NEO as a single output of the transaction. It becomes slightly more complicated when we need to send an amount where we cannot create a perfect sum of UTXOs. Let's sat that we want to send 4 NEO to someone. No combination of UTXO's will allows us to get get 4 exactly. The best we can do is use UTXO_1 and UTXO_2 together which will combine to equal 5 NEO. So we use UTXO_1 and UTXO_2 as inputs to our transaction, but instead of having a single output like the previous examples we need to have to output UTXO's. One worth 4 NEO is generated for the recipient, and then a new UTXO worh 1 NEO is returned as change back to our account.

So for core NEO transactions they mus satisfy this formula in order to be considered valid on the network.

Sum(NEO_i) + Sum(GAS_i) = Sum(NEO_o) + (Sum(GAS_I) - Sum(GAS_sys_fee) - Sum(GAS_net_fee))

In this sense UTXO's are not created or destroyed, but recycled into new ones. Inclusion of UTXO's allows for parallel transaction execution, as each UTXO is unique it is impossible to double spend. 

Account Model
The account model which is adopted by other blockchain platforms like Ethereum creates a global state for each account which has coins. So instead of having a set of UTXOs which you can use for your transaction, you would simply have a balance of 10 associated with your account. Because of this, the global state of all acounts must be store locally on the nodes in the network. Transactions are interpreted by the virtual machine in the network, and make the corresponding state changes to all accounts in the global state. 

NEP-5 token contracts deployed on the NEO network typically follow the account model of balance storage. They do not have any associated UTXO data, and changes in balance state are done via smart contract executions. These executions are interpreted by the NEO virtual machine, and recorded in the smart contract storage area. 

### Creating a wallet
There are several options avialable to you when creating a new wallet.

For full blockchain syncronization, consider
* NEO-GUI -> https://docs.neo.org/en-us/node/gui/install.html
* NEO-CLI -> https://docs.neo.org/en-us/node/cli/cli.html

For light clients which do nor require synchronization consider
* O3 Wallet -> https://o3.network/
* NEON Wallet -> https://neonwallet.com/

You can find more detailed usage guides at the relevant wallet links







