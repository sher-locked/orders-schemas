## ```Orders``` Schema

This documents the schema design of ```orders``` schema postgres DB, with the tables, columns, types and constraints under the current micro-service spec. 

### ##. orders.tables
A list of all tables under schema ```orders``` managed by ```orderserv```. 

| Schema   		| Name 			   			| Type  			| Owner   		|
| ------------- | ------------------------- | ----------------- | ------------- |
| orders    	| **orders** 				| **```table```** 	| orderserv 	|
| orders   		| **items**	  				| **```table```** 	| orderserv		|
| orders   		| **item_options**	  		| **```table```** 	| orderserv		|
| orders   		| **item_charges**	  		| **```table```** 	| orderserv		|
| orders   		| **item_taxes**	  		| **```table```** 	| orderserv		|
| orders   		| **charges**	  			| **```table```** 	| orderserv		|
| orders   		| **taxes**	  				| **```table```** 	| orderserv		| 
| orders 		| **discounts**     		| **```table```** 	| orderserv		| 
| orders		| **payments**	  			| **```table```** 	| orderserv 	| 
| orders 		| **logs**		     		| **```table```** 	| orderserv		|
| orders 		| **customers**     		| **```table```** 	| orderserv		| 
| orders 		| **customer_addresses**    | **```table```** 	| orderserv		|
| orders 		| **riders**	     		| **```table```** 	| orderserv		| 
| orders 		| **rider_logs**	     	| **```table```** 	| orderserv		|
| orders 		| **unfiltered**	     	| **```table```** 	| orderserv		| 
| orders 		| **source**	     		| **```enum```** 	| orderserv		|
| orders 		| **demand_channels**	    | **```enum```** 	| orderserv		|
| orders 		| **platform_states**	    | **```enum```** 	| orderserv		|
| orders 		| **hyprful_states**	    | **```enum```** 	| orderserv		|
| orders 		| **fulfilment_types**	    | **```enum```** 	| orderserv		|
| orders 		| **delivery_state**	    | **```enum```** 	| orderserv		|


### 01. orders.orders
The is the primary table of this schema. Designed is such a way so as to fully provide analytics on orders queryable by brand or store (```outlet_id```). 
This also containes 6 enumerated values to ease queries by platforms and state, values of which are listed below. 

| Coulmn   				| Type(Length)    	| Nullable  | Default   		| Example				| Comment                        						|
| ---------------------	| ----------------- | --------- | ------------------| --------------------- | ----------------------------------------------------- |
| **id (pk)**    		| ```bigint(8)``` 	| **false** | ```nextval```     | ```346735```			| **[Primary-Key]**. *Unique* and *self-generated*.	 	|
| **source**   			| ```enum```	  	| **false** | ```UNKNOWN```		| ```URBAN_PIPER```		| The source of Integration.					 		|
| **demand_channel**	| ```enum```	  	| **false** | ```UNMAPPED```    | ```SWIGGY```			| The demand channel.						 			|
| **channel_order_id** 	| ```text```     	| **false** | ```"0"```		    | ```“635424578998”```	| The order-id on the demand channel.		 			|
| **platform_state**	| ```enum```	    | **false** | ```UNKNOWN```		| ```ACKNOWLEDGED```	| The status (mapped) state from demand-channel.		|
| **hyprful_state**		| ```enum```	    | **false** | ```UNKNOWN```	    | ```ASSIGNED```		| The status of the order on Hyprful platform.   		| 
| **fulfilment_type**	| ```enum```	    | true	    | none            	| ```TAKEAWAY```		| The type of fulfilment. 					 			|
| **delivery_state**	| ```enum```	    | true 		| ```UNASSIGNED```	| ```OUT_FOR_DELIVERY```| The status of the order on delivery platform.   		|
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


Listed below are the current enums in the table and their base values. More will be added with integrations and use-cases. 

**```source```** - ```URBANPIPER```, ```HYPRFUL_POS_OFFLINE```, ```UNKNOWN```

**```demand_channel```** - ```SWIGGY```, ```ZOMATO```, ```UBER_EATS```, ```DUNZO```, ```SWIGGY_STORES```, ```UNMAPPED``` 

**```platform_state```** - ```RECEIVED```, ```ACCEPTED```, ```REJECTED```, ```PREPARING```, ```FOOD_READY```, ```DISPATCHED```, ```DELIVERED```, ```CANCELLED```, ```UNKNOWN```

**```hyprful_state```** - ```ACCEPTED```, ```ASSIGNED```, ```PREPARING```, ```FOOD_READY```, ```PACKED```, ```DISPATCHED``` and more.

