# Stafi Protocol vs Kira Protocol

## References
* Stafi Whitepaper: https://docs.stafi.io/stafi-whitepaper/stafi-whitepaper ([mirror-1](http://web.archive.org/web/20200720103318/https://docs.stafi.io/stafi-whitepaper/stafi-whitepaper))


## Product

**Stafi** is a staking derivatives issuance protocol which provides liquidity to assets at stake.

**Kira** is a staking derivatives exchange protocol which provides liquidity to assets at stake, as well as permissionless and trustless exchange of those assets.

## Consensus

**Stafi** uses a Substrate framework with its standard NPoS consensus (known from Polkadot/Kusama) which does not appear to be substantially modified. NPoS is well justified for ensuring equal distribution of stake among validators. It does not prevent creation of sybil validators (splitting staking tokens between many accounts to create many nodes) and does not prevent security leaks (tokens obtained in malicious way or otherwise without consent of their owners can be used to host validator nodes or used in governance process - e.g. Centralized Exchanges can vote on proposals).

**Kira** uses a Cosmos SDK framework with a custom [MBPoS](https://medium.com/kira-core/introduction-to-the-multi-bonded-proof-of-stake-60d95905c32b) consensus. Kira consensus provides ways to prevent creation of sybil validators and security leaks through governance driven node curation. Kira consensus is geared towards in-house validator hosting rather than strong reliance on the large cloud infrastructure providers.

## Staking Derivatives

**Stafi** ensures locking assets on the chain of origin (to issue derivatives) and secures those original assets by a subset of SSVs (Special Stafi Validators) holding ownership over the multisig where coins are deposited. SSVs are required to bond certain amount of FIS tokens. The tokens are secure on the chain of origin as long as value of FIS at stake that can be slashed is lower than amount of tokens locked in the multisig. Because NPoS allows operators to create sybil nodes there is a possibility that a single operator could hold a control over the multisig account. Growing amounts of assets at stake effectively decreases security of Stafi. Only derivatives of the native staking tokens can be issued with Stafi. 

**Kira** ensures locking assets on the destination chain (Kira Network) by a full validators set -  mechanism is built into the consensus. Kira  shares portion of the block and fee rewards with delegators in exchange for increasing network security and exchange protocol liquidity. Effectively Kira increases its own security with growing amount of assets at stake. There is no limitation regarding types of tokens that can be staked, which means that even staking of NFT's, commodities, fiat, stablecoins is possible. Kira further allows staking on leverage - tokens can be staked on the chain of origin and Kira at the same time, as long as the sum of possible slashing penalties is lower then 100%.

## Business Model

**Stafi** charges commission fees for staking, which are later used to incentivize its own token holders network. The core business model works by assuming that proof of stake networks will never update their chains to provide staking derivatives natively. Stafi functionality can be replaced by a smart contracts on the native chains, which can potentially harm its business model. Stafi is a good solution for PoS networks which do not have a concept of smartcontracts or are unwilling to provide liquidity to staking assets. To summarize, the smaller the number of PoS networks provide native staking derivatives the bigger the potential and utility of the Stafi Protocol.

**Kira** charges network fees for interacting with its network and exchange protocol. The core business model works by assuming that more PoS networks will natively support staking derivatives or more products similar to Stafi enter the market. With growing number of derivatives and tokens in the ecosystem Kira can offer market access and liquidity to greater number of networks and assets. Finally Kira allows token holders to utilize 100% of their capital regardless of the token type. Kira not only does NOT charge any commission for staking but pays out additional revenues in form of block and fee rewards. Growing number of assets at stake probabilistically  increases liquidity of the exchange protocol which in turn increases  trading activity, thus  increasing revenues from the network fees. Growing revenue from the network fees provide more incentives to stake even greater number of assets. To summarize, the greater the number of PoS networks provide native staking derivatives the bigger the potential and utility of the Kira Protocol.

## Summary

Stafi protocol is a complementary product, that can increase market reach and use of the Kira Interchain Exchange Protocol. If few of the basic security flaws of the Stafi Protocol are addressed it should be possible for secure use within interchain ecosystem. 



















