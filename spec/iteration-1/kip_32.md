# [⏎](README.md#Roadmap) KIP_32
> Deposits & Withdraws Query 

INTERX has to enable ease of query all deposit and withdraw transactions so that they can easily be presented on the frontend side. The `/deposits` and `/withdraws` endpoints should be created and return a dictionary of keys as tx hashes and values as object containing further details.

It should be possible to query all transaction types or specify only specific ones as query parameter (as per `KIP_31` metadata). 

Additionally request parameters should contain a page number in form of the last hash from the previous query as well as maximum number of results that should be returned. The maximum number of results must be within `1` and `1000`.

Results should be always ordered by date in the descending order.

## Example Request

```
/withdraws?account=kira1FFF...FFF&type=all&max=10&last=0xAAA...AAA
```

## Example Response

```
{
    "0xBBB...BBB": {
        "time": <Uint64>,
        "txs": [
            {
               "type": <string>,
               "address": "kira1XXX...XXX",
               "amount": <Uint64>,
               "denom": <string>
            }, { ... }, ...
        ]
    },
    "0xCCC...CCC": { ... }, ...
}
```

_NOTE: `account` should be a withdraw or deposit account depending on the endpoint type (if `/withdraws` then `account == destination`, if `/deposits` then `account == source` )_
