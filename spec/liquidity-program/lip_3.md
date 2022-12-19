# [⏎](README.md#Roadmap) LIP_3
>  Liquidity Auction

In the final round of the token distribution, KEX will be offered though the Liquidity Auction mechanism to enable fair chances of acquiring tokens while at the same time inducing liquidity to the market.  

The Liquidity Auction works in the similar fashion to the Polkadot Reverse Dutch auction with the difference that in case of oversubscription all tokens that overflowed the hard cap would be used to add liquidity to the uniswap or as MM war chest in case of listing to support price on the market.

Reverse Dutch auction starts with a very very high initial valuation that cannot possibly be fulfilled and decreases towards predefined valuation at predefined rate. Auction ends instantly if value of assets deposited is greater or equals to the valuation, or if auction times out.

## Implementation

KEX Liquidity Auction will have two linear distribution slopes. Price will start at the valuation `P1` and progress quickly towards `P2` (decrease) with defined rate `R1`, then after reaching `P2`, the price will fall towards P3 with a slower rate `R2`. The auction ends if `P3` is reached of if value of funds contributed `V` exceed the value of `P1*number_of_tokens`.

Value of `R1` and `R2` must be defined programmatically, meaning that we will specify prices of ETH per `1` KEX `P1`, `P2`, `P3` and define number of blocks `T1` and `T2` within which price must decrease. In other words: `R1=P1/T1` and `R2=P2/T2`. 

Once the auction ends, meaning value contributed is greater than current price times number of tokens to distribute, the tokens will be transferred to the holders in proportion to their contribution at the valuation between `P1` and `P3` that the auction ended at. 

All the participants in the auction get the tokens at the same price that the auction finalized at without any bonuses or other type of discounts for early participation.

_NOTE: Time at which auction starts must be configurable_

## Limitations

The whitelist mechanism should be implemented, meaning that only whitelisted and KYC'd addresses must have ability to contribute to the auction.

## Security

After finalization of the auction, tokens (KEX) should be sent to the participants that contributed funds, and the funds (ETH) should be transferred to configurable account address. It should not be possible to change withdraw address for the funds after the auction ends.

Contract owner should have ability to withdraw funds manually in case of malfunction of the withdraw mechanism (e.g. insufficient gas). It should be however not possible to withdraw funds to any other address then the one preconfigured when the auction starts.

Contract should not allow to deposit tokens before the auction starts and after the auction ends.

