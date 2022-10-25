# Recurring Payment

A recurring payment represents a payment that will trigger on an interval.

You can create a recurring payment with a set number of payments to enable a payment plan for a payor.

You can also create a recurring payment with no set payment amounts to enable a subscription for a payor.

## The Recurring Payment Object

```js
{
    recurring_id: String
    merchant_uid: String
    payor: Payor
    payment_method: PaymentMethodToken
    amount_per_payment: Int
    fee_per_payment: Int
    total_amount_per_payment: Int
    currency: String
    prev_payment_date: Date
    next_payment_date: Date
    payment_interval: RecurringInterval
    fee_mode: FeeMode
    remaining_payments: Int
    status: RecurringStatus
    is_active: Boolean
    is_processing: Boolean
    recurring_name: String
    recurring_description: String
    account_code: String
    reference: String
    created_date: DateTime
    metadata: JSON
}
```

**`recurring_id`: String**  
The Pay Theory unique identifier assigned to the recurring payment.

**`merchant_uid`: String**  
The Pay Theory unique identifier assigned to the merchant that the recurring payment belongs to.

**`payor`: Payor**  
The payor that the recurring payment belongs to. Refer to the [Payor](payor) for more info.

**`payment_method`: PaymentMethodToken**  
The payment method used to make the recurring payment. Refer to the [Payment Method Token](payment_method_token) for more info.

**`fee_mode`: FeeMode**  
The fee mode for the recurring payment.

- `INTERCHANGE`: Merchant pays interchange fees
- `SERVICE_FEE`: Payor pays service fee

**`amount_per_payment`: Int**
The amount of the recurring payment.

**`fee_per_payment`: Int**  
The fee for the recurring payment.

**`total_amount_per_payment`: Int**  
The amount the payor will be charged for the recurring payment.

- Same as `amount_per_payment` if your fee mode is `INTERCHANGE`
- Adjusted total of `amount_per_payment` + `fee_per_payment` if your fee mode is `SERVICE_FEE`

**`currency`: String**  
The type of currency for the recurring payment.

**`prev_payment_date`: Date**  
The date of the last payment made for the recurring payment.

**`next_payment_date`: Date**  
The date of the next payment to be made for the recurring payment.

**`payment_interval`: RecurringInterval**  
The interval of the recurring payment. The following intervals are available:

- `WEEKLY`
- `BI_WEEKLY`
- `MONTHLY`
- `QUARTERLY`
- `BI_ANNUAL`
- `ANNUAL`

**`remaining_payments`: Int**  
The number of payments remaining for the recurring payment. For a recurring date with no set amount of payments, this will be `null`.

**`status`: RecurringStatus**  
The status of the recurring payment.

- `SYSTEM_FAILURE`: The recurring payment failed due to a system error. Will retry automatically till system issue resolved.
- `INSTRUMENT_FAILURE`: The recurring payment failed due to a payment method error. Will not retry until payment method is updated.
- `SUCCESS`: The last payment was successful.

**`is_active`: Boolean**  
Whether the recurring payment is active or been disabled.

**`is_processing`: Boolean**  
Whether the recurring payment is currently processing.

**`recurring_name`: String**  
Custom name for the recurring payment.

**`recurring_description`: String**  
Custom description for the recurring payment.

**`account_code`: String**  
Custom account code for the recurring payment that will be tied to each payment.

**`reference`: String**  
Custom reference for the recurring payment that will be tied to each payment.

**`created_date`: DateTime**  
The date the recurring payment was created.

**`metadata`: JSON**  
Custom metadata for the recurring payment that will be tied to each payment.

## Get Recurring Payments

```js
{
    recurringPayments(direction: MoveDirection, limit: Int, offset: String, offset_id: String, query: QueryObject) {
        items {
            account_code
            amount_per_payment
            created_date
            currency
            fee_mode
            fee_per_payment
            is_active
            is_processing
            merchant_uid
            metadata(query_list: $metadata_query)
            next_payment_date
            payment_interval
            payment_method(query_list: $payment_method_query)) {
                ...payment_method_object
            }
            prev_payment_date
            payor(query_list: $payor_query) {
                ...payor_object
            }
            recurring_description
            recurring_id
            recurring_name
            reference
            remaining_payments
            status
            total_amount_per_payment
        }
    }
}
```

