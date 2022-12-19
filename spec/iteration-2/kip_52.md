# [⏎](README.md#Roadmap) KIP_52
> Fee Rewards Distribution

It is essential to incentivize block producers and other network actors. Initially KIRA Network will not have a KEX inflation mechanism and for this reason we have to start with distribution of rewards generated in the process of on-chain payments (fee rewards).

Fee rewards relate to all types of payments occurring when user submits a transaction on-chain, for example transfers or execution fees. When fees are paid they should be transferred to the "fee pool" from where distribution to block producers and other set of network actors will take place.

Fee distribution module should automatically distribute fee rewards to **active** (live & not jailed) validators. The distribution should be equal among all validators and configurable through Network Properties in the range of from `1%` to `50%` (max). 

Fee distribution module should further automatically distribute fee rewards to the community Spending Pool (As per [KIP_53](./kip_53.md)) owned by all accounts possessing `Community Pool Manager` role. The amount distributed to the Community Spending Pool should be set within the range of `1% to 25%` (max). 

The remaining `25%` to `98%` of fee rewards should remain in the fee distribution module and will be distributed to the delegators after staking of tokens becomes possible

Each distribution of assets should occur not more often than once every configurable in the Network Properties Registry number of seconds (default `4 hours`). Only validators who had no downtime (didn't miss a block during their turn to propose it) within that predefined time period should be rewarded.

_NOTE: Kira should allow for creating a blocks with just `2/3 + 1` of validators, it is not guaranteed that every validator will take part in each block production and for that reason we should not count missed blocks unless given validator who missed a block was the one who was supposed to propose it_
