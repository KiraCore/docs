# [⏎](README.md#Roadmap) KIP_31
> List Functions & Metadata

It must be possible to query all functions (transaction types) which can be executed along metadata such as description, accepted parameters and their type (Similar to ethereum ABI json).

## Example Structure

```
{
    "TxSend": {
        "function_id": <int>,
        "description": <string>,
        "parameters": {
            "account": {
                "type": <string>, // e.g. bool, string, int, int[], ...
                "description": <string>
            },
            "sequence": { ... }, 
            "<other_param_names>": { ... }, ...
        }
    },
    <tx_type>: { ... }, ...
}
```
