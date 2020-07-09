
# Iteration 0

The `Iteration 0` or Proof of Concept (PoC) is a first evolutionary step of the Kira Protocol. The main goal of this iteration is to create minimum viable blockchain-application based service that end user can interact with. The subject of the PoC is a Decentralized Exchange (DEX), a blockchain application containing minimum set of instructions necessary to enable 2 or more users to trustlessly exchange their tokens.

_NOTE: Kira Protocol is NOT a Decentralized Exchange, however DEX was chosen by us as the simplest DeFi system that can be used for the purpose of demonstrating protocol capabilities in the later iterations._

## Roadmap

The PoC consist of 3 milestones that must be completed with defined below capabilities. This iteration scope is also divided into **OPTIONAL** :new_moon: and **ESSENTIAL** :full_moon: tasks. Each task also contains a status, which is **PENDING** :x:, **ACTIVE** :pick: or **COMPLETED** :white_check_mark:. Each feature :zap: MUST have assigned [:bookmark:KIP] (Kira Improvement Proposal) tags to each of it's tasks and dedicated documentation page.

To learn more about branches and development process, see [versioning](../versioning.md) documentation.

_NOTE: All OPTIONAL :new_moon: features CAN become the scope of the future iterations and are NOT a priority for delivery._

1. :link: **Blockchain Application**
   * :zap: Order Books Management
     * :pick: :full_moon: [[:bookmark:KIP_1]](kip_1.md) Create Order Book
   * :zap: Orders Management
     * :pick: :full_moon: [[:bookmark:KIP_2]](kip_2.md) Place Order
     * :pick: :full_moon: [[:bookmark:KIP_3]](kip_3.md) Cancel Order
   * :zap: Key Management
     * :x: :new_moon: [[:bookmark:KIP_4]](kip_4.md) Signer Key Registration
  
2. :globe_with_meridians: **[REST Server / JSON RPC](../rpc/README.md)**
   * :zap: Queries 
      * :pick: :full_moon: [[:bookmark:KIP_5]](kip_5.md) List Order Books
      * :pick: :full_moon: [[:bookmark:KIP_6]](kip_6.md) List Orders
      * :x: :new_moon: [[:bookmark:KIP_7]](kip_7.md) List Signer Keys
   * :zap: Security
      * :x: :new_moon: [[:bookmark:KIP_8]](kip_8.md) Response Signing
      * :x: :new_moon: [[:bookmark:KIP_9]](kip_9.md) Endpoints Whitelist
  
3. :computer: **Web User Interface** (Static Page)
   * :zap: Account Management
      * :x: :full_moon: [:bookmark:KIP_10] Token Balances (Preview)
      * :x: :new_moon: [:bookmark:KIP_11] Token Transfers (Withdraw)
      * :x: :new_moon: [:bookmark:KIP_12] Trusted Signatures (Whitelist)
   * :x: Order Book Management
      * :x: :full_moon: [:bookmark:KIP_13] Select & Preview Order Books
      * :x: :full_moon: [:bookmark:KIP_14] Place & Cancel Orders
   * :zap: Orders Management
      * :x: :new_moon: [:bookmark:KIP_15] List & Cancel Orders 
   * :zap: Network Management
      * :x: :new_moon: [:bookmark:KIP_16] List Validator 
      * :x: :new_moon: [:bookmark:KIP_17] List Registered Signer Keys 
   * :zap: Security
      * :x: :new_moon: [:bookmark:KIP_18] Signer Key Verification

### Dependency Map

_This map defines order in which tasks must be executed to fulfill goals of the `Iteration 0`_

* **KIP 14** ⬅ **KIP 3** ⬅ **KIP 2** ⬅ **KIP 1**
* **KIP 13** ⬅ **KIP 5** ⬅ **KIP 1**
* **KIP 12** ⬅ **KIP 8** ⬅ **KIP 4**

## Objective

The main goal of `Iteration 0` is to create a minimum viable DEX 

## Foreword

Various technical definitions relevant to `Iteration 0` can be found in [glossary](../glossary.md). In case any part of the documentation is not clear, or difficult to understand, please create a relevant github issue.

We should always aim to maintain CLI up to date with latest changes and ensure that default output is JSON formatted as well as all exceptions result in appropriate exit code's != 0.

