# Payor

Payors are used to track payor info that can be tied to other data objects in Pay Theory.

## The Payor Object
```js
{
    payor_id: String
    merchant_uid: String
    full_name: String
    address_line1: String
    address_line2: String
    country: String
    region: String
    city: String
    postal_code: String
    email: String
    phone: String
}
```

**`merchant_uid`: String**  
The PayTheory unique identifier assigned to the merchant that the payor belongs to.

**`payor_id`: String**  
The unique payor id.

**`full_name`: String**  
The full name of the payor.

**`address_line1`: String**  
The first line of the address of the payor.

**`address_line2`: String**  
The second line of the address of the payor.

**`country`: String**  
The country of the payor.

**`region`: String**  
The region of the payor.

**`city`: String**  
The city of the payor.

**`postal_code`: String**  
The postal code of the payor.

**`email`: String**  
The email address of the payor.

**`phone`: String**
The phone number of the payor.


## Query Payors
```js
{
  payors(direction: FORWARD, limit: 10, offset: "", offset_id: "", query: QueryObject) {
    items {
        payor_id
        merchant_uid
        full_name
        address_line1
        address_line2
        country
        region
        city
        postal_code
        email
        phone
    }
    total_row_count
  }
}
```

**Arguments**

**`limit`: Int**  
The number of payors to return.

**`direction`: String**  
The direction of the pagination. Makes sure the results are returned in the correct order.
* `FORWARD`
* `BACKWARD`

**`offset`: String**  
The value of the offset item for which the list is being sorted.  
If the direction is `FORWARD`, the offset item is the last item in the previous list.  
If the direction is `BACKWARD`, the offset is the first item in the previous list.

**`offset_id`: String**  
The `payor_id` of the offset item. If the direction is `FORWARD`, the offset item is the last item in the list. If the direction is `BACKWARD`, the offset is the first item in the list.

**`query`: QueryObject**  
The query to filter the payors with based on PayTheory defined data.  Detailed information about the query object can be found [here](query).

**Returns**

```js
{
    "data": {
        "payors": {
            "items": [
                {
                    "payor_id": "pt_pay_XXXXX",
                },
                {
                    "payor_id": "pt_pay_XXXXX",
                },
              ...
            ],
            "total_row_count": 256
        }
    }
}
```

**`items`: [Payor]**  
The list of payors that are returned from the query. Objects will include all keys that are passed in with the query.

**`total_row_count`: Int**  
The total number of payors that match the query. Used to help with pagination.