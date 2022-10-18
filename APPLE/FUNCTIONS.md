# Functions

These are the functions available to you in the PayTheory SDK. These allow you to use the data collected by the inputs to create a payment or tokenize a payment method.

These functions will return a response to the completion handler you set on the `PayTheory` object. More info on the completion handler can be found [here]().

## transact

This function is used to submit a payment to Pay Theory.

```swift
let payTheory = PayTheory(apiKey: "YOUR_API_KEY")

paytheory.transact(amount: 3300, accountCode: "custom_account_code", payorId: "pt_payor_id", ...)
```

These are the parameters that you can pass into the `transact` function to customize the payment.  
The only required key is `amount`.

* **amount**: Int
    * Represents the amount to be charged in cents.


* **payor**: Payor
    * An initialized Payor object with the details you want to assign to the payment for the payor.

```swift
let payor = Payor(
    firstName: "John",
    lastName: "Doe",
    email: "test@paytheory.com"
)
```


* **payorId**: String
    * The PayTheory payor ID to use for the payment. Allows for user to manage identities.
    * This cannot be used if also using the `payor` parameter.


* **feeMode**: FEE_MODE (enum)
    * Defaults to `FEE_MODE.INTERCHANGE`. If available to merchant and set to `FEE_MODE.SERVICE_FEE` the fee will be added to the amount and charged to the payor. More details about the fee modes in your PayTheory Portal.


* **fee**: Int
    * Represents the fee to be charged in cents.
    * If you are using `SERVICE_FEE` mode and want to skip the confirmation step, you must provide the fee amount. This will be validated to make sure it matches the fee amount that would be charged. If the fee amount does not match, an error will be thrown.


* **accountCode**: String
    * Code that can be used to track a payment or group of payments. Will be included in the transaction schema and in the PayTheory Portal.


* **reference**: String
    * Custom description assigned to a payment. Will be included in the transaction schema and in the PayTheory Portal.


* **paymentParameters**: String
    * The payment parameters to use for the payment.
    * For more information on payment parameters check out the [Payment Parameters](payment-parameters) documentation.


* **invoiceId**: String
    * The PayTheory invoice ID to use for the payment. Allows for user to assign a payment to an invoice.


* **recurringId**: String
    * The PayTheory recurring ID to use for the payment. Allows for user to assign a payment to a recurring payment.
    * If you pass in a recurring ID, the transactions amount must be an interval of the recurring payments amount per payment.


* **sendReceipt**: Boolean
    * Pass *true* to send a receipt to the payor. Must have an email address on the payorInfo object or pass in a payorId that has an email address tied to it.


* **receiptDescription**: String
    * Description to be included in the receipt. Defaults to "Payment from {merchant name}".
    * For more info on receipts check out the [Receipts](email-receipts) documentation.


* **confirmation**: Boolean
    * Defaults to `false`. If set to `true` the payment will return a response to the completion handler before it needs to be confirmed.

    
* **metadata**: [String: Any]
    * custom metadata that will be assigned to the payment and can later be queried back with the payment.


If you require a confirmation step this function will return a `PT_CONFIRMATION` type response to the completion handler.

If you do not require a confirmation step this function will return a `PT_COMPLETE` type response to the completion handler.

If you are running this function with the cash fields it will return a `PT_BARCODE` type response to the completion handler.


## confirm

This function is used to confirm a payment that has been created and required the confirmation step.

```swift
let payTheory = PayTheory(apiKey: "YOUR_API_KEY")

payTheory.transact(amount: 3000, feeMode: FEE_MODE.SERVICE_FEE, confirmation: true)

paytheory.confirm()
```

Once you click confirm it will return a `PT_COMPLETE` type response to the completion handler. 


## cancel

This function is used to cancel a payment that has been created and required the confirmation step.

```swift
let payTheory = PayTheory(apiKey: "YOUR_API_KEY")

payTheory.transact(amount: 3000, feeMode: FEE_MODE.SERVICE_FEE, confirmation: true)

paytheory.cancel()
```

Once you click cancel the transaction will be cancelled. There is no response sent to the completion handler.


## tokenizePaymentMethod

This function is used to tokenize a card or bank account.

```swift
let payTheory = PayTheory(apiKey: "YOUR_API_KEY")

paytheory.tokenizePaymentMethod(payorId: "pt_payor_id")
```

These are the parameters that you can pass into the `tokenizePaymentMethod` function to assign data to the payment method.

* **payor**: Payor
  * An initialized Payor object with the details you want to assign to the payment for the payor.

```swift
let payor = Payor(
    firstName: "John",
    lastName: "Doe",
    email: "test@paytheory.com"
)
```


* **payorId**: String
    * The PayTheory payor ID to use for the payment method. Allows for user to manage identities.
    * This cannot be used if also using the `payor` parameter.


* **metadata**: [String: Any]
    * metadata to be assigned to the payment method.


This function will return a `PT_TOKENIZE` type response to the completion handler.