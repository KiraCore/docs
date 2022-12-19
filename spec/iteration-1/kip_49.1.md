# [⏎](README.md#Roadmap) KIP_49
> Transaction Processing

In the standard Cosmos SDK transaction are processed is sequential order, each tx contains signed sequence number which is incremented one higher than the last request or the tx is rejected. This creates an issue where user might want to parallelize tx execution, but if any of the tx'es does not reach the full node due to the network failure then all following tx'es become automatically rejected. Another issue is, that it is impossible to sign transactions offline without knowledge of the latest tx sequence number (which requires internet access). [While the sequential execution of transactions prevents any possible inconsistency, it adversely impacts performance and scalability](https://arxiv.org/pdf/1902.01457.pdf).

We have to extend transaction processing logic to enable parallel execution. That is extend current ABCI or implement a custom ABCI interface and base app. We have to also ensure that transactions do not flood the tx pool forever by creating time constraints within which those transactions can be accepted.

_NOTE: The original sequential processing of transactions should be preserved in it's original form, all improvements in this proposal should be an extension so that we can compare final performance, security and spam resistance._

## Timestamp Sequence

It should be possible to define a time-based sequence with low chance of collision, for example a microseconds timestamp, if timestamp sequence is detected then transaction should NOT be processed in sequential order unless a reference sequence is added and implies that current tx should not be executed unless reference transaction is processed first. 

All timestamp-sequences should be stored on-chain to enable their lookup, if sequences were not unique and it could not be verified that they are then attacker could wipeout money the account by repeatedly submitting the same tx on-chain. 

By choosing timestamp as sequence we can enable each individual account to submit up to 1M TPS (in-parallel) while keeping the sequence size compact (`int64`). While sending transaction client has to simply check current date-time and to parallelize - iterate the timestamp value.

By defining reference timestamp as sequence we can guarantee sequential execution while enabling parallelization at the same time.

## Limits

We have to limit number of timestamp-sequences stored in the state by defining `max_sequences` parameter (in the Network Properties Registry). If `max_sequences` is reached than the last (oldest) timestamp-sequences should be removed before new one is added.

All sequences younger than `current_block_time` should be automatically rejected and removed from the tx pool.

To ensure we can garbage collect efficiently old timestamp-sequences, a `max_sequence_time` parameter should be defined (in the Network Properties Registry). If any of the sequences is older than `last_block_time - max_sequence_time` then it should be automatically removed from the state. If it happens that all tx were to be removed than the last sequence should be set to `last_block_time - max_sequence_time`.

## Throttling 

To ensure we can throttle timestamp-sequences, a `min_sequence_time` parameter should be defined (in the Network Properties Registry). If difference between two sequences is lower than `min_sequence_time` then such transactions should not be allowed in the block or rejected from the tx pool.

## Safety

To prevent repeat-attacks we have to guarantee that all timestamp-sequences accepted in the block are newer than the last timestamp-sequence saved in the state. If the state is garbage collected to remove ancient timestamp-sequences then this rule must be honored and at least a single timestamp-sequence must be present in the state.

Users should have ability to instantly expire hanging (or otherwise censored) transactions that they wish to cancel, by submitting `halt` tx user should have ability to cancel all tx in current block, wipe all sequences and set the last sequence to `last_block_time`.

_NOTE: As the result of this KIP governance of the network should have ability to effectively throttle network thruput even in case of adversary attacks_