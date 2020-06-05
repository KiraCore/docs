# KIP_4
> Register Signer Key

Every account should be able to register multiple public keys which can later be used to sign RPC responses or in general used for other various purposes that require a hot key - including creating session keys which can help to rate-limit queries and help prevent DOS attacks. 

To submit `rotate_signer_key` transaction following properties are required:

* `pubkey` - New public key, should not exceed `4096 bits` (max `512 Bytes`)
* `type` - Key type enum (e.g. `RSA`, `ECC`, `AES`)
* `expires` - UTC time (`4 Bytes`) when key expires
* `enabled` - boolean field defining if key is use
* `owner` - user or curator public key (max `512 Bytes`, defines who can modify this key, this field does not have to be used in PoC)
* `permissions` - array of bytes defining permissions of the owner (max `32 Bytes`, this field does not have to be used in PoC)
* `memo` - Generic string field that can act as name, signature, explain purpose of the `pubkey` and act as data/info store (max `1024 Bytes`)

_NOTE: When new key is added all expired keys of the user should be removed from the state to save storage space, for this reason we need to create index that will help us quickly identify keys belonging to specific user._

Metadata index for the KV store must consist of the `4 Bytes` prefix of the blake hashed user public key, `4 Bytes` blake hashed `pubkey` and `4 Bytes` of unique, iterative id.

## Fees (OPTIONAL)

### Execution Fee

There must be a custom, flat [execution fee](../fees.md#execution-fee) `Ε` paid when signer key is created as the result of `rotate_signer_key` execution.

Execution Fee should be set at `$1` or (`20 KEX`)

### Fees Distribution

All fees paid in the native or non native currencies should be sent to the address controlled by the "rewards distribution module" (RDM). The RDM module does not have to be be implemented in the `Iteration 0`