**Arguments**

**`limit`: Int**  
The number of recurring payments to return.

**`direction`: String**  
The direction of the pagination. Makes sure the results are returned in the correct order.

- `FORWARD`
- `BACKWARD`

**`offset`: String**  
The value of the offset item for which the list is being sorted.  
If the direction is `FORWARD`, the offset item is the last item in the previous list.  
If the direction is `BACKWARD`, the offset is the first item in the previous list.

**`offset_id`: String**  
The `recurring_id` of the offset item. If the direction is `FORWARD`, the offset item is the last item in the list. If the direction is `BACKWARD`, the offset is the first item in the list.

**`query`: QueryObject**  
The query to filter the recurring payments with based on Pay Theory defined data.  Detailed information about the query object can be found [here](query).

**Nested Arguments**

**Metadata, PaymentMethod, PaymentMethod/Payor, Payor**

**`query_list`: QueryPair[]**  
The query list to filter the Metadata, Payor, Payment Method, or Payor tied to the Payment Method.  
This will ensure that only Recurring Payments that have Metadata, Payors, Payment Methods, or Payment Method Payors that match these queries.  
Detailed information about the query list can be found [here](query).

**Returns**

```js
{
    "data": {
        "recurringPayments": {
            "items": [
                {
                    "recurring_id": "pt_rec_1",
                },
                {
                    "recurring_id": "pt_rec_2"
                },
              ...
            ],
            "total_row_count": 256
        }
    }
}
```

**`items`: [Settlement]**  
The list of recurring payments that are returned from the query.

**`total_row_count`: Int**  
The total number of recurring payments that match the query. Used to help with pagination.

## Create Recurring Payment

```js
mutation {
  createRecurringPayment(input: {
            account_code: String, 
            amount: Int, 
            currency: String, 
            fee_mode: FEE_MODE, 
            first_payment_date: Date, 
            merchant_uid: String, 
            metadata: JSON, 
            payment_count: Int, 
            payment_interval: PAYMENT_INTERVAL, 
            payment_method_id: String, 
            payor_id: String, 
            payor: Payor, 
            recurring_description: String, 
            recurring_name: String, 
            reference: String
        }) {
    account_code
    amount_per_payment
    created_date
    currency
    fee_mode
    fee_per_payment
    is_active
    is_processing
    merchant_uid
    metadata
    next_payment_date
    payment_interval
    payment_method {
      ...payment_method_object
    }
    payor {
      ...payor_object
    }
    prev_payment_date
    recurring_description
    recurring_id
    recurring_name
    reference
    remaining_payments
    status
    total_amount_per_payment
  }
}
```

**Arguments**

**`input`: CreateRecurringPaymentInput**  
This object contains all the details needed to create a recurring payment.

**Required Arguments**

**`merchant_uid`: String**  
The Pay Theory merchant unique identifier for the merchant that is creating the recurring payment.

**`amount`: Int**  
The amount of the recurring payment.

**`payment_interval`: PAYMENT_INTERVAL**  
The interval of the recurring payment. The following intervals are available:
- `WEEKLY`
- `BI_WEEKLY`
- `MONTHLY`
- `QUARTERLY`
- `BI_ANNUAL`
- `ANNUAL`

**`payment_method_id`: String**  
The `payment_method_id` of the tokenized payment method that will be used for the recurring payment.

**`recurrring_name`: String**  
Custom name for the recurring payment. This is used to identify the recurring payment in customer emails and the Pay Theory dashboard.

You need one of the following two variables:  

**`payor_id`: String**  
The `payor_id` of the payor that the recurring payment will be tied to.

**`payor`: Payor**  
The payor object that the recurring payment will be tied to.  
This will create a new payor in the system. Refer to the [Payor](payor) for more info.

**Optional Arguments**

**`account_code`: String**  
Custom account code for the recurring payment that will be tied to each payment.

**`currency`: String**  
The currency of the recurring payment. If not provided, the currency will default to 'USD'.

**`fee_mode`: FEE_MODE**  
The fee mode of the recurring payment. The following fee modes are available:

- `INTERCHANGE` (default)
- `SERVICE_FEE`

**`first_payment_date`: Date**  
The date of the first payment. If not provided, the first payment will be made immediately.

