# Order Matching 

Order matching refers to matching buy and sell orders.


## KIP_2
> Place Order

Orders placed using `create_order` transaction must be linked to a particular order book by the ID. When order is placed user tokens must be locked in the account controlled by the order book module.   

The `create_order` transaction requires following properties
 * `order_book_id` - globally unique order book identifier
 *  









## KIP_3
> Cancel Order