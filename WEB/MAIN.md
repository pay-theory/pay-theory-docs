# Pay Theory Javascript SDK

This is the Pay Theory Web SDK. It is a collection of hosted fields, event listeners, and functions to help you integrate Pay Theory into your webpage.

Here is some steps to get you started with a basic implementation.

## Importing the SDK

To use the Pay Theory Web SDK, you need to add this script to your web header:

```html
<script src="https://PARTNER_NAME.sdk.STAGE.com/index.js"></script>
```

## Step 1: Add Pay Theory Elements to Your Form

You need to add the Pay Theory elements to your form. You can place these elements anywhere in your form and the SDK will place the hosted fields in the correct place.

```html
<form>
...
<div id="pay-theory-credit-card"></div>
<div id="pay-theory-credit-card-zip"></div>
...
</form>
```

## Step 2: Initialize Pay Theory Object

The SDK will be available in the window as follows:

```javascript
window.paytheory
```

You would then need to initialize the Pay Theory object with your API key. You can find this in the Pay Theory Merchant dashboard under `Settings`.

```javascript
const API_KEY = "YOUR_API_KEY"
const STYLES = {
    ...style_options
}

window.paytheory.errorObserver(error => {
    // Logic to respond to errors
})

const myPayTheory = await window.paytheory.create(
        API_KEY,
        STYLES)
```

## Step 3: Mount Fields and Add Event Listeners

After you have initialized the Pay Theory object, you can mount the fields and add event listeners.

The [mount](web/functions#mount) function will mount all the hosted fields with any styles you passed into the create function.

[Event Listeners](web/event_listeners) will listen for any messages from the hosted fields.

```javascript
// Mount the fields
myPayTheory.mount()

// Add event listeners
myPayTheory.readyObserver(ready => {
    // Logic to respond when the fields are ready
})

myPayTheory.validObserver(valid => {
    // Logic to respond when the form is valid
})

myPayTheory.stateObserver(state => {
    // Logic to respond to state changes
})

myPayTheory.transactedObserver(result => {
    // Logic to respond when you receive the transaction result
})
```

## Step 4: Submit Payment once Form is Valid

Use [State Listeners](web/state_listeners) to tell when the hosted fields are valid and you can submit the payment.

The only required field for the [transact function](web/functions#transact) is the `amount` field.

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

const FEE_MODE = window.paytheory.MERCHANT_FEE

// optionally provide custom metadata to help track payments
const PAYMENT_METADATA = {
  "pay-theory-account-code": "code-123456789",
  "pay-theory-reference": "field-trip"
};

// optional parameter to require confimation step for Card or ACH
const REQUIRE_CONFIRMATION = true

// Parameters that you will pass into the transact function. More details below.
const TRANSACTING_PARAMETERS = { 
        amount: AMOUNT, 
        payorInfo: PAYOR_INFO, // optional
        payorId: "pt_pay_XXXXXXXXX", // optional
        metadata: PAYMENT_METADATA, // optional 
        feeMode: FEE_MODE, // optional
        fee: 100, // optional
        confirmation: REQUIRE_CONFIRMATION, // optional 
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

## Step 5: Handle the Result In the transactedObserver

Once a payment completes the `transactedObserver` will be called with the result of the transaction. You can then handle the result as you see fit.
