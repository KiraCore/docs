# [⏎](README.md#Roadmap) MIP_4
> Node Setup Page

Create a setup sequence page to enable user create or connect to the network and launch his KIRA node.

Before user can take any action a `login` page should be displayed where user can input his password. Frontend application should then generate `secp256k1` keys as per `MIP_3` and request response from the `/status` endpoint. If response is `401` or if response cant be `AES256` decrypted into a valid json string then appropriate error message should be shown. 

Once user is logged in - frontend should await a `running` status from the `/status` endpoint. If node is `running` and deployment `mode` is set to `undefined` then setup page should be presented to the user. In the case where status value is set to `setup` (pending) of `failed` then a progress log should be showed as explained later in the MIP. Otherwise a dashboard page should be shown.

_Note: All inputs text/buttons/etc..  should have a dedicated `(?)` button or a help text box in each view precisely explaining their functionality to the user._

## Creating or Joining the network

First step of the setup process is selecting whether new network should be created or if user wants to join a new network. Options can be represented as buttons.

### Creating New Network

If user chose to create a new network then there is only one deployment mode in which node can start (validator), in such case user should be presented with option to name his new new network. Network name must follow following requirements: 

```
* Network name can't be longer than 14 characters
* Network name can't be shorter than 3 characters
* Network name must contain single '-'
* Network name prefix must be a lowercase word (a-z)
* Network name suffix must be a number (0-9)
```

Example: `mynet-1`

If the name is not valid, user should pre presented with detailed error and not be able to proceed further with the setup.

Once the name is selected user should be presented with option to select github branches (or checkout hashes) used to deploy the new network.

Following repositories should be available for the setup:

* validator node - `sekai`, default `master`
* interx - `sekai`, default `master`

Once the branches are selected user should have an option to finalize the setup process by approving a summary of all his previous choices. 

_Note: At any point of time user should be able to go back and select different option in the setup_

### Joining Existing Network

If user chose to join existing network then he should be presented with option to select one of the following node types:

* seed
* sentry
* validator

Once deployment mode is defined user should input a trusted node address which will be used to connect his node with the existing network.

If provided node address is not an address which exposes INTERX API (port 11000) then user should be present with an error message.

Once a valid address is provided (dns or ip) then INTERX should be queried in order to establish following information:

* Does the trusted node exposes snapshot (shared resources query)
* What is the size & checksum of the snapshot (if exposed)
* What is a genesis checksum (status query)
* What is a network name (status query)
* What is the latest block height (status query)
* What are the branches of the current deployment plan (upgrade module query)

Once above data is queried user should be presented with option to accept genesis checksum, network name, branches and block height at which he will be joining the network. If any of the above can't be queried then appropriate error message should be displayed and user should input different trusted node address.

If the setup options are accepted by the user then he should be presented with following options:

* Use snapshot exposed by trusted node (if present)
* Provide URL to a snapshot
* Use snapshot auto-discovery

Once above options are selected user should be able to finalize setup.

## Finalizing Setup

To finalize setup - frontend should send AES encrypted POST request to a dedicated `/setup` endpoint with all the choices that user made in the form of a json string.

**Example Request (creating new network):**

```
HEADERS -> timestamp & contend body signature
POST -> /setup

BODY -> (AES256 encrypted)
{
    "new-network": true,
    "network-name": "mynet-1",
    "node-type": "validator",
    "resources": {
        "repository": "https://github.com/KiraCore",
        "sekai": "master",
        "interx": "master"
    }
}
```

**Example Request (joining existing network):**

```
HEADERS -> timestamp & contend body signature
POST -> /setup

BODY -> (AES256 encrypted)
{
    "new-network": false,
    "network-name": "mynet-1",
    "block-height": 12345,
    "node-type": "sentry",
    "trusted-node": "123.456.789.123",
    "resources": {
        "repository": "https://github.com/KiraCore",
        "sekai": "master",
        "interx": "master"
    },
    "genesis-checksum": "<SHA256-HASH>",
    "snapshot-checksum": "<SHA256-HASH>",
    "snapshot-source": "<URL>",
    "snapshot-discovery": false
}
```

## Setup Progress

A dedicated `/setup_logs` endpoint should be created returning a json file with a detailed, timestamped KM debug log (all processes / services) as well as start and end timestamp.

Frontend should periodically retrieve the logs and display them until `running` or `failed` status is indicated by the KM API.

If the status is `failed` then user should be presented with option to retry setup or see dashboard (in case at least some of the containers managed to launch).

If the status is `running` a success result should be shown to the user with option to continue to the dashboard.

User should also be presented with option to save logs to the file and informed how long is/was the setup ongoing. It should also be expected that the KM will periodically restart the instance during setup and frontend should notify user when that occurs and that it is an expected behavior. 