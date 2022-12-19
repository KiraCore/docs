# [⏎](README.md#Roadmap) KIP_47
> INTERX Response Caching

It is expected that responses such as tx hash, validator set queries, orderbook state and others are going to repeat often, we have to enable INTERX service to cache all queries on the disk to ensure minimal possible impact on the full node operations. 

Each individual endpoint should have a configurable time (`CACHING_DURATION` in seconds) during which query responses are persisted in the memory. Every time a request is made, the INTERX service should first check if a cached record exists.

## Implementation

1. Users sends request to the INTERX endpoint `E` with query parameters `P`
2. If endpoint `E` does not have caching enabled then full node is queried and response `R` with status code `C` is returned
3. If `E` has caching enabled, then INTERX calculates a hash `H = Blake2(E + P)`
4. We lookup `H` in the KV store of objects stored in on disk
    * Values within KV store are JSON objects containing expiration time `T`, block number `B`, `C` and `R`
    * Values should be stored on the disk with a filename `H`
    * When persisting cached responses on the disk we should separate them within directories corresponding to hashes of the individual endpoints and chain-id's, for example: `CACHE_DIR/<chain-id-hash>/<endpoint-hash>/H`
5. If `H` is found the `R` is returned if `T` is greater than `TIME_NOW` or if block number `B` did not changed
    * Appropriate status code must be returned
    * If `H` is expired then object must be disposed from memory
6. If `H` is NOT found the full node is queried and response `R` cashed and returned
    * `R` is cached along `C`, `B` and `T = TIME_NOW + CACHING_DURATION`

## Further Considerations

* The `rpc_methods` endpoint must be modified to contain information regarding caching time duration as optional parameter.
* Every endpoint request (with exception for POST) must be cached, if `CACHING_DURATION` is set to `0` then minimum duration of caching is 1 block 
* We must be able to define maximum size (in MB) of the cache, if the limitation is reached then `10%` of objects stored should be deleted at random until sufficient memory is available (with exception for endpoints for which user wants to persist all non-expired data).
* It must be possible to define if the data for a specific endpoint should be or should not be randomly garbage collected to save space (e.g. with a bool flag ture/false in the config).
* It must be configurable for each endpoint if hashes should be stored only for the current `chain-id` or all (archive mode)
* The `CACHE_DIR` must be configurable in the INTERX config
* A garbage collection routine must be implemented where an individual task in the INTERX iterates over the cache and removes expired data