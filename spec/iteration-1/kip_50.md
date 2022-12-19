# [⏎](README.md#Roadmap) KIP_50
> Multi-Token Fee Payment

Fee payments for token transfers using only a single token are problematic for new users to onboard into the platform, we must provide means of interacting with KIRA Network without need for buying or otherwise acquiring KEX tokens first.

The **Token Rates Registry** should be created where each token denom is valued in terms of KEX token, for example `1 BTC` might be equivalent to `10'000 KEX`. Token Rates should be gov defined and do not have to be in any way correlated with a market value of the token. Because different currencies might have different denominations we should use decimals to define value of exchange rate in terms of KEX token. By default lowest denominations of the all the tokens should be used in the registry. For example rate of `0.00001` for `ubtc` denom would imply that paying `1` micro-BTC is equivalent to paying `10'000` micro-KEX for transaction.

Each token denom in the Token Rates Registry should further contain a boolean properties defining if it is enabled or disabled as fee payment method. 

To ensure security and that no single token (stolen or over-inflated) can be used to to fill up entire block space we should enforce that 50% of the block space should belong to those wo utilize KEX tokens. Remaining 50% should be equally divided among all other token types used for fee payments.


> Pseudo Algorithm Example

```
1. Descend-Order all tx by fees paid denominated in KEX for each token denom
2. Iterate over all tx paid in ukex denom and add them to block until 50% of the block space is filled
3. Switch to another denom stack, and add first tx to the block (just one)
4. Repeat step 3 by switching interactively between denoms until block is full
```

To simplify logic only one token should be used for the fee payments, for example it should not be possible to pay tx fee using one token type and execution fee using another token type.

## Token Rates Registry Example

```
{
    "ukex": {
        "rate": 1.0,
        "fee_payments": true
    },
    "ubtc": {
        "rate": 0.00001,
        "fee_payments": true
    },
    "xeth": {
        "rate": 0.0001,
        "fee_payments": false
    },
    { ... }, ...
}
```

_NOTE: For the ease of deployment token rates registry should be configurable in genesis_

## Modifying Token Rates Registry

Governance process (upsert function) should be used to modify Token Rates Registry.

## Further Considerations

Network Properties Registry (as introduced in KIP_44) registry should be extended to contain `max_block_size` so that governance of the network can adjust the block size as needed. The `max_block_size` will be necessary to define how much space can be occupied by transactions paid in which token.

We have to modify CLI so that tx payments are possible using diffrent token types, best way to achieve it is to use `<amount><denom>` syntax, for example: `--fee 10ubtc --exFee 100ubtc`.

### Security

We have to ensure that the lowest possible fee that can be paid is not smaller then the lowest denomination of any particular token to prevent situations where the blockchain application would allow a 0 fee tx to be processed because of a very small or very large fraction defined in the Rates Registry.

The KEX record should not be possible to modify or disable, meaning that `rate` has to be set to `1` and `fee_payments` set to `true`. This must be done to ensure that no other token can be used to take control over the network operations and so blockchain operations can't be halted by a malicious governance council managing the Token Rates Registry. 