**```fulfilment_type```** - ```DELIVERY```, ```TAKEAWAY```, ```UNKNOWN```

**```delivery_state```** - ```UNASSIGNED```, ```ASSIGNED```, ```RE_ASSIGNED```, ```AT_MERCHANT```, ```OUT_FOR_DELIVERY```, ```DELIVERED```, ```UNKNOWN```


### 02. orders.items
Represents items of an order. Arguably the second most important table as it represents the menu-items and SKUs to be fulfilled for an order. 

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
Represents options along with an item in an order. Also should modify menu-items and SKUs upon changes.

| Coulmn   					| Type(Length)    	| Nullable  | Default   		| Example				| Comment                        							|
| ---------------------		| ----------------- | --------- | ------------------| --------------------- | ----------------------------------------------------- 	|
| **id (pk)**    			| ```bigint(8)``` 	| **false** | ```nextval```     | ```456723```			| **[Primary-Key]**. *Unique* and *self-generated*.	 		|
| **order_item_id (fk)**   	| ```bigint(8)``` 	| **false** | ```unique```		| ```54567735```		| **[Foreign-Key]**. *Unique* ID of ```orders.items``` row.	|
| **item_id**   			| ```bigint(8)``` 	| true		| none				| ```4327```			| Unique identifier of option-item within Hyprful.			|
| **add**   				| ```boolean``` 	| **false**	| ```t```			| ```f```				| Should option be added or removed.						|
| **title**		   			| ```text``` 		| true		| none				| ```Cheese (small)```	| Title of option-item.					 					|
| **price**   				| ```float``` 		| true		| none				| ```40.00```			| Base price of option-item.					 			|


### 04. orders.logs
Represents all the different states of an order across different platforms. 

| Coulmn   					| Type(Length)    	| Nullable  | Default   		| Example				| Comment                        							|
| ---------------------		| ----------------- | --------- | ------------------| --------------------- | ----------------------------------------------------- 	|
| **id (pk)**    			| ```bigint(8)``` 	| **false** | ```nextval```     | ```456723```			| **[Primary-Key]**. *Unique* and *self-generated*.	 		|
| **order_item_id (fk)**   	| ```bigint(8)``` 	| **false** | ```unique```		| ```54567735```		| **[Foreign-Key]**. *Unique* ID of ```orders.items``` row.	|
| **platform**		   		| ```enum``` 		| true		| none				| ```Cheese (small)```	| Title of option-item.					 					|
| **state**   				| ```text``` 		| true		| none				| ```4327```			| Unique identifier of option-item within Hyprful.			|
| **prev_state**   			| ```text``` 		| **false**	| ```t```			| ```f```				| Should option be added or removed.						|
| **epoch**		   			| ```bigint(8)``` 	| true		| none				| ```Cheese (small)```	| Title of option-item.					 					|

**```platform**```** - ```DEMAND```, ```HYPRFUL```, ```DELIVERY```



*The tables henceforth are primarily related to billing and invoicing.*


### 05. orders.item_charges
Charges applied to an item in an order. 

| Coulmn   					| Type(Length)    	| Nullable  | Default   		| Example					| Comment                        							|
| ---------------------		| ----------------- | --------- | ------------------| ------------------------- | ----------------------------------------------------- 	|
| **id (pk)**    			| ```bigint(8)``` 	| **false** | ```nextval```     | ```4335677545```			| **[Primary-Key]**. *Unique* and *self-generated*.	 		|
| **order_item_id (fk)**   	| ```bigint(8)``` 	| **false** | ```unique```		| ```54567735```			| **[Foreign-Key]**. *Unique* ID of ```orders.items``` row.	|
| **title**		   			| ```text``` 		| true		| none				| ```Packaging Charge```	| Title of charge applied.					 					|
| **value**   				| ```float``` 		| **false**	| ```0.00```		| ```10.00```				| Value of charge applied.					 			|


### 06. orders.item_charges
Charges applied to an item in an order. 

| Coulmn   					| Type(Length)    	| Nullable  | Default   		| Example					| Comment                        							|
| ---------------------		| ----------------- | --------- | ------------------| ------------------------- | ----------------------------------------------------- 	|
| **id (pk)**    			| ```bigint(8)``` 	| **false** | ```nextval```     | ```4335677545```			| **[Primary-Key]**. *Unique* and *self-generated*.	 		|
| **order_item_id (fk)**   	| ```bigint(8)``` 	| **false** | ```unique```		| ```54567735```			| **[Foreign-Key]**. *Unique* ID of ```orders.items``` row.	|
| **title**		   			| ```text``` 		| true		| none				| ```Packaging Charge```	| Title of charge applied.					 					|
| **value**   				| ```float``` 		| **false**	| ```0.00```		| ```10.00```				| Value of charge applied.					 			|


