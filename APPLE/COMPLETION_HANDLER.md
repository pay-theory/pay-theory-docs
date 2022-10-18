# Completion Handler

The completion handler is a function that will receive any success or failure responses from Pay Theory. You can set the completion handler on the `completion` value of the `PayTheory` object.

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

The completion handler should expect `Result<[String: Any], FailureResponse>` as an input and return `Void`.

The `Result` type is an enum that can be either a success or a failure. 

The success case will contain a `[String: Any]` dictionary of the response from Pay Theory. 

The failure case will contain a `FailureResponse` object which is a class defined in our SDK.


## Success Response

The success response will be a `[String: Any]` dictionary. The dictionary will contain the following keys:

* `type`: String - The type of the message coming back from Pay Theory. It will be one of the following:
    * `PT_CONFIRMATION`
    * `PT_COMPLETE`
    * `PT_BARCODE`
    * `PT_TOKENIZE`

* `body`: [String: Any] - The specific contents of the message which are detailed below.

Each string value for the `type` is available as a constant in the SDK.

### PT_CONFIRMATION

This response you get back after calling `transact` if requiring confirmation. The `body` will contain the following keys:

*Note that the service fee is included in amount*

```swift
[   
    "receipt_number": String,
    "amount": Int,
    "service_fee": Int,
    "brand": String,
    "first_six": String,
    "last_four": String
]
```

### PT_COMPLETE

This response you get back after calling `transact` if not requiring confirmation or calling `confirm`. The `body` will contain the following keys:

*Note that the service fee is included in amount*

```swift
[   
    "receipt_number": String,
    "last_four": String,
    "created_at": String,
    "amount": Int,
    "service_fee": Int,
    "state": String,
    "metadata": [String: Any],
    "payor_id": String,
    "payment_method_id": String,
    "brand": String
]
```

### PT_BARCODE

This response you get back after calling `transact` if the payment method is `cash`. The `body` will contain the following keys:

```swift
[   
    "BarcodeUid": String,
    "Merchant": String,
    "barcode": String,
    "barcodeFee": String,
    "barcodeUrl": String,
    "mapUrl": String
]
```

### PT_TOKENIZE

This response you get back after calling `tokenize`. The `body` will contain the following keys:

```swift
[   
    "payment_method_id": String,
    "payor_id": String,
    "last_four": String,
    "brand": String,
    "metadata": [String: Any],
    "expiration": String,
    "payment_type": String
]
```


## Failure Response

The failure response will be a `FailureResponse` object. The object will contain the following properties:

* `type`: String
* `last_four`: String?
* `brand`: String?
* `state`: String?
* `receipt_number`: String?

If the FailureResponse is a failed transaction that made it to the processor but had a failure due to the instrument all these values will be present.

If it is any other type of error coming back from the SDK it will only have a `type` value.

The `type` value will begin with one of the following if not a transaction failure:

* `NO_AUTH` - Error you get back if you try to call `confirm` without calling `transact` first or have already called `cancel`.
* `NO_FIELDS` - Error you get when you try to call `transact` or `tokenizePaymentMethod` without have valid fields that are visible on the screen.
* `NO_TOKEN` - Error you will get if the call to fetch the token needed to connect to the socket fails.
* `SOCKET_ERROR` - Error that is returned from the socket.
* `INVALID_APP` - Error you will get if the app fails app attestation.
* `SESSION_EXPIRED` - Error you will get if the session has expired and the socket disconnects.


## Example Handler

```swift
func completion(result: Result<[String: Any], FailureResponse>) -> Void {
        switch result {
        case .success(let token):
            let type = token["type"] as? String ?? ""
            let body = token["body"] as? [String: Any] ?? [:]
            switch type {
                case PT_CONFIRMATION:
                    // Handle confirmation
                case PT_COMPLETE:
                    // Handle complete
                case PT_BARCODE:
                    // Handle barcode
                case PT_TOKENIZE:
                   // Handle tokenize
                default:
                    // Handle unknown, which should never happen
            }
        case .failure(let error):
            if let receiptNumber = error.receipt_number {
                // Logic for handling a transaction failure
            } else {
                // Logic for handling a non-transaction failure
            }
    }
```