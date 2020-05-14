
# Order Book

Order book is represented by a unique identifier that references list of orders. Order book links two different tokens and enables trading of those tokens by providing an easy to query reference.

## KIP_1
> Create Order Book

Any user with non zero balance of at least `2` **different** tokens (can be a same token originating from 2 different networks) must be able to crate an order book by sending a `create_order_book` transaction.

The order book creation must involve a custom fee (other than transaction fee), paid in the whitelisted fee token. For the sake of the PoC we will simplify this functionality to a fixed fee payment in a native [Kira Protocol Token (KEX)](..\native-token.md). Order book creation fee can be defined (hard coded) in the genesis file.

_NOTE: Order book creation fee is necessary to prevent spam attacks_

The `create_order_book` transaction requires following properties
* `index` - globally unique iterative index
* [base](https://www.investopedia.com/terms/b/basecurrency.asp) - base currency
* [quote](https://www.investopedia.com/terms/q/quotecurrency.asp) - quote currency
* `mnemonic` (optional) - friendly nickname (or hash pointing to description), no longer than 64 characters
* `curator` - address or identifier of the account (or group of governance actors) that can edit the order book

The `curator` address or id should be automatically assigned as the address of the order book creator to prevent attack where someone lists illicit asset while referencing legitimate user or governance group as curator.

It MUST NOT be possible to create two order books with the same ID, however there can be many order books with the same base and quote currencies or even the same curator account.

For the purpose of the PoC `curator` property will not be used as NO `update_order_book` transactions will be required in that particular milestone. Finally message structure should take into account future updates and possibility to query by any of the above properties.

The construction of the ID should be designed in a way to enable query by prefix using curator address, base currency & quote currency in that specific order, all of the arbitrary length while maintaining the ID possibly short. The ID should always be of fixed length (16 Bytes)

When the order book is created an incremental only and unique `last_order_book_index` (4 Bytes) aka `index` should be assigned to it, in order to ensure that orders can reference order-book in the non expensive manner (query by suffix).

Example (Pseudo-Code) Solution:
```
ID == 
(blake(curator).take(4).toHex() << 12) + 
(blake(base).take(4).toHex() << 8) + 
(blake(quote).take(4).toHex() << 4) + 
len(last_order_book_index).toHex()
```

As the result of `create_order_book` transaction `id` should be returned to the user, otherwise error response if `index` already exists.

_NOTE: This might not be the most optimal way to create ID, and there is a possibility of currency name or creator address collisions, but those should not be an issue as results from the prefix query can be further refined_ 

## KIP_5
> List Order Books

By using `query_order_books` it must be possible to quickly query order books by:
* ID
* Index
* Quote Currency
* Base Currency
* Trading Pair (Base & Quote) - in specific order
* Curator

We should expect large number of queries requesting full list of order book Id's as well sub-lists such as list queried by only specific base or quote currency.

Every response of the `query_order_book` command should be a JSON array containing all info regarding order books (with exception for orders, which must be queried separately for by individual order books)

Example:
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

_NOTE: We should aim for no mare than 512B stored per each order book record, that implies that 2048 unique order books would occupy 1 MB of data that client side would have to digest._







