# [⏎](README.md#Roadmap) LIP_4
>  Interchain Registrar

Goal of this proposal is to implement a multi-directional peg-contract enabling token and data transfers between uncapped number of decentralized networks.

Pegging is a mechanism of `locking` or otherwise temporarily or permanently immobilizing fungible or non fungible tokens on the chain `A` in order to issue them on the chain `B` or vice versa. `Locking` of the asset is usually achieved though a smart-contract or a dedicated state-machine module deployed on the chain `A`. Validators of the chain `B` **collectively** have the ability to observe the state and fully control the locking contract/module. Validators of the chain `B` have further ability to issue `1:1` assets representing locked tokens to the address designated by the user making a deposit.

## Controllers Registrar (XARC-1)

Peg-contract/s must have the ability to register its `owners/controller accounts` that is accounts that have the ability to collectively unlock tokens deposited by the users. `XARC-1` (CrossChain Alliance Request for Comments 1) is an MVP implementation and should be the simplest, most light weight type of the controller registrar thus only consist of the minimum instruction set.

Controllers Registrar can be implemented as a part of a peg-contract or as a separate, standalone smart-contract. It's goal is to only mark/designate accounts capable of controlling peg-contact, for example a as a curated list of accounts:

```
<address-1>
<address-2>
...
<address-N>
```

_NOTE: A non-transferable token can be used to mark designated accounts if preferred in the implementation_ 

Controllers Registrar should contain information in regards to how many accounts `M` out of all accounts `N` are required to apply changes to the account list of the `Controllers Registrar` contract, so that the curation of its content is possible after the initial setup and can only be executed though a proposal passed by a minimum defined `treshold` of accounts. (e.g. via a dedicated proposal)

Due to limitation of the transaction sizes (e.g. to 48kB on ethereum) it must be possible to setup the Controllers Registrar contract with just single account so that accounts can be added to the list is sets and only when the initial setup is completed the contract would be activated.

### Example JSON Structure

```
{
    "active": <boolean>,    // defines if initial setup was completed
    "threshold": <integer>, // the M count from the M/N parameter
    "count": <integer>,     // the N count, always equal to number of accounts
    "accounts": [           // list of accounts taking part in the consensus
      "<account-1>",
      "<account-2>",
      ...
      "<account-N>"
    ],
    "owner": <address>      // initial deployer of the contract
}
```

### Function Prototypes

* `setActivate` - Defines if initial setup was completed, default `false`. Once set to `true` it can't be set back to `false`.

_Example: setActivate()_

* `setChange` - Defines threshold and list of accounts to be added and/or removed to/from the list, repeating accounts can be ignored. Threshold  defines minimum number of accounts required to manage the accounts list. For the safety reasons threshold value can never be lower then `ceiling(count / 2) + 1`. If any of the values is not set then it should remain unmodified. All values should be optional. This function can only be used by the owner when the contract is NOT active.

_Example: setAccounts(add: ["0x123...456","0x654..321"], remove: ["0x987...765"], treshold: 154)_

* `proposeChange` - Proposal to add accounts to the account list, remove accounts or change treshold, proposal can be raised by any account in the `acounts` list. This function can only be used when the contract is active. Proposal passes and changes are applied only if `approveProposal` tx was submitted by no less then `treshold` number of accounts. If proposal passes then changes are applied and all proposals older then the one that passed should be cancelled/rejected. If any of the values is not set then it should remain unmodified. All values should be optional.

_Example: proposeChange(add: ["0x123...456","0x654..321"], remove: ["0x987...765"], treshold: 156)_

* `approveProposal` - Vote on change proposal can only be used by the accounts in the list of `accounts`. This function can only be used when the contract is active.

_Example: approveProposal(5)_

## Chains Registrar (XARC-2)

Peg-contract/s must have the ability to register foreign chain-id's to which and from which tokens can be transferred. When users make a cross-chain transfers a destination chain information can then be included in those tx's and identified by the contract. `XARC-2` (CrossChain Alliance Request for Comments 2) is an MVP implementation and should be the simplest, most light weight type of the chain registrar thus only consist of the minimum instruction set.

Chains Registrar can be implemented as a part of a peg-contract or as a separate, standalone smart-contract. It's goal is to only mark/designate which sets of accounts present in the `Controller Registrar` can collectively control locking and unlocking assets within the Peg-contract and via which `Bank Controller` contract those operations can be achieved. Each `Controller Registrar` and `Bank Registrar` contract address must be associated with the `chain-id`. The `XARC-2` can be treated as a routing table or an analogy to the DNS registrar where human readable names can be associated with the corresponding IP address.

Only one `chain-id` should be associated with one `XARC-1` and one `XARC-3` contract and only the owner of those contracts can create the initial `XARC-2` record.

