## ```Orders``` Schema

This documents the schema design of ```orders``` postgres DB, with the tables, columns, types and constraints under the current micro-service spec. 

### orders.tables
A list of all tables under schema ```orders``` managed by ```orderserv```. 

| Schema   		| Name 			   			| Type  	| Owner   		|
| ------------- | ------------------------- | --------- | ------------- |
| orders    	| **orders** 				| table 	| orderserv 	| 
| orders   		| **items**	  				| table 	| orderserv		|
| orders   		| **item_charges**	  		| table 	| orderserv		|
| orders   		| **item_options**	  		| table 	| orderserv		|
| orders   		| **item_taxes**	  		| table 	| orderserv		| 
| orders 		| **discounts**     		| table 	| orderserv		|
| orders		| **billing**	  			| table 	| orderserv 	| 
| orders		| **payments**	  			| table 	| orderserv 	| 
| orders 		| **logs**		     		| table 	| orderserv		|
| orders 		| **customers**     		| table 	| orderserv		| 
| orders 		| **customer_addresses**    | table 	| orderserv		|
| orders 		| **riders**	     		| table 	| orderserv		| 
| orders 		| **rider_logs**	     	| table 	| orderserv		|
| orders 		| **unfiltered**	     	| table 	| orderserv		| 
| orders 		| **source**	     		| enum 		| orderserv		|
| orders 		| **demand_channels**	    | enum 		| orderserv		|
| orders 		| **platform_states**	    | enum 		| orderserv		|
| orders 		| **hyprful_states**	    | enum 		| orderserv		|
| orders 		| **fulfilment_types**	    | enum 		| orderserv		|


### 01. orders.orders
Primary details of an order. 

| Coulmn   				| Type(Length)    	| Nullable  | Default   		| Example				| Comment                        						|
| ---------------------	| ----------------- | --------- | ------------------| --------------------- | ----------------------------------------------------- |
| **id (pk)**    		| ```bigint(8)``` 	| **false** | ```nextval```     | ```346735```			| **[Primary-Key]**. *Unique* and *self-generated*.	 	|
| **source**   			| ```enum```	  	| **false** | ```UNMAPPED```	| ```URBAN_PIPER```		| The source of Integration.					 		|
| **demand_channel**	| ```enum```	  	| **false** | ```UNMAPPED```    | ```SWIGGY```			| The demand channel.						 			|
| **channel_order_id** 	| ```text```     	| **false** | ```"0"```		    | ```“635424578998”```	| The order-id on the demand channel.		 			|
| **platform_state**	| ```enum```	    | **false** | ```UNKNOWN```		| ```ACKNOWLEDGED```	| The status (mapped) state from demand-channel.		|
| **hyprful_state**		| ```enum```	    | **false** | ```UNKNOWN```	    | ```ASSIGNED```		| The status of the order on Hyprful platform.   		|
| **fulfilment_type**	| ```enum```	    | true	    | none            	| ```TAKEAWAY```		| The type of fulfilment. 					 			|
| **order_live**		| ```boolean```     | **false** | ```f```         	| ```t```				| Is currently live on demand platform.		 			|
| **pod_live**			| ```boolean```     | **false** | ```f```         	| ```t``` 				| Is currently live on Hyprful platform.	 			|
| **outlet_id**			| ```bigint(8)```	| **false** | ```0```         	| ```56735``` 			| The unique outlet to which order has been placed.		|
| **customer_id**		| ```bigint(8)```	| true      | none         		| ```43216``` 			| Unique customer who placed the order.					|
| **rider_id**			| ```bigint(8)```	| true      | none         		| ```89742```			| Unique rider who placed the order.					|
| **ordered_epoch**		| ```bigint(8)```	| true      | none         		| ```1580412171000```	| Epoch millis of placed on demand channel. 			|
| **received_epoch**	| ```bigint(8)```	| **false** | ```0```        	| ```1580432756970```	| Epoch millis of order received to Hyprful. 			|
| **prepared_epoch**	| ```bigint(8)```	| **false** | ```0```         	| ```1580312586964```	| Epoch millis of order prepared.               		|
| **dispatched_epoch**	| ```bigint(8)```	| **false** | 0         		| ```1580316548964```	| Epoch millis of order dispatched.             		|
| **delivered_epoch**	| ```bigint(8)```	| true      | 0         		| ```1580321457456```	| Epoch millis of order delivered.        	    		|
| **total_charges**		| ```float```		| true      | 0         		| ```40.00```			| Sum of all item and order level charges.    			|
| **total_taxes**		| ```float```		| true      | 0         		| ```56.50```			| Sum of all item and order level taxes.				|
| **subtotal**			| ```float```		| true      | 0         		| ```340.00```			| The sum of all the items, excluding taxes.			|
| **discount**			| ```float```		| true      | 0         		| ```75.00```         	| The total discount amount applied on order.			|
| **total_with_tax**	| ```float```		| **false** | ```0.00```        | ```341.50```	    	| The total applied on the order.						|


