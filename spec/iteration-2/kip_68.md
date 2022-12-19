# [⏎](README.md#Roadmap) KIP_68
> Uptime Counters v2

To prevent consensus from halting we must create reliable uptime counters which can be used to determine whether or not validator is still participating in the consensus. 

In the KIRA Network case we can't jail validators based on how many time they misses to participate in the block signing process (like in the case of the Cosmos Hub and their slashing module). Reason is that in case of KIRA all blocks must be produced as soon as `(2/3)+1` of all **active** validators sign the block. The consensus must progress immediately the second minimum number of validators is reached. This implies that not all validators will be participating in the block signing process, thus missing opportunity to sign a proposed block is expected and unavoidable.

Chances that a random validator is not going to make it and sing proposed block in time should average at around 1/3. This means that if validator consecutively misses 9 blocks in the row the chances that he was not participating in the consensus in that time period should have 5 sigma confidence level.



## Implementation

Incremental counter `mischance` should be created (as part of each validator status data) marking number of times that the validator was not taking part in the consensus since the last time his node obtained `active` status. The `mischance` should be zeroed every time validator successfully proposed or signed a block.

To determine that `mischance` occurred and increment it by `1` we have to store **number of the last signed block** as `last_present_block`. Every time validator signs a block `last_present_block` should be set to `latest_block_height`.

We have to also implement a `mischance_confidence` property which will imply number of blocks that have to pass since `last_present_block` for us to be confident that the `mischance` occurred. 

In other words only if `latest_block_height - last_present_block > mischance_confidence` only then `mischance` can be incremented.

By default `mischance_confidence` should be set to `10` but it also must be possible to modify it using governance module.

A corresponding `max_mischance` property in the Network Properties Registry should be created indicating maximum number of times a validator might miss a chance to propose a block in his round before becoming inactivated.

By default `max_mischance` should be set to `110` but it also must be possible to modify it using governance module.

## Inactivating

If `mischance > max_mischance`, then validator status should be changed to `inactive`. Validator in the `inactive` state should have ability to call an `MsgActivate` to re-activate his node and join the validator set again with the status `active`. The `MsgActivate` should further automatically set the `mischance` to `0` and `last_present_block` to `latest_block_height`. It is also important that `MsgActivate` can't be called if the validator status is set to anything else but `inactive`.

Validator must also have ability to pause his operations in a graceful manner by submitting `MsgPause` which would change his status to the `paused` - implying the the intended maintenance. In order to re-activate the node, the `MsgUnPause` should be submitted on-chain. The reason to create two transaction types `MsgActivate` and `MsgUnPause` is to provide ability for the governance to define different fees for calling those functions (e.g. `MsgActivate` might be more expensive to call than `MsgUnPause`). Furthermore `MsgUnPause` should **NOT** reset the `mischance` counter and should **NOT** change the `last_present_block`. 

## Validator Rank

To judge validator performance a `streak` and `rank` properties should be present as part of each validator status data. The `streak` would imply consecutive number of times that given validator managed to successfully propose a block (since the last time he failed) that was accepted into the blockchain state. The `streak` property should be set to `0` every time validator misses to propose, that is when the `mischance` property is incremented. You can treat `streak` in similar way to kill-streaks in video games - which imply your short term performance.

The `rank` property is a long term statistics implying the "longest" `streak` that validator ever achieved, it can be expressed as `rank = MAX(rank, streak)`. Under certain circumstances we should however decrease the rank of the validator. If the `mischance` property is incremented, the rank should be decremented by `1`, that is `rank = MAX(rank - 1, 0)`. Every time node status changes to `inactive` the rank should be divided by 2, that is `rank = FLOOR(rank / 2)`

The `streak` and `rank` will enable governance to judge real life performance of validators on the mainnet or testnet, and potentially propose eviction of the weakest and least reliable operators. 
