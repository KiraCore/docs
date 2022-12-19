# [⏎](README.md#Roadmap) KIP_44
> Transaction Fees

Transaction spam prevention mechanism in the form of a fixed transaction fee must be implemented to ensure that a single user can't spam the network with unlimited number of messages sent at zero cost.

No transaction should be processed unless `min_tx_fee` is paid. We have to also ensure that no transaction is processed if more then `max_tx_fee` is paid, otherwise users might be vulnerable to loosing all their assets due to frontend application bug or inputting wrong denomination.

## Network Properties Registry

Fixed transaction cost (in KEX) must be defined in the Network Properties Registry. Network Properties Registry should be a KV store containing global parameters such as inflaiton rate, minimum time span between consecutive blocks, minimum/maximum duration of the voting proposal etc.

```
{
   "min_tx_fee": <UInt64>,
   "max_tx_fee": <UInt64>,
   "inflation_rate": <UInt64>,
   "min_block_time": <UInt64>,
   ...
}
```
It must be possible for to curate the Network Properties Registry by submitting the transaction by the user with appropriate permission or by creating a governance proposal.

Once the change is applied to `min/max_tx_fee`, blockchain application should automatically reject transactions outside of the min/max range. 

## Fees Distribution

All fees paid to the network including transaction fees should be deposited into the "Fee Pool" that is an address or module responsible for the fees distribution to stakeholders and validators in the future iterations.

The Fee Pool does not have to be functional, but it must be possible to query it's token balances.
