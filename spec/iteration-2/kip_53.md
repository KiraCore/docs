# [⏎](README.md#Roadmap) KIP_53
> Spending Pools

It is essential to incentivize various network actors for their work. This should be achieved though the concept of "spending" pools associated with roles that can be assigned to individual accounts, those accounts can then be treated as `owners` and `beneficiaries` of those spending pools. Finally This module should be token generic, meaning that it must be possible to deposit and spend any fungible asset.

Both governance system and individuals should be bale to create spending pools. The spending pools will also be used internally by the state machine to distribute fee rewards and other types of incentives. Spending pools might be associated with any network actors such as validators, governance members or individual accounts.

## Implementation

Create a Spending Pool Registry with associated identifiers mapping to a list of properties specifying how the pool operates:

```
<pool-name-1>: {
    enabled: <bool>,
    owners: [ <role-name>, <permission>, <account>, ... ], 
    beneficiaries: [ <pool-name-x>, <role-name>, <permission>, <account>, ... ],
    parameters: [ { "<key>":<object> }, { ... } ... ],
    mode: <operation-mode>,
},
<pool-name-2>: { ... }, ...
```

* `enabled` - defines whether the pool is enabled (true) and funds within can be used or (false) frozen
* `owners` - defines which actors possessing defined roles, permissions or account addresses can control the pool (upsert). In certain cases the owner can be a state machine or a particular function meaning that network actors can't modify the spending pool.
* `beneficiaries` - defines set of accounts, roles, permissions, accounts or even other pools to which funds can be distributed
* `parameters` - list of objects acting as input parameters to the function defined by the `mode` field
* `mode` - defines the function according to which funds can be distributed (distribution mechanism). 

There should be no restrictions in terms of who can deposit assets to the pool (must be possible from any account or other pool) while the way how the assets are spent is defined by the pool `owners` and in accordance to the `mode` of operation. Difference from normal accounts would be that accounts who are not "owners" would have no ability to execute any operations on the account. Spending pools would operate in similar fashion to multisig accounts, but utilize proposals to spend or define how assets are utilized.

## Operation Modes

Initially we will support following three modes of operation. 

### Manual

In the `manual` mode of operations the `owners` of the spending pool must create a spending proposal and pass it. If beneficiaries are defined they can be the only ones to receive assets. If beneficiaries list is not defined then any account will be able to receive funds. The proposal should be of a multi-spend type, meaning that many accounts can be defined as receiving parties. The `parameters` are optional and if not defined the funds can be distributed without restrictions or limits.

_NOTE: It must be further possible to define not only addresses as receiving parties but also other spending pools._

### Passthrough 

The `passthrough` mode is used to instantly distribute the funds from the spending pool to the `beneficiaries`. There is no manual distribution though proposals, all must happen automatically. All funds deposited are spent as per limits further defined by the `parameters`.

## Parameters

The `parameters` list contains key - object pairs describing behavior and restrictions of each operation mode. 

### Delay

`delay` - an integer defining how often (in seconds) distribution of funds can occur. Every time funds are spent a timestamp should be preserved indicating when the next distribution of funds can take place (current time + delay). 

### Max Share

`max-share` - maximum share (%) of all funds that can be spent during each distribution round. If not defined the maximum spend amount should be `0%`.

### Max Share Beneficiaries

`max-share-ben` - maximum share (%) of `max-share` that can be spent during each distribution round to specific `beneficiaries`. If a group of addresses is defined for example as `role` then assets will be split equally between them.

```
"max-share-ben": [{
    "bens": [ <string>, ... ],
    "share": <decimal>,
    "max-fix": [ { 
        "amount": <integer>,
        "denom": <string>,
        "bens": [ <string>, ... ] },
        { ... }, ... ],
    "stack": <bool>
}, { ... }, ... ]
```
The sum of `share` must be less or equal to `1` (100%), if the sum is below `1` then remaining funds (of the `max-share`) are distributed to all remaining `beneficiaries`.  The `stack` boolean should define whether or not accounts defined multiple times (e.g. multiple overlapping roles) can receive multiple the rewards or only a single (being the `max` / single highest reward of all defined `max-share-ben`s).

The `max-fix` property is used to define maximum amounts of each individual tokens that can be received by each beneficiary (if specific `bens` are not defined then `max-fix` applies to all `max-share-ben`s). If `max-fix` property is not defined then tokens should be distributed equally among all `max-share-ben`s in the amount of `(max-share*share*nr_of_tokens)/n` where `n` is number of accounts where the funds supposed to be distributed. If `max-fix` property is defined then the same rule applies but the number of tokens distributed to each account can't exceed `amount` as defined in the `max-fix` and only tokens defined as `denom` can be credited.

## Interacting With the Module

Anyone must be able to create the Spending Pool, query Spending Pools, query active proposals and send a deposit transaction. The `owners` should have ability to modify the Spending Pool (upsert) through proposal process and approve the voting or spending proposals.

Spending proposals int the `manual` mode should have additional `memo` field which can be used to describe the purpose or reference a document describing use of funds.