### 02. orders.items
Rows in an order. 

| Coulmn   				| Type(Length)    	| Nullable  | Default   		| Example				| Comment                        						|
| ---------------------	| ----------------- | --------- | ------------------| --------------------- | ----------------------------------------------------- |
| **id (pk)**    		| ```bigint(8)``` 	| **false** | ```nextval```     | ```54567735```		| **[Primary-Key]**. *Unique* and *self-generated*.	 	|
| **order_id (fk)**   	| ```bigint(8)``` 	| **false** | ```unique```		| ```346735```			| **[Foreign-Key]**. *Unique* ID of ```orders.orders``` row							|
| **item_id**   		| ```bigint(8)``` 	| true		| none				| ```4327```			| Unique identifier of menu-item within Hyprful.		|
| **title**		   		| ```text``` 		| true		| none				| ```Chicken Biryani```	| Title of menu-item.					 				|
| **price**   			| ```float``` 		| true		| none				| ```223.00```			| Base price of menu-item.					 			|
| **discount**   		| ```float``` 		| true		| none				| ```75.00```			| Item-level discount applied.					 		|
| **quantity**   		| ```int``` 		| **false** | ```0```			| ```1```				| Quantity.					 							|
| **total**   			| ```float``` 		| **false** | ```0.00```		| ```187.55```			| Total including charges and taxes.					|


### 03. orders.item_options
Options along with an item in an order. 

| Coulmn   					| Type(Length)    	| Nullable  | Default   		| Example				| Comment                        							|
| ---------------------		| ----------------- | --------- | ------------------| --------------------- | ----------------------------------------------------- 	|
| **id (pk)**    			| ```bigint(8)``` 	| **false** | ```nextval```     | ```456723```		| **[Primary-Key]**. *Unique* and *self-generated*.	 		|
| **order_item_id (fk)**   	| ```bigint(8)``` 	| **false** | ```unique```		| ```54567735```			| **[Foreign-Key]**. *Unique* ID of ```orders.items``` row.	|
| **item_id**   			| ```bigint(8)``` 	| true		| none				| ```4327```			| Unique identifier of option-item within Hyprful.			|
| **add**   				| ```boolean``` 	| **false**	| ```t```			| ```f```				| Should option be added or removed.						|
| **title**		   			| ```text``` 		| true		| none				| ```Cheese (small)```	| Title of option-item.					 					|
| **price**   				| ```float``` 		| true		| none				| ```40.00```			| Base price of option-item.					 			|


### 04. orders.item_charges
Charges applied to an item in an order. 

| Coulmn   					| Type(Length)    	| Nullable  | Default   		| Example					| Comment                        							|
| ---------------------		| ----------------- | --------- | ------------------| ------------------------- | ----------------------------------------------------- 	|
| **id (pk)**    			| ```bigint(8)``` 	| **false** | ```nextval```     | ```4335677545```			| **[Primary-Key]**. *Unique* and *self-generated*.	 		|
| **order_item_id (fk)**   	| ```bigint(8)``` 	| **false** | ```unique```		| ```54567735```			| **[Foreign-Key]**. *Unique* ID of ```orders.items``` row.	|
| **title**		   			| ```text``` 		| true		| none				| ```Packaging Charge```	| Title of charge applied.					 					|
| **value**   				| ```float``` 		| **false**	| ```0.00```		| ```10.00```				| Value of charge applied.					 			|
