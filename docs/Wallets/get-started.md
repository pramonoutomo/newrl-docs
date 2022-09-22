---
sidebar_position: 6
slug: /wallets
---


# Wallets

   This section describes on what a wallet is in the Newrl network and how to create and interact with Newrl network using it.

   
## Wallets on Newrl

A wallet in general is a set of keys that can be used to send, receive and track ownership of assets. 

A usual wallet contains a securely produced private key and a public key derived from it. Address is a unique identifer for the wallet that is further derived from the public key. Three of these components would consititute a wallet.

A private key represents ownership of the wallet, and thus **should never be shared**. Private keys are used to digitally sign a transaction which can be of
fund trasnfer or smart contract invocation. It is important to note that funds can be permentantly lost if a priate key lost. 

A public key for wallet address derived from it would not give access to funds but would help in identifying the wallet, thus **it is ok to share public key** or wallet address across. If we consider a token transfer usecase,
 where a party wants to send money they need to know the receiving parties wallet address.

