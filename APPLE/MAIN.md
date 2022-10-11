# Pay Theory Apple SDK

This is the Pay Theory Apple SDK. It is a collection of Swift UI inputs and utilities to help you integrate Pay Theory into your app.

## Importing the SDK

To add PayTheory as dependency to your Xcode project, select `File` > `Add Packages...`, enter its repository URL: `https://github.com/pay-theory/pay-theory-ios.git` and import `PayTheory`.

Then, to use it in your source code, add:

```swift
import PayTheory
```

## Step 1: Initialize PayTheory Object

To initialize the PayTheory object, you need to provide your API key. You can find this in the Pay Theory Merchant dashboard under `Settings`.

You also need to set your completion handler on `completion` value of the PayTheory object. This is where you will receive any responses from Pay Theory.

*We recommend you set the completion handler in `onAppear`, `viewDidAppear`, or `viewWillAppear` to allow referring to self in the completion handler.*

```swift
let payTheory = PayTheory(apiKey: "YOUR_API_KEY")

payTheory.completion = { result in
    switch result {
    case .success(let response):
        print(response)
    case .failure(let error):
        print(error)
    }
}
```

## Step 2: Place Inputs in Your View

You need to place the inputs into your view wrapped in a `PTForm` view. The `PTForm` view allows the inputs to talk to each other and validate the data.

You also need to pass your PT object to the `PTForm` view as an environment object.

```swift
PTForm {
    PTCardName()
    PTCombinedCard()
    PTCardPostalCode()
}
.environmentObject(payTheory)
```

## Step 3: Check for validity

You can check if the inputs have collected enough information to create a payment by checking the `valid` property on the `PayTheory` object.

You can check to see if `card`, `ach`, or `cash` inputs are valid by checking the `valid` property.

Set submit payment button to disabled if not valid.

```swift
Button("Make Payment") {
    // Submit payment logic
}
.disabled(!payTheory.valid.card)
``` 

## Step 4: Submit Transaction

You can submit a transaction by calling the `transact` method on the `PayTheory` object. This method returns a `Payment` object.

```swift
Button("Make Payment") {
    payTheory.transact(amount: 3300, payorId: "pt_payor_id")
}
.disabled(!payTheory.valid.card)
```

## Step 5: Handle Response

The response from Pay Theory will be sent to the completion handler you set in step 1. A successful response will return a `[String: Any]` dictionary. A failure response will return a `FailureResponse` object.

The `Failure Response` object is defined [here](completion_handler#failure-response) and the success response dictionary is defined [here](completion_handler#success-response).


