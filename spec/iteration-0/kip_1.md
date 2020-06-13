# [⏎](README.md#Roadmap) KIP_1
> Create Order Book

Any user with non zero balance of at least `2` **different** tokens (can be a same token originating from 2 different networks) must be able to create an order book by sending a `create_order_book` transaction.

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

## Example (Pseudo-Code) Solution:
```
ID == 
(blake(curator).take(4).toHex() << 12) + 
(blake(base).take(4).toHex() << 8) + 
(blake(quote).take(4).toHex() << 4) + 
len(last_order_book_index).toHex()
```

As the result of `create_order_book` transaction `id` should be returned to the user, otherwise error response if `index` already exists.

_NOTE: This might not be the most optimal way to create ID, and there is a possibility of currency name or creator address collisions, but those should not be an issue as results from the prefix query can be further refined_ 

## Fees (OPTIONAl)

### Execution Fee

There must be a custom, flat [execution fee](../fees.md#execution-fee) `Ε` paid when order book is created as the result of `create_order_book` execution.

Execution Fee should be set at `$50` or (`1000 KEX`)

### Fees Distribution

All fees paid in the native or non native currencies should be sent to the address controlled by the "rewards distribution module" (RDM). The RDM module does not have to be be implemented in the `Iteration 0`
