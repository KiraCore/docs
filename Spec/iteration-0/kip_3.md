# KIP_3
> Cancel Order

By using `cancel_order` it must be possible to cancel (remove from state) existing orders by using order `id`

## Fees (OPTIONAl)

### Execution Fee

There must be a custom, flat [execution fee](../fees.md#execution-fee) `Ε` paid when order is canceled as the result of `cancel_order` execution.

Execution Fee should be set at `$0.2` or (`4 KEX`)

### Fees Distribution

All fees paid in the native or non native currencies should be sent to the address controlled by the "rewards distribution module" (RDM). The RDM module does not have to be be implemented in the `Iteration 0`

