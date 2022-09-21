---
sidebar_position: 5
slug: /sign-transaction
---

# Sign Transactions

A transaction which is to be submitted to the blochain need to be signed using the private key of signer. This is best as an offline operation at the user's end.
Certain applications require the user's keys be stored on their backend but recommendation is to let the users store their private keys offline and sign using Newrl wallet app, plugin or website. 

A transaction can be signed using Newrl Python SDK, Javascript SDK or using the native constructs of the DApp programming language.

Below is a signing function implemented in Python for signing a transaction on Newrl
```python
def sign(private_key, transaction):
    pvtkeybytes = bytes.fromhex(private_key)
    msg = json.dumps(transaction).encode()
    sk = ecdsa.SigningKey.from_string(pvtkeybytes, curve=ecdsa.SECP256k1)
    msgsignbytes = sk.sign(msg)
    msgsign = msgsignbytes.hex()
    return msgsign
```

A list of SDKs for Newrl are listed [here]