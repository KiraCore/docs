# [⏎](README.md#Roadmap) KIP_13
> Trusted Signatures Whitelist

Frontend application needs ability to verify integrity of information provided by full node operators without a need for lite clients or forcing clients to operate full node on their own. This is essential to ensure user is communicating with a real network and not a malicious mock of the network due to hijacked DNS record.

[KIP_8](./kip_8.md) introduces a JSON-API service capable of cryptographically signing responses. We have to provide user ability to verify those signatures and identify if the node that user is communicating with can be trusted. 

## Settings

A settings view with "security" sub-page must be created where user can define list of public keys (kira accounts) he trusts. 

Frontend must query signing keys by using kira account as a query parameter and fetch all data related to the signing keys submitted by that account.  The `data` field from the query repose must contain following information in the JSON string format that can be parsed:

```
{
    "node-id": <string>,
    "address": <string> // http or https url
}
```

If the data field can't be properly parsed or url is invalid the node should not be used by the frontend. To verify if the node is responsive frontend should await response using `/health` endpoint.

The `/health` endpoint (in the INTERX) must be implement in a way that if full-node is healthy (is updating blocks every expected period of time) the response should be `200 OK`, if the full node is not updating blocks or last block time is longer than expected period of time the response should be `500 Bad Gateway` with error message `Stale block, last update <block_time>, height <height>.`. To verify if the full node is healthy we should check the node status which contains block time and height once every configurable period of time.

## Verification

Every response from the INTERX service (except for the `/health` and other INTERX specific endpoints) should be verified against a pre-configured public or previously queried signing key.

To verify INTERX response a `data` field must be replaced with a BLAKE2 hash of the data. Signatures should be verified using `sr25519` 
