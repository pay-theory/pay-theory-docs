# Recurring Quick Start

This guide will walk you through the steps to create a recurring payment in Pay Theory. You will need to use both the Pay Theory SDK and the Pay Theory GraphQL API.

## Step 1: Create a Payment Method Token

You will need to have a payment method token to create a recurring payment. You can create a payment method token by using one of our SDKs and calling the tokenizePaymentMethod function:

* [Web/JS](../web/functions#tokenizepaymentmethod)

```js
const TOKENIZE_PAYMENT_METHOD_PARAMETERS = {
  payorInfo: PAYOR_INFO, // optional
  payorId: "pt_pay_XXXXXXXXX", // optional
  metadata: PAYMENT_METADATA, // optional 
}


myPayTheory.tokenizePaymentMethod(TOKENIZE_PAYMENT_METHOD_PARAMETERS)
```

* [Apple](../apple/functions#tokenizepaymentmethod)

```swift
let payTheory = PayTheory(apiKey: "YOUR_API_KEY")

paytheory.tokenizePaymentMethod(payorId: "pt_payor_id")
```

You also may already have a payment method token on file that you can use to create a recurring payment.

## Step 2: Call GraphQL API

You will need to call the GraphQL API to create a recurring payment. This call is detailed in our [GraphQL API documentation](../api/recurring#create-recurring-payment).

```graphql
mutation {
  createRecurringPayment(input: {
            account_code: String, 
            amount: Int, 
            currency: String, 
            fee_mode: FEE_MODE, 
            first_payment_date: Date, 
            merchant_uid: String, 
            metadata: JSON,
            mute_all_emails: Boolean
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
    mute_all_emails
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

In the call you can either pass in a payor_id if you already have a payor on file that you want to assign the recurring payment to, or you can pass in a payor object to create a new payor.

## Step 3: Manage Recurring Payments

You can manage your recurring payments using the GraphQL API. These calls are available to manage your recurring payments:

* [Cancel Recurring Payment](/api/recurring#cancel-recurring-payment)
  * Cancels a recurring payment. Once it is cancelled, it cannot be reactivated.
* [Update Recurring Payment](/api/recurring#update-recurring-payment)
  * Updates the payment method on the recurring payment. If you want to update any other fields, you will need to cancel the recurring payment and create a new one.
* [Missed Recurring Payment Data](/api/recurring#get-missed-recurring-payment-data)
  * Query how many payments have been missed and the total amount of those payments. This is useful if you want to charge a customer for all missed payments at once when updating their payment method.
* [Retry Failed Recurring Payment](/api/recurring#create-retry-for-failed-recurring-payment)
  * Retry a failed recurring payment. This should be used if the payment method on file is still valid, and you want to retry the payment.


## Optional: Manage Recurring Emails

If the `mute_all_emails` field is set to `true` and you want to send emails for recurring payments you can do so by utilizing the following GraphQL API calls:

### Successful and Failed Recurring Payment

If you want to check for successful and failed recurring payments you can use the following GraphQL API calls:

```graphql
query RecurringTransactions {
  transactions(query: { query_list: [
                        { conjunctive_operator: NONE_NEXT, 
                          key: "recurring_id", 
                          operator: IS_NOT_NULL, 
                          conjunctive_operator:AND_NEXT
                        }, 
                        { conjunctive_operator: NONE_NEXT, 
                          key: "transaction_date", 
                          operator: GREATER_THAN, 
                          value: "2023-03-30T06:00:00" # Choose date based on the interval you are checking to send emails
                        }]}) {
    items {
      transaction_id
      recurring {
        recurring_id
      }
      status
      transaction_type
      ...
    }
  }
}
```
This query will return all transactions that belong to a recurring payment. You can set the date based on the range you want to check to send out emails. You would then just need to check the `status` and `transaction_type` fields to determine if the transaction was successful or failed and send the appropriate email.

### Upcoming Expiring Payment Method

If you want to check for recurring payments that have an expiring payment method you can use the following GraphQL API call:

```graphql
query ExpiringPaymentMethods {
  recurringPayments {
    items {
      payment_method(query_list: 
                        [{ key: "exp_date", 
                          operator: EQUAL, 
                          value: "1225", 
                          conjunctive_operator: NONE_NEXT }]) 
        {
          exp_date
        }
      recurring_id
    }
  }
}
```

This query will return all recurring payments that have a payment method that is expiring on the value you pass in.  
The `exp_date` field is a string that represents the date as "MMYY". You can then decide how far out you want to check for expiring payment methods and send out emails accordingly.


### Upcoming Recurring Payment

If you want to check for upcoming recurring payments you can use the following GraphQL API call:

```graphql
query MyQuery {
  recurringPayments(query: { query_list: [
    { conjunctive_operator: NONE_NEXT,
      key: "next_payment_date",
      operator: EQUAL,
      value: "2023-03-30" # Choose date based on the interval you are checking to send emails
    }]}) {
    items {
      recurring_id
      next_payment_date
      payment_interval
    }
  }
}
```

This query will return all recurring payments that have a next payment date that matches the date you pass in. 
You can set the date based on the range you want to check to send out emails. You would then just need to check the `status` and `transaction_type` fields to determine if the transaction was successful or failed and send the appropriate email.