**`metadata`: JSON**  
Custom metadata for the recurring payment that will be tied to each payment.

**`payment_count`: Int**  
The number of payments to be made. If not provided, the recurring payment will continue until cancelled.

**`recurring_description`: String**  
Custom description for the recurring payment that will be tied to each payment.

**`reference`: String**  
Custom reference code for the recurring payment that will be tied to each payment.

**Returns**

The call will return the newly created recurring payment.

```js
{
    "data": {
        "createRecurringPayment": {
            ...recurring_payment_object
        }
    }
}
```

## Update Recurring Payment

```js
mutation {
    updateRecurringPayment(input: {
                payment_method_id: String, 
                recurring_id: String, 
                pay_all_missed_payments: Boolean
            }) {
        account_code
        amount_per_payment
        created_date
        currency
        fee_mode
        fee_per_payment
        is_active
        is_processing
        merchant_uid
        metadata
        next_payment_date
        payment_interval
        payment_method {
        ...payment_method_object
        }
        payor {
        ...payor_object
        }
        prev_payment_date
        recurring_description
        recurring_id
        recurring_name
        reference
        remaining_payments
        status
        total_amount_per_payment
    }
}
```

**Arguments**

**`input`: CreateRecurringPaymentInput**  
This object contains all the details needed to create a recurring payment.

**Required Arguments**

**`recurring_id`: String**  
The `recurring_id` of the recurring payment to be updated.

**`payment_method_id`: String**  
The `payment_method_id` of the tokenized payment method that will be used for the recurring payment.  
If recurring payment is in a Failed state it will try to run a payment with the new payment method and get it back in to a Successful state.

**Optional Arguments**

**`pay_all_missed_payments`: Boolean**  
If the recurring payment has a set number of payments (Payment Plan) and is in a Failed state, this will make a one time charge to account for all missed payments to get it back in to a Successful state.

If the recurring payment does not have a set number of payments (Subscription) you are unable to use this option and only a single payment will be charged.

**Returns**

The call will return the updated recurring payment object.

```js
{
    "data": {
        "updateRecurringPayment": {
            ...recurring_payment_object
        }
    }
}
```

## Cancel Recurring Payment

*Once a recurring payment is cancelled, it cannot be reactivated.*

```js
mutation {
    cancelRecurringPayment(recurring_id: String)
}
```

**Arguments**

**`recurring_id`: String**  
The `recurring_id` of the recurring payment to be cancelled.

**Returns**

The call will return `true` if the recurring payment was successfully cancelled.

```js
{
    "data": {
        "cancelRecurringPayment": true
    }
}
```

## Get Missed Recurring Payment Data

This call will return details you need to display the proper amount to the customer to catch up on missed payments for a recurring payment.

```js
{
    missedRecurringPaymentData(recurring_id: String) {
        fee
        number_of_payments_missed
        total_amount_owed
    }
}
```

**Arguments**

**`recurring_id`: String**  
The `recurring_id` of the recurring payment to be cancelled.

**Returns**

```js
{
    "data": {
        "missedRecurringPaymentData": {
            "fee": 0,
            "number_of_payments_missed": 5,
            "total_amount_owed": 5000
        }
    }
}
```

**`fee`: Int**  
If the recurring payment has fee_mode set to `SERVICE_FEE`, this will be the total amount of fees that will be charged to the customer to payoff the missed payments.

If the recurring payment has fee_mode set to `INTERCHANGE`, this will be 0.

**`number_of_payments_missed`: Int**  
The number of payments that have been missed.

**`total_amount_owed`: Int**  
The total amount that the customer will owe to payoff the missed payments.


## Create Retry for Failed Recurring Payment

This call will allow you to retry a payment for a recurring payment that is in a Failed state.  
This should be used when the state is `INSTRUMENT_FAILURE` and the issue with the payment method has been addressed.  
*EX: Payment failed for insufficient funds on the card, but the customer has since made a payment on the balance to resolve the issue.*

```js
mutation {
    createRetryForFailedRecurringPayment(recurring_id: "")
}
```

**Arguments**

**`recurring_id`: String**  
The `recurring_id` of the recurring payment to be retried.

**Returns**

```js
{
    "data": {
        "createRetryForFailedRecurringPayment": true
}
```
