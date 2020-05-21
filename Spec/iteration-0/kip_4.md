# KIP_4
> Register Signer Key

Every account should be able to register multiple public keys which can later be used to sign RPC responses or in general used for other various purposes that require a hot key - including creating session keys which can help to rate-limit queries and help prevent DOS attacks. 

To submit `rotate_signer_key` transaction following properties are required:

* `pubkey` - New public key, should not exceed `1024 bits` (`128 Bytes`)
* `type` - Key type enum (e.g. `RSA`, `ECC`, `AES`)
* `expires` - UTC time (4 Bytes) when key expires
* `valid` - boolean field defining if key is valid
* `owner` - user or curator public key

There has to be a custom fee paid on top of transaction fee in order to create new key (to prevent spam, the lifetime of the key should exponentially influence the cost of persisting it).

_NOTE: When new key is added all expired keys of the user should be removed from the state to save storage space, for this reason we need to create index that will help us quickly identify keys belonging to specific user._

Metadata index for the KV store must consist of the `4 Bytes` prefix of the blake hashed user public key, `4 Bytes` blake hashed `pubkey` and `4 Bytes` of unique, iterative id.

