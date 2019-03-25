# 调用 CGAS

在 CGAS 所有的代码中，调用 CGAS 的代码难度是很高的，甚至不亚于 Refund 的部分的代码

首先调用智能合约的过程就是构造一笔 InvocationTransaction 的过程，一笔 InvocationTransaction 的数据结构如下：

| 字节数 | 字段       | 类型      | 描述                           |
| ------ | ---------- | --------- | ------------------------------ |
| 1      | Type       | byte      | 交易类型                       |
| 1      | Version    | byte      | 交易版本号，目前为 0 或 1      |
| ?      | -          | -         | 特定交易的数据                 |
| ?*?    | Attributes | tx_attr[] | 该交易所具备的额外特性         |
| 34*?   | Inputs     | tx_in[]   | 输入                           |
| 60 * ? | Outputs    | tx_out[]  | 输出                           |
| ?*?    | Witnesses  | Witness[] | 用于验证该交易的脚本列表       |
| ?*?    | Script     | byte[]    | 包含该交易中智能合约的调用脚本 |

目前调用 CGAS 只能通过 SDK 来自行构造 InvocationTransaction，neo-gui，neo-cli 等客户端无法完美支持 CGAS（只支持 NEP-5 中的方法，不支持 CGAS 的 mintTokens，refund 等方法）。

具体的项目创建、引用 SDK，构造交易可以参考这个项目：[CGAS UnitTests](https://github.com/neo-ngd/CGAS-Contract/blob/master/UnitTests)。

以 MintTokens 为例，下面对构造交易的代码进行讲解。

```c#
public static void MintTokens()
{
    var inputs = new List<CoinReference> {
        new CoinReference(){
            PrevHash = new UInt256("0xf5088ce508d86197c991ff0ef7651ddf01f3e555f257039c972082250e899210".Remove(0, 2).HexToBytes().Reverse().ToArray()),
            PrevIndex = 0
        }
    }.ToArray();

    var outputs = new List<TransactionOutput>{ new TransactionOutput()
    {
        AssetId = Blockchain.UtilityToken.Hash, //Asset Id, this is GAS
        ScriptHash = ScriptHash, //CGAS 地址
        Value = new Fixed8((long)(1 * (long)Math.Pow(10, 8)))
    }}.ToArray();

    Transaction tx = null;

    using (ScriptBuilder sb = new ScriptBuilder())
    {
        sb.EmitAppCall(ScriptHash, "mintTokens");
        sb.Emit(OpCode.THROWIFNOT);

        byte[] nonce = new byte[8];
        Random rand = new Random();
        rand.NextBytes(nonce);
        sb.Emit(OpCode.RET, nonce);
        tx = new InvocationTransaction
        {
            Version = 1,
            Script = sb.ToArray(),
            Outputs = outputs,
            Inputs = inputs,
            Attributes = new TransactionAttribute[0],
            Witnesses = new Witness[0]
        };
    }
    var sign = new SignDelegate(SignWithWallet);
    sign.Invoke(tx, "1.json", "11111111");
    Verify(tx);
}
```

最开始是构造交易输入和交易输出，交易输入是来自自己的地址，交易输出是 CGAS 的地址。其中交易输入不可复用，每次测试都要使用未花费的交易输入。

交易输入包含以下两个字段：PrevHash，PrevIndex，分别表示所使用的交易输出的交易 ID 和索引。

这里要清楚一个概念，在 UTXO 模型中，所有的交易输入必定是之前某个交易的交易输出，这样形成了一个完成的链条。一个交易输出可以用一个“复合主键”来表示，就是交易 ID 和交易输出的索引。这就是交易输入的字段，它引用了一个交易输出的“主键”，而并非引用交易输出的完整数据。

然后开始构造一个 InvocationTransaction，最重要的就是里面的 Script 字段。这里通过 SDK 中的 ScriptBuilder 类进行创建。

交易构造完成后，对交易进行签名，签名的内容会写在交易的 Witness 字段中。

最后在本地进行交易验证，交易验证包括：

1、验证交易的格式是否正确。

2、交易的大小是否超过限制。

3、在一笔交易中是否使用了两个相同的 UTXO。

4、内存池中的交易是否包括该交易所使用的 UTXO。

5、是否双重花费（区块链中的的交易是否包括该交易所使用的 UTXO）。

6、交易中的资产是否过期，转账精度是否符合要求，资产是否存在。

7、手续费是否合法。

8、交易属性是否合法

9、交易的验证脚本是否通过（即执行 Trigger.Verify 部分的代码）。

建议开发者要构造交易的时候要本地单步调试以便发现问题所在。