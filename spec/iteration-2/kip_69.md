# [⏎](README.md#Roadmap) KIP_69
> Network Visualizer

Using Dart/Flutter/Figma create a responsive 3D, 2.5D (preferably) or a 2D network visualizer (visual explorer) showcasing connectivity between decentralized P2P clients (nodes) in the KIRA Relay Chain (KIRA Hub). Explorer must be designed in a way to enable easy identification of potential vulnerabilities and congestion/centralization of nodes in specific regions of the world. In other words the most essential is visualization of the [Network Topology](https://en.wikipedia.org/wiki/Network_topology). We will use IP address ranges to identify country where nodes are located, ping (handshake) time to identify relative distance between nodes.

Core Features
* Full List of all Public Nodes + search by Node ID or IP by highlighting the item on the list and visualization map
* Visualization of active connections between nodes (e.g. more pronounced lines highlight on click)  
* Unique visualization of different client types (e.g. seed,sentry,priv_sentry,validator) based on the port number
* Unique visualization of the public and private nodes (based on local and public IP's)
* Responsive interaction with visualized nodes, action on click or hover should enable to preview details
* Live visualization of the block production (block proposer, round, block number and signers) - see `/consensus` endpoint
* Scalable yet precise overview with growing number of nodes (from dozens to thousands of items)
* Desktop and Mobile friendly versions

_NOTE: In the design take into account that each node (server) can have hundreds of outbound connection and inbound connections. There is further no limit to how many nodes there can be in the network._

Design Example References:
* Cosmos 2.5D: https://mapofzones.com/?period=24&zone=cosmoshub-4
* Cosmos 3D: https://nylira.net/3d
* Dfinity 3D: https://www.dfinityexplorer.org/#/datacenters
* Polkadot 2D: https://telemetry.polkadot.io/#map/Kusama

To acquire API data a mock string designed according to the [KIP_66](./kip_66.md) can be used.

