# [⏎](README.md) Status

Information relevant to status and health of the node

## Node Info

The `/node_info` endpoint allows to query information about the connected node such as node `id`, and `network` name. 

<details> 
    <summary>Command Example</summary>
<pre>
RPC_ADDRESS="localhost:10002" && \
 curl "$RPC_ADDRESS/node_info" | jq '.'
</pre>
Output Example
<pre>
{
  "node_info": {
    "protocol_version": {
      "p2p": "7",
      "block": "10",
      "app": "0"
    },
    "id": "fe3b878a9878d2448b6b04470bf53a697ea7f4cc",
    "listen_addr": "tcp://101.1.0.1:26656",
    "network": "kira-1",
    "version": "0.33.4",
    "channels": "4020212223303800",
    "moniker": "Local Kira Hub Validator",
    "other": {
      "tx_index": "on",
      "rpc_address": "tcp://127.0.0.1:26657"
    }
  },
  "application_version": {
    "name": "sekai",
    "server_name": "sekaid",
    "client_name": "sekaid",
    "version": "1.0.0",
    "commit": "8e58f3998ed4d60a2e29f2cb774c0ffbb38c7c35",
    "build_tags": "",
    "go": "go version go1.14.2 linux/amd64"
  }
}
</pre>
</details>

