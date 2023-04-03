# Functions

These are the functions available to you in the Pay Theory SDK. These allow you to configure a payment or tokenize a payment method.

These functions return a response via the Payable handlers you add to your Activity.

## configure

This function is used to configure fields to submit a payment to Pay Theory.

```Kotlin
payTheoryFragment.configure(apiKey = apiKey, amount = 1000)
```

These are the parameters that you can pass into the `configure` function to customize the payment.  

### Required parameters:

* **amount**: Int
    * Represents the amount to be charged in cents.


* **apiKey**: String
  * Your Pay Theory API Key.

### Optional parameters:

* **transactionType**: TransactionType
  * The type of transaction requested.

***by default the SDK provides a simple card input***

```Kotlin
// debit/credit card fields
transactionType = TransactionType.CARD 
```

```Kotlin
// bank account fields
transactionType = TransactionType.BANK 
```

```Kotlin
// cash/barcode fields
transactionType = TransactionType.CASH 
```

* **requireAccountName**: Boolean
  * Require the account name input field for a card or bank payment

***by default the SDK sets requireAccountName to false***

```Kotlin
// will include account name field
requireAccountName = true 
```

```Kotlin
// will not include account name field
requireAccountName = false 
```

* **requireBillingAddress**: Boolean
  * Require full billing address input fields for a card or bank payment

***by default the SDK sets requireBillingAddress to false***

```Kotlin
// will include billing address fields
requireBillingAddress = true 
```

```Kotlin
// will not include billing address fields
requireBillingAddress = false 
```

* **confirmation**: Boolean
  * Require a confirmation step for a card or bank payment

***by default the SDK sets confirmation to false***

```Kotlin
// will include a confirmation step 
confirmation = true 
```

```Kotlin
// will not include a confirmation step 
confirmation = false 
```

* **feeMode**: FeeMode
  * Set the fee mode for a card or bank payment

***By default, FeeMode.MERCHANT_FEE mode is used, FeeMode.SERVICE_FEE mode is available only when enabled by Pay Theory***

```Kotlin
//set fee mode as merchant_fee
feeMode = FeeMode.MERCHANT_FEE
```

```Kotlin
//set fee mode as service fee (enabled by Pay Theory)
feeMode = FeeMode.SERVICE_FEE
```

* **metadata**: HashMap<Any,Any>
  * Track payments with custom metadata for a card or bank payment

```Kotlin
//Set optional metadata configuration
val metadata: HashMap<Any,Any> = hashMapOf(
    "studentId" to "student_1859034",
    "courseId" to "course_1859034"
)

payTheoryFragment.configure(
    apiKey = "API_KEY",
    amount = 1000,
    metadata = metadata
)
```

* **payorInfo**: PayorInfo & Address
  * Additional customer data for a card or bank payment

***Use data models PayorInfo and Address from Pay Theory SDK***

```Kotlin
//Set optional payorInfo configuration
val payorInfo = PayorInfo(
    "John",
    "Smith",
    "john@gmail.com",
    "5135555555",
    Address(
        "10549 Reading Rd",
        "Apt 1",
        "Cincinnati",
        "OH",
        "45241",
        "USA")
)

payTheoryFragment.configure(
    apiKey = "API_KEY",
    amount = 1000,
    payorInfo = payorInfo
)
```

* **payorId**: String
  * Set Pay Theory payor id for a card or bank payment

***By default, a new payorId is created with payorInfo***

```Kotlin
//set payor id
payTheoryFragment.configure(
    apiKey = "API_KEY",
    amount = 1000,
    payorId = "PAYOR_ID"
)
```

* **accountCode**: String
  * Set Pay Theory account code for a card or bank payment

***accountCode is tracked for each transaction in Pay Theory portals***

```Kotlin
//set account code
payTheoryFragment.configure(
    apiKey = "API_KEY",
    amount = 1000,
    accountCode = "ACCOUNT_CODE"
)
```

* **reference**: String
  * Set Pay Theory reference for a card or bank payment

***reference is tracked for each transaction in Pay Theory portals***

```Kotlin
//set reference
payTheoryFragment.configure(
    apiKey = "API_KEY",
    amount = 1000,
    reference = "REFERENCE"
)
```

* **paymentParameters**: String
  * Set Pay Theory payment parameters for a card or bank payment

```Kotlin
//set payment parameter
payTheoryFragment.configure(
    apiKey = "API_KEY",
    amount = 1000,
    paymentParameters = "PAYMENT_PARAMETERS"
)
```

* **invoiceId**: String
  * Set an invoice id to submit the transaction for a Pay Theory invoice

```Kotlin
//set invoice id
payTheoryFragment.configure(
    apiKey = "API_KEY",
    amount = 1000,
    invoiceId = "INVOICE_ID"
)
```

* **sendReceipt**: Boolean
  * Enable receipts for a card or bank payment

***By default, sendReceipt is set to false***

```Kotlin
//set invoice id
payTheoryFragment.configure(
    apiKey = "API_KEY",
    amount = 1000,
    sendReceipt = true,
    receiptDescription = "School Fees"
)
```

* **receiptDescription**: String
  * Enable receipts description to be added

***The default receipt message will send if receiptDescription is not included***

```Kotlin
//set invoice id
payTheoryFragment.configure(
    apiKey = "API_KEY",
    amount = 1000,
    sendReceipt = true,
    receiptDescription = "School Fees"
)
```