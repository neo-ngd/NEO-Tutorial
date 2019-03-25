# Building a Dapp based on NEO

In this tutorial we are going to build your first Dapp on the NEO, which is an online Monster game shop. This tutorial is for general developers who has basic knowledge of web development and understanding the usage of javascript. And before this tutorial, it is better to learn the [NEO smart contract development](https://github.com/neo-ngd/NEO-Tutorial/blob/master/9-smartContract/Smart_Contract_basics.md) first.

## Smart contract

The main purpose of this demo is to writing the smart contract and using the javascript to handle the front-end logic for its usage.

First of all, we need to write the smart contract part. The general idea of this monster game is each Monster in the shop has an ID, and the shop offer a kind of coupon which we can exchange from the store and then buy these monsters. Therefore, we can use the most part of [ITO] contract, to let players exchange the coupon within the store, and then use the coupon to buy the Monsters.

  
