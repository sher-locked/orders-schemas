## API of ```fulfilserv```

All methods exposed by ```fulfilserv``` require basic authentication by Hyprful system. 

They can be further broken down based on two Authorization levels - 

* **Fulfilment Related** (*Create & Modify*) - Mostly by store personnel & CC
* **Analytics Related** (*View*) - By biz teams, brand-owners and entrepreneurs

### Fulfilment Related

* Create a new Order : `POST /orders/`
* Get Orders by Pod : `GET /orders?pod=:podId`
* Get specific Order Information: `GET /orders/:id`
* Update Order Status (*Accept, Food-Ready*) : `PUT /orders/:id/status`
* Get Order Status: `GET /orders/:id/status`
* Assign Order-Item (*KOT or handshake - manual or automated*): `PUT /orders/:id/steps?itemId=:id&staff=:id`
* Get Orders by Status - `GET /orders?pod=:podId&status='enum'`

### Analytics Related

* Get Orders of a Brand : `GET /orders?brand=:brandId`
* Get Orders of a Pod by Status : `GET /orders?pod=:podId&status='enum'`
* Get Orders within a time-range : `GET /orders?start=1580432756970&end=1580316548964`

& many more combinations.
