# [⏎](README.md#Roadmap) KIP_26
> Claim Validator Seat

By submitting `claim_validator_seat` transaction an account must be able to assume a `validator` role.

Before account owner can submit transaction a relevant permission in the Network Actors registry must be set either in genesis file or though passing relevant proposal by the network governance body. The identifier of the `claim_validator_seat` function should be `4` (0x0004).

_OPTIONALLY: It should be possible to define execution cost in genesis  - [Execution Fee](/spec/fees.md) to submit `claim_validator_seat` proposal._

While submitting a validator seat claim transaction, the account owner must supply following information, that must be stored in the Validator Identity Registry:
* `moniker` - string - 64 characters max
* `website` - string - 64 characters max
* `social` - string - 64 characters max
* `identity` - string - 64 characters max
* `commission` - decimal
* `valkey` - string (kiravaloper)
* `pubkey` - string (kiravalconspub)

As result of the claim transaction the account owner must be able to immediately join validator set.

_NOTE: Account owner should NOT be able to update the validator identity registry by submitting a second claim transaction_

It must be possible to use `query_validators_registry` function to acquire detailed information regarding validator identity. Both CLI and RPC should be modified to facilitate following functionality:
* query by moniker
* query by valkey
* query by owner account

## Example Response
Result of the query should be a JSON object containing following information:
```
{
    "address": <string>,
    "moniker": <string>,
    "website": <string>,
    "social": <string>,
    "identity": <string>
    "commission": <decimal>
    "valkey": <string>
    "pubkey": <string>
}
```



