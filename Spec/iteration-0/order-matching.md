# Order Matching 

Order matching refers to matching buy and sell orders.


## KIP_2
> Place Order

The default order type in Kira Protocol should be a limit order. A limit buy order can only be executed at the limit price or lower, and a limit sell order can only be executed at the limit price or higher. A limit order can only be filled if the market price reaches it. Limit orders do NOT guarantee execution, but ensure that users do not pay more than a defined price.

Orders placed using `create_limit_order` transaction must be linked to a particular order book by the ID. When order is placed user tokens must be locked in the account controlled by the order book module.   

The `create_limit_order` transaction requires following properties
 * `order_book_id` - globally unique order book identifier or index
 * `type` - type of order (`limit_buy`, `limit_sell`) - should be an enum
 * `amount`- int64 value (by the lowest denomination)
 * `price` - int64 value - limit price
 * `expires` - int32 UTC (seconds) timestamp when the order will be cancelled

_NOTE: Prices, amounts and all other values should always be integers to prevent rounding errors or different (per-region) comma representations when parsing, that float/decimal values might introduce._

_OPTIONAL: In the first iteration of the Kira PoC application limit orders can be ordered by gas paid and executed with a pseudo-random priority, that means, a fully deterministic  pseudo random function can be initialized with the seed equal to first `N` Bytes of the previous block hash. This strategy is a simplest front-running & gas-war prevention method, as it is not guaranteed your order will be processed first even if you pay more GaS._

When orders are placed in the blockchain state they should be prefixed by the order book `index` (4 Bytes), `type` (4 Byte), `price` (8 Bytes) and end with a unique iterative `last_order_index` (4 Bytes). Order Id's should be of fixed length (20 Bytes). Note that only already matched orders should be placed in the state. This will enable us to quickly query subset of specific order by the prefix.

_OPTIONAL: Order `index` has additional function, that is order prioritization when matching of old orders with new ones is taking place. This enables prioritization of the order execution even in the case where two orders are placed with the same price._

To determine which orders match we need to first delete all expired orders and unlock assets belonging to original holders. Furthermore if there are `cancel_order` transactions in current block their execution should be prioritized before any orders are matched. It would be ideal if all data can be loaded to RAM memory during processing. As the second step we can query sell orders by prefix to match new buy orders and query buy orders to match new sell orders.

As the result of `create_limit_order` transaction `id` should be returned to the user, otherwise error response if order was not created.

## KIP_3
> Cancel Order

By using `cancel_order` it must be possible to cancel (remove from state) existing orders by using order `id`.
