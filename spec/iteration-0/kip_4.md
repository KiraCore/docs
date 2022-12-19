# [⏎](README.md#Roadmap) KIP_4
> Register Signer Key

Every account should be able to register multiple public keys which can later be used to sign RPC responses or in general used for other various purposes that require a hot/sub keys. 

To submit `upsert_signer_key` transaction following properties are required:

* `pubkey` - New public key to register (max `4096 Bytes`)
* `type` - Key type enum (e.g. `secp256k1`, `ed25519`)
* `expires` - UTC time (`8 Bytes`) when key expires
* `enabled` - boolean field defining if key is use
* `data` - defines information related to the key e.g. json string or memo indicating use case (`4096 characters` max)
* `permissions` - array of integers defining permissions that the owner assigned to the key (this field does not have to be used in PoC)

_NOTE: When new key is added all expired keys of the user should be removed from the state to save storage space, for this reason we need to create index that will help us quickly identify keys belonging to specific user._

Metadata index for the KV store must consist of the `4 Bytes` prefix of the blake hashed user public key, `4 Bytes` blake hashed `pubkey` and `4 Bytes` of unique, iterative id.

Users should have ability to update key properties such as expires, permissions, data or enabled fields. It must also be possible to remove the key entirely.

It should not be possible to store two keys with the same public key.

## Fees (OPTIONAL)

### Execution Fee

There must be a custom, flat [execution fee](../fees.md#execution-fee) `Ε` paid when signer key is created as the result of `upsert_signer_key` execution.

Execution Fee should be set at `$1` or (`20 KEX`)

### Fees Distribution

All fees paid in the native or non native currencies should be sent to the address controlled by the "rewards distribution module" (RDM). The RDM module does not have to be be implemented in the `Iteration 0`
