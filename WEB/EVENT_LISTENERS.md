# Event Listeners

The Pay Theory SDK provides a set of event listeners that can be used to respond messages from the hosted fields and inside the SDK. 

The `errorObserver` is available at the root of our SDK.

The rest of the listeners are available on the object returned by the `create` function.

Each of these listeners takes a callback function as an argument. Below we will detail the arguments that are passed to each callback function.

## readyObserver

The `readyObserver` will fire when the hosted fields are ready to be used. This is where you should add any logic that should be executed when the hosted fields are ready.

```javascript
myPayTheory.readyObserver(ready => {
    // Logic to respond when the fields are ready
})
```

### Callback Argument

When the fields are ready it will pass a boolean value of `true` to the callback function.

## errorObserver

The `errorObserver` will fire when an error occurs anywhere inside the Pay Theory SDK. This is a good place to add any logic that should only be executed when an error occurs.

```javascript
window.paytheory.errorObserver(error => {
    // Logic to respond to errors
})
```

### Callback Argument

The callback will be passed a string indicating what happened in the SDK. The string should start with one of the following error types:

- **FIELD_ERROR**: Issue with fields on the DOM when mounting
- **NO_TOKEN**: There was an error fetching the auth token when initializing the SDK
- **NO_FIELDS**: There were no fields found when mounting
- **NOT_VALID**: The fields are not yet valid when trying to submit a transaction or tokenize
- **INVALID_PARAM**: Parameters used to transact or tokenize are not valid parameters
- **SESSION_EXPIRED**: The SDK session has expired and is unable to send messages to Pay Theory

Most errors will require the user to refresh the page and try again.

The exception to this is the NOT_VALID error which will require the user to change the data in the payment fields until you get a proper response to the valid observer and then you may try and transact or tokenize again.

## tokenizeObserver

The `tokenizeObserver` will fire when a function has tokenized a payment when the `transact` function is called with `true` passed in to the `require_confirmation` parameter.

```javascript
myPayTheory.tokenizeObserver(token => {
    // Logic to respond to payment tokenization
})
```

### Callback Argument

You will be passed back an object with the following properties:

*note that the service fee is included in amount*

- **first_six** (String): The first six digits of the card number
- **last_four**: (String) The last four digits of the card number or account number
- **amount** (Int): The amount of the transaction
- **service_fee** (Int): The service fee of the transaction
- **receipt_number** (String): The Pay Theory receipt number


## captureObserver

The `captureObserver` will fire when the `capture` function has captured a payment following a successful tokenized payment.

```javascript
myPayTheory.captureObserver(capture => {
    // Logic to respond to payment capture
})
```

### Callback Argument

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

## transactedObserver

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

### Callback Argument


## cashObserver

The `cashObserver` will fire when a barcode has been generated after the `transact` function is called using the cash fields.

```javascript
myPayTheory.cashObserver(result => {
    // Logic to respond when you receive the cash barcode result
})
```

### Callback Argument

While generating the Barcode it will use the geoloaction to return a map url for the users specific location.

If this is the first time it has been requested the user will have the opportunity to accept or decline the request.

You will be passed back an object with the following properties:

- **barcodeUrl** (String): The url for the barcode image
- **mapUrl** (String): The url for the map to find retail locations to pay the barcode

[//]: # (## cardPresentObserver)

[//]: # ()
[//]: # (The `cardPresentObserver` will fire with responses for the card present flow. You will receive a message when the fields are ready, when the terminal is active, and when the terminal is complete with the transaction.)

[//]: # ()
[//]: # (```javascript)

[//]: # (myPayTheory.cardPresentObserver&#40;result => {)

[//]: # (    // Logic to respond when you receive the card present message)

[//]: # (}&#41;)

[//]: # (```)

[//]: # ()
[//]: # (### Callback Argument)

[//]: # ()
[//]: # (You will be passed back an object with the following properties:)

[//]: # ()
[//]: # (**status** &#40;String&#41;: The status of the card present flow)

[//]: # (- `READY` - The hosted element is ready to activate the terminal)

[//]: # (- `ACTIVATED` - The terminal has been activated and is ready to accept a card)

[//]: # (- `COMPLETE` - The terminal has completed the interaction)

[//]: # ()
[//]: # (**details**: The details that come with a card present status)

[//]: # ()
[//]: # (`READY` - Nothing will be returned)

[//]: # ()
[//]: # (`ACTIVATED` - String of the `transaction_id`)

[//]: # ()
[//]: # (`COMPLETE` - The details of the transaction as follows:)

[//]: # ()
[//]: # (- **status** &#40;String&#41;: The status of the transaction)

[//]: # (  - `PENDING` or `SUCCEEDED` - The transaction was successful)

[//]: # (  - `FAILURE` - The transaction failed or the connection was aborted or timed out)

[//]: # (- **amount** &#40;Int&#41;: The amount of the transaction)

[//]: # (- **card_brand** &#40;String&#41;: The brand of the card)

[//]: # (- **last_four** &#40;String&#41;: The last four digits of the card number)

[//]: # (- **service_fee** &#40;Int&#41;: The service fee of the transaction)

[//]: # (- **currency** &#40;String&#41;: The currency of the transaction)

[//]: # (- **transaction_id** &#40;String&#41;: The Pay Theory transaction id)

[//]: # (- **created_at** &#40;String&#41;: The date and time the transaction was created)

[//]: # (- **failure_reason** [String]: The reason for the failure)

[//]: # (  - This will only be returned if the transaction failed)
