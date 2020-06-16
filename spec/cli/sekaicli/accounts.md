# [⏎](README.md) Account Management 

## Query Account

The `query account` command allows to check if account exists, that is view `sequence` and `account_number`

_NOTE: You will only be able to query account balance if our account has or had non-zero amount of tokens deposited_

> Command Example
```
ACCOUNT_NAME="test-1" && \
 KEYRING_PASSWORD="1234567890" && \
 ACCOUNT_ADDRESS=$(echo "$KEYRING_PASSWORD" | sekaicli keys show $ACCOUNT_NAME -a) && \
 sekaicli query account $ACCOUNT_ADDRESS
```

<details> 
    <summary>Output Example</summary>
    <pre>kira1ufak8sc7g6w7pnlmalq9adqmj7cktcrk073ctz</pre>
</details>

## Transfer Tokens

```
FROM_ACCOUNT="test-1" && \
 KEYRING_PASSWORD="1234567890" && \
 TO_ADDRESS="kira1l35kjmuupwhn4tevfm4ykj9hgrfvmpwjazpqft" && \
 AMOUNT="1000ukex" && \
 FEES="15ukex" && \
 sekaicli tx send $FROM_ACCOUNT $TO_ADDRESS $AMOUNT --fees=$FEES --yes --output json << EOF
$KEYRING_PASSWORD
$KEYRING_PASSWORD
EOF
```
<details> 
    <summary>Output Example</summary>
    <pre>{"height":"0","txhash":"85D45F3685D4389FDEA03EC6DF9C53F3EB6C89574770A919E35996CFB6E9C912","raw_log":"[]"}</pre>
</details>
