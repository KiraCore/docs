# [⏎](README.md) Account Key Management

Management of the locally stored account keys

## Create Key

The `keys add` command allows you to add new account where coins can be deposited. To execute this command you will have to specify account name and [keyring](https://web.archive.org/web/20200616113125/https://github.com/cosmos/cosmos-sdk/blob/2be38d0304d4c269671d6aefeeffbb7af127b234/docs/interfaces/keyring.md) password

> Command Example

```
ACCOUNT_NAME="test-x" && \
 KEYRING_PASSWORD="1234567890" && \
 sekaicli keys add $ACCOUNT_NAME << EOF
$KEYRING_PASSWORD
$KEYRING_PASSWORD
EOF
```

<details> 
    <summary>Output Example</summary>
    <pre>
- name: test-x
  type: local
  address: kira1agqjyfkmtg86qddpc7a7d2dchaxtxf7p42j5ls
  pubkey: kirapub1addwnpepq2pvgw2mg89gejysdlfh4tvxw76keq3qkfxsnmd8szrp79wvlctw6n7ke47
  mnemonic: ""
  threshold: 0
  pubkeys: []

**Important** write this mnemonic phrase in a safe place.
It is the only way to recover your account if you ever forget your password.

pipe message gas space disorder scissors left hotel remind trash caution exit twenty virtual trash style pyramid spread awake silent spin combine expand spirit
    </pre>
</details>

## List Keys

The `keys list` allows to list details of all account keys that you created
> Command Example
```
KEYRING_PASSWORD="1234567890" && \
 echo "$KEYRING_PASSWORD" | sekaicli keys list
```

<details> 
    <summary>Output Example</summary>
    <pre>
- name: test-1
  type: local
  address: kira1ufak8sc7g6w7pnlmalq9adqmj7cktcrk073ctz
  pubkey: kirapub1addwnpepqg3e7d4w6z28dlke7n9cjjydu4yqgwlnwh5gcyjgzn4wnpj4n96ysx6yr98
  mnemonic: ""
  threshold: 0
  pubkeys: []
- name: test-x
  type: local
  address: kira14fx5q9su3h2ptevmxv7y3lnmn07dfdkdlujdd9
  pubkey: kirapub1addwnpepq0dvc57parg4fjeacksq85yactfjnl7ya68vuuyq886xl095m6fpgv8hc79
  mnemonic: ""
  threshold: 0
  pubkeys: []

</pre>
</details>

## Show Key

The `keys show` allows to display public key of your account

> Command Example
```
ACCOUNT_NAME="test-1" && \
 KEYRING_PASSWORD="1234567890" && \
 echo "$KEYRING_PASSWORD" | sekaicli keys show $ACCOUNT_NAME -a
```
<details> 
    <summary>Output Example</summary>
    <pre>kira1ufak8sc7g6w7pnlmalq9adqmj7cktcrk073ctz</pre>
</details>

## Delete Key

The `keys delete` removes existing account

> Command Example
```
ACCOUNT_NAME="test-1" && \
 KEYRING_PASSWORD="1234567890" && \
 echo "$KEYRING_PASSWORD" | sekaicli keys delete $ACCOUNT_NAME --yes
```
<details> 
    <summary>Output Example</summary>
    <pre>Key deleted forever (uh oh!)</pre>
</details>