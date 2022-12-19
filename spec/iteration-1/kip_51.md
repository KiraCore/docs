# [⏎](README.md#Roadmap) KIP_51
> Deployment and Management Scripts

Deployment and management scripts must be created, that will enable ease of operating validator, full nodes and sentry nodes by using a single console tool called `kira`. To ensure consistency we will only support a single operating system - [Ubuntu 20.04 LTS](https://releases.ubuntu.com/20.04/) and all scripts will be written using [bash](https://en.wikipedia.org/wiki/Bash_(Unix_shell)).

This KIP must be tested using [VMWare Workstation 15.5+](https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html). After installation of Ubuntu, developer/tester should zip and backup all the VM files for the ease of instant recovery.

## Abstract

1. User installs Ubuntu 20.04
2. User opens terminal and logs in as sudo
3. User executes a command that will setup the environment by downloading setup file from github or other source, check integrity of the file, start it and install all essential dependencies
4. Setup scrip will further download and install `kira` management tool
5. By typing `kira` in the terminal user will have ability to deploy, scale and manage his infrastructure

## Setup

It must be assumed that a fresh Ubuntu release will not contain many essential tools such as curl or git. We have to create a script that will install all essential dependencies along `kira` management tool. The setup script must be possibly short and only reference and download all other dependencies and deployment script.

Integrity of the script must be verifiable by the user before it's execution (SHA256 check). All the necessary setup commands must be compressed into a single line of code that can be easily copied and pasted into the terminal.

### Example Setup Command

```
sudo -s

cd /home/$SUDO_USER && \
 rm -f ./setup.sh && \
 wget <URL_TO_THE_SCRIPT> -O ./setup.sh && \
 chmod 555 ./setup.sh && \
 echo "<SHA_256_CHECKSUM> setup.sh" | sha256sum --check && \
 ./setup.sh
```

## Installation

All components of the infrastructure will be deployed within the docker containers, for that reason we have to pre-install latest version of docker.

Every time a machine is re/started all containers and a docker network interface should be re/started so that all software components are isolated and any possible networking issues are eliminated (it is common for docker networking to break during restarts or unexpected shutdowns)

Furthermore a local docker container registry should be created to preserve all images that are later deployed in one of the subnets.

### NetworkManager restart script example
```

WAKEUP_ENTRY="#!/bin/sh
case \"\$1\" in
    resume)
        echo \"INFO: Restartin docker network manager...\"
        systemctl restart NetworkManager docker || echo \"ERROR: Failed to restart docker network manager\"
esac
exit 0"

    WAKEUP_SCRIPT="/usr/lib/pm-utils/sleep.d/99ZZZ_KiraWakeup.sh"
    cat > $WAKEUP_SCRIPT <<< $WAKEUP_ENTRY
```
### Containers restart script example
```
CONTAINERS=$(docker ps -a | awk '{if(NR>1) print $NF}' | tac)
for CONTAINER in $CONTAINERS ; do
    $KIRA_SCRIPTS/container-restart.sh $CONTAINER
done

# container-restart.sh
id=$(docker inspect --format="{{.Id}}" $1 2> /dev/null)
docker container restart $id || echo "ERROR: Failed to restart container $1 ($id)"
```

## Networking

Each part of the infrastructure should have a dedicated subnet:

```
KIRA_REGISTRY_SUBNET="10.0.0.0/8"
KIRA_KMS_SUBNET="10.1.0.0/8"
KIRA_VALIDATOR_SUBNET="10.2.0.0/8"
KIRA_SENTRY_SUBNET="10.3.0.0/8"
KIRA_FRONTEND_SUBNET="10.4.0.0/8"
```

Docker registry should have ip `10.0.0.1` and operate on port `5000`
KMS node should have ip `10.1.0.1`
Validator node should have ip `10.2.0.1`
Sentry nodes should have ip's `10.3.0.X`
A static (frontend) page should have ip `10.4.0.1` and operate on port `80`

### Subnet creation example

```
docker network rm regnet || echo "Failed to remove registry network"
docker network create --subnet=$KIRA_REGISTRY_SUBNET regnet
```

## Architecture

We have to provide three types of deployment options
1. Demo Mode (local testnet)
2. Full Node Mode
3. Validator Mode

### Demo Mode

Primary use case of the Demo Mode is to facilitate ease of testing latest releases of the software for non sophisticated users. Scripts should deploy a single validator node along INTERX service and frontend application, all within independent containers.

#### Deployment Flow
0. `kira` tool installation
1. User selects Demo Mode and then quick setup or advanced
    * In the quick setup master branches of all repositories are used
    * In the advanced setup user can select branches of each repository
2. Docker images are built and uploaded to the local registry
3. Genesis file is created along three pre-configured demo accounts containing basket of tokens. (seed words must be known)
4. Validator node should be started using one of the pre-configured accounts
5. INTERX service should be setup using one of the pre-configured accounts as faucet
6. Static frontend page should be created using NGINX
7. Proper network routing should be ensured along exposure of all necessary ports to enable communication with the gRPC, RPC, P2P, INTERX and frontend

_NOTE: There is no requirement in Demo Mode for any type of hardening, all ports should be exposed._

_NOTE: Public key used to sign INTERX responses should be further registered in the signer registry as per KIP_4 spec_

#### Architecture Overview

![picture 1](https://i.imgur.com/qHjRU9a.png)  


### Full Node Mode

Full Node mode should enable users communication with the localnet, testnet/s or mainnet. One of the core features of this mode should be ease of creating snapshots and syncing from the backup. Just like in case of Demo Mode an INTERX service should be started along the full node to enable frontend ease of communication with the backend.

#### Deployment Flow
0. `kira` tool installation
1. User selects Demo Mode and then quick setup or advanced
    * In the quick setup master branches of all repositories are used
    * In the advanced setup user can select branches of each repository
2. User imports or provides URL to genesis file and seed node/s
3. User imports singer key or uses default (auto-generated)
4. User imports faucet key or uses default (auto-generated)
5. Proper network routing should be ensured depending on the user choice
   * Unsafe - all ports open
   * Sentry - outbound only traffic except P2P
   * Public - INTERX open, outbound only except P2P

_NOTE: In the Full Node Mode hardening of all networking should be ensured, we should also enable verification _

#### Architecture Overview

![picture 2](https://i.imgur.com/xglPSu8.png)  

### Validator Mode

Validator mode should enable users to easily deploy their validator node on the testnet or mainnet. In this mode we must ensure highest level of security and safe key management (enable KMS). In case of networking a sentry architecture has to be used where one full one is connected internally with a validator and trusted seeds, and another available publicly - exposing INTERX but no RPC or P2P.

#### Deployment Flow
0. `kira` tool installation
1. User selects Validator Mode and then quick setup or advanced
    * In the quick setup master branches of all repositories are used
    * In the advanced setup user can select branches of each repository
2. User imports or provides URL to genesis file and seed node/s (option to verify hash of the genesis file must be provided)
4. For the private sentry - User imports singer key or uses default (auto-generated)
5. For the public sentry - User imports singer key or uses default (auto-generated)
6. For the public sentry / INTERX - user imports faucet key or uses default (auto-generated)
7. Proper network routing should be ensured depending on the user choice
   * Private - outbound only traffic except P2P, no public sentry
   * Public - INTERX open, public sentry enabled

#### Architecture Overview

![picture 3](https://i.imgur.com/Vqs7DNl.png)  

## Management

One of the important aspects of the `kira` service should be container management, health checks in each container, access to all logs (dump), state snapshots, recovery form snapshots and live access to resources utilization statistics such as CPU, RAM and disk space. 

### Container Management

Container management should include ability to inspect any existing container, stop, restart, dump logs and other log info in case of deployment failure.

### Logs

We have to ensure that logs dump to not overflow the disk space (log size limit), we should also consider use of ELK or Graylog+Grafana to enable easy management of logs and events.

### Resources Utilization

Live resource utilization info should be displayed directly in the `kira` console tool, it should also be logged to that monitoring tools can tract peak traffic.

### Backups

When making backups of the blockchain state a sekai app must be halted in a graceful manner otherwise db will be corrupted and recovery from such snapshot would not be possible 

## Security

Local Docker Registry should only be available within the docker network to enable deployment of containers. It should be otherwise fully isolated from the the public internet. 




