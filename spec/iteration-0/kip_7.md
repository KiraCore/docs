# [⏎](README.md#Roadmap) KIP_7
> List Signer Keys

By using `query_signer_keys` it must be possible to list all public keys belonging to specific user as well as a specific key to for example find out if it belongs to a specific user.

In other words we must be able to query signer keys by user public key and optionally limit results by pubkey itself as a single user can have up to `65536` keys.

To prevent queries that can overflow node memory the results must be [paginated](../rpc/README.md#Pagination) and limited to no more than `100` results at a time.

Every response of the `query_signer_keys` command should be a JSON array containing all info regarding signer keys.

## Response Example:
```
[
    {
        "pubkey": "sxjgyWuNONTP5Q4gnS+a2PYxEcCjQdEbXjWZ9I0NN2U=",
        "type": "ed25519",
        "expires": 1591387535,
        "enabled": True,
        "permissions": [123,456,5678,...],
    }, { ... }, ...
]
```