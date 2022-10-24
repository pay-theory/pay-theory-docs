# Functions

The Pay Theory SDK provides a set of functions that can be used to perform actions in tandem with the Hosted Fields.

These functions are available on the object returned by the `create` function.

## create

The `create` function is used to initialize the Pay Theory object. This function takes two arguments, the API key and the [styles object](web/hosted-fields#stylying-hosted-fields).

```javascript
const API_KEY = "YOUR_API_KEY"
const STYLES = {
    ...style_options
}

const myPayTheory = await window.paytheory.create(API_KEY, STYLES)
```

This function returns a promise that resolves to the Pay Theory object containing the functions listed below and all available [Event Listeners](web/event-listeners) and [State Listeners](web/state-listeners).

## mount

The `mount` function is used to mount the Hosted Fields to the DOM. This function takes no arguments.

```javascript
myPayTheory.mount()
```

## transact

This function is used to submit a payment to Pay Theory or generate a barcode using the cash hosted fields.

```javascript
//Amount passed in is in cents
const AMOUNT = 1000

// optionally provide details about the payor
const PAYOR_INFO = {
  "first_name": "Some",
  "last_name": "Body",
  "email": "somebody@gmail.com",
  "phone": "3335554444",
  "personal_address": {
    "city": "Somewhere",
    "country": "USA",
    "region": "OH",
    "line1": "123 Street St",
    "line2": "Apartment 17",
    "postal_code": "12345"
  }
}

const PAYMENT_METADATA = {
  "student-name": "Jane Doe"
};

// Parameters that you will pass into the transact function. More details below.
const TRANSACTING_PARAMETERS = { 
        amount: AMOUNT, 
        payorInfo: PAYOR_INFO, // optional
        payorId: "pt_pay_XXXXXXXXX", // optional
        metadata: PAYMENT_METADATA, // optional 
        feeMode: FEE_MODE, // optional
        fee: 100, // optional
        confirmation: false, // optional 
        accountCode: "code-123456789", // optional 
        reference: "field-trip", // optional
        paymentParameters: "expires-in-30-days", // optional
        invoiceId: "pt_inv_XXXXXXXXX", // optional
        sendReceipt: true, // optional 
        receiptDescription: "School Technology Fees" // optional
        recurringId: "pt_rec_XXXXXXXXX", // optional
}

myPayTheory.transact(TRANSACTING_PARAMETERS)
```

These are the values that you can pass into the `transact` function to customize the payment.  
The only required key is `amount`.

- `amount`: (Int)
  - represents the amount to be charged in cents


- `payorInfo`: (Object)
  - see the [Payor Info object](#payor-info-object) below for details


- `metadata`: (Object)
  - An object that will be stored with the transaction and can be used to track the payment. 


- `feeMode`: (String)
  - Defaults to `window.paytheory.INTERCHANGE`. If available to merchant and set to `window.paytheory.SERVICE_FEE` the fee will be added to the amount and charged to the payor. More details about the fee modes in your PayTheory Portal.


- `fee`: (Int)
  - Represents the fee to be charged in cents.
  - If you are using `SERVICE_FEE` mode and want to skip the confirmation step, you must provide the fee amount. This will be validated to make sure it matches the fee amount that would be charged. If the fee amount does not match, an error will be thrown.


- `confirmation`: (Boolean)
  - Defaults to `false`. If set to `true` the payment will return a response to the tokenizeObserver before it needs to be confirmed. Required if using `SERVICE_FEE` fee mode.


- `accountCode`: (String)
  - Code that can be used to track a payment or group of payments. Will be included in the transaction schema and in the PayTheory Portal.


- `reference`: (String)
  - Custom description assigned to a payment. Will be included in the transaction schema and in the PayTheory Portal.


- `paymentParameters`: (String)
  - The payment parameters to use for the payment.
  - For more information on payment parameters check out the [Payment Parameters](payment-parameters) documentation.


- `payorId`: (String)
  - The PayTheory payor ID to use for the payment. Allows for user to manage identities.
  - This cannot be used if also using the `payorInfo` parameter.


- `invoiceId`: (String)
  - The PayTheory invoice ID to use for the payment. Allows for user to assign a payment to an invoice.


- `recurringId`: (String)
  - The PayTheory recurring ID to use for the payment. Allows for user to assign a payment to a recurring payment.
  - If you pass in a recurring ID, the transactions amount must be an interval of the recurring payments amount per payment.


- `sendReceipt`: (Boolean)
  - Pass `true` to send a receipt to the payor. Must have an email address on the payorInfo object or pass in a payorId that has an email address tied to it.


- `receiptDescription`: (String)
  - Description to be included in the receipt. Defaults to "Payment from {merchant name}".
  - For more info on receipts check out the [Receipts](email-receipts) documentation.

Once the transact function is called, if you passed `true` to `requireConfirmation` you will receive a response from the `tokenizeObserver`.

If you did not pass `true` to `requireConfirmation`, you will receive a response to the `transactedObserver`.

## confirm

This function is used to confirm a payment that has been created and required the confirmation step.

```javascript
paytheory.confirm()
```

Once you confirm it will return a message to the `captureObserver`.


## cancel

This function is used to cancel a payment that has been created and required the confirmation step.

```javascript
paytheory.cancel()
```

Once you click cancel the transaction will be cancelled and you will be able to run `transact` again.


## tokenizePaymentMethod

This function is used to tokenize a card or bank account.

```javascript
/ optionally provide details about the payor
const PAYOR_INFO = {
  "first_name": "Some",
  "last_name": "Body",
  "email": "somebody@gmail.com",
  "phone": "3335554444",
  "personal_address": {
    "city": "Somewhere",
    "country": "USA",
    "region": "OH",
    "line1": "123 Street St",
    "line2": "Apartment 17",
    "postal_code": "12345"
  }
}

const TOKEN_METADATA = {
  "system_id": "123456789"
};

const TOKENIZE_PAYMENT_METHOD_PARAMETERS = {
  payorInfo: PAYOR_INFO, // optional
  payorId: "pt_pay_XXXXXXXXX", // optional
  metadata: TOKEN_METADATA // optional 
}


myPayTheory.tokenizePaymentMethod(TOKENIZE_PAYMENT_METHOD_PARAMETERS)
```

These are the values that you can pass into the `tokenizePaymentMethod` function to tokenize a card or bank account.

- `payorInfo`: (Object)
  - see the [Payor Info object](#payor-info-object) below for details
- `metadata`: (Object)
  - An object that will be stored with the token and can be used to track the token.
- `payorId`: (String)
  - The PayTheory payor ID to use for the payment. Allows for user to manage identities.
  - This cannot be used if also using the `payorInfo` parameter.

Once the `tokenizePaymentMethod` function is called, you will receive a response from the `transactedObserver` function.


## activateCardPresentDevice

This function is used to activate a card present device and accept a payment.

```javascript
//Amount passed in is in cents
const AMOUNT = 1000

// optionally provide details about the payor
const PAYOR_INFO = {
  "first_name": "Some",
  "last_name": "Body",
  "email": "somebody@gmail.com",
  "phone": "3335554444",
  "personal_address": {
    "city": "Somewhere",
    "country": "USA",
    "region": "OH",
    "line1": "123 Street St",
    "line2": "Apartment 17",
    "postal_code": "12345"
  }
}

const PAYMENT_METADATA = {
  "student-name": "Jane Doe"
};

// Parameters that you will pass into the transact function. More details below.
const CARD_PRESENT_PARAMS = { 
        amount: AMOUNT, 
        deviceId: "pt_dev_XXXXXXXXX",
        payorInfo: PAYOR_INFO, // optional
        payorId: "pt_pay_XXXXXXXXX", // optional
        metadata: PAYMENT_METADATA, // optional 
        feeMode: FEE_MODE, // optional
        fee: 100, // optional
        confirmation: false, // optional 
        accountCode: "code-123456789", // optional 
        reference: "field-trip", // optional
        paymentParameters: "expires-in-30-days", // optional
        invoiceId: "pt_inv_XXXXXXXXX", // optional
        sendReceipt: true, // optional 
        receiptDescription: "School Technology Fees" // optional
        recurringId: "pt_rec_XXXXXXXXX", // optional
}

myPayTheory.activateCardPresentDevice(CARD_PRESENT_PARAMS)
```

These are the values that you can pass into the `transact` function to customize the payment.  
The only required key is `amount`.

- `amount`: (Int)
  - represents the amount to be charged in cents


- `payorInfo`: (Object)
  - see the [Payor Info object](#payor-info-object) below for details


- `metadata`: (Object)
  - An object that will be stored with the transaction and can be used to track the payment.


- `feeMode`: (String)
  - Defaults to `window.paytheory.INTERCHANGE`. If available to merchant and set to `window.paytheory.SERVICE_FEE` the fee will be added to the amount and charged to the payor. More details about the fee modes in your PayTheory Portal.


- `fee`: (Int)
  - Represents the fee to be charged in cents.
  - If you are using `SERVICE_FEE` mode and want to skip the confirmation step, you must provide the fee amount. This will be validated to make sure it matches the fee amount that would be charged. If the fee amount does not match, an error will be thrown.


- `confirmation`: (Boolean)
  - Defaults to `false`. If set to `true` the payment will return a response to the tokenizeObserver before it needs to be confirmed. Required if using `SERVICE_FEE` fee mode.


- `accountCode`: (String)
  - Code that can be used to track a payment or group of payments. Will be included in the transaction schema and in the PayTheory Portal.


- `reference`: (String)
  - Custom description assigned to a payment. Will be included in the transaction schema and in the PayTheory Portal.


- `paymentParameters`: (String)
  - The payment parameters to use for the payment.
  - For more information on payment parameters check out the [Payment Parameters](payment-parameters) documentation.


- `payorId`: (String)
  - The PayTheory payor ID to use for the payment. Allows for user to manage identities.
  - This cannot be used if also using the `payorInfo` parameter.


- `invoiceId`: (String)
  - The PayTheory invoice ID to use for the payment. Allows for user to assign a payment to an invoice.


- `recurringId`: (String)
  - The PayTheory recurring ID to use for the payment. Allows for user to assign a payment to a recurring payment.
  - If you pass in a recurring ID, the transactions amount must be an interval of the recurring payments amount per payment.


- `sendReceipt`: (Boolean)
  - Pass `true` to send a receipt to the payor. Must have an email address on the payorInfo object or pass in a payorId that has an email address tied to it.


- `receiptDescription`: (String)
  - Description to be included in the receipt. Defaults to "Payment from {merchant name}".
  - For more info on receipts check out the [Receipts](email-receipts) documentation.

Once the function is called you should receive a response to the `cardPresentObserver`.


## Payor Info Object

This data will be used to create a payor in PayTheory system that will be represented by a Payor ID.

- **first_name**: (String) The first name of the payor
- **last_name**: (String) The last name of the payor
- **email**: (String) The email address of the payor
- **phone**: (String) The phone number of the payor
- **personal_address**: (Object) The address of the payor
  - **line1**: (String) The street address of the payor
  - **line2**: (String) The street address of the payor
  - **city**: (String) The city of the payor
  - **region**: (String) The region (state) of the payor
  - **postal_code**: (String) The postal code of the payor
  - **country**: (String) The country of the payor

```json
{
  "first_name": "Some",
  "last_name": "Body",
  "email": "somebody@paytheory.com",
  "phone": "3335554444",
  "personal_address": {
    "city": "Somewhere",
    "country": "USA",
    "region": "OH",
    "line1": "123 Street St",
    "line2": "Apartment 17",
    "postal_code": "12345"
  }
}
```

You can also optionally pass in a boolean to use the same info that is passed into the card address fields if you want to avoid collecting this information twice on a card transaction.

When you pass this boolean in as true the only other fields allowed to be passed in are the email and phone fields. It will error if you pass in any other fields.

This only works when you run the `transact` function.

```json
{
  "same_as_billing": true,
  "phone": "3335554444",
  "email": "somebody@paytheory.com"
}
```