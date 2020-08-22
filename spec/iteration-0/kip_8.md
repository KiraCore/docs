# [⏎](README.md#Roadmap) KIP_8
> RPC Response Signing & Proxy

Every response of the JSON-RPC should be signed using `ed25519` signature. Every full-node should have a random signing key created when started for the first time that can be used to sign responses. Before responses can be signed we need to create a service that will be able to communicate with full nodes over gRPC or Tendermint RPC and respond to the client queries.

With introduction of the stargate upgrade to cosmos-sdk there no longer exists an LCD (REST) service and we have to create a replacement for that service in form of the JSON-RPC. Purpose of this new service will be to enable client applications (browser/mobile app) ease of trusted communication with the full nodes as well as loadbalancing in the future.

![picture 1](https://i.imgur.com/1eWTHiz.png)  

For the minimal viable functionality it is essential that we must be able to execute following requests:
* Query Status - GET => `/status` - must contain node-id, latest block and timestamp
* Query Transaction Hash - GET => `/txs/{hash}`
* Query Balances - GET => `/bank/balances/{address}`
* Post Transaction - POST => `/txs`

Above requests must be available ASAP so that frontend developers are not blocked with tasks depending on the access to the JSON-API.

_NOTE: The new JSON/API service code should be a part of the [sekai](https://github.com/kiracore/sekai) repository and placed within sub-folder with the name `INTERX` (analogy to NGINX). INTERX service should be created using Golang programming language so that all backend of the kira blockchain application can easily extend its functionality with new endpoints._

## Response Format

All responses from the `INTERX` service should have a following json format:

```
{
    "chain-id",              // Chain/Network Identifier
    "block": <UInt64>,       // Block number
    "timestamp": <UInt64>,   // UTC Unix Timestamp
    "response": {Object},    // Response Object
    "error": {Object},       // Null or Object if exception was thrown
}
```

References: https://github.com/grpc-ecosystem/grpc-gateway

## Response Signing

Once minimal viable functionality of the proxy service is achieved we need to extend its functionality and add ability to sign responses so tat client application can verify their integrity.

When the JSON response is created all fields such as `chain-id`, `block`, `timestamp`,`response`,`error` should be signed. Once signature is created a new field called `signature` should be added to the json.

_NOTE: The JSON must be minimized before signature is created for the purpose of cross-platform compatibility_

## Response Verification

To verify the signature against a trusted public key client needs to first remove the `signature` field and then minify (remove escape/new line characters for cross-platform compatibility) from the JSON while keeping the exact order of all other fields.
