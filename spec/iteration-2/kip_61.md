# [⏎](README.md#Roadmap) KIP_61
> Query Transactions

Crate a new tab or view similar to validator node explorer where it is possible to query transactions by their hash (single tx) as well as query all transactions within given block (list of many tx'es) - that is query by block height and block hash.

By default the view should display real-time list of latest blocks and all relevant information's such as height, time, nr of transactions etc. If the list is expanded it should transition into the list of transactions.

We can also have an option where current block is shown live (and all transactions within)

_NOTE: It must be possible to easily distinguish between transactions which failed and those that succeeded (it might be required to generate failed tx'es to test that behavior). It would be ideal if different transaction types were visually distinguishable (interx should contain endpoint returning all tx types available)_
