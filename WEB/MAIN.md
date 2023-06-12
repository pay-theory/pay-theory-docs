# Pay Theory Javascript SDK

This is the Pay Theory Web SDK. It is a collection of hosted fields, event listeners, and functions to help you integrate Pay Theory into your webpage.

Here is some steps to get you started with a basic implementation.

## Step 1: Importing the SDK

To use the Pay Theory Web SDK, you need to add this script to your web header:

```html
<script src="https://PARTNER_NAME.sdk.STAGE.com/index.js"></script>
```

The SDK will be available on the window object as `paytheory`.

It can be accessed as follows:

```javascript
paytheory
//or
window.paytheory
```

## Step 2: Add Pay Theory Elements to Your Form

You need to add the Pay Theory elements to your form. You can place these elements anywhere in your form and the SDK will place the hosted fields in the correct place.

```html
<form>
...
<div id="pay-theory-credit-card"></div>
<div id="pay-theory-credit-card-zip"></div>
...
</form>
```

## Step 3: Set up the event listeners

You would then need to call any event listeners before you initialize the Pay Theory fields.

A full list of event listeners can be found [here](web/event_listeners).

```javascript
paytheory.errorObserver(error => {
    // Logic to respond to errors
})
```

## Step 4: Initialize the Pay Theory Fields

After you have set up all the event listeners, you can initialize the Pay Theory fields by calling the `payTheoryFields` function.


```javascript
const API_KEY = 'YOUR_API_KEY'

paytheory.payTheoryFields({
  apiKey: API_KEY
})
```

## Step 5: Once the fields are valid, run transaction

Use the [valid observer](web/event_listeners#validobserver) to tell when the hosted fields are valid, so you can submit the payment.

The only required field for the `transact` parameters is the `amount` field.

You can customize the `transact` function more. Check out the full list of parameters [here](web/functions#transact).

```javascript
//Amount passed in is in cents
const AMOUNT = 1000

// Parameters that you will pass into the transact function. More details below.
const TRANSACTING_PARAMETERS = { 
        amount: AMOUNT
}

let result = await paytheory.transact(TRANSACTING_PARAMETERS)

if (result.type === "SUCCESS") {
    // Logic to respond to successful transaction
} else if (result.type === "FAILURE") {
    // Logic to respond to failed transaction
} else if (result.type === "ERROR") {
    // Logic to respond to error
}
```
