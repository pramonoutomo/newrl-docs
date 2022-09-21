---
sidebar_position: 2
slug: /client-apis
---

# Newrl Client APIs

Newrl Dapps interact with the chain using API calls to public/public nodes. A list of public nodes available is listed on the Newrl website.
Once the Dapp finish the prototyping phase, it's reccomended to use own nodes for querying and transaction submissions.

Newrl client exposes a few query APIs and two POST APIs for submitting transaction to the node. A swagger page listing the APIs can be found on `/docs` API on every node. 

### APIs

- [/get-block](client-apis#get-block)
- [/get-balance](client-apis#get-balances)
- [/sc-state](client-apis#sc-state)

### /get-block

Returns the balance of the account of provided Pubkey

#### Parameters:

- `<integer>` - Block index to get

#### Results:

- `Block` - Returns a JSON with the block data

#### Example:

Request:

```bash
curl http://devnet.newrl.net:8420/get-block?block_index=50
```

Result:

```json
{
    "index": 6459,
    "timestamp": 1663751953000,
    "proof": 0,
    "status": 1,
    "text": {
        "transactions": [],
        "previous_block_receipts": [
            {
                "data": {
                    "block_index": 6457,
                    "block_hash": "4d009febdeb7d8ec3d0b3d7dbe0d71bcd8a266d316776afb58b967c722a47163",
                    "vote": 1,
                    "timestamp": 1663751933000,
                    "wallet_address": "0x61b1de29d392d7fd9be06f7c743275f966b6d507"
                },
                "public_key": "22208bfb50d243036330b0902f1a8c1bd3e653409601cf05b921227de9325167235c8f1e833de437ac495c6b1eb354254456f69c3d34f41b5f3d8b87393be1c4",
                "signature": "4b3c562d416ca36e256408ca2e452ab60bf5d1f2d18497604f4cd47b227d8b934a6d3d6a387bff8692342260605eefdb987823f8ebf76726d24a963ded1f6d02"
            },
            {
                "data": {
                    "block_index": 6457,
                    "block_hash": "4d009febdeb7d8ec3d0b3d7dbe0d71bcd8a266d316776afb58b967c722a47163",
                    "vote": 1,
                    "timestamp": 1663751933000,
                    "wallet_address": "0xff57ff553eff99242ac612af770a8baae9a0cd26"
                },
                "public_key": "a4333f448add29992e612714a136d4585bfd561edd66e33d585b64d64f40914f99eef1c9e9cc4d36462be0983b752d4cc72b40d4a7b89d2e09971fa03034a5ee",
                "signature": "f3015830b36b75650fb98c62d8829ddd6ef37ffbdc1794c917a04f3374f4a1fb92e836c37d33cdf989ac6f805d269ec079cfbd442d138ea1f7652ea858695050"
            },
            {
                "data": {
                    "block_index": 6457,
                    "block_hash": "4d009febdeb7d8ec3d0b3d7dbe0d71bcd8a266d316776afb58b967c722a47163",
                    "vote": 1,
                    "timestamp": 1663751933000,
                    "wallet_address": "0x2ff981300feaadbf48b00a72f03761a7b6aedbfd"
                },
                "public_key": "bb233f2dee2f89dbfe3158c406d2f42f02e55cb604268a7162782d995a1ff74be0dc14a7e6b6697ddb8de365f7ee9d0636b6310ee06c10a300a84202b574f354",
                "signature": "bd06480920f53c9aee4501de63cb698a7ea372769db49b026af6a5913700e05443726210b23a9549ecf29c82ed6cf0df9df17fc392fe8087fcb65889874f1ac4"
            },
            {
                "data": {
                    "block_index": 6457,
                    "block_hash": "4d009febdeb7d8ec3d0b3d7dbe0d71bcd8a266d316776afb58b967c722a47163",
                    "vote": 1,
                    "timestamp": 1663751933000,
                    "wallet_address": "0xf38b0ad67bfff4d8aaac00192d0c8f6e9e5d1905"
                },
                "public_key": "9275fb1d9a44614edc612bda187089f4c05fe0dae62f8e34d765f9e95e56fd21ef528a9192ddae51088e7074ccbf177ee141f777f6e444b9cc6855b18c274c0a",
                "signature": "255575a39b245b6cbaf69fa31d0a9e14f61563af44eda6375f2ca294e476f4bfda8a09bec75eedb663795134752bee1267b508d308fe6a1b407638408ad178fb"
            },
            {
                "data": {
                    "block_index": 6457,
                    "block_hash": "4d009febdeb7d8ec3d0b3d7dbe0d71bcd8a266d316776afb58b967c722a47163",
                    "vote": 9,
                    "timestamp": 1663751933000,
                    "wallet_address": "0x2f92c94a03884a328adbe9876a8160d17792eb55"
                },
                "public_key": "ddce4bd6b48e98532007b9b578535b08ed5e0e03ff7daf0844513623066127eaf3b853979624d6b182ae3e6f21bc7074f85da1267f9e93da635f5e21d95d58b2",
                "signature": "963a5c594a2432491d998000c7dc3a4cab754bc570f9486237364a8aa42f0e04fc310345721f7019ab66c2ed98def29ca6607f0a28e41b68ef447dba056b76cd"
            },
            {
                "data": {
                    "block_index": 6457,
                    "block_hash": "4d009febdeb7d8ec3d0b3d7dbe0d71bcd8a266d316776afb58b967c722a47163",
                    "vote": 1,
                    "timestamp": 1663751933000,
                    "wallet_address": "0x39f6bf764db1e547fe1b1e9f2d3e0cd89638e8c1"
                },
                "public_key": "68786eba5ad53c5187ba994d629514133080ed23439f125a323610d58b66c0036b48ac6051d830720df762932659d54a6d5cff6f50847e0c82ae80a31e305c36",
                "signature": "fde3a6b9628d08a177a134a541a58ebe40d948767727f265306a187f895df077edb0dfc9c29e4cb7e9a202aedb61cffab144ee0f128207aa4b7af5e51a83436f"
            }
        ]
    },
    "creator_wallet": "0x2ea5d98b4041d035a65b8386fc7bf24eabb30838",
    "expected_miner": "0x2ea5d98b4041d035a65b8386fc7bf24eabb30838",
    "committee": [
        "0x2cc63af299e9ae88751a05fc824c27b1a300abbf",
        "0x2ea5d98b4041d035a65b8386fc7bf24eabb30838",
        "0x2f92c94a03884a328adbe9876a8160d17792eb55",
        "0x395a7ccaea22c3fcae318cacfe49846e2694aadf",
        "0x39f6bf764db1e547fe1b1e9f2d3e0cd89638e8c1",
        "0xabc61330745a9d15c232149e4193578d56caafb8",
        "0xe9af88fc42076ad3a42e0f0e140b44d482210bf9",
        "0xf38b0ad67bfff4d8aaac00192d0c8f6e9e5d1905",
        "0xf8d48f7a08997f9764bf372346be264ff81f19cc",
        "0xff57ff553eff99242ac612af770a8baae9a0cd26"
    ],
    "previous_hash": "7b472a1cc54ea446a5e8f1016ae3fffb8ee431f92869b20b8734169986b54e4f"
}
```


### /get-balances

Returns the balance for 
- Token in wallet
- All tokens in wallet
- All wallets for a token

#### Parameters:

- `<enum>` - balance_type [TOKEN_IN_WALLET, ALL_TOKENS_IN_WALLET, ALL_WALLETS_FOR_TOKEN]
- `<string>` - (optional) wallet_address
- `<string>` - (optional) token_code

#### Results:

- `JSON<string>` - RpcResponse JSON object with `value` field set to the balance

#### Example:

Request:

```bash
curl -X 'GET' \
  'http://3.230.172.119:8420/get-balances?balance_type=TOKEN_IN_WALLET&token_code=NWRL&wallet_address=0x61b1de29d392d7fd9be06f7c743275f966b6d507' \
  -H 'accept: application/json'
```

Result:

```json
{
  "balance": 25000000000
}
```

### /sc-state

Get the state variables stored for a smart contract

#### Parameters:

- `<string>` - table_name - smart contract table name
- `<string>` - contract_address - deployed smart contract address
- `<string>` - unique_column - column of sc to query by
- `<string>` - unique_value - query string


#### Results:

- `JSON<string>` - RpcResponse JSON object with `value` field set to the balance

#### Example:

Request:

```bash
curl -X 'GET' \
  'http://3.230.172.119:8420/get-balances?balance_type=TOKEN_IN_WALLET&token_code=NWRL&wallet_address=0x61b1de29d392d7fd9be06f7c743275f966b6d507' \
  -H 'accept: application/json'
```

Result:

```json
{
  "balance": 25000000000
}
```


### /submit-transaction

Submit a signed transactionto the network

#### Parameters:

- `<json>` - transaction to submit

#### Results:

- `<json>` - Response json object with validation result from the node

#### Example:

Request:

```bash
curl --header "Content-Type: application/json" \
  --request POST \
  --data '
  {
    "transaction": {
        "timestamp": 1663756139000,
        "trans_code": "bdf6d7ce2bc984f9f3e507219f19111ac863769a",
        "type": 3,
        "currency": "NWRL",
        "fee": 0.0,
        "descr": "",
        "valid": 1,
        "specific_data": {
            "address": "ctb020e608d11c235724e676d021a08f8da6c64fb9",
            "function": "transfer",
            "signers": [
                "0x667663f36ac08e78bbf259f1361f02dc7dad593b",
                "0xd506831f17f6936e27bd1a9187efd48c23c0bcbb",
                "0xbc54ef523d92b6acaf16a49b328cfffca84503ca"
            ],
            "params": {
                "amount_to_send": 100,
                "receiver": "0xe9af88fc42076ad3a42e0f0e140b44d482210bf9",
                "asset_code": "NWRL"
            }
        }
    },
    "signatures": [
        {
            "wallet_address": "0x667663f36ac08e78bbf259f1361f02dc7dad593b",
            "msgsign": "501372670621d7dfede1e5cf797297b8a670aa1f08986d3405baaaacbc74ddfe20179084fe7265e290035e4266075e715ea3874475afaff050df5756f1f68b70"
        },
        {
            "wallet_address": "0xd506831f17f6936e27bd1a9187efd48c23c0bcbb",
            "msgsign": "33640f63be308e0fd9a03ac0637b67d6b9ad89224d7e3150322703033006d820cdf4081979e8a15d8e6cb06edeb22ce4aabd484492b7a2820dbbb4c1a78c2473"
        },
        {
            "wallet_address": "0xbc54ef523d92b6acaf16a49b328cfffca84503ca",
            "msgsign": "eadb22829872f6664371317126ce25376607aabe72d0d55af960781580d8dff67cd39cd79e298b7c6c5ebe4e01165e5220086476e0d1057c18afce2deddebbde"
        }
    ]
}
  ' \
  http://devnet.newrl.net:8420/submit-transaction
```

Result:

```json
{
    "status": "SUCCESS",
    "response": {
        "valid": true,
        "msg": "Transaction economic validation successful",
        "new_transaction": true
    }
}
```


### /get-trustscore-wallets

Return the trust score for a person with another by their wallet ids

#### Parameters:

- `<string>` - src_wallet_address - From wallet address
- `<string>` - dst_wallet_address - To wallet address

#### Results:

- `<json>` - Field `trust_score` denoting trust score between wallets

#### Example:

Request:

```bash
curl -X 'GET' \
  'http://34.205.131.228:8420/get-trustscore-wallets?src_wallet_address=0x667663f36ac08e78bbf259f1361f02dc7dad593b&dst_wallet_address=0xf38b0ad67bfff4d8aaac00192d0c8f6e9e5d1905' \
  -H 'accept: application/json'
```

Result:

```json
{
  "status": "SUCCESS",
  "trust_score": 413377
}
```