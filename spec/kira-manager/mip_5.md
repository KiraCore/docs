# [⏎](README.md#Roadmap) MIP_5
> Infrastructure Setup 

As per **MIP_4** a `/setup` endpoint should provide all details required to fully deploy KIRA Infrastructure. Depending on whether or not new network will be created KM must ensure that a proper genesis file is either generated or imported as well as all essential parts of the infrastructure correctly launched.

All deployment modes (seed/sentry/validator) should match the following schematic:

![picture 2](https://i.imgur.com/LL3AdQK.png)  

Default KIRA Ports

* P2P
  * Internal: `26656`
  * External: `X6656`
* RPC
  * Internal: `26657`
  * External: `X6657`
* PROMETHEUS
  * Internal: `26660`
  * External: `X6660`
* KIRA Frontend: `80` (external)
* KM Frontend: `9000` (external)
* SSH: `22` (external, user defined)
  
Depending on the KIRA node type `X` should be replaced with `1` for seed nodes, `2` for sentry nodes and `3` for validator nodes.

All other ports such as Docker Registry `5000` should remain internal.

Before build and deployment of any docker containers can occur a `firewalld` must be setup in order to facilitate exposure of external ports as defined in the above default ports list. To apply changes after a successful firewall setup instance should be gracefully restarted.

Example of the `firewalld` setup can be found [here](https://github.com/KiraCore/kira/blob/master/workstation/networking.sh).

Next step is deployment of the Docker Registry and build of all images relevant to the KIRA Infrastructure stack. All the images stored in the registrar should inherit their source from the "base image". The base image should contain all dependencies shared between all containers to speed up the build process. 

... TO BE CONTINUED

