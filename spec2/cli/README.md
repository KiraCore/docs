
# Command Line Interface

The CLI is a main tool used in Kira for the purpose of the automation and testing. We should always aim to maintain it up to date with latest changes and ensure that default output is JSON formatted as well as all exceptions result in appropriate exit code's != 1.

`sekai` (Japanese for "world") is the name of the Cosmos SDK application for the Kira Hub. It should come with 2 main entry points:

* `sekaid`: The Sekai Daemon, runs a full-node of the sekai application.
* `sekaicli`: The Sekai command-line interface, which enables interaction with a Sekai full-node.


