# [⏎](README.md#Roadmap) KIP_24
> Upsert Token Alias (Proposal)

It must be possible to define and update token aliases. The tokens usually have a denomination such as ukex, uatom or even longer if they originate from outside of the network. We have to provide aliasing and naming registry for the otherwise unredeable token denominations.

## Upsert Token Alias

By submitting `upsert_token_alias` a proposal should be created that will enable governance set to update **Token Alias**. The upsert token alias function should have an assigned identifier in the functions registry.

_NOTE: Define execution cost in genesis  - [Execution Fee](/spec/fees.md) to submit `upsert_token_alias` proposal._

Proposal should have following properties:
* Configurable in genesis `expiration` time (uint32) - seconds since submission
* Configurable in genesis `enactment` time (uint32) - seconds since expiration
* Allowed vote types: `yes`, `no`, `veto`, `abstain`
* `symbol` - Ticker (eg. ATOM, KEX, BTC)
* `name` - Token Name (e.g. Cosmos, Kira, Bitcoin)
* `icon` - Graphical Symbol  (url link to graphics)
* `decimals` - Integer number of max decimals
* `denoms` - An array of token denoms to be aliased 
* Status with following types:
  * `undefined` - 0x00
  * `active` - 0x01
  * `rejected` - 0x02
  * `passed` - 0x03
  * `enacted` - 0x04

**IMPORTANT: It has to be ensured that a single token denom can have only a single alias otherwise a tx should fail.**

_NOTE: Network actors with assumed `governance` role should by default posses ability to propose and vote on `upsert_token_alias`. Network actors with assumed roles `validator` should by default posses ability to vote on `upsert_token_alias`. Those types of permissions should be predefined in genesis_

_NOTE: It must be possible to predefine token aliases in genesis file so that deployment of test network can be simplified_

## Proposal Vote

By submitting `vote` transaction along proposal identifier it should be possible for the governance members with the whitelisted permission to vote on `upsert_token_alias` to submit relevant vote types to pass or reject action of upsetting Token Aliases.




