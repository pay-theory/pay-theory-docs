# Disputes

Disputes are charges that have been contested by a cardholder to their card issuer. 

They could be an `INQUIRY` which is just a request for information, or an actual chargeback in the status `PENDING` which is a request for a charge to be reversed. 

## The Dispute Object

```graphql
{
    merchant_uid: ID
    transaction: Transaction
    dispute_id: String
    status: DisputeStatus
    reason: DisputeReason
    amount: Int
    dispute_date: AWSDateTime
    evidence_last_send_date: AWSDateTime
    updated_date: AWSDateTime
    expiration_date: AWSDateTime
    reason_message: String
    settlement_withdrawal_batch: String
    settlement_deposit_batch: String
}
```

**merchant_uid**: ID  
The Pay Theory unique identifier assigned to the merchant that the dispute belongs to.

**transaction**: Transaction  
The transaction object for the transaction that is in dispute. More information on the transaction object can be found [here](transaction).

**dispute_id**: String  
The Pay Theory unique dispute identifier.

**status**: DisputeStatus  
The status of the dispute. It can be one of the following:
- INQUIRY 
- LOST 
- PENDING 
- WON

**reason**: DisputeReason  
The reason code for the dispute as passed by the card issuer. It can be one of the following:
- CLERICAL 
- FRAUD 
- INQUIRY 
- QUALITY 
- TECHNICAL

**amount**: Int  
The amount of the transaction in dispute.

**dispute_date**: String  
The date the dispute was created.

**evidence_last_send_date**: String  
The last date evidence was sent to the processor. If no evidence was sent, this will be null.

**updated_date**: String  
The date the dispute was last updated.

**expiration_date**: String  
The final date to submit evidence to contest a dispute.

**reason_message**: String  
A more detailed reason provided by the card issuer for the dispute.

**settlement_withdrawal_batch**: String  
The settlement batch number where funds were withdrawn from the merchants account for the dispute.

**settlement_deposit_batch**: String  
The settlement batch number where funds were deposited into the merchants account if a dispute is `WON`.

## Query Disputes

```graphql
query DisputeQuery($direction: MoveDirection, $limit: Int, $offset: String, $offset_id: String, $query: SqlQuery) {
  disputes(direction: $direction, limit: $limit, offset: $offset, offset_id: $offset_id, query: $query) {
    items {
        merchant_uid
        transaction {
            ...
        }
        dispute_id
        status
        amount
        dispute_date
        evidence_last_send_date
        updated_date
        expiration_date
        reason_message
        settlement_withdrawal_batch
        settlement_deposit_batch
    }
    total_row_count
  }
}
```

### Arguments

**`limit`: Int**  
The number of disputes to return.

**`direction`: String**  
The direction of the pagination. Makes sure the results are returned in the correct order.
* `FORWARD`
* `BACKWARD`

**`offset`: String**  
The value of the offset item for which the list is being sorted.  
If the direction is `FORWARD`, the offset item is the last item in the previous list.  
If the direction is `BACKWARD`, the offset is the first item in the previous list.

**`offset_id`: String**  
The `dispute_id` of the offset item. If the direction is `FORWARD`, the offset item is the last item in the list. If the direction is `BACKWARD`, the offset is the first item in the list.

**`query`: QueryObject**  
The query to filter the disputes with based on Pay Theory defined data.  Detailed information about the query object can be found [here](query).

### Returns
```json
{
    "data": {
        "disputes": {
            "items": [
                {
                    "dispute_id": "pt_disp_xxxx"
                },
                {
                    "dispute_id": "pt_disp_xxxx"
                }
            ],
            "total_row_count": 256
        }
    }
}
```


**`items`: [Dispute]**  
The list of disputes that are returned from the query.

**`total_row_count`: Int**  
The total number of disputes that match the query. Used to help with pagination.