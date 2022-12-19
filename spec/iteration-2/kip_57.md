# [⏎](README.md#Roadmap) KIP_57
> INTERX Signature Verification

INTERX responses contain `sr25519` signature included in the response headers. That signature should be verified against a pre-configured by user public key which identifies the node (INTERX signer key). 

All responses with exception for INTERX specific endpoints such as `/health` should always be verified before being presented to the user. If the response is invalid the user should be informed about it and communication with the node stopped. 

# Response Verification

When the JSON response is created fields `chain-id`, `block`, `block_time`, `timestamp` and `response` are signed where `response` is replaced with BLAKE2 hash before the signature is generated.

### Example Message Ready To be Verified

```
{"chain-id":"kira-1","block":12345,"block_time":231987654321,"timestamp":  231987654323,"response":"0x123456789234567812345678"}
```
## Logic Flow

1. Read message from the node
2. BLAKE2 hash `reponse`
3. Read response headers to get `chain-id`, `block`, `block_time` & `timestamp`
4. Combine all data to create a minified json similar to the one from the example
5. Verify signature using `sr25519`

# Design & Configuration

User must have ability to define one or more public keys he trusts along corresponding node addresses which can be either domains or IP addresses. If response verification fails for one node or other connectivity issue occur, the frontend should automatically switch to another node set in his configuration.

