
# [⏎](../README.md) Iteration 1

The `Iteration 1` is a second evolutionary step of the KIRA Network. The main goal of this iteration is to deploy a minimum usable Public Testnet, where independent testers could control and manage the network. The subject of the testnet will be a minimum viable governance system and ensuring that early testers become familiar with operating validator nodes.

## Roadmap

The Public Testnet iteration consist of 3 milestones that must be completed with defined below capabilities. This iteration scope is also divided into **OPTIONAL** :new_moon: and **ESSENTIAL** :full_moon: tasks. Each task also contains a status, which is **PENDING** :x:, **ACTIVE** :pick: or **COMPLETED** :white_check_mark:. Each feature :zap: MUST have assigned [:bookmark:KIP] (Kira Improvement Proposal) tags to each of it's tasks and dedicated documentation page.

To learn more about branches and development process, see [versioning](../versioning.md) documentation.

_NOTE: All OPTIONAL :new_moon: features CAN become the scope of the future iterations and are NOT a priority for delivery._

1. :link: **Blockchain Application**
   * :zap: Governance
     * :white_check_mark: :full_moon: [[:bookmark:KIP_20]](kip_20.md) Curated Governance Set
     * :pick: :full_moon: [[:bookmark:KIP_21]](kip_21.md) Claim Governance Seat
     * :x: :full_moon: [[:bookmark:KIP_22]](kip_22.md) Upsert Network Actor (Proposal)
     * :x: :full_moon: [[:bookmark:KIP_22.1]](kip_22.1.md) Upsert Roles (Proposal)
     * :x: :new_moon: [[:bookmark:KIP_23]](kip_23.md) Upsert Code of Conduct (Proposal)
     * :x: :new_moon: [[:bookmark:KIP_24]](kip_24.md) Upsert Token Alias (Proposal)
   * :zap: Consensus
     * :pick: :full_moon: [[:bookmark:KIP_25]](kip_25.md) Curated Validator Set
     * :pick: :full_moon: [[:bookmark:KIP_26]](kip_26.md) Claim Validator Seat
   * :zap: Security
     * :x: :full_moon: [[:bookmark:KIP_44]](kip_44.md) Transaction Fee
     * :x: :full_moon: [[:bookmark:KIP_45]](kip_45.md) Execution Fees
     * :x: :full_moon: [[:bookmark:KIP_46]](kip_46.md) Parameterized Permissions
     * :x: :new_moon: [[:bookmark:KIP_49]](kip_49.md) Transactions Challenge
     * :x: :full_moon: [[:bookmark:KIP_50]](kip_50.md) Multi-Token Fee Payment
2. :globe_with_meridians: **[REST Server / JSON RPC](../rpc/README.md)**
   * :zap: Queries 
      * :x: :full_moon: [[:bookmark:KIP_27]](kip_27.md) List Network Actors & Permissions
      * :x: :full_moon: [[:bookmark:KIP_28]](kip_28.md) List Proposal/s
      * :x: :new_moon: [:bookmark:KIP_29] Show Code of Conduct
      * :x: :new_moon: [[:bookmark:KIP_30]](kip_30.md) List Token Alias/es
      * :x: :new_moon: [[:bookmark:KIP_31]](kip_31.md) List Functions & Metadata
   * :zap: Availability
      * :x: :new_moon: [[:bookmark:KIP_32]](kip_32.md) Deposits & Withdraws Query
      * :x: :new_moon: [[:bookmark:KIP_47]](kip_47.md) INTERX Response Caching 
      * :x: :new_moon: [[:bookmark:KIP_48]](kip_48.md) INTERX Faucet
3. :computer: **User Interface / Deployment**  
   * :zap: Network Management
      * :x: :full_moon: [[:bookmark:KIP_33]](kip_33.md) Preview Validator Set
      * :x: :new_moon: [:bookmark:KIP_34] Claim Validator Seat
      * :x: :new_moon: [:bookmark:KIP_35] Rotate RPC URLs 
   * :zap: Governance
      * :x: :full_moon: [:bookmark:KIP_36] Preview Governance Set
      * :x: :full_moon: [:bookmark:KIP_37] Preview Proposals
      * :x: :new_moon: [:bookmark:KIP_38] Preview Code of Conduct
      * :x: :new_moon: [:bookmark:KIP_39] Claim Governance Seat
      * :x: :new_moon: [:bookmark:KIP_40] Accept/Reject Proposal
      * :x: :new_moon: [:bookmark:KIP_41] Create Proposal
   * :zap: Asset Management
      * :x: :new_moon: [:bookmark:KIP_43] Alias Token Name/s
   * :zap: Node Management
      * :x: :full_moon: [[:bookmark:KIP_51]](kip_51.md) Deployment and Management Scripts v1

### Dependency Map

_This map defines order in which tasks must be executed to fulfill goals of the `Iteration 1`_

**KIP 26** ⬅ **KIP 25** ⬅ **KIP 21** ⬅ **KIP 27** ⬅ **KIP 20**

**KIP 28** ⬅ **KIP 23**

## Objective

The main goal of `Iteration 1` is to deploy a minimum viable public testnet with a validator and governance set permissioining.

### Network Actor Permissioining 

![picture 2](https://i.imgur.com/28ONnVW.png)  

## Foreword

Various technical definitions relevant to `Iteration 1` can be found in [glossary](../glossary.md). In case any part of the documentation is not clear, or difficult to understand, please create a relevant github issue.

We should always aim to maintain CLI up to date with latest changes and ensure that default output is JSON formatted as well as all exceptions result in appropriate exit code's != 0.

