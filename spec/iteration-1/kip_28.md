# [⏎](README.md#Roadmap) KIP_27
> List Proposals

By using `query_proposals` function it must be possible to list all proposals. Both CLI and RPC should be modified to facilitate following functionality:
* query all proposal id's
* query single or multiple proposals by id
* query single or multiple proposals by status

Every response of the `query_proposals` command should be a JSON array of the object defining relevant properties. 

### Query All ID's Response Example:
```
[ <uint32>, ... ]
```

### Query Proposals Response Example:
```
[{
    "id": <uint32>,
    "fid": <uint16>,
    "status": <byte>,
    "votes": [ <byte>, ... ],
    "expiration": <uint32>,
    "enactment": <uint32>,
    "start": <uint32>,
    "data": <string>
}, { ... }, ... ]
```
#### fid
Function identifier which must be whitelisted in the network actors permissions. Network actor can't vote on the proposal without whitelisted `fid`. 

#### Votes
Defines vote types (`yes`, `no`, `abstain` ... )

#### Start
Defines unix time when proposal was created.

#### Data
Subject of the proposal