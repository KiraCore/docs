
# Iteration 1

The `Iteration 1` is a second evolutionary step of the Kira Protocol. The main goal of this iteration is to deploy a minimum usable Public Testnet, where independent testers could control and manage the network. The subject of the testnet will be a minimum viable governance system and ensuring that early testers become familiar with operating validator nodes.

## Roadmap

The Public Testnet iteration consist of 3 milestones that must be completed with defined below capabilities. This iteration scope is also divided into **OPTIONAL** :new_moon: and **ESSENTIAL** :full_moon: tasks. Each task also contains a status, which is **PENDING** :x:, **ACTIVE** :pick: or **COMPLETED** :white_check_mark:. Each feature :zap: MUST have assigned [:bookmark:KIP] (Kira Improvement Proposal) tags to each of it's tasks and dedicated documentation page.

To learn more about branches and development process, see [versioning](../versioning.md) documentation.

_NOTE: All OPTIONAL :new_moon: features CAN become the scope of the future iterations and are NOT a priority for delivery._

1. :link: **Blockchain Application**
   * :zap: Governance
     * :x: :full_moon: [:bookmark:KIP_20] PoA Governance Set
     * :x: :full_moon: [:bookmark:KIP_21] Claim Governance Seat
     * :x: :full_moon: [:bookmark:KIP_22] Add/Remove Network Actor (Proposal)
     * :x: :new_moon: [:bookmark:KIP_23] Amend Code of Conduct (Proposal)
     * :x: :new_moon: [:bookmark:KIP_24] Add/Remove Token Alias (Proposal)
   * :zap: Consensus
     * :x: :full_moon: [:bookmark:KIP_25] PoA Validator Set
     * :x: :full_moon: [:bookmark:KIP_26] Claim Validator Seat  
2. :globe_with_meridians: **[REST Server / JSON RPC](../rpc/README.md)**
   * :zap: Queries 
      * :x: :full_moon: [:bookmark:KIP_27] List Validator/s
      * :x: :full_moon: [:bookmark:KIP_28] List Councilor/s
      * :x: :full_moon: [:bookmark:KIP_29] List Proposal/s
      * :x: :new_moon: [:bookmark:KIP_30] Show Code of Conduct
      * :x: :new_moon: [:bookmark:KIP_31] List Token Alias/es
   * :zap: Availability
      * :x: :new_moon: [:bookmark:KIP_32] Trusted Address Book
3. :computer: **Web User Interface**  
   * :zap: Network Management
      * :x: :full_moon: [:bookmark:KIP_33] Preview Validator Set
      * :x: :full_moon: [:bookmark:KIP_34] Preview Governance Set
      * :x: :full_moon: [:bookmark:KIP_35] Preview Proposals
   * :x: Asset Management
      * :x: :new_moon: [:bookmark:KIP_36] Alias Token Name
   * :zap: Network Management
      * :x: :new_moon: [:bookmark:KIP_37] Rotate Addresses 

### Dependency Map

_This map defines order in which tasks must be executed to fulfill goals of the `Iteration 1`_

* **KIP 25** ⬅ **KIP 24** ⬅ **KIP 21** ⬅ **KIP 20** ⬅ **KIP 19**
* **KIP 31** ⬅ **KIP 26**
* **KIP 32** ⬅ **KIP 27**
* **KIP 33** ⬅ **KIP 28**

## Objective

The main goal of `Iteration 1` is to deploy a minimum viable public testnet

## Foreword

Various technical definitions relevant to `Iteration 1` can be found in [glossary](../glossary.md). In case any part of the documentation is not clear, or difficult to understand, please create a relevant github issue.

We should always aim to maintain CLI up to date with latest changes and ensure that default output is JSON formatted as well as all exceptions result in appropriate exit code's != 0.