### Example JSON Structure

```
{

  "registrar": [
    {
        "chain-id": "<string>",    // 32 Bytes max (256 bits)
        "controller": "<address>", // must be a valid XARC-1 contract address
        "bank": "<address>",       // must be a valid XARC-3 contract address
    }, { ... }, ...
  ]
}
```

### Function Prototypes

* `registerChain` - Defines `XARC-1, XARC-3 contract` and `chain-id` to be registered. Tx can only be submitted by the `XARC-1 & XARC-3` owner/deployer when the `XARC-1` and `XARC-3` is NOT active. The chain-id can never be changed after registration is finalized.

_Example: registerChain(chain_id: "kira-1", controller: "0xADDRESS...XARC1", bank: "0xADDRESS...XARC3")_

* `proposeChange` - Proposal to change `XARC-1` and/or `XARC-3` contracts, proposal can be raised by any account in the `XARC-1 accounts` list. This function can only be used when the `XARC-1` contract is active. Proposal passes and changes are applied only if `approveProposal` tx was submitted by no less then `XARC-1 treshold` number of accounts. If proposal passes then changes are applied and all proposals associated with the chain-id older then the one that passed should be cancelled/rejected.

_Example: proposeChange(chain_id: "kira-1", controller:"0xNEW...XARC1", bank: "0xNEW...XARC3")_

* `approveProposal` - Vote on change proposal can only be used by the accounts in the list of `accounts`. This function can only be used when the contract is active.

_Example: approveProposal(123)_

## Bank Registrar (XARC-3)

Peg-contract/s must have the ability to receive, transfer and issue cross-chain assets. When users make a cross-chain transfer to the peg-contract/s (deposit) on the source chain the tokens should be locked within the Banking Registrar. When users  make a cross-chain transfer out of the peg-contract (withdraw) on the destination chain the tokens should be unlocked or issued by the collective of accounts controlling the Bank Registrar. `XARC-3` (CrossChain Alliance Request for Comments 3) is an MVP implementation and should be the simplest, most light weight type of the bank registrar thus only consist of the minimum instruction set.

Bank Registrar can be implemented as a part of a peg-contract or as a separate, standalone smart-contract. It's goal is to only lock/unlock or issue tokens as commanded by the set of accounts present in the corresponding `Controller Registrar`. Each `Bank Registrar` contract address must be associated with the `chain-id` and the corresponding controller contract.

Only one chain-id and one `XARC-1` contract should be associated with one `XARC-3` contract and only the owner of the `XARC-1` contract can create the initial `XARC-3` record.

Bank Registrar contract should consist of the account list that is depositing or withdrawing assets. Deposits and withdraws can be either accepted or rejected though proposal process. Only deposits older then the minimum `confirmations` time can be included in the proposal. If proposal passes the proposer receives `(proposer_reward*100) %`  of all deposit or withdraw fees while all `XARC-1` accounts voting on the proposal equally split between each other `((1 - proposer_reward)*100) %`.

Withdraw and deposits can have following status codes:

* `0` - Pending
* `1` - Accepted
* `2` - Rejected

Token transfers freeze functionality should be implemented to enable transition to the new bank contracts in the future. 

### Pegged Tokens Issuance

Bank registrar `XARC-3` requires functionality that would allow it issuance of pegged assets. There exist at least three methods to enable that functionality:

