# [⏎](README.md#Roadmap) KIP_54
> Uptime Counters & Jailing

KIRA Network uses permissioned validator set to prevent sybil attacks, this also implies that there is no need to slash delegators for validator node downtimes or other not intended misbehavior's as validators misbehaving on purpose can be evicted manually though the governance process.

Not misbehaving on purpose does not automatically imply that validators might not be under attack, which can potentially cause halt of the consensus and we need ability judge which validators manage to protect themselves best.

To prevent consensus from halting we must create uptime counters which can be used to determine whether or not validator is still participating in the consensus. 

In the KIRA Network case we can't jail validators based on how many time they misses to participate in the block signing process (like in the case of the Cosmos Hub and their slashing module). Reason is that in case of KIRA all blocks must be produced as soon as `(2/3)+1` of all **active** validators sign the block. The consensus must progress immediately the second minimum number of validators is reached. This implies that not all validators will be participating in the block signing process, thus missing opportunity to sign a proposed block is expected and unavoidable.

## Implementation

Incremental counter `mischance` should be created (as part of each validator status data) marking number of times given validator missed it's turn to **PROPOSE** a block (or proposed a block that was not accepted) since the last time his node obtained `active` status. The `mischance` should be zeroed every time validator successfully proposed a block.

A corresponding `max_mischance` property in the Network Properties Registry should be created indicating maximum number of times a validator might miss a chance to propose a block in his round before becoming inactivated.

## Inactivating

If `mischance > max_mischance`, then validator status should be changed to `inactive`. Validator in the `inactive` state should have ability to call an `MsgActivate` to re-activate his node and join the validator set again with the status `active`. The `MsgActivate` should further automatically set the `mischance` to `0`. It is also important that `MsgActivate` can't be called if the validator status is set to anything else but `inactive`.

Validator must also have ability to pause his operations in a graceful manner by submitting `MsgPause` which would change his status to the `paused` - implying the the intended maintenance. In order to re-activate the node, the `MsgUnPause` should be submitted on-chain. The reason to create two transaction types `MsgActivate` and `MsgUnPause` is to provide ability for the governance to define different fees for calling those functions (e.g. `MsgActivate` might be more expensive to call than `MsgUnPause`). Furthermore `MsgUnPause` should **NOT** reset the `mischance` counter. 

## Halting Consensus

Governance must be able to define a minimum threshold of validators (in the Network Properties Registry) that can operate a network, meaning that if number of `active` validators were to fall below the `min_validators` then the network should be incapable of producing a new block which does not include sufficient number of `MsgActivate` and `MsgUnPause` transactions of validator that halted. This feature must be present to ensure that a malicious attack targeting individual validator nodes could not enable a small group of validators to take over the full control over the network. It must be also ensured that network does not permanently "halt" meaning that transaction propagation and recovery once all validator come online should be possible. 

In the case of consensus halting (due to insufficient number of validators with the status `active`) all transactions should be rejected by full nodes with exception for `MsgUnPause` and `MsgActivate` originating **ONLY** from accounts that are whitelisted as validators. 

## Validator Rank

To judge validator performance a `streak` and `rank` properties should be created (as part of each validator status data). The `streak` would imply consecutive number of times that given validator managed to successfully propose a block (since the last time he failed) that was accepted into the blockchain state. The `streak` property should be zeroed every time validator misses to propose a block and the `mischance` property is incremented. You can treat `streak` in similar way to kill-streaks in video games - which imply your short term performance.

The `rank` property is a long term statistics implying the "longest" `streak` that validator ever achieved, it can be expressed as `rank = MAX(rank, streak)`. Under certain circumstances we should however decrease the rank of the validator. If the `mischance` property is incremented, the rank should be decremented by `X` (default 10), that is `rank = MAX(rank - X, 0)`. Every time node status changes to `inactive` the rank should be divided by 2, that is `rank = FLOOR(rank / 2)`

The `streak` and `rank` will enable governance to judge real life performance of validators on the mainnet or testnet, and potentially propose eviction of the weakest and least reliable operators. 

## Full Node Operation & Block Production

The validator node configuration should be modified in such a way that block production should happen immediately, that is in the instance where `(2/3)+1` of `active` validators sign the the proposed block. 

Furthermore a `max_proposal_time` property should be defined within the Network Properties Registry - implying maximum time each validator has to propose a new block before the consensus progresses without them. If this property and configuration was not honored, than validators with a very fast hardware and connection could effectively force validators operating in the home or private DC environment to become inactivated.

Finally a `max_dilation_time` property should be included within the Network Properties Registry and enforce that validator nodes should not accept (sign) blocks where proposed block time is smaller or equal to previous block time or differs by more than `max_dilation_time` with their internal clocks. 

Above set of features will enable for maximally fast block production without limiting validator ability to operate from outside of the cloud environment.
