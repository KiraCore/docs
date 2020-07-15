# [⏎](README.md) Order Book Management

Management of the asset exchange module

## Create Order Book

The `sekaicli tx kiraHub createOrderBook` command allows to create a custom orderbook for any particular set of two assets.

<details>
<summary>Command Example</summary>
<pre>
FROM_ACCOUNT="test-1" && \
 FEES="15ukex" && \
 KEYRING_PASSWORD="1234567890" && \
 BASE="uatom" && \
 QUOTE="ubtc" && \
 MNEMONIC="ATOM/BTC" && \
 expect -c "spawn sekaicli tx kiraHub createOrderBook $BASE $QUOTE "$MNEMONIC" --yes --fees=$FEES --from=$FROM_ACCOUNT --output json ; send \"$KEYRING_PASSWORD\r\" ; send \"$KEYRING_PASSWORD\r\"; interact"
</pre>
Output Example
<pre>
{"height":"0","txhash":"083182101D0694EE90C25B74CC129A087888D8B88A2604DDA77A8758877BDBD0","raw_log":"[]"}
</pre>
</details>

## List Order Books

The `sekaicli query kiraHub listorderbooks` command allows to query existing order books

<details>
<summary>Command Example</summary>
<pre>
ACCOUNT_NAME="test-1" && \
 KEYRING_PASSWORD="1234567890" && \
 ACCOUNT_ADDRESS=$(echo "$KEYRING_PASSWORD" | sekaicli keys show $ACCOUNT_NAME -a) && \
 sekaicli query kiraHub listorderbooks Curator $ACCOUNT_ADDRESS --output json
</pre>
Output Example
<pre>
[{"id":"482bd7d06aae98587014e2da91b6e5db","index":9,"base":"uatom","quote":"ubtc","mnemonic":"ATOM/BTC","curator":"kira1ufak8sc7g6w7pnlmalq9adqmj7cktcrk073ctz"}]
</pre>
</details>

## Create Order

> Available Order Types
* `0` - Limit Buy
* `1` - Limit Sell

### Limit Order

The `sekaicli query kiraHub createOrder` command allows to create buy or sell limit order

<details>
<summary>Command Example</summary>
<pre>
ACCOUNT_NAME="test-1" && \
 KEYRING_PASSWORD="1234567890" && \
 ACCOUNT_ADDRESS=$(echo "$KEYRING_PASSWORD" | sekaicli keys show $ACCOUNT_NAME -a) && \
 AMOUNT=1000 && \
 ORDER_TYPE=0 && \
 PRICE=9000 && \
 FEES="15ukex" && \
 ORDER_BOOKS=$(sekaicli query kiraHub listorderbooks Curator $ACCOUNT_ADDRESS --output json) && \
 ORDER_BOOK_ID=$(echo $ORDER_BOOKS | jq -r '.[0].id') && \
 sekaicli tx kiraHub createOrder $ORDER_BOOK_ID $ORDER_TYPE $AMOUNT $PRICE --fees=$FEES << EOF
$KEYRING_PASSWORD
$KEYRING_PASSWORD
EOF[flags]
</pre>
</details>


