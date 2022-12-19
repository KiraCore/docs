# [⏎](README.md#Roadmap) KIP_10
> Network Setup & Login

Before user can begin interaction with the frontend application the web-app must be initialized using demo account and URL to the demo node providing JSON-RPC that the web app will use to communicate with the blockchain application (backend). While working with [Kira Manager](/infra/setup.md) and the test environnement and for the purpose of the PoC the default JSON-RPC URL should be set to `101.1.0.1:10002/node_info`

## Overview

```
-----------------------     -----------------------------     ---------- ---
|   Chrome / Firefox  |     |   Github Pages | AWS S3   |     |  Full Node |
| Browser Application | <=> | Statically Hosted Web-App | <=> |  JSON-RPC  |
-----------------------     -----------------------------     --------------
```

The website will be accessible on the [interchain.exchange](https://interchain.exchange) domain and must be a single page, progressive web app that can be hosted directly from [GitHub Pages](https://pages.github.com/) or via [S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) 

## Login

Before accessing the website, user should be presented with a "login" window/view where he would be able to define following information:

* Network Id (Dropdown)
* Create New Account (Button)
* Login With Mnemonic (Button)
* Login With Key File (Button)

In the PoC we will be supporting software signing of transactions only, this implies, that the user must provide a [Bip39 mnemonic](https://github.com/bitcoinjs/bip39) or receive one auto mnemonic generated while creating new account.

There must exist "advanced options" where users could input trusted RPC URL

Login view should only be presented to the users if their existing account is not cached yet or in the case where user looses connection with the full node / lite client. In such case a login screen should be presented with option to change the Node Id or specify different RPC URL.

### Network Id
Network Id must be a part of the login view and can be obtained through [/node_info](/spec/rpc/endpoints/status.md) endpoint. There also must be a status indicator specifying if the node is healthy. To determine health of a node a `/syncing` endpoint in the combination with the `/blocks/latest` can be used. If node is syncing or latest block was created more then a minute ago then node can be determined not healthy.

## Create New Account

If user does not specify the mnemonic a new one has to be created using [cryptographically secure random generator](https://api.dart.dev/stable/2.9.0/dart-math/Random/Random.secure.html)

Mnemonic of the user should have at leas 12 words and be protected by the passphrase. Mnemonic encrypted with the [BLAKE2 hash](https://github.com/dcposch/blakejs) of the user password can be then stored as cookie or otherwise cached to preserve user session.

There must be an option to export encrypted key in the form of a json with following structure:
```
{
    "version":"v0.0.1",
    "algorithm": "AES-256",
    "pubkey": "kiraXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    "privkey": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX...",
    "data": ""
}
```

_NOTE: Data is an optional field that will contain all user preferences_

## Login With Mnemonic

Users that have existing mnemonic must have ability to provide the secret words by inputting them within a textbox field. Once seed words are provided they should be encrypted in the same way as in the case of new account creation with key export option. 

## Login With Key File

In case of the key-import a drag & drop method or file picker can be used. Application should cache all the key files and further allow to select one of them. In the key-file login view users should also have ability to remove cached key files.

_NOTE: This option should enable ease of switching between different account types_

## Logout 

Account view/tab must be created where user must have ability to "log-out" - which implies dropping key-store encryption key and redirecting to the login tab. Account view/tab must contain current Network Id with status and corresponding RPC URL. It would be ideal if within Account view/tab user had ability to change the network id as well as RPC URL.

_NOTE: Account can be the first view where user is redirected after login (in the case of the first fronted related KIP)_



