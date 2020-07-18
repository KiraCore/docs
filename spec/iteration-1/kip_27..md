# [⏎](README.md#Roadmap) KIP_27
> List Network Actors & Permissions

## List Network Actors

By using `query_network_actors` function it must be possible to list all network actors. Both CLI and RPC should be modified to facilitate following functionality:
* query by single or multiple roles
* query by single or multiple roles & status
* query by address

Every response of the `query_network_actors` command should be a JSON array of the NetworkActor type objects:

### Response Example:
```
[{
    "address": <string>
    "roles": [ <uint>, ... ],
    "status": <uint>,
    "votes": [ <byte>, ... ],
    "permissions: {
        "blacklist": [ <uint16>, ... ],
        "whitelist": [ <uint16>, ... ]
    },
    "skin": <uint64>
}, { ... }, ... ]
```

## List Permissions

By using `query_permissions` function it must be possible to list all network actors permissions. Both CLI and RPC should be modified to facilitate following functionality:
* query list of all permissions

Every response of the `query_permissions` command should be a JSON array of the NetworkActor type objects:

### Response Example:
```
{
    "councilor" : {
        "whitelist": [ <uint16>, ... ],
        "blacklist": [ <uint16>, ... ] 
    },
    "validator" : {
        "whitelist": [ <uint16>, ... ],
        "blacklist": [ <uint16>, ... ] 
    },
    "govleader" : {
        "whitelist": [ <uint16>, ... ],
        "blacklist": [ <uint16>, ... ] 
    }
}
```