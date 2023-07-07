# Transaction

Transactions are a data object that can represent a payment, failed or successful, or a refund.

## The Transaction Object

```js
{
    merchant_uid: String
    transaction_id: String
    payment_method: PaymentMethodTokenWithQuery
    recurring: RecurringPayment
    invoice: Invoice
    account_code: String
    gross_amount: Int
    net_amount: Int
    refunded_amount: Int
    fees: Int
    currency: String
    reference: String
    settlement_batch: Int
    status: TransactionStatus
    dispute_status: DisputeStatus
    is_settled: Boolean
    transaction_date: DateTime
    transaction_type: TransactionType
    refund_reason: RefundReason
    failure_reasons: [String]
    fee_mode: FeeMode
    timezone: String
    metadata: JSON
}
```

**`merchant_uid`: String**  
The Pay Theory unique identifier assigned to the merchant that the transaction belongs to.

**`transaction_id`: String**  
The unique transaction id.

**`payment_method`: PaymentMethodTokenObject**
The payment method used to make the transaction. Refer to the [Payment Method Token](payment_method_token) for more info.

**`recurring`: RecurringPayment**  
The recurring payment that the transaction belongs to if any. Refer to the [Recurring Payment](recurring) for more info.

**`account_code`: String**  
Metadata passed in at the time of the transaction under the key `pay-theory-account-code`

**`gross_amount`: Int**  
The total amount of the transaction.

**`net_amount`: Int**  
The total amount of the transaction after fees.

**`refunded_amount`: Int**  
The amount of the transaction that has been refunded if any.

**`fees`: Int**  
The fees that were taken out of the gross amount of the transaction.

**`currency`: String**  
The type of currency for the transaction.

**`reference`: String**  
Metadata passed in at the time of the transaction under the key `pay-theory-reference`

**`settlement_batch`: Int**  
The unique settlement batch number the transaction belongs to if settled.

**`status`: String**  
The status of the transaction.
* `PENDING`
* `SUCCESS`
* `SETTLED`
* `FAILED`
* `REFUNDED`
* `PARTIALLY_REFUNDED`

**`dispute_status`: String**  
Shows status of associated dispute if any.
* `PENDING`
* `INQUIRY`
* `WON`
* `LOST`

**`is_settled`: Boolean**  
Whether the transaction has been settled.

**`transaction_date`: String**  
The date the transaction was made returned as an ISO 8601 string.

**`transaction_type`: String**  
The type of transfer that was made.
* `DEBIT`
* `REVERSAL`
* `FAILURE`

**`refund_reason`: JSON**  
The reason for the refund if any.
* `reason_code`: String
  * `requested_by_customer`
  * `fradulent`
  * `duplicate`
  * `other`
* `reason_details`: String
  * Optional additional details passed at the time of the refund

**`failure_reasons`: [String]**  
List of strings, if any, detailing the reason a transaction failed.

**`metadata`: JSON**  
The metadata that was passed in at the time of the transaction.

## Query Transactions
```graphql
{
  transactions(direction: FORWARD, limit: 10, offset: "", offset_id: "", query: QueryObject) {
    items {
      account_code
      currency
      dispute_status
      failure_reasons
      fee_mode
      fees
      gross_amount
      is_settled
      merchant_uid
      metadata(query_list: [])
      net_amount
      payment_method(query_list: []) {
        payment_method_id
        payor(query_list: []) {
            payor_id
        }
      }
      recurring {
        recurring_id
      }
      reference
      refund_reason {
        reason_code
        reason_details
      }
      refunded_amount
      settlement_batch
      status
      timezone
      transaction_date
      transaction_id
      transaction_type
    }
    total_row_count
  }
}
```

**Arguments**

**`limit`: Int**  
The number of transactions to return.

**`direction`: String**  
The direction of the pagination. Makes sure the results are returned in the correct order.
* `FORWARD`
* `BACKWARD`

**`offset`: String**  
The value of the offset item for which the list is being sorted.  
If the direction is `FORWARD`, the offset item is the last item in the previous list.  
If the direction is `BACKWARD`, the offset is the first item in the previous list.

**`offset_id`: String**  
The `transaction_id` of the offset item. If the direction is `FORWARD`, the offset item is the last item in the list. If the direction is `BACKWARD`, the offset is the first item in the list.

**`query`: QueryObject**  
The query to filter the transactions with based on Pay Theory defined data.  Detailed information about the query object can be found [here](query).

### Nested Arguments
#### Metadata, PaymentMethod, PaymentMethod/Payor
**`query_list`: QueryPair[]**
The query list to filter the Metadata, Payment Method, or Payor tied to the Payment Method. This will ensure that only Transactions that have Metadata, Payment Methods, or Payors that match these queries. Detailed information about the query list can be found [here](query).


**Returns**

```js
{
    "data": {
        "transactions": {
            "items": [
                {
                    "transaction_id": "pt-start-paytheorylab-rbdg98004adg"
                },
                {
                    "transaction_id": "pt-start-paytheorylab-rbdgaf004adh"
                },
              ...
            ],
            "total_row_count": 256
        }
    }
}
```

**`items`: [Settlement]**  
The list of transactions that are returned from the query.

**`total_row_count`: Int**  
The total number of transactions that match the query. Used to help with pagination.


## Create One Time Payment

