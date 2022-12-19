# [⏎](README.md#Roadmap) KIP_64
> Global Token Transfers Freeze

Crate a governance process allowing for freezing all token transfers based on whitelist and blacklist. If whitelist is active then all whitelisted assets can be transferred freely. If blacklist is active then tokens included in the blacklist will not be transferable.

The proposal should allow for adding and removing one or many tokens at the same time to the blacklist or whitelist. Adding or removing items should however not impact whether whitelist or blacklist mode is `active` (true/false/null), that should be a separate parameter which can be changed or left unchanged.

### Example Proposal Structure

```
{
    "type": "blacklist", // or "whitelist"
    "mode": "add", // or "remove"
    "tokens": [ 
        "ubtc", 
        "ueth", ... ],
    "active": true // or false or null (default)
}
```

If tokens are frozen then user accounts should not be able to use them for any purpose with exception for KEX, which must be possible to be used as fee payments, so its still possible to vote and create governance proposals.

In this KIP an appropriate governance permissions should be created, allowing to create a "freeze assets proposal" and separate permission allowing for voting on the freeze proposal.
