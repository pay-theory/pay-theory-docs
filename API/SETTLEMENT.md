# Settlement

Settlements are a batch of payments, disputes, and refunds that are grouped together and paid out to a merchant.

## The Settlement Object

```js
{
    merchant_uid: String
    settlement_batch: Int
    settlement_date: DateTime
    transaction_debit_count: Int
    transaction_reversal_count: Int
    transaction_dispute_count: Int
    gross_amount: Int
    net_amount: Int
    total_fees: Int
    total_adjustments: Int
    currency: String
    status: String
}
```

**`merchant_uid`: String**  
The Pay Theory unique identifier assigned to the merchant that the settlement belongs to.

**`settlement_batch`: Int**  
The unique settlement batch number.

**`settlement_date`: String**  
The date the settlement was created in an ISO 8601 String format.

**`transaction_debit_count`: Int**  
The number of transactions of type DEBIT that were included in the settlement.

**`transaction_reversal_count`: Int**  
The number of transactions of type REVERSAL that were included in the settlement.

**`transaction_dispute_count`: Int**  
The number of transactions of type DISPUTE that were included in the settlement.

**`gross_amount`: Int**  
The total amount of the settlement before any fees and adjustments.

**`net_amount`: Int**  
The total amount of the settlement after any fees and adjustments.

**`total_fees`: Int**  
The total amount of fees that were applied to the settlement.

**`total_adjustments`: Int**  
The total amount of adjustments that were applied to the settlement.

**`currency`: String**  
The currency of the settlement.

**`status`: String**  
The status of the settlement.
* `PENDING`
* `SUCCEEDED`

## Query Settlements
```js
{
    settlements(limit: Int, direction: MoveDirection, offset: String, offset_id: String, query: QueryObject) {
        items {
            currency
            gross_amount
            merchant_uid
            net_amount
            settlement_batch
            settlement_date
            status
            total_adjustments
            total_fees
            transaction_dispute_count
            transaction_debit_count
            transaction_reversal_count
        }
        total_row_count
    }
}
```

**Arguments**

**`limit`: Int**  
The number of settlements to return.

**`direction`: String**  
The direction of the pagination. Makes sure the results are returned in the correct order.
* `FORWARD`
* `BACKWARD`

**`offset`: String**  
The value of the offset item for which the list is being sorted.  
If the direction is `FORWARD`, the offset item is the last item in the previous list.  
If the direction is `BACKWARD`, the offset is the first item in the previous list.

**`offset_id`: String**  
The `settlement_batch` of the offset item. If the direction is `FORWARD`, the offset item is the last item in the list. If the direction is `BACKWARD`, the offset is the first item in the list.

**`query`: QueryObject**  
The query to filter the settlements with.  Detailed information about the query object can be found [here](query).

**Returns**

```js
{
    "data": {
        "settlements": {
            "items": [
                {
                    "settlement_batch": "42"
                },
                {
                    "settlement_batch": "41"
                },
              ...
            ],
            "total_row_count": 256
        }
    }
}
```

**`items`: [Settlement]**  
The list of settlements that are returned from the query.

**`total_row_count`: Int**  
The total number of settlements that match the query. Used to help with pagination.