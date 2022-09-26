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
`http://devnet.newrl.net:8420/sc-state?table_name=stake_ledger&contract_address=ctcdb91798f3022dee388b7ad55eeea527f98caee4&unique_column=wallet_address&unique_value={wallet_to_stake}'`