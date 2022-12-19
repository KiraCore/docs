# [⏎](README.md#Roadmap) MIP_3
> Secure Communication Channel

All functions of the the KM must be controlled by a dedicated client facing frontend application (dart + flutter). It can NOT be guaranteed that validator will utilize HTTPS thus a secure end to end encrypted communication channel must be created between KM REST API and the frontend application.

As per MIP_1 user creates a secure password during setup which `SHA256` hash can be used to derive a public and private `secp256k1` key. While the `secp256k1` can be used to create signatures, the private key can also be used to encrypt all data served or received by the KM API using `AES 256`.

To prevent spam attacks any request from the client should contain a header with a `secp256k1` signature of a current UNIX timestamp. If the timestamp is older then 1 minute or if signature is not valid then IP address should be immediately rate limited and every unauthorized request should result with `HTTP 401 Unauthorized` response.


Example Request Headers:
timestamp: `1632458131`
signature: `30450....56ec46`

To test this functionality a `/status` endpoint should be created containing following fields:

* status - single word lowercase string such as `setup`, `upgrading`, `starting`, `running`, `failed`, `etc...`
* timestamp - unix timestamp (always UTC) of the current server time
* version - KM release version
* mode - deployment mode eg. `undefined`, `sentry`, `validator`, `seed`, `etc...`

_Note: To prevent reply attacks all reposes from the KM API should be signed along the server timestamp, exactly the same as in case of INTERX API._