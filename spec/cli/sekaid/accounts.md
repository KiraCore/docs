# [⏎](README.md) Account Management 

Token holder account management

## Query Account

The `sekaid query account` command allows to check if account exists, that is view `sequence` and `account_number`

_NOTE: You will only be able to query account balance if our account has or had non-zero amount of tokens deposited_

<details>
<summary>Command Example</summary>
<pre>
ACCOUNT_NAME="test-1" && \
 KEYRING_PASSWORD="1234567890" && \
 ACCOUNT_ADDRESS=$(echo "$KEYRING_PASSWORD" | sekaid keys show $ACCOUNT_NAME -a) && \
 sekaid query account $ACCOUNT_ADDRESS
</pre>
Output Example
<pre>{"height":"0","txhash":"788794B2D607D0963CB4FA9A2978B2FCDEC3A0781590301951B3BFE79E983073","raw_log":"[]"}
</pre>
</details>

## Transfer Tokens

The `sekaid tx send` allows to transfer tokens between two different accounts

<details> 
    <summary>Command Example</summary>
    <pre>
FROM_ACCOUNT="test-1" && \
 KEYRING_PASSWORD="1234567890" && \
 TO_ADDRESS="kira1l35kjmuupwhn4tevfm4ykj9hgrfvmpwjazpqft" && \
 AMOUNT="1000ukex" && \
 FEES="15ukex" && \
 sekaid tx send $FROM_ACCOUNT $TO_ADDRESS $AMOUNT --fees=$FEES --yes --output json << EOF
$KEYRING_PASSWORD
$KEYRING_PASSWORD
EOF
</pre>
</details>
