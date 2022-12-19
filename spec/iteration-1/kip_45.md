# [⏎](README.md#Roadmap) KIP_45
> Execution Fees

Execution fees differ from transaction fee (KIP_44) in a way that they are fixed and can't be set by the individual user. Execution fee should only be paid by the user in case of successful execution, if function fails or returns an error a `failure_fee` should be paid which should be configured separately and must be not greater than `execution_fee`. Failure fee has so be present as a mean of spam prevention. 

Each individual function (`f`, `f'`, `f''`...) should have corresponding execution fee (`Ε`,`Ε'`,`Ε''` ... ) that has to be paid on top of the transaction fee. Execution fee should be a flat fee that can be pre-defined in genesis as well as changed on-chain through governance process.

Flat fee for executing specific functions (submitting specific transaction types) should be defined in the Functions Registry - which is also part of the [Kira Network Permissioining System](https://i.imgur.com/28ONnVW.png).

Opposed to transaction fees, execution fee can have value of 0. Thanks to execution fees and because Kira is a governance permissioned there is no requirement for the concept of GAS which should meaningfully decrease computational footprint of calculating gas prices.

Despite lack of GAS there must exist concept of the timeout as any function can be an arbitrary state machine as well as smartcontract e.g. CosmWasm. Each individual function (`f`, `f'`, `f''`...) should have a configurable `timeout` property to prevent users from being able to halt the chain in case of the bug where function being triggered takes a long time to execute.

## Implementation

![picture 1](https://i.imgur.com/p3bG6m5.png)  

Functions (Permissions) Registry - a KV store should be expanded to reference object describing properties of each individual function that user can execute.

Following fields should be present:
* `name` - Friendly Name of the Function (max 128 characters)
* `transaction_type` - Type of the transaction that given permission allows to execute
* `execution_fee` - How much user should pay for executing this specific function
* `failure_fee` - How much user should pay if function fails to execute
* `timeout` - After what time function execution should fail
* `default_parameters` - Default values that the function in question will consume as input parameters before execution

It must further be possible to upsert and query the Functions Registry as well as enforce that when given function id (transaction type) is being executed the `execution_fee` or `failure_fee` is paid.

When user submits a transaction a corresponding `transaction_type` is present in the message submitted, we should ensure that each Function Id is unique and corresponds to only single `transaction_type` to ensure that user can easily identify the operation when signing message on his hardware or software wallet. 

_NOTE: In this KIP we do not have to enforce timeouts_