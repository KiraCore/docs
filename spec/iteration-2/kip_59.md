# [⏎](README.md#Roadmap) KIP_59
> Double-Sign Prevention

KMS it is causing incompatibility issues with latest releases of the Cosmos SDK, so we have to create a separate and explicit way of preventing double-signing.

We should implement a mechanism directly within ```sekai``` which can prevent validators from creating or signing new blocks unless the node is fully synced. We should also incorporate a parameter in the dedicated json configuration file ( `sync.json` ) which prevents validator from signing a block before the hight and/or time specified. 

When the node starts the time parameter in the config should be automatically updated to the current time plus 10 seconds but ONLY IF `min-time` is less then current time. 

Every time the block is produced the configuration file should be updated with the latest height plus one but ONLY IF `min-height` specified in the config is less then the current height.

Both `min-height` and `min-time` should be honored at the same time meaning that if any of the minimums is not reached blocks should not be produced.

## Example

```
{
    "min-height": 1234567890,        // block height
      "min-time": 1612454377,        // unix timestamp
}
```