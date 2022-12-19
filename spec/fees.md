# Network Fees

Types of network fees in the KIRA Network

1. Transaction Fee
2. Execution Fee
3. Incentivization Fee

## Transaction Fee

Transaction fee `Τ` (`Tau`) is paid by the users to the network for accepting and processing his signed transactions. Although `Τ` can dynamically change there must exist a global `min_tx_fee` below which no transaction can be accepted. It is not in the scope of the PoC to define functions that dynamically adjust transaction fees, however such possibility should be taken into account in case of future iterations.

_No full node should ever accept a transaction with `Τ` below `min_tx_fee`, and this property should be defined globally. Initially `min_tx_fee` can be defined in genesis but it must be possible to edit it in the future. Full nodes should remove from their tx pool all transactions with `Τ` set below `min_tx_fee`._

When submitting transactions to the network user must have ability to define amount and currency in which he wishes to pay the fee. Furthermore there must exist a `max_tx_per_block` property defining maximum number of transactions that can be processed in each block. By allowing user to define any `Τ` we allow for his transactions to be accepted despite high network congestion, e.g. due to tx spam.

When processing transactions they must be ordered by the `Τ` paid to determine which of them should be processed first (up to `max_tx_per_block`). Ideally full node operators should have ability to individually define  `max_tx_in_pool` and maximum transactions they would propagate to validator nodes in each block (which should be below  `max_tx_per_block`). This enables full nodes to remove from address book other full nodes which would forward to them more than `max_tx_per_block` within a given time instance to prevent propagation of the network spam attacks.

By checking `Τ` of any submitted transaction full node will have instant ability to reject invalid tx'es even before client signature is verified, this should immensely speed up transaction processing in case of tx spam.

It is not guaranteed that all submitted transaction will be successful, for example operation to execute function `f` might timeout, or parameters submitted by the users might not be valid in the context of `f`. In such case transaction should be appended in the state and `min_tx_fee` charged despite failure to execute `f`. This should prevent tx spam attacks where user submits invalid properties or constantly triggers failing functions.

To protect user from accidentally paying unreasonable amount of transaction fees (e.g. due the fault of the client application) there must exist a `max_tx_fee` above which no transaction should be accepted.

Given initial valuation of KEX at `$0.05` per token, `min_tx_fee` is suggested to be initially defined at the amount of `0.1 KEX` (`~ $0.005`) and  `max_tx_fee` as `100 KEX` (`$5`).

Existence of the `max_tx_fee` implies that (although unlikely) there might occur events where users indeed wish to pay `$5` for transactions to be processed as soon as possible filling up tx pool of the full nodes. For this reason full nodes must have ability to drop expired transactions. It should not be guaranteed that transaction submitted to the network will be processed and for that reason user must be able to further define `expires` time. Property `expires` will give user ability to safely repeat the transaction by simply awaiting that time and checking if tx hash is present in the blockchain state.

## Execution Fee

Execution fee `Ε` (`Epsilon`) differs from transaction fee `Τ` in a way that it can't be set by the user. Execution fee should only be paid by the user in case of successful execution of any function `f`. 

Each individual function (`f`, `f'`, `f''`...) should have individual execution fee (`Ε`,`Ε'`,`Ε''` ... ) that has to be paid on top of the transaction fee. Execution fee should be a flat fee that can be pre-defined in genesis however there must exist possibility to adjust it in the future for each function.

Opposed to transaction fees, execution fee can have value of 0. Thanks to execution fees and because Kira is a governance permissioned there is no requirement for the concept of GAS which should meaningfully decrease computational footprint of calculating gas prices.

Despite lack of GAS there must exist concept of the timeout. Each individual function (`f`, `f'`, `f''`...) should have individual, configurable `timeout` property to prevent users from being able to halt the chain in case of the bug where function being triggered takes a long time to execute.

To protect user from accidentally triggering functions that might incur high costs to his balance, he must be able to to define maximum fee `max_fee` he is willing to pay. If `max_fee` is defined it should apply to both execution fee `Ε` he might not be aware of as well as Incentivization fee `Ι`.

## Incentivization Fee

Incentivization fee `Ι` (`Iota`) differs from the execution fee `Ε` in a way that it can be curated individually by individuals owning a contract or controlling certain types of flavoured functions `FF` which other users can trigger (for example order book curators could control maker and taker fees of the order book they manage). Incentivization fee does not have to be flat, it can be defined as % or change dynamically for example based on the individual user token balance. In other words `Ι` is a fee paid on user per user basis rather than being fixed (the same for everyone). Finally `Ι` can be paid in any currency, not just a whitelisted fee currency.

For each individual function (`f`, `f'`, `f''` ... ) for which individual fee (`Ι`, `I'`, `I''` ...) applies there must be a defined `min_ic_fee` (`min_I`) to ensure that governance of the chain can control to ensure incentivization of the native token holders and network operators (Death spiral protection mechanism). Any difference between `min_I` and `Ι` would be paid to the `FF` curators. One thing to note is that a global `min_I` must be individual for each function `f`.

_Example: if order book curator defines taker fee at `0.1%` while `min_ic_fee` is set to `0.05%` then difference that is `0.1% - 0.05` would be paid to his address (or range of curator addresses). This mechanism would incentivize users to pay for creation of custom order books and have them managed by their own communities rather than chain governance - which is necessary in case where there is hundreds or thousands of order books that must be managed._

While there is only a single transaction fee `Τ` and execution fee `Ε` applied to each transaction, there can be many different incentivization fees `Ι` defined depending on the logic of any particular function `f`.



