# [⏎](README.md#Roadmap) KIP_63
> KIRA Network Manager - Sentry Mode

Add Sentry Mode to the KIRA Network Manager by reusing similar deployment system as for the Validator Mode with the difference that there is no "validator" container and no "kiranet" network. Instead there is a new "seed" container (in the "sentrynet" network) which is simply a full node with appropriate flag set, gathering information about other nodes in the network and disconnecting from them right after.

## Architecture Overview

![picture 1](https://i.imgur.com/qR1BjYB.png)  

In the setup there will only be an option to join existing network without option to create new one as in the validator mode. Both advanced and quick setup must be operational. In the quick setup it must be possible to join the network using IP alone with automatic snapshots and genesis discovery just like in the validator mode.

When it comes to main application UI there will be new configuration regarding seed container port in the networking manager, also appropriate firewall rules must be added and all has to be tested.

## Test Setup

![picture 3](https://i.imgur.com/i8J3HBj.png)  

Four instances will be provided for testing, we should run 2 validators, and 2 sentries. Network should be setup in the way that public sentry node should not be aware of any of the validators (see address book file for clues). If any of the public sentry containers becomes aware of any of the validator IP's then those validators could be DDOS'ed or otherwise attacked.

Note: Create new branch from master without any source code changes before the work starts.
