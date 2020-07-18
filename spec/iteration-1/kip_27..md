# [⏎](README.md#Roadmap) KIP_27
> List Network Actors

By using `query_network_actors` function it must be possible to list all network actors. Both CLI and RPC should be modified to facilitate following functionality:
* query by single or multiple roles
* query by single or multiple roles & status
* query by address

Every response of the `query_network_actors` command should be a JSON array of the NetworkActor type objects:

## Response Example:
```
[{
    "address": <string>
    "roles": [ <uint>, ... ],
    "status": <uint>,
    "votes": [ <uint>, ... ],
    "permissions: {
        "blacklist": [ <uint>, ... ],
        "whitelist": [ <uint>, ... ]
    },
    "skin": <uint64>
}, { ... }, ... ]
```