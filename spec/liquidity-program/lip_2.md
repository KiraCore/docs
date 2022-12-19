# [⏎](README.md#Roadmap) LIP_2
>  Uniswap v2 LP Rewards

Uniswap is a great way induce liquidity for ERC20 KEX token, we have to create incentivization program which will reward all users providing liquidity into two markets:

* ETH/KEX
* USDT/KEX

Each liquidity pair is represented by a unique, freely-transferable ERC20 token (LP-tokesn). All fees (0.3% per trade) are added to the relevant liquidity pool; thus all fees go to liquidity providers in proportion to their share of the pool's liquidity.

In similar fashion we will distribute KEX rewards to everyone providing liquidity - based on the amount of (LP-tokens) they have.

Our goal is to distribute `X` KEX (eg. 10'000 KEX) every `Y hours` (eg. 24 hours or equivalent in blocks) to all who provide liquidity to the uniswap pool based proportionally on their LP token balances. The value `X` and `Y` should be configurable by the contract creator individually for each token pair.

Only KEX token pairs defined by the contract creator should be incentivized with bonus KEX tokens and we must have ability to define/change those token pairs if needed.

## Upgrades

Token contract should be [upgradable](https://ethereum.stackexchange.com/questions/2404/upgradeable-smart-contracts) through proxy contract or other viable method, so that we can preserve ability to maintain the address in case changes would be required in the future.

## Requirements

* Configurable token paris (minimum 3)
* Token Distribution upon user request
* Configurable block time within which user must claim rewards
* Rewards should only be distributed for minimum balance between two claims
* Check if LP tokens were not withdrawn by the user within last claim period
* Configurable amount of rewards to distribute for each token pair
* Contract must be upgradeable though proxy contract.


## Testing

Code must be commented and full detailed documentation must pe provided regarding deployment and use of every function in form of the README.

Contract compatibility with uniswap v2 must be tested on the kovan testnet

