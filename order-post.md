## Get Order by ID

Endpoint to get order details by order ID.  

### Request 

**Method** : 
```enpoint
GET /orders/:id
```

**Data Constraints** - ```:id``` - must be a long value and a valid order id. 

#### Payload Example

```json
{
	"source": "URBAN_PIPER",
	"channel": "ZOMATO",
	"orderNumber": "ZOM453728",
	"channelState": "PREPARING",
	"localState": "PACKING",
	"delivery_state": "ASSIGNED",
	"fulfilmentType": "DELIVERY",
	"outletId": 23521344,
	"customerId": 354663,
	"riderId": 23245,
	"receivedEpoch": 1580432756970,
	"dispatchedEpoch": 1580316548964,
	"charges": 5.50,
	"taxes": 8.40,
	"totalCharges": 55.50,
	"totalTaxes": 30.00,
	"discount": 75.00,
	"total": 342.00,
	"charges" : [
		{
			"title": "Packing Charges",
			"mode": "fixed",
			"rate": 20.00,
			"value": 20.00
		},
		{
			"title": "Extra Ketchup",
			"mode": "fixed",
			"rate": 10.00,
			"value": 10.00
		}
	],
	"taxes" : [
		{
			"title": "CGST",
			"mode": "percentage",
			"rate": 2.50,
			"value": 14.50
		},
		{
			"title": "SGST",
			"mode": "percentage",
			"rate": 2.50,
			"value": 14.50
		}
	],
	"discounts" : [
		{
			"code": "SWIGGYIT",
			"title": "Flat 30% discount.",
			"mode": "rate",
			"rate": 30.00,
			"value": 50.00,
			"isMerchantDiscount": false
		},
		{
			"code": "SWIGGYIT",
			"title": "Flat 30% discount.",
			"mode": "rate",
			"rate": 30.00,
			"value": 25.00,
			"isMerchantDiscount": true
		}
	],
	"payments" : [
		{
			"option": "CASH",
			"amount": 242.00
		},
		{
			"option": "WALLET_CREDIT",
			"amount": 100.00
		}
	],
	"items": [
		{
			"id": 54567735,
			"orderId": 345456466,
			"menuItemId": 7864,
			"title": "Chicken Shawarma",
			"price": 153.00,
			"quantity": 2,
			"discount": 30.00,
			"total": 123.00,
			"charges" : [
				{
					"title": "Roll Charge",
					"mode": "fixed",
					"rate": 10.00,
					"value": 10.00
				}
			],
			"taxes" : [
				{
					"title": "CGST",
					"mode": "percentage",
					"rate": 2.50,
					"value": 3.83
				},
				{
					"title": "SGST",
					"mode": "percentage",
					"rate": 2.50,
					"value": 3.83
				}
			],
			"subItems" : [
				{
					"id": 545677245,
					"menuItemId": 5432,
					"type": "VARIANT",
					"title": "Khubus",
					"price": 20.00,
					"total": 20.00
				},
				{
					"id": 545677246,
					"menuItemId": 2135,
					"type": "ADD_ON",
					"title": "Extra Chicken",
					"price": 35.00,
					"total": 35.00
				},
				{
					"id": 545677247,
					"menuItemId": 8789,
					"type": "ADD_ON",
					"title": "Extra Mayo",
					"price": 18.00,
					"total": 18.00
				}
			]
		},
		{
			"id": 54567735,
			"orderId": 345456466,
			"menuItemId": 7864,
			"title": "Chicken Salad",
			"price": 175.00,
			"quantity": 1,
			"discount": 0.00,
			"total": 175.00,
			"taxes" : [
				{
					"title": "CGST",
					"mode": "percentage",
					"rate": 2.50,
					"value": 7.00
				},
				{
					"title": "SGST",
					"mode": "percentage",
					"rate": 2.50,
					"value": 4.37
				}
			]
		}
	]
}
```

### Response


#### Success Sample

**Condition** : If all necessary details for order-creation are sent.

**Code** : `201 CREATED`

**Response JSON**

