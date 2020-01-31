# markup

## orders.orders

Primary details of an order. 

| Coulmn   				| Type(Length)    	| Nullable  | Default   		| Example				| Comment                        				|
| ----------------- 	| --------------  	| ----------| ------------------| --------------------- | ------------------------------------------ 	|
| **id (pk)**    		| ```bigint(8)``` 	| **false** | none         		| ```346735```			| [Primary-Key], Unique and self-generated.	 	|
| **source**   			| ```enum```	  	| **false** | ```UNMAPPED```	| ```URBAN_PIPER```		| Source of integration.					 	|
| **demand_channel**	| ```enum```	  	| **false** | ```UNMAPPED```    | ```SWIGGY```			| The demand channel.						 	|
| **channel_order_id** 	| ```text```     	| **false** | ```"0"```		    | ```“635424578998”```	| The order-id on the demand channel.		 	|
| **platform_state**	| ```enum```	    | **false** | ```UNKNOWN```		| ```ACKNOWLEDGED```	| Mapped state from demand-channel.			 	|
| **hyprful_state**		| ```enum```	    | **false** | ```UNKNOWN```	    | ```ASSIGNED```		| Status of the order on Hyprful platform.   	|
| **fulfilment_type**	| ```enum```	    | true	    | none            	| ```TAKEAWAY```		| The type of fulfilment. 					 	|
| **order_live**		| ```boolean```     | **false** | ```f```         	| ```t```				| Is currently live on demand platform.		 	|
| **pod_live**			| ```boolean```     | **false** | ```f```         	| ```t``` 				| Is currently live on Hyprful platform.	 	|
| *outlet_id*			| ```bigint(8)```	| **false** | ```0```         	| ```56735``` 			| Unique outlet to which order has been placed.	|
| **customer_id?**		| ```bigint(8)```	| true      | none         		| ```43216``` 			| Unique customer who placed the order.			|
| **rider_id?**			| ```bigint(8)```	| true      | none         		| ```89742```			| Unique rider who placed the order.			|
| **ordered_epoch**		| ```bigint(8)```	| true      | none         		| ```1580412171000```	| Epoch millis of placed on demand channel. 	|
| **received_epoch**	| ```bigint(8)```	| **false** | ```0```        	| ```1580432756970```	| Epoch millis of order received to Hyprful. 	|
| **prepared_epoch**	| ```bigint(8)```	| **false** | ```0```         	| ```1580312586964```	| Epoch millis of order prepared.               |
| **dispatched_epoch**	| ```bigint(8)```	| **false** | 0         		| ```1580316548964```	| Epoch millis of order dispatched.             |
| **delivered_epoch**	| ```bigint(8)```	| true      | 0         		| ```1580321457456```	| Epoch millis of order delivered.        	    |
| **total_charges**		| ```float```		| true      | 0         		| ```40.00```			| Sum of all item and order level charges.    	|
| **total_taxes**		| ```float```		| true      | 0         		| ```56.50```			| Sum of all item and order level taxes.		|
| **subtotal**			| ```float```		| true      | 0         		| ```340.00```			| The sum of all the items, excluding taxes.	|
| **discount**			| ```float```		| true      | 0         		| ```75.00```         	| The total discount amount applied on order.	|
| **total_with_tax**	| ```float```		| **false** | ```0.00```        | ```341.50```	    	| The total applied on the order.				|
