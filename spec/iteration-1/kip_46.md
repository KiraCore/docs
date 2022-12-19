# [⏎](README.md#Roadmap) KIP_45
> Parameterized Permissions

Each permission which corresponds to the individual function in the Functions/Permissions Registry can have individually assigned parameters that can become an input to the function that each network actor triggers when submitting a given transaction type. Thanks to parameters the permissions can be more granular - e.g. parameters can be a whitelist of addresses where a given account can send transactions, transfer limits, a minimum KEX amount that some particular user must hold in his wallet to be a validator or others values that are relevant to each individual function or transaction type that can be executed by the user.

Parameterized permissions can be applied to both individual accounts as well as roles. Individual parameterized permissions should always override the role permissions. Furthermore it should not be possible to assign more than one permission with different parameters to the same address or role. If more then one permission with different set parameters is required - the parameters should be structured as array or objects or a new record in the functions registry should be created.

We must ensure that Functions Registry contains default permissions as suggested by [KIP_45](./kip_45.md) so that if no parameters are applied when assigning a permission - the defaults should be used. If the default is changed it should however NOT apply to all already assigner permissions. This is essential to ensure that gov actors with permissions to edit functions registry can't control permissions of the entire network that uses defaults. If major changes are ever required to any particular function its better to crete a new function type rather then upgrade every single record on-chain. 

_NOTE: Blacklist of permissions can never be parameterized, only whitelist permissions can be parameterized_

To ensure that parameters can't be misplaced (order independent) when assigned we must define them as array of key-value pairs.

## Implementation

When implementing curation logic we must ensure that upsert approach is used, that means when updating parameters of a particular permission we should have option to add item of a particular key or entirely remove whole key. For example if 1000 accounts must be whitelisted as parameter in some lets say "token transfer permission" - we should have ability to add smaller baches of accounts to the given key otherwise hardware signers might not be capable to handle such large tx'es. 

To showcase use case of parameterized permissions we should implement a proposal where permission to vote contains vote types as parameters. We can than assign to certain accounts permission to vote `yes` or `no` while to others `yes`, `no`, and `veto`.

## Visualization

![picture 1](https://i.imgur.com/KpJ3Ycy.png)  
