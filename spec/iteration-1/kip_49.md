# [⏎](README.md#Roadmap) KIP_49
> Transactions Challenge

One of the common pitfalls when sending transactions is mistyping addresses and sending money to accounts where user was coerced by the malicious application to input fictitious address (clipboard address swap, dependency injection, etc). We have to enable protection from such exploits if from of the challenge that can only be known by a second party communicating with user though separate communication channels.

## Execution

We have to enable creation of transactions with predefined `expiration` timestamp before which the transaction can be accepted by the receiving account. This mechanism can be conditional or unconditional. Meaning that additional `challenge` field can be present. If the `challenge` is not present then receiving account can send empty `challenge_response` transaction to claim the assets. If the `challenge_response` is not submitted then money will not be sent (effectively preventing user from sending money to non existing accounts by mistake). In the case where `challenge` is present then receiving account must send the `challenge_response` message containing solution to the `Blake2` hash riddle. Money will be send to the receiving account if correct solution is sent before `expiration` time.

## Security

We have to ensure that when `challenge` transaction is sent the assets in question become locked and unaccessible to any of the two parties until the `expiration` time passes.

When sending the transaction the destination account should be specified. It should not be possible to claim the money using accounts that were not explicitly specified.

To generate a riddle a string provided by the user should be hashed twice using Blake2 algorithm. Hashing has to occur on the frontend / CLI side. The solution has also be hashed but only once (this also has to take part on the frontend / CLI side). Backend should check the solution by Blake2 hashing the provided `solution` once and checking if the `Blake2(solution) == challenge`. This way the solution being potentially compromising information can be protected (e.g. name or often used password). 
