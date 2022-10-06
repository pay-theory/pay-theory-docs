# Payment Method Token

Payment Method Tokens are meant to store info that represents a tokenized Bank Account, Credit Card, or Debit Card.

## The Payment Method Token Object
```js
{
    payment_method_id: String
    merchant_uid: String
    payor: Payor
    payment_type: PaymentType
    last_four: String
    exp_date: String
    card_brand: String
    full_name: String
    address_line1: String
    address_line2: String
    country: String
    region: String
    city: String
    postal_code: String
    is_active: Boolean
}
```

**`payment_method_id`: String**  
The unique payment method id.

**`merchant_uid`: String**  
The PayTheory unique identifier assigned to the merchant that the payment_method_token belongs to.

**`payor`: PayorObject**
The payor object. Refer to the [Payor](payor) docs for more info.

**`payment_type`: PaymentType**
The type of payment method.
* `CARD`
* `ACH`
* `CASH`

**`last_four`: String**  
The last four digits of the card or bank account number.

**`exp_date`: String**  
The expiration date of the card. Null if the payment_type is not a card. Format: `MMYY`

**`card_brand`: String**  
The brand of the card. Null if the payment_type is not a card.

**`full_name`: String**  
The name on card or bank account.

**`address_line1`: String**  
The first line of the billing address.

**`address_line2`: String**  
The second line of the billing address.

**`country`: String**  
The country of the billing address.

**`region`: String**  
The region of the billing address.

**`city`: String**  
The city of the billing address.

**`postal_code`: String**  
The postal code of the billing address.

## Query Payment Method Tokens
```js
{
  paymentMethodTokens(direction: FORWARD, limit: 10, offset: "", offset_id: "", query: QueryObject) {
    items {
        payment_method_id
        merchant_uid
        payor(query_list: [QueryPair]) {
            payor_id
            ...
            }
        payment_type
        last_four
        exp_date
        card_brand
        full_name
        address_line1
        address_line2
        country
        region
        city
        postal_code
    }
    total_row_count
  }
}
```

**Arguments**

**`limit`: Int**  
The number of payment_method_tokens to return.

**`direction`: String**  
The direction of the pagination. Makes sure the results are returned in the correct order.
* `FORWARD`
* `BACKWARD`

**`offset`: String**  
The value of the offset item for which the list is being sorted.  
If the direction is `FORWARD`, the offset item is the last item in the previous list.  
If the direction is `BACKWARD`, the offset is the first item in the previous list.

**`offset_id`: String**  
The `payment_method_id` of the offset item. If the direction is `FORWARD`, the offset item is the last item in the list. If the direction is `BACKWARD`, the offset is the first item in the list.

**`query`: QueryObject**  
The query to filter the payment_method_tokens with based on PayTheory defined data.  Detailed information about the query object can be found [here](query).

### Nested Arguments
#### Payor
**`query_list`: QueryPair[]**
The query list to filter the payment_method_tokens with based on PayTheory defined data. This will ensure that only return payment_method_tokens that match this query. Detailed information about the query list can be found [here](query).


**Returns**

```js
{
    "data": {
        "paymentMethodTokens": {
            "items": [
                {
                    "payment_method_id": "pt_pmt_XXXXX",
                },
                {
                    "payment_method_id": "pt_pmt_XXXXX",
                },
              ...
            ],
            "total_row_count": 256
        }
    }
}
```

**`items`: [PaymentMethodToken]**  
The list of payment_method_tokens that are returned from the query. Objects will include all keys that are passed in with the query.

**`total_row_count`: Int**  
The total number of payment_method_tokens that match the query. Used to help with pagination.