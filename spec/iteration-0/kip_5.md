# [⏎](README.md#Roadmap) KIP_5

> List Order Books

By using `query_order_books` it must be possible to quickly query order books by:
* ID
* Index
* Quote Currency
* Base Currency
* Trading Pair (Base & Quote) - in specific order
* Curator

We should expect large number of queries requesting full list of order book Id's as well sub-lists such as list queried by only specific base or quote currency.

Every response of the `query_order_books` command should be a JSON array containing all info regarding order books (with exception for orders, which must be queried separately for by individual order books)

## Example:
```
[
    {
        "id":"c8f655f24a0a7361713f5da200000001",
        "base": "ukex",
        "quote": "transfer/ibc_token_id/uatom"
        "curator": "kira15v50ymp6n5dn73erkqtmq0u8adpl8d3ujv2e74",
        "mnemonic": "My First Demo Order Book or Hash"
    }, { ... }, ...
]
```

_NOTE: We should aim for no more than 512B stored per each order book record, that implies that 2048 unique order books would occupy 1 MB of data that client side would have to digest._





