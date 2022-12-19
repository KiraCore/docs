# [⏎](README.md#Roadmap) KIP_22
> Upsert Network Actor (Proposal)

## Upsert Network Actor

By submitting `upsert_network_actor` a proposal should be created that will enable governance set to update **Network Actors Registry** (as defined by KIP_20). The upsert network actor function should have am identifier `2` (0x0002). 

_OPTIONALLY: It should be possible to define execution cost in genesis  - [Execution Fee](/spec/fees.md) to submit `upsert_network_actor` proposal._

Proposal should have following properties:
* Function identifier `3` (0x0003)
* Configurable in genesis `expiration` time (uint32) - seconds since submission
* Configurable in genesis `enactment` time (uint32) - seconds since expiration
* Allowed vote types: `yes`, `no`, `veto`, `abstain`
* Unique iterative identifier (1,2,3,4...)
* Status with following types:
  * `undefined` - 0x00
  * `active` - 0x01
  * `rejected` - 0x02
  * `passed` - 0x03
  * `enacted` - 0x04

_NOTE: Network actors with assumed roles `councilor` should by default posses ability to propose `0x0002` and vote on `0x0003`. Network actors with assumed roles `validator` should by default posses ability to vote on `0x0003`. Those types of permissions should be predefined in genesis_

Proposal should contain a configuration json object of the **NetworkActor** type (as defined by KIP_20) defining how the role should be changed.

Once network actor state is set to `removed` it should no longer be possible to submit `upsert_network_actor` function.

## Proposal Vote

By submitting `vote` transaction along proposal identifier it should be possible for the governance members with the whitelisted permission `0x0003` to submit relevant vote types to pass or reject action of upserting Network Actor Registry. 




