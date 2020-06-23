# [⏎](README.md) Bank Module

## Query Balances

The `/bank/balances/<address>` endpoint allows to query token balances of any kira `<account>` address

<details> 
    <summary>Command Example</summary>
<pre>
ACCOUNT_NAME="test-1" && \
 KEYRING_PASSWORD="1234567890" && \
 RPC_ADDRESS="localhost:10002" && \
 ACCOUNT_ADDRESS=$(echo "$KEYRING_PASSWORD" | sekaicli keys show $ACCOUNT_NAME -a) && \
 curl "$RPC_ADDRESS/bank/balances/$ACCOUNT_ADDRESS"
</pre>
Output Example
<pre>
{"height":"2538","result":[{
    "denom": "samoleans", "amount": "10000000" }, {
    "denom": "uatom", "amount": "100000000" }, {
    "denom": "ubtc", "amount": "100000000" }, {
    "denom": "ukex", "amount": "100000000000000" }, {
    "denom": "usent", "amount": "1000000"} ]}
</pre>
</details>

