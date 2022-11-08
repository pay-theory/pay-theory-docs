# Input Views

These SwiftUI views are used to collect payment information from the user. They are designed to be used with a Pay Theory Object to make payments and tokenize payment methods.

## PTForm

The PTForm view is used to wrap the inputs. It allows the inputs to talk to each other and validate the data.

The PTForm needs to be passed the Pay Theory object as an environment object.

As long as the PTForm wraps a higher level view containing the inputs, the inputs will be able to communicate with each other.

```swift
let payTheory = PayTheory(apiKey: "YOUR_API_KEY")

PTForm {
    PTCardName()
    PTCombinedCard()
    PTCardPostalCode()
}
.environmentObject(payTheory)
```

## Card Inputs

These inputs are used to collect card information from the user.

### Separate Card Inputs

These TextField inputs are used to collect the required info from the user for a valid card payment.

```swift
PTCardNumber()
PTExp()
PTCvv()
PTCardPostalCode()
```

### Combined Card Input

These inputs are used to collect the required info from the user for a valid card payment while combining the card number and expiration date and CVV into one input.

```swift
PTCombinedCard()
PTCardPostalCode()
```

### Billing Address Inputs

These inputs are used to collect Billing Address information from the user.

```swift
PTCardLineOne()
PTCardLineTwo()
PTCardCity()
PTCardRegion()
PTCardPostalCode()
PTCardCountry()
```

## ACH Inputs

These inputs are all required to collect ACH information from the user.

```swift
PTAchAccountName()
PTAchAccountNumber()
PTAchRoutingNumber()
PTAchAccountType()
```

## Cash Inputs

These inputs are all required to collect Cash information from the user.

```swift
PTCashName()
PTCashContact()
```

## Payor Inputs

These inputs are available if you want to collect info about the payor from the user.

```swift
PTPayorFirstName()
PTPayorLastName()
PTPayorEmail()
PTPayorPhone()
PTPayorLineOne()
PTPayorLineTwo()
PTPayorCity()
PTPayorRegion()
PTPayorPostalCode()
PTPayorCountry()
```

## Customizing Placeholder Text

You can customize the placeholder on all TextFields by setting the `placeholder` property on the field's initializer.

```swift
PTCardPostalCode(placeholder: "Zip")
```

The combined card has the ability to pass in separate placeholders for the card number, expiration date, and cvv.

```swift
PTCombinedCard(
    numberPlaceholder: "Number",
    expPlaceholder: "EXP",
    cvvPlaceholder: "CVC"
)
```