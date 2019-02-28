# UTXO
If you are familiar with blockchain or use some digital coins before, you may hear of the word `UTXO`. The UTXO stands for `Unspent Transaction Output`

The NEO blockchain supports native assets, the two most important ones being NEO and GAS. Native assets are Unspent Transaction Output (UTXO) based and are understood natively by the blockchain. Contrast this with tokens like the one we’ve built so far which live entirely in custom smart contracts. Unlike the account balance model, the UTXO model does not directly record account assets, but calculates user assets through unspent output. Each UTXO asset (such as a global asset) is an input-output association model, `input` specifies the source of funds, and `output` indicates the asset destination. 

In the picture below, Alice gets 8 GAS's share from her holded NEO, which is recorded in the first output in transaction *#101*. When Alice transfers 3 GAS to Bob, input of new transaction records the asset is 8 GAS, which is represented by output position 0 of transaction *#101*. Furthermore, in another transaction *#201*, one output points to the 3 GAS transferred to Bob, while another one to 5 GAS back to Alice herself (small change).

<p align="center">
    <img src="https://docs.neo.org/developerguide/en/images/blockchain/utxo_en.jpg">
</p>
