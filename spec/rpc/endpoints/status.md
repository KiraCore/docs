# [⏎](README.md) Status

Information relevant to status and health of the node

## Status

The `/status` endpoint allows to query information about the connected node such as node `id`, `network`, `latest_block_height`, `latest_block_hash` and `latest_block_time`. 

### Command Example

```
RPC_ADDRESS="101.1.0.1:10001" && \
 curl "$RPC_ADDRESS/status" | jq '.'
```

### Output Example
```
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "node_info": {
      "protocol_version": {
        "p2p": "8",
        "block": "11",
        "app": "0"
      },
      "id": "fe3b878a9878d2448b6b04470bf53a697ea7f4cc",
      "listen_addr": "tcp://101.1.0.1:26656",
      "network": "kira-1",
      "version": "0.34.0",
      "channels": "40202122233038606100",
      "moniker": "Local Kira Hub Validator",
      "other": {
        "tx_index": "on",
        "rpc_address": "tcp://127.0.0.1:26657"
      }
    },
    "sync_info": {
      "latest_block_hash": "4D4E019A82805011E6CDC3CDC439C8DD2DA0C100B52D390873B280CE2D992430",
      "latest_app_hash": "143EA637D0609493785D5D4B5A7DD74ED1AF71B89B74205F9B18D72AF4277659",
      "latest_block_height": "2718",
      "latest_block_time": "2020-08-22T20:19:39.716401162Z",
      "earliest_block_hash": "6965B685AA28828FF5F13A96D3620DEB014CFCFBA8ED26C65A8C79A6EE822C3D",
      "earliest_app_hash": "",
      "earliest_block_height": "1",
      "earliest_block_time": "2020-08-22T16:31:34.060468469Z",
      "catching_up": false
    },
    "validator_info": {
      "address": "71D9D37F5FD698D848DEC5BDD301CBFDB1318FA4",
      "pub_key": {
        "type": "tendermint/PubKeyEd25519",
        "value": "/s5iQwa7oemQJIb5zVBBbV1iAFIUKGerYmCzAfSXe1I="
      },
      "voting_power": "0"
    }
  }
}
```