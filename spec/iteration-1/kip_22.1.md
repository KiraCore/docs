# [⏎](README.md#Roadmap) KIP_22.1
> Upsert Roles (Proposal)

Roles are a set of permissions that can be assigned to any individual address. By assigning roles to account rather then individual permissions it is possible to modify permissions of all accounts by modifying just a single role assigned to all those accounts.

The Roles Registry should have following structure: (the id and description should be added)

```
{
  "<role-name-1>": {
      "id": <uint>,
      "description": <string>
      "whitelist": [ <uint>, ... ],
      "blacklist": [ <uint>, ...]
  }, "<role-name-2>": { ... }, ...
}
```

## Implementation

A proposal should be created enabling creation and upserting of any role with any set of permission ID's as defined in the Permissions Registry. A proposal should further have a corresponding permissions enabling for raising and voting for it.

The process of adding permissions (blacklist or whitelist) to a role should occur in either `replace`, `append` or `wipe` mode. In the `replace` mode all existing permissions will be changed to a set of newly defined permissions (whitelist and/or blacklist). In the `append` mode new permissions will be added to the whitelist and/or blacklist. In the `wipe` mode the specified permissions will be removed from the whitelist and/or blacklist.

We should further facilitate removal of roles (`remove`) from the registry. In the case where role is removed we should also garbage collected it's id from all accounts that have it assigned.

## Security Considerations

It should not be possible to define the same permission in the blacklist and the whitelist at the same time. It should also not be possible to add two permissions with the same ID (the whitelist and blacklist arrays should be fully distinct).

It should not be possible to create two roles with the same names, the role names and should have a maximum of 32 characters `[a-Z,0-9,-,_]`. It should not be possible to change role names (case + special characters insensitive) and role id's.

It is important that the role id's should be iterative and even if role is removed the new role that would be created can't have the same id again. 
