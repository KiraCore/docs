# [⏎](README.md#Roadmap) KIP_27
> List Token Aliases

By using `query_aliases` and function it must be possible to list all token aliases. Both CLI and RPC should be modified to facilitate following functionality:
* query single or multiple aliases by a single or multiple denoms

Frontend might not want to fetch all aliases in case where the list grows too much, in such case it must be possible to define parameter `denoms` and explicitly specify denom of each token of which alias must be returned.

If `denoms` property is NOT specified, response of the `query_aliases` command should be array of alias objects.

If `denoms` property is specified, response of the `query_aliases` command should be a dictionary of object containing a denom as key and alias object containing all detailed properties.

## All Aliases Query Example

```
[{
    "symbol": <string>,
    "name": <string>,
    "icon": <string>,
    "decimals": <int>,
    "denoms": [ <string>, ... ]
}, ... ]
```

## Query Aliases by Denoms

```
{
    "<denom>": {
    "symbol": <string>,
    "name": <string>,
    "icon": <string>,
    "decimals": <int>,
    "denoms": [ <string>, ... ]
    }
}
```

