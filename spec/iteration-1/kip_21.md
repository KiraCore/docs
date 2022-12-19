# [⏎](README.md#Roadmap) KIP_21
> Claim Governance Seat

By submitting `claim_governance_seat` transaction an account must be able to assume a `councilor` role.

Before account owner can submit transaction a relevant permission in the Network Actors registry must be set either in genesis file or though passing relevant proposal by the network governance body. The identifier of the `claim_governance_seat` function should be `1` (0x0001).

_OPTIONALLY: It should be possible to define execution cost in genesis  - [Execution Fee](/spec/fees.md) to submit `claim_governance_seat` proposal._

While submitting a governance seat claim transaction, the account owner must supply following information, that must be stored in the Council Identity Registry:
* moniker - string - 64 characters max
* website - string - 64 characters max
* social - string - 64 characters max
* identity - string - 64 characters max

_NOTE: Account owner should NOT be able to update the council identity registry by submitting a second claim transaction_

It must be possible to use `query_council_registry` function to acquire detailed information regarding councilor identity. Both CLI and RPC should be modified to facilitate following functionality:
* query by moniker
* query by address

## Example Response
Result of the query should be a JSON object containing following information:
```
{
    "address": <string>,
    "moniker": <string>,
    "website": <string>,
    "social": <string>,
    "identity": <string>
}
```