### 07. orders.charges
Taxes applied at the order level.  

| Coulmn   					| Type(Length)    	| Nullable  | Default   		| Example					| Comment                        						|
| ---------------------		| ----------------- | --------- | ------------------| ------------------------- | ----------------------------------------------------- |
| **id (pk)**    			| ```bigint(8)``` 	| **false** | ```nextval```     | ```928677545```			| **[Primary-Key]**. *Unique* and *self-generated*.	 	|
| **order_id (fk)**		   	| ```bigint(8)``` 	| **false** | ```unique```		| ```54567735```			| **[Foreign-Key]**. *Unique* ID of ```orders``` row.	|
| **title**		   			| ```text``` 		| true		| none				| ```Packiging Charges```	| Title of charges applied.					 			|
| **mode**		   			| ```text``` 		| true		| none				| ```fixed```				| Mode ```fixed``` or ```percentage```.					|
| **rate**		   			| ```float``` 		| true		| none				| ```25.00```				| Rate of charges applied.					 			|
| **value**   				| ```float``` 		| **false**	| ```0.00```		| ```25.00```				| Value of charges applied.					 			|


### 08. orders.taxes
Taxes applied at the order level.  

| Coulmn   					| Type(Length)    	| Nullable  | Default   		| Example					| Comment                        						|
| ---------------------		| ----------------- | --------- | ------------------| ------------------------- | ----------------------------------------------------- |
| **id (pk)**    			| ```bigint(8)``` 	| **false** | ```nextval```     | ```928677545```			| **[Primary-Key]**. *Unique* and *self-generated*.	 	|
| **order_id (fk)**		   	| ```bigint(8)``` 	| **false** | ```unique```		| ```54567735```			| **[Foreign-Key]**. *Unique* ID of ```orders``` row.	|
| **title**		   			| ```text``` 		| true		| none				| ```CGST```				| Title of taxes applied.					 			|
| **mode**		   			| ```text``` 		| true		| none				| ```percentage```			| Mode ```fixed``` or ```percentage```.					|
| **rate**		   			| ```float``` 		| true		| none				| ```2.50```				| Rate of taxes applied.					 			|
| **value**   				| ```float``` 		| **false**	| ```0.00```		| ```21.00```				| Value of taxes applied.					 			|


### 09. orders.discounts
Discounts applied at the order level.  

| Coulmn   					| Type(Length)    	| Nullable  | Default   		| Example					| Comment                        						|
| ---------------------		| ----------------- | --------- | ------------------| ------------------------- | ----------------------------------------------------- |
| **id (pk)**    			| ```bigint(8)``` 	| **false** | ```nextval```     | ```928677545```			| **[Primary-Key]**. *Unique* and *self-generated*.	 	|
| **order_id (fk)**		   	| ```bigint(8)``` 	| **false** | ```unique```		| ```54567735```			| **[Foreign-Key]**. *Unique* ID of ```orders``` row.	|
| **is_merchant_discount**	| ```boolean``` 	| **false** | ```false```		| ```true```				| Discout sponsored by merchant or not.					|
| **code**		   			| ```text``` 		| true		| none				| ```FLAT30```				| Discount-code.					 					|
| **title**		   			| ```text``` 		| true		| none				| ```Restaurant Promo```	| Title of discount.					 				|
| **mode**		   			| ```text``` 		| true		| none				| ```fixed```				| Mode ```fixed``` or ```percentage```.					|
| **rate**		   			| ```float``` 		| true		| none				| ```30.00```				| Rate of discount applied.					 			|
| **value**   				| ```float``` 		| **false**	| ```0.00```		| ```30.00```				| The discount amount applied.					 		|



### 10. orders.payments
Discounts applied at the order level.  

| Coulmn   					| Type(Length)    	| Nullable  | Default   		| Example					| Comment                        						|
| ---------------------		| ----------------- | --------- | ------------------| ------------------------- | ----------------------------------------------------- |
| **id (pk)**    			| ```bigint(8)``` 	| **false** | ```nextval```     | ```928677545```			| **[Primary-Key]**. *Unique* and *self-generated*.	 	|
| **order_id (fk)**		   	| ```bigint(8)``` 	| **false** | ```unique```		| ```54567735```			| **[Foreign-Key]**. *Unique* ID of ```orders``` row.	|
| **option**		   		| ```text``` 		| true		| none				| ```Simpl```				| Payment option used.					 				|
| **title**		   			| ```float``` 		| true		| none				| ```252.00```				| Amount paid through option.					 		|

