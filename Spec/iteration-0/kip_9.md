# KIP_9
> Endpoint Whitelist

Every full node exposing RPC must be able to define individual endpoints that can be accessed in the rate-limited manner by authenticated or unauthenticated clients.

For every endpoint there must exist a record that defines:
* If endpoint is enabled (`Boolean`)
* Rate limit for unauthenticated clients (`Float` - requests per second) [OPTIONAL]
* Rate limit for authenticated clients (`Float` - requests per second) [OPTIONAL]

If given endpoint is not enabled then it should not be possible for a client to access it and `403 Forbidden` response code should be returned to the client.

It must be possible for a client to discover which endpoints are accessible to him using `rpc_methods` endpoint.

## Example Response
```
[
    "node_info": { 
        "enabled": True,
        "rate_limit": 0.1,
        "auth_rate_limit": 1,
        ...
    }
    "txs": { ... },
    ...
]
```

_NOTE: In the PoC the rate limits do not have to be enforced nor present in the rpc_methods response_