# [⏎](README.md#Roadmap) KIP_54
> Double-Sign Jail & Unjail

It must be possible to prevent malicious actors and misconfigured validator nodes from double-signing (proposing two blocks at the same height). The misbehavior however can't instantly cause slashing when reported as the cause of the double-signing event might not be intentional and non threatening to the network operations.

If the single validator commits a double-signing fault his state should instantly change to `jailed` in which validator can't propose new blocks or re-activate his node by himself. 

The `jailed` state can then only be resolved though the governance process in which the jailed node creates a governance proposal with the request to be `unjailed`. The proposal should contain a reference URL to the document (similar to structure of the `Data Reference Registry`) where the reason for the fault has to be elaborated upon and judged by the governance body. If the proposal reaches quorum and is accepted the validator becomes `unjailed`. If the proposal is rejected the validator becomes permanently and irreversibly evicted. If the proposal does not reach the quorum or result of the vote is indecisive then node operator should have ability to resubmit the proposal again. 

The node should have only a limited time `T` to submit an `unjail` request. Time `T` should be further defined and configurable via the network properties registry. If request is not sent within predefined time `T` then the node operator should be permanently evicted (incapable to ever again join the validator set or sign blocks). 

Governance members voting on the `unjail` proposal should further have ability to define `unjailing` period when casting `yes` votes. The MEDIAN of time submitted via all votes should further determine how long the node operator will have to await until the node becomes unjailed. The time that can be specified should be within an hour range of `1` and `365`.

In case of successful governance unjailing. The `unjailed` status of the validator can only be changed to `inactive`, from where node operator has to submit `active` transaction and pay for it a corresponding execution fee.



