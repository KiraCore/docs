# [⏎](README.md#Roadmap) KIP_11
> Token Balances

Users of the Kira Network need ability to preview their token balances, there can be many different tokens that user can hold in his wallet. Users also require ability to deposit and transfer their tokens between different accounts.

[Balances query](\spec\rpc\endpoints\bank.md#Query-Balances) of the Bank module can be used to fetch information about available account balances of the user. Users are identified by their public key, e.g. `kira1ufak8sc7g6w7pnlmalq9adqmj7cktcrk073ctz`

Token balances of the user can originate from different networks such as Kira Hub or Kira Zones. Independent networks are usually identified by different network ID's and for that reason network origin should be clear for the user. To identify network identifier (e.g. `kira-1`) a [node_info](\spec\rpc\endpoints\status.md#Node-Info) endpoint can be used.

## Token Balances

User interface must contain a list of token balances and incorporate following information
* Graphical Symbol
* Asset Name
* Ticker
* Amount (Available Balance)
* Denomination
* Decimals

By default list should be ordered by the amount in decreasing order and only contain assets that have non zero amount. Ideally it should be possible to toggle ordering of the list based on the amount and asset name.

In the Proof of Concept (PoC) we will only have access to Denomination and Amount, for this reason we need to store information about tokens within the state of the user interface.

List of tokens and their detailed information that will be available in the PoC:
```
Name        | Ticker | Denom | Decimals
------------|--------|-------|---------
Kira        | KEX    | ukex  | 6
Bitcoin     | BTC    | ubtc  | 6
Cosmos      | ATOM   | uatom | 6
Sentinel    | SENT   | usent | 6
Ethereum    | ETH    | ueth  | 6
e-money USD | eUSD   | euusd | 6
e-money EUR | eEUR   | eueur | 6
```

### Deposit Address 

Token Balances view should also incorporate "deposit address" of the user along the [gravatar](https://github.com/gitcoinco/ethavatar) to ensure that users can easily identify their account address along the corresponding network id.

A "reveal-more", "spoiler" or "button with popup" type element should be present on the page that would list networks from which deposits can be accepted - all of the networks should have a name and logo associated with them.

For the purpose of PoC, following network support can be presented:
* Bitcoin
* Ethereum
* Cosmos Hub
* e-money.com
* Sentinel

Each entry should have a dedicated button that would reference documentation informing users how to deposit or withdraw different token types into/from their deposit address.

