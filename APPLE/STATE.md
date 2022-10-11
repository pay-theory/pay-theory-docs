# State Variables

These are variables that are available to you from the Paytheory object to tell when fields have been filled out and if we have valid payment details.

These variables can be used to enable or disable elements in your app.

They are made available to you as `@Published` variables so you can use them in your SwiftUI views.

## Valid

This variable is used to tell if any of the three payment types are valid.

```swift
// Card Valid
payTheory.valid.card

// ACH Valid
payTheory.valid.ach

// Cash Valid
payTheory.valid.cash
```

These ensure that all the required fields to call `transact` or `tokenizePaymentMethod` are filled out and valid.

## Specific isValid and isEmpty Variables

These variables are available to ensure you can tell if a specific field is valid or empty.

This will help you tell if you need to display an error or if you want to style fields based on validity.

Example:

```swift
// Card Number Valid
payTheory.cardNumber.isValid

// Card Number Empty
payTheory.cardNumber.isEmpty
```

* **isValid**: Bool
  * This variable is used to tell if the field passes validation.

* **isEmpty**: Bool
  * This variable is used to tell if the field is has been typed into.

These two variables are available in the paytheory object for each of the following:

* cashName
* cashContact
* accountName
* accountNumber
* routingNumber
* cvv
* exp
* cardNumber
* postalCode