```js
mutation {
  createOneTimePayment(amount: Int, 
          merchant_uid: String, 
          payment_method_id: String, 
          account_code: String, 
          currency: String, 
          fee: Int, 
          fee_mode: FeeMode,
          invoice_id: String, 
          metadata: JSON,
          receipt_description: String, 
          recurring_id: String, 
          reference: String, 
          send_receipt: Boolean) {
    amount
    card_brand
    created_at
    currency
    last_four
    service_fee
    status
    transaction_id
  }
}
```

**Arguments**

**Required Arguments**

**`amount`: Int**  
The amount of the transaction. If the FeeMode is `SERVICE_FEE`, this is the amount of the transaction before fees.

**`merchant_uid`: String**  
The Pay Theory unique identifier for the merchant the transaction is for.

**`payment_method_id`: String**  
The Pay Theory unique identifier for the payment method the transaction will be charged to.

If your fee mode is `SERVICE_FEE`, you must also pass in the `fee` and `fee_mode` arguments.
**`fee`: Int**  
The amount of the fee that will be charged to the payor for the transaction if the FeeMode is `SERVICE_FEE`.

**`fee_mode`: FeeMode**
The fee mode on the transaction. `SERVICE_FEE` charges the fees to the payor. `MERCHANT_FEE` charges the fees to the merchant. Options are:

* `SERVICE_FEE`
* `MERCHANT_FEE` (default)

**Optional Arguments**

**`account_code`: String**  
Customer defined account code for the transaction.

**`currency`: String**  
The type of currency for the transaction. Defaults to `USD`.

**`invoice_id`: String**  
The Pay Theory unique identifier for the invoice the transaction is for.

**`metadata`: JSON**  
Custom defined JSON object to be stored with the transaction.

**`receipt_description`: String**  
The description of the transaction that will be displayed on the receipt.

**`recurring_id`: String**  
The Pay Theory unique identifier for the recurring payment the transaction is for.
If you pass in a recurring id, the transactions amount must be an interval of the recurring payments amount per payment.

**`reference`: String**  
Customer defined reference for the transaction.

**`send_receipt`: Boolean**  
If the receipt should be sent to the payor. Defaults to `false`. It is sent to the email address on file with the payment method.

**Returns**

```js
{
    "data": {
        "createOneTimePayment": {
            status: TransactionStatus
            amount: Int
            card_brand: String
            last_four: String
            service_fee: Int
            currency: String
            transaction_id: String
            created_at: DateTime
            failed_reason: String
        }
    }
}
```

**`status`: TransactionStatus**  
The status of the transaction. Options are:
* `PENDING`
* `SUCCEEDED`
* `FAILED`

**`amount`: Int**  
The amount of the transaction. This is the amount after fees if the FeeMode is `SERVICE_FEE`.

**`card_brand`: String**  
The brand of the card used for the transaction.

**`last_four`: String**  
The last four digits of the card or bank account used for the transaction.

**`service_fee`: Int**  
The amount of the service fee charged for the transaction. This will be 0 if the FeeMode is `MERCHANT_FEE`.

**`currency`: String**  
The type of currency for the transaction.

**`transaction_id`: String**  
The Pay Theory unique identifier for the transaction.

**`created_at`: DateTime**  
The date and time the transaction was created.

**`failed_reason`: String**
The failure_code explaining why the transaction failed. This will be null if the transaction succeeded. You can find a list of failure codes [here](/overview/failed_transactions)


## Create Refund

This call will create a refund for a transaction.

```js
mutation {
  createRefund(amount: Int, 
               refund_reason: { reason_code: RefundReasonCode, reason_details: String }, 
               transaction_id: String, 
               refund_email: String )
}
```

**Arguments**

**Required Arguments**

**`amount`: Int**
The amount of the refund. This must be less than or equal to the amount of the transaction.

**`refund_reason`: RefundReason**
The reason for the refund. This is required for all refunds and is made up of the following.

- **`reason_code`: RefundReasonCode**
  - The reason code for the refund. Options are:
    - `DUPLICATE`
    - `FRAUDULENT`
    - `REQUESTED_BY_CUSTOMER`
    - `OTHER`
- **`reason_details`: String** (optional)
  - Additional details about the reason for the refund. 

**`transaction_id`: String**
The Pay Theory unique identifier for the transaction to refund.

**Optional Arguments**

**`refund_email`: String**
The email address to send the refund receipt to. If not provided an email will not be sent out.

**Returns**

The call will return a boolean `true` if the refund was created successfully or errors if it fails.

## Calculate Service Fee Amount

This call will allow you to calculate what the fee amount should be if using `SERVICE_FEE` for a transaction.

```js
{
  serviceFeeAmount(amount: Int, merchant_uid: String) {
    ach {
      adjusted_total
      fee
      total
    }
    card {
      adjusted_total
      fee
      total
    }
  }
}
```

**Arguments**

**Required Arguments**

**`amount`: Int**  
The amount of the transaction.

**`merchant_uid`: String**  
The Pay Theory unique identifier for the merchant the transaction amount being calculated is for.

**Returns**

```js
{
    "data": {
        "serviceFeeAmount": {
            ach: {
              fee: Int
              total: Int
              adjusted_total: Int
            },
            card: {
              fee: Int
              total: Int
              adjusted_total: Int
            }
        }
    }
}
```

You are returned two objects, one for ACH and one for Card. Each object contains the following fields:

**`fee`: Int**  
The amount that the service fee should be based on the amount passed in.

**`total`: Int**  
The total amount of the transaction before the service fee.  
This is what you would want to pass in the `amount` argument for the `createOneTimePayment` call.

**`adjusted_total`: Int**  
The total amount of the transaction after the service fee.  
This is what you would want to show the payor the total amount of the transaction will be.

