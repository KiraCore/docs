# [⏎](README.md#Roadmap) KIP_65
> Upgrade Module & Fork Automation

In order to offer seamless upgrade experience we have to create an upgrade module enabling export of the blockchain state at given block height into new forks though genesis file. We must further crate an automation routine in the [kira manager tool](https://github.com/kiracore/kira) enabling full automation of the upgrade process.

#### Logic Flow
* Upgrade Proposal Submission
* Voting Process
* Graceful Reboot of Nodes
* Creation of New Docker Images
* Network Halt
* State Export
* Containers Launch From New Docker Images

## Upgrade Proposal

Create new governance proposal according to the rules of the [kira gov permissioining model](../iteration-1/kip_20.md#Permissions) that is create new permission to create proposal, new permission to vote on the proposal as well as new proposal type. Proposal should further include following properties:

* List of repository names along their identifiers, checksums, URL's and associated branches (checkout tags or hashes) which will be used to upgrade the chain
* Unix timestamp after which new blocks will not be produced
* Chain id of current network (network that will be upgraded - this is required as proposals might be relevant to parthians in the future)
* New Chain id (can be the same as old chain id)
* Rollback genesis checksum
* Max enrolment time defining max time when new blocks must be produced before the rollback will be triggered.
* memo (text content of the proposal, max 1024 characters)


### Example Proposal Structure

```
{
    "resources": [ {
            "id": "infra",
            "git": "<url-string>",
            "checkout": "<branch-or-tag-string>",
            "checksum": "sha256-string"
        }, {
            "id": "chain",
            "git": ...
        }, { ... }, ...
    ],
    "min_halt_time": <uint>,
    "old_chain_id": <string>,
    "new_chain_id": <string>,
    "rollback_checksum": <sha256-string>,
    "max_enrolment_duration": <uint>,
    "memo": <string>
}
```

It has to be ensured that proposal `min_halt_time` can't occur earlier then time of the proposal enactment. Furthermore if the `old_chain_id` is the same as kira chain id where proposal is being executed then the network should halt on the block where `min_halt_time` block time has been reached, so that on the old chain the last block time must equal or greater then `min_halt_time`. Effectively blockchain will be stopped with maximum time delta error of 1 block.

### Rollbacks

To execute a rollback old genesis file should be modified to include `rollback_times` array invalidating one or many chain halts. The `rollback_checksum` should be a checksum of the genesis file with `rollback_times` array includes `min_halt_time` of the last upgrade proposal. This should enable validators to produce new blocks after network already halted. So that we are not stuck on the network where upgrade failed.

Rollback should be automatically triggered if validators do not manage to produce new block before `max_enrolment_time` passes. During `max_enrolment_time`period it should not be possible to transfer tokens or execute other on-chain actions as defied by KIP_54 (in similar fashion as in the case where insufficient number of validators is present - poor network conditions). If `max_enrolment_time` passed the new fork should halt and be considered invalid while validators rollback to the old chain.

## State Export Tool

Create a pattern for each module enabling state export and transition into new genesis from the previous version. Appropriate upgrade tests should be further added to ensure every new release enables smooth testnet transition into the new version. State export tool should always enable export of latest deployed version on the mainnet to the latest release, no intermediary upgrades are required.

CLI should be extended with `sekaid update-genesis` and `sekaid rollback-genesis` commands.

### Upgrades Logic

```
MAINNET -> v1 ---------> v4 (PASS) ---------> v6 (FAIL) ---------> v4
TESTNET -> v1 ---------> v4 (PASS) ---------> v6 (PASS) ---------> v4
           |             |
           -> v2 (FAIL)  -> v5 (FAIL)
           |             |
           -> v3 (FAIL)  -> v6 (PASS)
           |
           -> v4 (PASS)
```


## Graceful reboot of all nodes & infra upgrade precess

Infrastructure deployment manager should be modified so that after proposal is passed an automated (if enabled) reinitalization should be performed (infrastructure should be re-installed) and new "upgrade" images created. During upgrade process old containers should be stopped and new ones started with newly exported genesis (checksum of the new genesis should be verified).

When performing first automated reinitlaization it should be ensured that nodes to not go offline at the same time. A pause tx should be submitted by validator at a randomized time interval and other nodes should not initialize automated upgrade if that would lead to network entering "low validator count conditions" as per KIP_54 where certain transaction types are not allowed.

After reinitlaization validator node should automatically unpause itself. Furthermore a new feature to enter maintenance mode should be added to the infra "Validator Mode" deployment which enables pausing/unpausing manually.

Validator should have option to manually trigger rollback, in which case infrastructure would be started using old docker images and rolled-back genesis file would be supplied to them (checksum of the old-new genesis should be verified). If network fails to produce new blocks infra should automatically perform a rollback.
