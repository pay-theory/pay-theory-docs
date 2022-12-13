# Checkout Button Quick Start

This guide will walk you through the steps to create a payment button to enable a PayTheory Hosted Checkout experience for your Payors.

## Step 1: Import the SDK

You need to use the Pay Theory Web SDK. This is available by adding this script to your web header:

```html
<script src="https://PARTNER.sdk.STAGE.com/index.js"></script>
```

## Step 2: Add Pay Theory Elements to DOM

You need to add the Pay Theory elements to your DOM. You can place this elements anywhere in your DOM and the SDK will place the button in the correct place.

```html
<div id="pay-theory-checkout-button"></div>
```

## Step 3: Prepare the Checkout Details

You need to put together the details of the checkout. This includes the amount, payment name, and any other details you want to pass to the checkout.

```javascript
//Amount passed in is in cents
const AMOUNT = 1000

const PAYMENT_METADATA = {
  "student-name": "Jane Doe"
};

// Parameters that you will pass in to configure the checkout page that opens when the button is clicked.
const CHECKOUT_DETAILS = { 
        amount: AMOUNT, 
        paymentName: "School Technology Fees",
        paymentDescription: "Technology Fee for the 2019-2020 school year", 
        requirePhone: true, 
        callToAction: paytheory.DONATE, 
        acceptedPaymentMethods: paytheory.CARD_ONLY, 
        payorId: "pt_pay_XXXXXXXXX", 
        metadata: PAYMENT_METADATA,  
        feeMode: paytheory.INTERCHANGE, 
        accountCode: "code-123456789",  
        paymentParameters: "expires-in-30-days", 
        invoiceId: "pt_inv_XXXXXXXXX", 
        recurringId: "pt_rec_XXXXXXXXX", 
}
```

Further details on the checkout details can be found in the [button](../web/functions#button) function details.

## Step 4: Style the Button

You can select styles for the button to match your website.

```javascript
// Object that will style the payment button
const STYLE_OBJECT = { 
    color: paytheory.WHITE, 
    callToAction: paytheory.DONATE, 
    pill: true, 
    height: "48px"
}
```

Further details on the style object can be found in the [button](../web/functions#button) function details.

## Step 5: Create functions to handle actions from the button

You can create functions to handle the actions from the button. These include `onReady`, `onClick`, `onSuccess`, `onError`, and `onCancel`.

```javascript
// Function that will be called when a payment is successful
const ON_SUCCESS = (response) => {
    console.log("Payment Successful")
    console.log(response)
}
```


## Step 6: Create the Button

You can create the button using the `button` function. Pass in the checkout details, style object, and functions to handle the actions.

```javascript
// Create the button
paytheory.button({
    apiKey: "PT_API_KEY",
    checkoutDetails: CHECKOUT_DETAILS,
    style: STYLE_OBJECT,
    onReady: ON_READY,
    onClick: ON_CLICK,
    onSuccess: ON_SUCCESS,
    onError: ON_ERROR,
    onCancel: ON_CANCEL
})
```

The button session will only enable a single payment. If you want to enable multiple payments, you will need to clear the button from the DOM and create a new button.
