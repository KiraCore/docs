# [⏎](README.md#Roadmap) KIP_60
> Query Validators

Create `/valopers` endpoint incorporating detailed information in regards to all network actors recorded in the validators registry as per KIP_26.

It must be possible to query the registry by address, valkey, pubkey, moniker & status. User should have option to paginate results which is an array of objects. 

## Example Response
```
[ {
     "address": "kiraXXXXXXXXXXXXXXXX",
      "valkey": "kiravaloperXXXXXXXXXXXXXXXX"
      "pubkey": "kiravalconspubXXXXXXXXXXXXXXXXXXXXX",
     "moniker": <string>,
     "website": <string>,
      "social": <string>,
    "identity": <string>,
  "commission": <decimal>,
      "status": <string>,
        "rank": <Int64>,
      "streak": <Int64>,
   "mischance": <Int64>
  }, { ... }, ...
]
```