```json
{
	"id": 345456466,
	"source": "URBAN_PIPER",
	"channel": "ZOMATO",
	"orderNumber": "ZOM453728",
	"channelState": "PREPARING",
	"localState": "PACKING",
	"delivery_state": "ASSIGNED",
	"fulfilmentType": "DELIVERY",
	"outletId": 23521344,
	"customerId": 354663,
	"riderId": 23245,
	"receivedEpoch": 1580432756970,
	"dispatchedEpoch": 1580316548964,
	"charges": 5.50,
	"taxes": 8.40,
	"totalCharges": 55.50,
	"totalTaxes": 30.00,
	"discount": 75.00,
	"total": 342.00,
	"charges" : [
		{
			"title": "Packing Charges",
			"mode": "fixed",
			"rate": 20.00,
			"value": 20.00
		},
		{
			"title": "Extra Ketchup",
			"mode": "fixed",
			"rate": 10.00,
			"value": 10.00
		}
	],
	"taxes" : [
		{
			"title": "CGST",
			"mode": "percentage",
			"rate": 2.50,
			"value": 14.50
		},
		{
			"title": "SGST",
			"mode": "percentage",
			"rate": 2.50,
			"value": 14.50
		}
	],
	"discounts" : [
		{
			"code": "SWIGGYIT",
			"title": "Flat 30% discount.",
			"mode": "rate",
			"rate": 30.00,
			"value": 50.00,
			"isMerchantDiscount": false
		},
		{
			"code": "SWIGGYIT",
			"title": "Flat 30% discount.",
			"mode": "rate",
			"rate": 30.00,
			"value": 25.00,
			"isMerchantDiscount": true
		}
	],
	"payments" : [
		{
			"option": "CASH",
			"amount": 242.00
		},
		{
			"option": "WALLET_CREDIT",
			"amount": 100.00
		}
	],
	"items": [
		{
			"id": 54567735,
			"orderId": 345456466,
			"menuItemId": 7864,
			"title": "Chicken Shawarma",
			"price": 153.00,
			"quantity": 2,
			"discount": 30.00,
			"total": 123.00,
			"charges" : [
				{
					"title": "Roll Charge",
					"mode": "fixed",
					"rate": 10.00,
					"value": 10.00
				}
			],
			"taxes" : [
				{
					"title": "CGST",
					"mode": "percentage",
					"rate": 2.50,
					"value": 3.83
				},
				{
					"title": "SGST",
					"mode": "percentage",
					"rate": 2.50,
					"value": 3.83
				}
			],
			"subItems" : [
				{
					"id": 545677245,
					"menuItemId": 5432,
					"type": "VARIANT",
					"title": "Khubus",
					"price": 20.00,
					"total": 20.00
				},
				{
					"id": 545677246,
					"menuItemId": 2135,
					"type": "ADD_ON",
					"title": "Extra Chicken",
					"price": 35.00,
					"total": 35.00
				},
				{
					"id": 545677247,
					"menuItemId": 8789,
					"type": "ADD_ON",
					"title": "Extra Mayo",
					"price": 18.00,
					"total": 18.00
				}
			]
		},
		{
			"id": 54567735,
			"orderId": 345456466,
			"menuItemId": 7864,
			"title": "Chicken Salad",
			"price": 175.00,
			"quantity": 1,
			"discount": 0.00,
			"total": 175.00,
			"taxes" : [
				{
					"title": "CGST",
					"mode": "percentage",
					"rate": 2.50,
					"value": 7.00
				},
				{
					"title": "SGST",
					"mode": "percentage",
					"rate": 2.50,
					"value": 4.37
				}
			]
		}
	]
}
```

#### Error Samples

**Condition** : If ```id``` is not a long value.

**Code** : `400 BAD REQUEST`

**Content** : 

```json
{
  "error_type": "INVALID_REQUEST",
  "http_code": 400,
  "error_code": "INVALID_ID",
  "error_message": "Order id should be a long value.",
  "display_message": "This is not a valid order id."
}
```

##### Or

**Condition** : If ```id``` is not found in DB.

**Code** : `404 NOT FOUND`

**Content**

```json
{
  "error_type": "NOT_FOUND",
  "http_code": 404,
  "error_code": "INVALID_ID",
  "error_message": "Order id is not present in the DB.",
  "display_message": "This is not a valid order id."
}
```
