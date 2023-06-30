# Payment QR Code

This is a function initializes a payment session based on the `checkoutDetails` you pass in. It then generates a QR Code that can be scanned to open a checkout page and accept a payment.


## qrCode

```javascript
//Amount passed in is in cents
const AMOUNT = 1000

const PAYMENT_METADATA = {
  "student-name": "Jane Doe"
};

// Parameters that you will pass in to configure the checkout page that opens when the qrCode is scanned.
const CHECKOUT_DETAILS = { 
        amount: AMOUNT, 
        paymentName: "School Technology Fees",
        paymentDescription: "Technology Fee for the 2019-2020 school year", 
        requirePhone: true, 
        callToAction: paytheory.DONATE, 
        acceptedPaymentMethods: paytheory.CARD_ONLY, 
        payorId: "pt_pay_XXXXXXXXX", 
        metadata: PAYMENT_METADATA,  
        feeMode: paytheory.MERCHANT_FEE, 
        accountCode: "code-123456789",  
        invoiceId: "pt_inv_XXXXXXXXX", 
        recurringId: "pt_rec_XXXXXXXXX", 
}

const OPTIONS = {
  apiKey: "PT_API_KEY",
  checkoutDetails: CHECKOUT_DETAILS,
  size: 150,
  onReady: () => {},
  onError: () => {},
  onSuccess: () => {}
}

paytheory.qrCode(OPTIONS)
```

These are the values that you can pass into the `qrCode` function to customize the payment session.  
You pass a single object into the function with the following keys.

**Required Values**
- `apiKey`: (String)
    - The API key for your Pay Theory account. You can find this in your Pay Theory Portal.


- `checkoutDetails`: (Object)
    - The details for the checkout page that opens when the qrCode is scanned. Details Below.

**Optional Values**
- `size`: (Int)
    - This is the size, height and width, of the qrCode in pixels. Defaults to `128` and must be above `128` and below `300`.


- `onReady`: (Function)
    - A function that will be called when the qrCode is ready to be displayed.


- `onError`: (Function)
    - A function that will be called when an error occurs. It is passed an error string.


- `onSuccess`: (Function)
    - A function that will be called when the payment is successful. It is passed an object with the following values.
        - **last_four** (String): The last four digits of the card number or account number
        - **amount** (Int): The amount of the transaction **(service fee is included)**
        - **service_fee** (Int): The service fee of the transaction
        - **receipt_number** (String): The Pay Theory receipt number
        - **brand** (String): The brand of the card
        - **created_at** (String): The date and time the transaction was created
        - **state** (String): The status of the transaction
        - **metadata** (JSON): The metadata of the transaction
        - **payor_id** (String): The Pay Theory id for the payor that was used for the transaction
        - **payment_method_id** (String): The Pay Theory id for the payment method token

**Checkout Details**

These are the values that you can pass into the `checkoutDetails` object to customize the checkout page that opens when the qrCode is scanned.

**Required Values**
- `amount`: (Int)
    - The amount of the payment in cents.


- `paymentName`: (String)
    - The name of the payment that will be displayed on the checkout page. Will also be passed in to the `reference` field of the transaction.

**Optional Values**
- `paymentDescription`: (String)
    - The description of the payment that will be displayed on the checkout page.


- `requirePhone`: (Boolean)
    - Pass `true` to require the user to enter a phone number on the checkout page.


- `callToAction`: (String)
    - The call to action that will be displayed on the payment button. Defaults to "PAY".
    - Constants are available from the SDK.
    - Available options are `paytheory.DONATE`, `paytheory.PAY` and `paytheory.BOOK`


- `acceptedPaymentMethods`: (String)
    - The payment methods that will be accepted for the payment. Defaults to `paytheory.ALL`.
    - Constants are available from the SDK.
    - Available options are `paytheory.ONLY_CARD`, `paytheory.ONLY_ACH`, `paytheory.ONLY_CASH`, `paytheory.NOT_CARD`, `paytheory.NOT_ACH`, `paytheory.NOT_CASH`, and `paytheory.ALL`


- `payorId`: (String)
    - The Pay Theory id for the payor that will be used for the payment. If this is not passed in a new payor will be created.


- `metadata`: (JSON)
    - The metadata that will be passed in to the transaction.


- `feeMode`: (String)
    - The fee mode that will be used for the payment. Defaults to `paytheory.FIXED`.
    - Constants are available from the SDK.
    - Available options are `paytheory.SERVICE_FEE` and `paytheory.MERCHANT_FEE`


- `accountCode`: (String)
    - The account code that will be used for the payment.


- `invoiceId`: (String)
    - The Pay Theory invoice ID to use for the payment. Allows for user to assign a payment to an invoice.


- `recurringId`: (String)
    - The Pay Theory recurring ID to use for the payment. Allows for user to assign a payment to a recurring payment.
    - If you pass in a recurring ID, the transactions amount must be an interval of the recurring payments amount per payment.


## Payor Info Object

This data will be used to create a payor in Pay Theory system that will be represented by a Payor ID.

- **first_name**: (String) The first name of the payor
- **last_name**: (String) The last name of the payor
- **email**: (String) The email address of the payor
- **phone**: (String) The phone number of the payor
- **personal_address**: (Object) The address of the payor
    - **line1**: (String) The street address of the payor
    - **line2**: (String) The street address of the payor
    - **city**: (String) The city of the payor
    - **region**: (String) The region (state) of the payor
    - **postal_code**: (String) The postal code of the payor
    - **country**: (String) The country of the payor

```json
{
  "first_name": "Some",
  "last_name": "Body",
  "email": "somebody@paytheory.com",
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
```

You can also optionally pass in a boolean to use the same info that is passed into the card address fields if you want to avoid collecting this information twice on a card transaction.

When you pass this boolean in as true the only other fields allowed to be passed in are the email and phone fields. It will error if you pass in any other fields.

This only works when you run the `transact` function.

```json
{
  "same_as_billing": true,
  "phone": "3335554444",
  "email": "somebody@paytheory.com"
}
```