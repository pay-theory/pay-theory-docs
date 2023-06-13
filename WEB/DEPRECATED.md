# Deprecations


## Create and Mount

These functions have been replaced with the [payTheoryFields](web/functions#paytheoryfields) function. You no longer need to run `create` and `mount` separately. Instead, you can run `payTheoryFields` and pass in the API key and styles object.

### create

The `create` function is used to initialize the Pay Theory object. This function takes two arguments, the API key and the [styles object](hosted_fields#styling-hosted-fields).

```javascript
const API_KEY = "YOUR_API_KEY"
const STYLES = {
    ...style_options
}

const myPayTheory = await window.paytheory.create(API_KEY, STYLES)
```

This function returns a promise that resolves to the Pay Theory object containing the functions listed below and all available [Event Listeners](event_listeners).

### mount

The `mount` function is used to mount the Hosted Fields to the DOM. This function takes an object as an argument.

```javascript
const customPlaceholders = {
    "card-number": "Card Number",
    "card-cvv": "CVV",
    "card-exp": "MM/YY"
}

myPayTheory.mount({
    placeholders: customPlaceholders
})
```

In the object you can pass in the following properties:

- **placeholders**: An object that contains any custom placeholders you would like to use for the fields. These keys can be used to customize the placeholders for the following fields:
  - **card-number**
  - **card-cvv**
  - **card-exp**
  - **card-name**
  - **billing-line1**
  - **billing-line2**
  - **billing-city**
  - **billing-state**
  - **billing-zip**
  - **routing-number**
  - **account-number**
  - **account-name**
  - **cash-name**
  - **cash-contact**

  *Use these values to customize the labels for the Account Type radio button*
  - **checking**
  - **savings**


## Event Listeners

These event listeners are no longer needed since the [transact](functions#transact), [tokenizePaymentMethod](functions#tokenizepaymentmethod), and capture functions will return a promise that resolves to the data you need.

### tokenizeObserver

The `tokenizeObserver` will fire when a function has tokenized a payment when the `transact` function is called with `true` passed in to the `require_confirmation` parameter.

```javascript
myPayTheory.tokenizeObserver(token => {
    // Logic to respond to payment tokenization
})
```

#### Callback Argument

You will be passed back an object with the following properties:

*note that the service fee is included in amount*

- **first_six** (String): The first six digits of the card number
- **last_four**: (String) The last four digits of the card number or account number
- **amount** (Int): The amount of the transaction
- **service_fee** (Int): The service fee of the transaction
- **receipt_number** (String): The Pay Theory receipt number


### captureObserver

The `captureObserver` will fire when the `capture` function has captured a payment following a successful tokenized payment.

```javascript
myPayTheory.captureObserver(capture => {
    // Logic to respond to payment capture
})
```

#### Callback Argument

You will be passed back an object with the following properties:

A successful capture will return the following properties:

*note that the service fee is included in amount*

- **last_four** (String): The last four digits of the card number or account number
- **amount** (Int): The amount of the transaction
- **service_fee** (Int): The service fee of the transaction
- **receipt_number** (String): The Pay Theory receipt number
- **brand** (String): The brand of the card
- **created_at** (String): The date and time the transaction was created
- **state** (String): The status of the transaction
- **metadata** (JSON): The metadata of the transaction
- **payor_id** (String): The Pay Theory id for the payor that was used for the transaction
- **payment_method_id** (String): The Pay Theory id for the payment method token

If a failure or decline occurs during the payment, the response will be similar to the following:

- **receipt_number** (String): The Pay Theory receipt number
- **last_four** (String): The last four digits of the card number or account number
- **brand** (String): The brand of the card
- **state** (String): The state of the transaction
    - This will be `FAILURE`
- **type** (String): Description of the failure

### transactedObserver

The `transactedObserver` will fire with a response after the `transact` function is called without the `require_confirmation` parameter passed in or set to `false`.

This will also fire with a response after calling the `tokenizePaymentMethod`

```javascript
myPayTheory.transactedObserver(result => {
    // Logic to respond when you receive the transaction result
})
```

If you called the `transact` function you will be passed back an object with the following properties:

A successful capture will return the following properties:

*note that the service fee is included in amount*

- **last_four** (String): The last four digits of the card number or account number
- **amount** (Int): The amount of the transaction
- **service_fee** (Int): The service fee of the transaction
- **receipt_number** (String): The Pay Theory receipt number
- **brand** (String): The brand of the card
- **created_at** (String): The date and time the transaction was created
- **state** (String): The status of the transaction
- **metadata** (JSON): The metadata of the transaction
- **payor_id** (String): The Pay Theory id for the payor that was used for the transaction
- **payment_method_id** (String): The Pay Theory id for the payment method token

If a failure or decline occurs during the payment, the response will be similar to the following:

- **receipt_number** (String): The Pay Theory receipt number
- **last_four** (String): The last four digits of the card number or account number
- **brand** (String): The brand of the card
- **state** (String): The state of the transaction
    - This will be `FAILURE`
- **type** (String): Description of the failure

If you called the `tokenizePaymentMethod` function you will be passed back an object with the following properties:

- **payment_method_id** (String): The Pay Theory id for the payment method token
- **payor_id** (String): The Pay Theory id for the payor that was used for the payment method token
- **last_four** (String): The last four digits of the card number or account number
- **brand** (String): The brand of the card
- **expiration** (String): The expiration date of the card formatted as MMYY
- **payment_type** (String): The type of payment method tokenized
    - This will be `card` or `ach`
- **metadata** (JSON): The metadata that was passed in when tokenizing the payment method

#### Callback Argument


### cashObserver

The `cashObserver` will fire when a barcode has been generated after the `transact` function is called using the cash fields.

```javascript
myPayTheory.cashObserver(result => {
    // Logic to respond when you receive the cash barcode result
})
```

#### Callback Argument

While generating the Barcode it will use the geoloaction to return a map url for the users specific location.

If this is the first time it has been requested the user will have the opportunity to accept or decline the request.

You will be passed back an object with the following properties:

- **barcodeUrl** (String): The url for the barcode image
- **mapUrl** (String): The url for the map to find retail locations to pay the barcode


## initTransaction

This function was deprecated and you should use `transact` in its place.

The initTransaction function is used to submit a payment to Pay Theory.

```javascript
myPayTheory.initTransaction(amount, payorInfo, confirmation)
```

### Arguments

- **amount** (Int): The amount of the transaction
- **payorInfo** (Object): The payor information. You can see more on the payorInfo object [here](functions#payor-info-object)
- **confirmation** (Boolean): Whether to require confirmation of the payment. If this is set to `true` then the `tokenizeObserver` will fire when the payment is tokenized and the `captureObserver` will fire when the payment is captured. If this is set to `false` then the `transactedObserver` will fire when the payment is completed.