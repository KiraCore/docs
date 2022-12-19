# [⏎](README.md#Roadmap) KIP_33
> Preview Validator Set

Frontend application must have a dedicated "Network" tab where users can preview validator set. We must be able to display detailed information in regards to all network operators.

As per KIP_26 validator set explorer tab should incorporate information from validator identity registry such as moniker, website, social media link but most importantly current validator rank as defined by the KIP_54. All validators in the tab should be sorted by their rank and each entry incorporate color-coded status indicator so that is easy to identify active and inactive validator nodes.

Users should have ability to preview detailed information when expanding each entry as well as easily search validators by their address & moniker.

Users should have further ability to "star/unstar" or otherwise mark selected validator nodes they trust. If the node is marked as trusted then frontend application should query signer key registry as per KIP_4 to discover all enabled (active) entries which contain INTERX IP address'es and corresponding public keys. 

Frontend should monitor active INTERX nodes and loadbalance the requests, so that a single node is never spammed. Furthermore a monitoring task should be created which periodically checks the signer key registry in case public keys or IP addresses change.

Expected data record in the key registry should have following json format:

```
{ 
    "id": "interx-public",
    "address": "<IP Address or DNS>"
}
```

If record contains `interx-public` as identifier the `address` field should be assumed to be INTERX IP or DNS address exposed to the public internet. Default ports and networking details can be found in following article: https://medium.com/kira-core/kira-testnet-phase-0-milestone-1-f667b1522b31


In the settings tab we should further visualize all trusted nodes along their status - healthy unhealthy (there is an endpoint for that). Traffic should never be loadbalanced toward "unhealthy nodes" or those who are not synced. We should use BFT algorithm to determine block height 2/3 imply the block height.

We should further give client ability to "trust all nodes" - meaning that traffic would be loadbalanced between all public endpoints. Loadbalancing can use an RNG to select node for each querry. 