# [⏎](README.md#Roadmap) KIP_22
> Add/Remove Network Actor (Proposal)

By submitting `upsert_network_actor` a proposal with unique, iterative identifier (1,2,3,4...) should be created that will enable governance set to update **Network Actors Registry** (as defined by KIP_20). The upsert network actor function should have am identifier `2` (0x02). It should be possible to define execution cost in genesis  - [Execution Fee](/spec/fees.md) to submit `upsert_network_actor` proposal.

Proposal should have following properties:
* Function identifier `3` (0x03)
* Configurable in genesis `expiration` time (uint32) - seconds since submission
* Configurable in genesis `enactment` time (uint32) - seconds since expiration
* Allowed vote types: `yes`, `no`, `veto`, `abstain`

Network actors with assumed roles `councilor` should by default posses ability to propose `0x02` and vote on `0x03`. Network actors with assumed roles `validator` should by default posses ability to vote on `0x03`.

A **Permissions Registry** should be created (ideally configurable from genesis) which would contain information - which functions can by default be executed by network roles.

## Example Structure





