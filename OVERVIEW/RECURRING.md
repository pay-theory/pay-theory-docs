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

In the call you can either pass in a payor_id if you already have a payor on file that you want to assign the recurring payment to, or you can pass in a payor object to create a new payor.

## Step 3: Manage Recurring Payments

You can manage your recurring payments using the GraphQL API. These calls are available to manage your recurring payments:

* [Cancel Recurring Payment](/api/recurring#cancel-recurring-payment)
  * Cancels a recurring payment. Once it is cancelled, it cannot be reactivated.
* [Update Recurring Payment](/api/recurring#update-recurring-payment)
  * Updates the payment method on the recurring payment. If you want to update any other fields, you will need to cancel the recurring payment and create a new one.
* [Missed Recurring Payment Data](/api/recurring#get-missed-recurring-payment-data)
  * Query how many payments have been missed and the total amount of those payments. This is useful if you want to charge a customer for all missed payments at once when updating their payment method.
* [Retry Failed Recurring Paymenr](/api/recurring#create-retry-for-failed-recurring-payment)
  * Retry a failed recurring payment. This should be used if the payment method on file is still valid, and you want to retry the payment.
