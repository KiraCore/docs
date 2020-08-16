# [⏎](README.md) Account Key Management

Management of the locally stored account keys

## Create Key

The `sekaid keys add` command allows to add new account where coins can be deposited. To execute this command you will have to specify account name and [keyring](https://web.archive.org/web/20200616113125/https://github.com/cosmos/cosmos-sdk/blob/2be38d0304d4c269671d6aefeeffbb7af127b234/docs/interfaces/keyring.md) password

<details> 
    <summary>Command Example</summary>
    <pre>
ACCOUNT_NAME="test-x" && \
 KEYRING_PASSWORD="1234567890" && \
 sekaid keys add $ACCOUNT_NAME << EOF
$KEYRING_PASSWORD
$KEYRING_PASSWORD
EOF
</pre>Output Example<pre>
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

The `sekaid keys list` allows to list details of all account keys that you created
<details> 
    <summary>Command Example</summary>
    <pre>
KEYRING_PASSWORD="1234567890" && \
 echo "$KEYRING_PASSWORD" | sekaid keys list
</pre>Output Example<pre>
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

The `sekaid keys show` allows to display public key of your account

<details> 
    <summary>Command Example</summary>
    <pre>
ACCOUNT_NAME="test-x" && \
 KEYRING_PASSWORD="1234567890" && \
 echo "$KEYRING_PASSWORD" | sekaid keys show $ACCOUNT_NAME -a
</pre>Output Example<pre>
    kira1ufak8sc7g6w7pnlmalq9adqmj7cktcrk073ctz</pre>
</details>

## Delete Key

The `sekaid keys delete` removes existing account

<details> 
    <summary>Command Example</summary>
    <pre>
ACCOUNT_NAME="test-x" && \
 KEYRING_PASSWORD="1234567890" && \
 echo "$KEYRING_PASSWORD" | sekaid keys delete $ACCOUNT_NAME --yes
</pre>Output Example<pre>
   Key deleted forever (uh oh!)</pre>
</details>

## Export Key

The `sekaid keys export` allows to export and encrypt key with custom password

<details> 
    <summary>Command Example</summary>
    <pre>
ACCOUNT_NAME="test-x" && \
 KEYRING_PASSWORD="1234567890" && \
 ENCRYPT_PASSWORD="1234567890" && \
 OUTPUT_FILE=./your-file-name.key && \
sekaid keys export $ACCOUNT_NAME -o text > $OUTPUT_FILE 2>&1 << EOF
$ENCRYPT_PASSWORD
$KEYRING_PASSWORD
EOF
</pre>Output Example (cat $OUTPUT_FILE)<pre>
-----BEGIN TENDERMINT PRIVATE KEY-----
kdf: bcrypt
salt: 46A10EAC224C0510B6095A13FC661F2E
type: secp256k1

lvyIiFCZBGw5uXUjI7Nt/qZ9I8fhMTSUsXJ9wRoZQoUDC/rf29ezLrZtliYqpjV7
46bK4AsbE2Ek98e2/xN+Tt5Y/VAjVywKRtDqQpA=
=bwi0
-----END TENDERMINT PRIVATE KEY-----
</pre>
</details>

## Import Key

The `sekaid keys import` allows to import and key encrypted key with password

<details> 
    <summary>Command Example</summary>
    <pre>
ACCOUNT_NAME="test-x" && \
 KEYRING_PASSWORD="1234567890" && \
 DECRYPT_PASSWORD="1234567890" && \
 INPUT_FILE=./your-file-name.key && \
sekaid keys import $ACCOUNT_NAME $INPUT_FILE << EOF
$DECRYPT_PASSWORD
$KEYRING_PASSWORD
$KEYRING_PASSWORD
EOF
</pre>
</details>

