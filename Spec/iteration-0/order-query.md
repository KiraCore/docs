# Order Query 

Order query refers to the functionality enabling clients to access live state of orders within individual order books

## KIP_6
> List orders

After initial order book list query, client application must be able to query (using `query_orders` command) all orders placed using `create_limit_order` and in the future other `create_xxx_oder` type commands.

It should be possible to query orders by using order book `id` which can be acquired through `query_order_books` command.

Due to possibility of spam attacks aiming at decreasing software performance it must be possible to limit number of orders received by the user through two types of custom parameters sent along the `query_orders` request:

* `max_orders` - integer defining maximum number of orders that client will receive as response from the query
* `min_amount` - orders below this amount will not be presented to the user

Result of the query should have following json format:

```
[
    {
        "id": 1,                # (int32)
        "amount": 1000,         # (int64)
        "type": 0,              # (int32)
        "price": 100,           # (int64)
        "expires": 1590012341   # (int32)
    }, ...
]
```

_NOTE: Order type is an enum e.g. (0 - `limit_buy`, 1 -`limit_sell`, 2 - `market_buy` ..)_

_NOTE: Prices, amounts and all other values should always be integers to prevent rounding errors or different (per-region) comma representations when parsing, that float/decimal values might introduce._

_OPTIONAL: It would be ideal if as result of query it was possible for the user to acquire pending/live orders, which will most likely be included in the next block. If this functionality was to be implemented a second function called `query_unconfirmed_orders` should be created to enable that while preserving similar behavior and anti-spam features as `query_orders`._
