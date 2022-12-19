# [⏎](README.md#Roadmap) KIP_9
> Session Keys

Session keys will be used by INTERX to quickly identify user to provide functionalities related to his external decentralized identity and / or rate limit requests to the API based on the KEX token balances. 

It should be guaranteed by the KIP_4 that all submitted singer keys are unique. The KIP_4 further maps account ownership to the specific signer keys acting as the hot-key for proving ownership. Thanks to signer keys client identity can be proven and account balance checked without need for signing any message using hardware wallet.

## Key Generation

The frontend application has to enable creation of secure-random `sr25519` signer keys and their submission along expiration time and description (name or use case). This has to be abstracted by a single widget enabling creation and re-creation of the session key. To identify that the key should be treated as session key following data can be attached:
```
{
    "use_case": "frontend_session_key"
}
```

After signature is generated the public and private key can be saved in the encrypted form within the local keystore to enable ease of access.

## Execution

When frontend communicates with the INTERX API the `kira_session` header in the json format should be present in the request:

```
{
 "pubkey": "<string>", // sr25519 pubkey
 "account": "<string>", // kiraXXX...XXX
 "block_hash": <Uint64>, // latest block hash
 "nonce": <Uint64>,
 "signature": "<string>" // sr25519 signature
}
```
Where signature is an `sr25519` of the `account`, `block_hash` & `nonce` fields. `kira_session` header can be reused with any request for maximum of `5 minutes` after which `block_hash` has to be updated. The nonce field does not have to be used in the scope of this KIP. The `block_hash` is used to make the signature unpredictable for the client stripping him from ability to spam the API by creating signatures beforehand. In the future `nonce` can be required by INTERX as part of the difficulty challenge to decrease intensity of potential spam attacks.


