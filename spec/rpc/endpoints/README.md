# Endpoints

List of external and internal RPC endpoints that client applications can access.

## External

External endpoints are available to clients that are authenticated (Private endpoints) or not authenticated (Public endpoints). External endpoints are available only in sentry mode and can be accessed by non-trusted client applications that are likely to cause full-node failure (e.g. UX).

* **GET** => [/txs/{hash}](txs.md#GET) - Search transactions by hash
* **GET** => [/bank/balances/{address}](bank.md) - Query Account balances 
* **POST** => [/txs](txs.md#POST) - Broadcast transactions

## Internal

Internal endpoints are available to clients that are authenticated (Private endpoints) or not authenticated (Public endpoints). External endpoints are available only in non-sentry mode and can be accessed only by trusted applications that are likely to cause full-node failure if abused (e.g. Monitoring Tools).


