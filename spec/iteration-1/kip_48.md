# [⏎](README.md#Roadmap) KIP_48
> INTERX Faucet

It is essential that new users must have ability to test functionality of the network, we have to modify INTERX service to enable them easily claim faucet tokens.

A new `faucet` endpoint must be created to enables users send the GET request with the kira account address as query parameter to claim tokens. 
If the endpoint is queried without any parameters a faucet deposit address and current balance of the faucet account should be returned

# Implementation

1. Users sends a GET request to `../faucet?claim=kiraXXX...XXX&token=XYZ`
2. INTERX queries the balance of `kiraXXX...XXX` account
3. If the account claimed tokens less than `T` seconds ago then error status should be returned with information how many seconds are left
4. If account has amount of `XYZ` tokens equal to `Y` where `Y` is less then `X` then faucet should send to the account `kiraXXX...XXX`no more than `Z`, where `Z = X - Y` and where `Z` is less then predefined minimum token claim `M`
5. Time should be stored (on the disk) along account address, when the account last claimed the tokens from the faucet

## Further Considerations

* It must be configurable by the node operator which tokens and with what minimum `M` can be claimed by the user (whitelist & blacklist)
* It is also essential that a minimum balance for each token can be defined so that so tokens will be claimed if that minimum is reached
* INTERX should automatically generate a random mnemonic (account) when started, if its not specified within the config which will be used as faucet account
* It must be configurable by the node operator how often (in seconds) each individual account can claim tokens
* Every time the account claims tokens a json file should be read/written to the disk with information when the last claim occurred so that after restart INTERX can persists the information and so it is not required to preserve that information in RAM (as that could easily cause overflow during a spam attack). It's recommended that the faucet
* It must be possible to define a whitelist of accounts that can claim tokens, if no whitelist is defined then any account should be able to claim tokens
