# [⏎](README.md#Roadmap) KIP_70
> Identity Registrar

Create a registrar enabling for storing and verifying public identity information as user defined set of Key-Value Pairs.

All keys and values should be data agnostic with following limitations:
* Keys can only contain latin lowercase alphanumeric characters `0-9`,`a-z` and two special characters `_`, `-`.
* Key length can't exceed 128 characters / Bytes
* Values should be limited to 2048 characters / Bytes 

Structure of the records should correspond to the following json:

```
[
    {
        "account": <string>, // kira account address associated with the record
        "records": [
            {
                "id": <number>, // integer key identifier
                "<key-name>": "<string>",
                "date": <integer>, // unix timestamp when the record was last created / modified
            },
            { ... }, ...
        ],
        "verifiers": [
            { 
                "address": "<string>", // kira address vouching for the records
                "records": [ 1, 2, 4 ... ] // records vouched for by the verifier
            },
            { ... }, ...
        ]
    }
]
```

In order to verify a specific record or a set of records user should submit a `recordsVerify` transaction and the verifier should accept it by sending `approveRecords` transaction which includes transaction hash of the `recordsVerify` tx.

Following properties should be included in the `recordsVerify` transaction:
* Address of the verifier
* Array of record-key id's to approve / verify
* Tip (in KEX) that will be paid to the verifier if the record is accepted

To create a new record or modify existing one user should submit a `createRecord` transaction which includes `key-name` and `value`. If the record is modified all exiting verifier approvals associated with the record should be removed.

It must be ensured that in case of state exports genesis includes all identity registrar records.