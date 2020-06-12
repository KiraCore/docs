# TXS Endpoint

## GET

> /txs/{hash}

Allows to search transactions by specific tx hash

### Request Example
```
curl -X GET "$URL/txs/BCBE20E8D46758B96AE5883B792858296AC06E51435490FBDCAE25A72B3CC76B" -H "accept: application/json"
```

### Response Example

```
[{
    "msg": [ "string" ],
    "fee": {
      "gas": "string",
      "amount": [ {
          "denom": "ukex",
          "amount": "50"
        }
      ]
    },
    "memo": "string",
    "signature": {
      "signature": "MEUCIQD02fsDPra...+iUb6VSdtlpgY=",
      "pub_key": {
        "type": "tendermint/PubKeySecp256k1",
        "value": "Avz04VhtKJh8ACCVzlI8aTosGy0ikFXKIVHQ3jKMrosH"
      },
      "account_number": "0",
      "sequence": "0"
}}]
```

## POST

> /txs

Allows to post transactions




