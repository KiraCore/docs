# [⏎](README.md#Roadmap) KIP_20
> Curated Governance Set

Kira Network requires a governance body that can constitute the Code of Conduct and enable network to self evolve by creating and passing proposals, resulting in on-chain and off-chain changes to the network or rules which community can use to self organize.

## High Level Overview

Kira governance operates through two fundamental concepts

1. Governance actors must be whitelisted by existing governance actors
2. Governance actors can only vote on proposals that they have permissions to vote on 

Those two initial rules allow to create generic and self-evolving [multicameral](https://en.wikipedia.org/wiki/Multicameralism) governance systems where certain sub-groups are responsible and specialize in certain types of proposals. One of good examples of good governace structure and power separation is [checks and balances](https://en.wikipedia.org/wiki/Separation_of_powers#:~:text=The%20principle%20of%20checks%20and,supreme%2C%20thereby%20securing%20political%20liberty.) system

## Voting Rules

The governing body of kira consist of `network actors` called `councilors`, where each `governance actor` possesses an **equal voting power**.

Kira proposals, which can be created by governance actors, can be passed, rejected or vetoed according to following rules:
* Only network actors with `councilor` type can vote on proposals (Governance actors).
* Governance actor can only vote on the specific proposal if he/she has assigned  `permissions` that allow this specific actor to vote on that specific proposal
* Governance actor can only vote on proposal if he/she is `active`
* Governance actor can only vote on proposal using `4` individually assigned vote types: `yes`, `abstain`, `no` or `veto`
* Proposal is **passed** only if MORE then `1/2` (>50%) of all votes were `yes` votes and quorum of MORE then `1/3` (>33%) of all actors that have active status and permissions to vote on a specific proposal was reached
* Proposal is **rejected** if votes of type different then `yes` sum to MORE or EQUAL to `1/2` (>=50%) of all votes
* Proposal is **rejected** if MORE or EQUAL to `1/2` (>=50%) of actors who posses veto privileges vote `veto` (minority with veto power can reject a proposal)
* Proposal can't pass, be rejected or vetoed before its `expiration` time passes. Proposal can't be enacted (take effect) sooner then after the `enactment` time passes. 
* During `enactment` time no votes can be casted.

## Network Actors

**NetworkActor** class describes permissions, vote types and roles that can be assumed or executed by the account. Information regarding network actors must be stored within on-chain registry. 

_NOTE: Permissions blacklist should at all times have priority over the whitelist_

```
{
    "address": <string>
    "roles": [ <uint>, ... ],
    "status": <byte>,
    "votes": [ <byte>, ... ],
    "permissions: {
        "blacklist": [ <uint16>, ... ],
        "whitelist": [ <uint16>, ... ]
    },
    "skin": <uint64>
}
```

### Address

Account address of the network actor, e.g: `kira1XXXXXXXXXXXXXXXXXXXXXXXXXXXX`

### Roles

A list of unsigned integer describing types of the network actors that the account can assume. Each individual address can claim one or more role types: 
* `undefined` - 0x00
* `councilor` - 0x01
* `validator` - 0x02
* `govleader` - 0x03

_NOTE: Each individual role must be claimed by the account owner._

### Status

An unsigned integer describing state of the network actor role. **Active** status implies that actor can perform his duties. **Paused** status implies that actor suspended his own duties and does not want to be accounted for during referenda. **Jailed** state implies that actor duties were suspended by the network or governance. **Removed** status implies that duties of the actor were permanently forfeit.

* `undefined` - 0x00
* `unclaimed` - 0x01
* `active` - 0x02
* `paused` - 0x03
* `inactive` - 0x04
* `jailed` - 0x05
* `removed` - 0x06

_NOTE: Network actor can have only a single status type assigned_

### Votes

A list of unsigned integers describing vote types that network actor can cast on proposals while having an active councilor role.

* `undefined` - 0x01
* `abstain` - 0x02
* `yes` - 0x03
* `no` - 0x04
* `veto` -  0x05

_NOTE: Casting vote multiple times overrides previous vote_

_NOTE: Individual proposals might define individual sub-set of vote types that can be casted_

### Permissions

Describes blacklist and whitelist of function identifiers that can be triggered by the network actor. For example "Add Network Actor" proposal might have identifier `0x01` and every network actor with that identifier in the permission whitelist would be able to create that proposal. Analogically no network actor with identifier `0x01` in their blacklist would be able to create "Add Network Actor" proposal.

Distinction between whitelist and blacklist is necessary because certain roles that network actor can assume might have default functions that can be triggered by the network actor. For example `validator` might by default be able to execute "Claim Validator Seat" function and `councilor` might be able to execute "Claim Governance Seat" function.

A **Permissions Registry** (ideally configurable in the genesis json) must be created and define which functions can by default be executed by which network `roles`. This is essential because without that functionality we would have to update hundreds of records in Network Actors registry every time we want to allow every role owner to have additional permissions.

_NOTE: Permissions Registry should always have priority over individual permissions of the network actor, that means that if the account owner assumes role then blacklist property associated with the role will override his whitelist if some of the functions overlap_ 

#### Example Structure
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

### Skin

[Skin in the game](https://en.wikipedia.org/wiki/Skin_in_the_game_(phrase)) is a property defining minimum liquid (available) balance of KEX tokens, that network actor must have. If the minimum available KEX balance of the `active` network actor is lower then defined by `skin` property then his status should be automatically set to `paused`.

This feature enables network actors to quickly disable/pause their accounts in case they are or suspect to be compromised (by transferring their tokens out of their account). Furthermore attacker would have to deposit his own tokens and risk them being transferred away by the account owner. Finally skin property ensures that account owners must maintain sufficient security level of their accounts.

## Genesis

It must be possible to predefine one or more **Network Actors** such as validators or councilors in the genesis file to ensure that network can be launched. Those predefined actors should be then able to submit genesis transactions such as Claim Validator Seat (KIP_26) or Claim Governance Seat (KIP_21).

_NOTE: Roles are not assigned in the genesis, only permissions, which are later used to claim the role_

