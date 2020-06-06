# KIP_8
> RPC Response Signing

Every response of the [RPC](../rpc/README.md) should be signed using `ed25519` signature. Every full-node should have a random signing key created when started for the first time. It must be possible to query public signing key of the node using CLI to later register it on-chain as defined by the [KIP_4](kip_4.md).

## Response Signing

When the [response](../rpc/README.md#Response-Interface) is created all fields following should be taken into account with exception for the `signature` field

```
{
    "block": <UInt64>,       // Block number
    "timestamp": <UInt64>,   // UTC Unix Timestamp
    "response": [Array],     // Array containing single or many objects
    "next": <Int32>,         // Next page number used for response pagination
    "error": {Object},       // Null or Object if exception was thrown
}
```

_NOTE: The JSON must be minimized before signature is created_

## Response Verification

To verify the signature against a trusted public key client needs to first remove the `signature` field and then minify the JSON while keeping the exact order of all fields.
