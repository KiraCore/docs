# [⏎](README.md#Roadmap) SIP_1
>  JS Mock Library

Before work on the HW app can begin we need a simple website page app that can communicate with full node, sign and propagate simplest type of transactions through the RPC.

To execute a transaction and create a signed message a range of js libraries can be used as good reference point:
* https://github.com/node-a-team/cosmosjs
* https://github.com/cosmostation/cosmosjs
 
Main goal of this task is to mock a basic logic flow on top of which the rest of the functionality will be based

1. Encrypting mnemonic
2. Creating unsigned transaction message (fetching sequence)
2. Creating a well structured data to be signed
3. Creating series of QR codes representing data that must be signed
4. Reading QR codes and decoding the data
5. Decrypting mnemonic
6. Signing the data in accordance to embedded information such as derivation path
7. Creating series of QR codes representing signed data
8. Reading QR codes and decoding the data
9. Propagating data to the blockchain over RPC

## Mnemonic Encryption/Decryption

For the [Bip39 mnemonic](https://github.com/bitcoinjs/bip39) encryption we will use AES-256, the password with which the mnemonic will be encrypted must be a hashed using BLAKE2 algorithm.

_NOTE: We must be able to support different mnemonic lengths when signing transactions_

## Unsigned Data

Data that must be signed must contain information such as algorithm or derivation path to enable onchain signer identify the account sign it.

### Example class structure:
```
{
    "version": "v0.0.1",
    "type": "cosmos",
    "algorithm": "secp256k1",
    "network-id": "kira-1",
    "path": "m/44/118/0/0/0",
    "prefix": "kira",
    "data": "...",
    "page": <page-number>/<max-pages>,
}
```

The `version`, `type`, `algorithm`, `network-id`, `prefix` & `path` should only be specified in the first QR code page, further pages should only contain data and page.

_NOTE: It must be configurable in the mock to split data into variable number of pages._

## QR Code

QR codes should incorporate a logo / graphical element with a page number - this will enable user to visually identify that QR code is changing. 

