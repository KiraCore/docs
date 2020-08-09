# [⏎](README.md#Roadmap) KIP_12
> Token Transfers

Users of the Kira Network need ability to transfer their tokens between different wallets.

The /txs endpoint along POST request can be used to propagate signed transactions while GET request to the same endpoint allows for the hash or account query. 

## Withdrawal

User interface requires following input options in the withdraw section:
* Token Type (dropdown)
* Amount (texbox)
* Withdrawal Address (texbox)

Users should be able to preview their balance of a specified token type and easily define max, 1/2 or other partial amount (slider). 

Users must also see information regarding total transaction/execution fee cost. 

When withdrawal address is defined a [gravatar](https://github.com/gitcoinco/ethavatar) should be set to enable user to visually identify the address.

To execute a withdrawal and create a signed message a range of js libraries can be used as good reference point:
* https://github.com/node-a-team/cosmosjs
* https://github.com/cosmostation/cosmosjs
 
After transaction in completed user should be informed if tx was successful or not as well as be able to see and copy the transaction hash.

## Transactions

Application should also display a table informing the user about all withdrawal transactions relevant to the current account along their timestamps & hashes. Such transactions can be queried using /txs endpoint.