
# [⏎](../README.md) Iteration 2

The `Iteration 2` is a third evolutionary step of the KIRA Network. The main goal of this iteration is to deploy a usable Mainnet, where independent users could control and manage the network. The subject of the testnet will be economic sustainability and seamless upgrade experience. 

## Roadmap

The Mainnet iteration consist of 3 milestones that must be completed with defined below capabilities. This iteration scope is also divided into **OPTIONAL** :new_moon: and **ESSENTIAL** :full_moon: tasks. Each task also contains a status, which is **PENDING** :x:, **ACTIVE** :pick: or **COMPLETED** :white_check_mark:. Each feature :zap: MUST have assigned [:bookmark:KIP] (Kira Improvement Proposal) tags to each of it's tasks and dedicated documentation page.

To learn more about branches and development process, see [versioning](../versioning.md) documentation.

_NOTE: All OPTIONAL :new_moon: features CAN become the scope of the future iterations and are NOT a priority for delivery._

1. :link: **Blockchain Application**
   * :zap: Consensus
     * :x: :full_moon: [[:bookmark:KIP_52]](kip_52.md) Fee Rewards Distribution
     *  :white_check_mark: :full_moon: [[:bookmark:KIP_54]](kip_54.md) Uptime Counters, Pausing & Inactivating
     *  :x: :full_moon: [[:bookmark:KIP_55]](kip_55.md) Double-sign Jail & Governance Unjail
     *  :x: :full_moon: [[:bookmark:KIP_65]](kip_65.md) Upgrade Module & Fork Automation
     *  :white_check_mark: :full_moon: [[:bookmark:KIP_68]](kip_68.md) Uptime Counters v2
   * :zap: Governance
     * :x: :full_moon: [[:bookmark:KIP_53]](kip_53.md) Spending Pools
     * :x: :full_moon: [:bookmark:KIP_X] Upgrade Proposal & Revert
   * :zap: Security
     * :x: :full_moon: [[:bookmark:KIP_64]](kip_64.md) Global Transfers Un/Freeze
     * :x: :full_moon: [[:bookmark:KIP_59]](kip_59.md) Double-sign Prevention
     * :white_check_mark: :full_moon: [[:bookmark:KIP_58]](kip_58.md) Priv Validator Key Generator
     * :x: :full_moon: [[:bookmark:KIP_64]](kip_70.md) Identity Registrar 
2. :globe_with_meridians: **[REST Server / JSON RPC](../rpc/README.md)**
   * :zap: Queries 
      * :x: :full_moon: [[:bookmark:KIP_60]](kip_60.md) Query Validators
      * :x: :full_moon: [[:bookmark:KIP_66]](kip_66.md) Nodes Discovery
   * :zap: Availability
      * :x: :new_moon: [:bookmark:KIP_X] TBD
3. :computer: **User Interface / Deployment**  
   * :zap: Security
      * :x: :full_moon: [:bookmark:KIP_56](kip_56.md) SAIFU Login & Tx Signing
      * :x: :full_moon: [:bookmark:KIP_57](kip_57.md) INTERX Signatures Verification
   * :zap: Network Explorer
      * :x: :full_moon: [:bookmark:KIP_67](kip_67.md) Explorer Mode
      * * :x: :full_moon: [:bookmark:KIP_69](kip_69.md) Network Visualizer
   * :zap: Asset Management
      * :x: :new_moon: [[:bookmark:KIP_61]](kip_61.md) Query Transactions
      * :x: :new_moon: [[:bookmark:KIP_62]](kip_62.md) Query & Manage Proposals 
   * :zap: Node Management
      * :x: :new_moon: [[:bookmark:KIP_63]](kip_63.md) Sentry Mode

### Dependency Map

_This map defines order in which tasks must be executed to fulfill goals of the `Iteration 2`_

**KIP X** ⬅ **KIP Y** 

## Objective

The main goal of `Iteration 2` is to deploy a mainnet network that can be easily maintained.

## Foreword

Various technical definitions relevant to `Iteration 2` can be found in [glossary](../glossary.md). In case any part of the documentation is not clear, or difficult to understand, please create a relevant github issue.

We should always aim to maintain CLI up to date with latest changes and ensure that default output is JSON formatted as well as all exceptions result in appropriate exit code's != 0.