1. Embedding functionality to the `XARC-3` that would enable deployment of a modified [ERC-20](https://eips.ethereum.org/EIPS/eip-20), [ERC-721](https://eips.ethereum.org/EIPS/eip-721), ... & other smart contracts that can be fully managed by the `XARC-1` controller accounts.
2. Embedding multi-token smartcontract standard such as [ERC-1155](https://eips.ethereum.org/EIPS/eip-1155) then enable `XARC-1` controller accounts management over its functions.
3. Deployment of a token contract, registration on the `XARC-3` and depositing 100% of its supply to the Bank Registrar from where the assets can be controlled by the `XARC-1` controller accounts.
   
_NOTE: It is expected that in Q4 2021 Metamask will fully support ERC-1155 making it a new NFT standard thus option 2 appears to be the cheapest, most lightweight solution._

The simplest implementation by far is to deploy a fixed-supply contract with maximum possible supply and deposit 100% of all tokens into the `XARC-3` while giving `XARC-1` accounts full control over the assets. To achieve that a new `donate` function should be created that will deposit assets directly to the `XARC-3` but will not create a `deposit` index that `XARC-1` controllers would have to vote on to accept as assets would NOT be credited to any account on the destination chain.

### Example JSON Structure

```
{
  "registrar": "<address>",     // must be a valid XARC-2 contract address
  "deposit-fee": <integer>,     // minimum fee that must be paid to deposit
  "withdraw-fee": <integer>,    // minimum fee that must be paid to withdraw
  "proposer-reward": <decimal>, // reward for proposing withdraws/deposits
  "confirmations": <integer>,   // minimum number of confirmations required
  "freeze": {
    "deposits": <boolean>        // enable/disable deposits
    "withdraws": <boolean>       // enable/disable withdraws
    "donations": <boolean>       // enable/disable donations
  },
  "balances": [
      {
        "address": "<address>",     // account address
        "deposits": [ {             // assets to be accepted for deposit
              "token": "<address>", // token deposited
              "amount": <integer>,  // amount deposited
              "index": <integer>,   // deposit identifier
              "fee": <integer>,     // fee paid by the user
              "status": <integer>   // deposit status
            }, { ... }, ... ], 
        "withdraws": [ {            // pending withdraws to claim
              "token": "<address>", // token that can be withdrawn
              "amount": <integer>,  // amount that can be withdrawn
              "index": <integer>,   // withdraw identifier
              "fee": <integer>,     // fee paid by the user
              "status": <integer>   // withdraw status
            }, { ... }, ... ] },
      { ... }, ...
    ],
  "owner": <address>      // initial deployer of the contract
}
```

### Function Prototypes

* `setActivate` - Defines if initial setup was completed, default `false`. Once set to `true` it can't be set back to `false`.

_Example: setActivate()_

* `setChange` - Defines cross-chain registrar address, fees, proposer rewards and token transfers freeze properties. If any of the values is not set then it should remain unmodified. All values should be optional. This function can only be used by the owner when the contract is NOT active.

_Example: setAccounts(registrar: "0xXARC2..ADDR", deposit_fee: 0, withdraw_fee: 123456, proposer_reward: 0.1)_

* `proposeChange` - Proposal to change registrar, deposit/withdraw fees, fees, proposer rewards and change token freeze properties. Proposal passes and changes are applied only if `approveProposal` tx was submitted by no less then `XARC-1 treshold` number of accounts. If proposal passes then changes are applied and all proposals older then the one that passed should be cancelled/rejected.

_Example: proposeChange(registrar: "0xXARC2..ADDR", deposit_fee: 999, withdraw_fee: 321, proposer_reward: 0.1, freeze_withdraws: true, freeze_deposits: false, freeze_donations: false)_

* `proposeDeposits` - Proposal to accept and/or reject deposits. Can only be raised for assets deposited at `current_block_heigh - confirmations`. If passed records are cleared to save space. This proposal can be raised by any account as per `XARC-1`, proposer receives `proposer-reward %` of all deposit fees. Remaining fees are split between all who vote on accepting proposal. Proposal must have at least one record present and all deposit proposals older then the accepted one become automatically rejected.

_Example: processDeposits(accept: [1,2,4,5], reject: [3,6])_

* `proposeWithdraws` - Proposal to accept and/or reject withdraws. This proposal can be raised by any account as per `XARC-1`, proposer receives `proposer-reward %` of all withdraw fees. Remaining fees are split between all who vote on accepting proposal. Proposal must have at least one record present and all withdraw proposals older then the accepted one become automatically rejected.

_Example: processDeposits(accept: [1,2,4,5], reject: [3,6])_

* `approveProposal` - Vote on change proposal can only be used by the accounts in the list of `accounts` as per `XARC-1`. This function can only be used when the contract is active.

_Example: approveProposal(123)_

* `deposit` - Deposit transaction can be made by anu user and requires a minimum `deposit-fee` to be included. Deposits should only be possible if `deposits-freeze` flag is det to false.

_Example: deposit(amount: 1000, token: "0xCONTRACT...ADDR", fee: 1234)_

* `withdraw` - Withdraw transaction can be made by any user and requires a minimum `withdraw-fee` to be included. Withdraws should only be possible if `withdraws-freeze` flag is det to false.

_Example: withdraw(amount: 1000, token: "0xCONTRACT...ADDR", fee: 1234)_

* `claim` - Claim transaction can be executed after `withdraw` status is changed to `accepted`.

_Example: claim()_

* `refund` - Refund transaction can be executed after `deposit` status changes to `rejected` or if transaction is not present in any of the active deposit proposals. If refund is called funds should be returned to the user.

_Example: refund()_

* `donate` - Transfers assets to the contract without need for accepting deposit. No tokens would be issued on the destination chain in such case. This action can **NOT** be refunded. There should be no deposit fees in case of token donations. Donations should only be possible if `donations-freeze` flag is det to false.

_Example: donate(amount: 1000, token: "0xCONTRACT...ADDR")_