---
sidebar_position: 4
---

# Staking

## Steps to Stake 
1. A wallet on chain
2. Minimum stake of `500k` NWRL tokens in wallet.
3. A computer with minimum performance of a t2.small for participating in block validation.

:::tip
Newrl foundation stakes for free on behalf of first 100 nodes on the mainnet. Faucet can be used to get NWRL tokens for staking on testnets. 
:::


### Faucet
Newrl [Faucet](https://wallet.newrl.net/faucet/) can be used to get NWRL tokens for staking.
![TestnetFaucet](/img/testnet_faucet.png)


### Staking using Wallet
Staking can be done using Newrl wallet app by importing the wallet from node as mentioned in `Run a validator Node` section. The existing stake and mining status can also be checked from the wallet app.

![WalletStake](/img/wallet_staking.png)

### Staking with direct transactions
Staking is done using a transaction call to the Newrl Stake smart contract. The transaction can be signed and submitted to any node.

```json
{
    "sc_address": "ctcdb91798f3022dee388b7ad55eeea527f98caee4",
    "function_called": "stake_tokens",
    "signers": [
        "0xf343ac661d3ae385dcb405cb1b393acc2d69b17a"
    ],
    "params": {
        "value": [
            {
                "token_code": "NWRL",
                "amount": amount_to_stake
            }],
            "token_amount": amount_to_stake,
            "wallet_address": wallet_to_stake
        
    }
}
```

Once staking transaction is submitted, the stake can be verified using the link 

Note: Please replace the the value '0xa9ce833fa8deaf8e7f21f493335beff4386c5c22' with your own node wallet address.
- Testnet `http://archive1-testnet1.newrl.net:8421/sc-state?table_name=stake_ledger&contract_address=ct1111111111111111111111111111111111111115&unique_column=wallet_address&unique_value=0xa9ce833fa8deaf8e7f21f493335beff4386c5c22`

Below is the sample response

```json
{
    "status": "SUCCESS",
    "data": [
        "ct1111111111111111111111111111111111111115",
        "piaebd403e1864223c07413075054c1936d6ad17dd",
        "0xa9ce833fa8deaf8e7f21f493335beff4386c5c22",
        800000000000,
        1664279562000,
        "[{\"0xa9ce833fa8deaf8e7f21f493335beff4386c5c22\": 100000000000}, {\"0xce4b9b89efa5ee6c34655c8198c09494dc3d95bb\": 700000000000}]"
    ]
}
```
Here, fourth parameter of data list "800000000000" is the amount of tokens staked.
