# [⏎](README.md#Roadmap) LIP_1
>  ERC20 KEX

KEX is a native asset of the Kira Network, however before the Kira Network goes live we have to provide community with the early access to the market on the Ethereum blockchain. For that purpose we will deploy [ERC20](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md) placeholder token that will be in the future swapped in the manual or half-automated way for the native Kira token. 

We must have ability to freeze the token transfers if necessary (eg. to enforce token swap in the future or simultaneous market access). In such case only token transfers to and from a configurable list of addresses should be possible.

## Requirements

Token Name: `KIRA Network`
Initial Supply: `300'000'000`
Symbol: `KEX`
Decimals: `6`
Freezing: `Yes with exception for curated list of addresses`
Whitelist: `Exception from the account freeze, should have 3 options: allow_deposit, allow_transfer, allow_unconditional_deposit, allow_unconditional_transfer`
Blacklist: `Prohibit all interaction of the address with the contract + option to remove accounts from blacklist`
Transactions: `Must have multi-send capability for whitelist and token transfers`

To be more precise whitelist should have 4 modes:
* allow_deposit - if account has allow deposit permission then it should be possible to deposit tokens to that account as long as accounts depositing have allow_transfer permission
* allow_transfer - if account has allow transfer permission then that account should be able to transfer tokens to other accounts with allow_deposit permission
* allow_unconditional_deposit - deposit to the account should be possible even if account depositing has no permission to transfer
* allow_unconditional_transfer - transfer from the account should be possible to any account even if the destination account has no deposit permission

_NOTE: Token must have implementation fully compatible with the ERC20 standard as it is essential to be compatible with the uniswap v2._

_NOTE: All functions such as whitelist or blacklist should be accepting array of accounts to enable timely execution without need to submit transactions multiple times to blacklist/whitelist multiple accounts_

_NOTE: Blacklist should take priority before all other permissions_

## Testing

Code must be commented and full detailed documentation must pe provided regarding deployment and use of every function in form of the README.

Contract compatibility with uniswap v2 must be tested on the kovan testnet along all other features such as asset freeze, whitelist and additional tokens issuance.


