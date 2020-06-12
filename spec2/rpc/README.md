# Kira RPC

Defines API', that is request-response protocol used by every full node in the Kira Network. Full list of available endpoints can be found [here](./endpoints/README.md).

Kira uses JSON to encode/decode every request-response made to and from the client. To minimize cost of client side operations and due to governance permissioned nature of the network there is no concept of the lite clients, and rather a public/private key authentication along on-chain registry is used to enable user secure means of communication with trusted full nodes operated by him, or any of the operators he trusts.

 Due to frequent attacks on the centralized DNS servers, possibility of the "man-in-the-browser" attacks and clients unlikely noticing that their connection is not secure, there is a need for authentication scheme that can guarantee integrity of code and content presented to the client.

 Finally network must be able to identify malicious actors and easily distinguish them from real users to prevent DOS attacks while maintaining censorship resistance.

## Response Interface

The default interface used as response

```
{
    "block": <UInt64>,       // Block number
    "timestamp": <UInt64>,   // UTC Unix Timestamp
    "response": [Array],     // Array containing single or many objects
    "next": <Int32>,         // Next page number used for response pagination
    "error": {Object},       // Null or Object if exception was thrown
    "signature": "String"    // ed25519 signature
}
```

_NOTE: Full node should never accept a request that is larger than the `max_request_size`. Finally client should never accept a response larger then `max_response_size`_

### Block & Time

The `block` and `timestamp` fields enable user to identify if information provided by the node is up to date or valid but potentially expired data is presented.

### Result

The `response` field contains a result of the query or response to the request, the expected response should always be an Array of objects.

### Pagination

The `next` field implies the next page of results that user can query. If the `next` field is negative it implies that only a single page is present. If the `next` field is 0 it implies that the last page was returned.

### Error

The `error` field is an object indicating that response computation resulted in error or that request was invalid. Error field should be set to `null` if the response was successful. Error responses do not have to be signed.

#### Error Interface

The default interface used as error response

```
{
    "code": <UInt32>,       // Globally identifiable error code
    "message": "String",    // Informative error message that user can see 
    "debug": "String"       // Full code autopsy
}
```

_NOTE: Debug field is only present if specific flag is set in the full node configuration file_

### Signature

The `signature` is a [KIP_4](../iteration-0/kip_4.md) registered `ed25519` type key signature. Presence of the signature enables user to verify integrity of the response and if it is coming directly from the trusted full node. Full details on how signature is created and how it can be verified can be found in the [KIP_8](../iteration-0/kip_8.md)



