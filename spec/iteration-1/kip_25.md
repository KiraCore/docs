# [⏎](README.md#Roadmap) KIP_25
> PoA Validator Set

Kira Network requires a governance curated validator set that can operate the network and allow the consensus to progress.

## Consensus Rules

The validator set of kira consist of `network actors` with assigned and claimed role `validators`, where each validator possesses an **equal voting power**.

Kira consensus must progress according to following rules:
* Each validator must have equal voting power
* Each validator should have equal chance to produce a block
* Only validators with `active` status in the Network Actors registry should be able to propose new blocks
* Validators who fail their chance (once elected) to propose a block more then `N` times (configurable in genesis) should have their status set to `inactive` in the Network Actors registry
* Block should be produced as soon as more then 2/3 of all validators vouch for the block
* Blocks should contain UNIX Nano UTC timestamps
* Two blocks should not have timestamps separated by less then 500 milliseconds (no full node or validator should accept or propagate such block)
* Blocks should be produced even if they are empty
* It should not be possible to produce two non-consecutive blocks (no block skipping)

## Inactive Operators

In case where validator was marked as `inactive` he must have ability to submit `activate` transaction (function identifier `0x0005`) that would allow him to change his status in the Network Actors registry to `active`. It must be possible to define execution cost of this transaction in genesis  - [Execution Fee](/spec/fees.md) to submit `upsert_network_actor` proposal.

## Pausing Operation

Validator must have ability to submit `pause` (function identifier `0x0006`) and `unpause` (function identifier `0x0007`) transactions to explicitly stop his operations or start. It must be possible to define execution cost of executing `unpause` transaction in genesis  - [Execution Fee](/spec/fees.md). There should be no Execution Fee for submitting `pause` transaction.

## Time Oracle

Validators when proposing and agreeing on the new block production should act as time oracle, this implies that every block should have associated UTC UNIX Nano timestamp proposed by individual validators that can be used for various time driven functionality in the code.

It should not be possible to propagate blocks which timestamp varies from the system time by configurable value by each full node. (Default 1 minute)

_NOTE: tlsdate or similar NTP alternative is required to run on the host machines_

_NOTE: Kira must NOT use blocktime concept in its logic known from the original modules of the cosmos sdk_


