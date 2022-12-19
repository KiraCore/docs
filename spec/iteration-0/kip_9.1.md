# [⏎](README.md#Roadmap) KIP_9
> Authentication

Account authentication is required to further implement rate limiting support external dID and provide flawless experience on the UI side.

The authentication process will consist of two parts.
* Checking address ownership & balance 
* Storing session information

## Logic Flow

1. Client creates a signer key as per KIP_4
2. Client submits request to the INTERX with a session header
3. INTEREX checks the signature, and account balance of the corresponding account to generate a local session record used to identify the connection

## Execution

It should be guaranteed by the KIP_4 that all submitted singer keys are unique. The KIP_4 further maps account ownership to the specific signer keys acting as the hot-key for proving ownership. Thanks to signer keys client identity can be proven and account balance checked without need for signing any message using hardware wallet.

The frontend application is expected to send `kira_session` headers in each requests as defined by the KIP_18. The INTERX service must verify integrity of the signature and query blockchain state to identify the key owner. Furthermore `block_hash` should be checked and how long time ego the block was created. If the `block_hash` originates from the block older than `X minutes` we should fail the authentication.

If identity is successfully verified, then a local session json key-value record should be stored in RAM memory with key as the kira `address` and contain further details regarding client connection.

### Session Record Example

```
{
    "session_pubkey": "<string>",
    "session_hash": "<string>",
    "first_request" <unix-tmiestamp> // UTC time of session record upsert
}
```

The `session_hash` is a hash of the `kira_session`. There is no need to verify signature if `address` is present in the session store and contains a corresponding `session_hash`.

Session records should only be stored in case of successfully authentication. Failed authentication should result in request being rejected with `401` status code. We have to further remove stale session records (older than `X minutes`) and define maximum size of the session store dictionary in the memory to prevent overflow of the RAM memory. 

_NOTE: In the future KIP's we will enable temporary blocking of IP addresses which fail authentication multiple times_
