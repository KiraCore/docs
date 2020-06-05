# KIP_7
> List Signer Keys

By using `query_signer_keys` it must be possible to list all public keys belonging to specific user as well as quickly a specific key to for example find out if belongs to a specific user.

We must be able to query by user public key and optionally limit results by pubkey itself as a single user can have up to `65536` keys. 

Every response of the `query_signer_keys` command should be a JSON array containing all info regarding signer keys

## Example:
```
[
    {
        "pubkey":"sxjgyWuNONTP5Q4gnS+a2PYxEcCjQdEbXjWZ9I0NN2U=",
        "type": "ed25519",
        "expires": 1591387535,
        "enabled": True,
        "owner": "kira00000000000000000000000000000000",
        "permissions": [0,0,0,1,7,7,7],
        "memo": "This is my hot account for trading small amount of coins on the zone nr 1"
    }, { ... }, ...
]
```