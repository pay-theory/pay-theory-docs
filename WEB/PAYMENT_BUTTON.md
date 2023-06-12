# Payment Button

This is a function initializes a payment session based on the `checkoutDetails` you pass in. It then generates a Button that can be clicked to open a checkout page and accept a payment.

## button
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
        feeMode: paytheory.MERCHANT_FEE, 
        accountCode: "code-123456789",  
        invoiceId: "pt_inv_XXXXXXXXX", 
        recurringId: "pt_rec_XXXXXXXXX", 
}

// Object that will style the payment button
const STYLE_OBJECT = { 
    color: paytheory.WHITE, 
    callToAction: paytheory.DONATE, 
    pill: true, 
    height: "48px"
}

const OPTIONS = {
  apiKey: "PT_API_KEY",
  checkoutDetails: CHECKOUT_DETAILS,
  style: STYLE_OBJECT,
  onReady: () => {},
  onClick: () => {},
  onError: () => {},
  onCancel: () => {},
  onSuccess: () => {},
  onBarcode: () => {}
}

paytheory.button(OPTIONS)
```

These are the values that you can pass into the `button` function to customize the payment session.  
You pass a single object into the function with the following keys.

**Required Values**
- `apiKey`: (String)
    - The API key for your Pay Theory account. You can find this in your Pay Theory Portal.


- `checkoutDetails`: (Object)
    - The details for the checkout page that opens when the button is clicked. Details Below.

**Optional Values**
- `style`: (Object)
    - The style object that will style the payment button. Details Below.


- `onReady`: (Function)
    - A function that will be called when the button is ready to be clicked.


- `onClick`: (Function)
    - A function that will be called when the button is clicked.


- `onError`: (Function)
    - A function that will be called when an error occurs. It is passed an error string.


- `onCancel`: (Function)
    - A function that will be called when the user cancels the payment from the pop-up window.


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


- `onBarcode`: (Function)
    - A function that will be called when a barcode is successfully created and the user closes the window. It is passed an object with the following values.
        - **barcodeUrl** (String): The url for the barcode image
        - **mapUrl** (String): The url for the map to find retail locations to pay the barcode

**Checkout Details**

These are the values that you can pass into the `checkoutDetails` object to customize the checkout page that opens when the button is clicked.

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

**Style Object**  
These are the values that you can pass into the `style` object to customize the payment button.

**Optional Values**

- `color`: (String)
    - The color of the payment button. Defaults to `paytheory.PURPLE`.
    - Constants are available from the SDK.
    - Available options are `paytheory.PURPLE`, `paytheory.WHITE`, `paytheory.BLACK`, and `paytheory.GREY`


- `callToAction`: (String)
    - The call to action that will be displayed on the payment button. Defaults to `paytheory.PAY`.
    - Constants are available from the SDK.
    - Available options are `paytheory.CHECKOUT`, `paytheory.DONATE`, `paytheory.PAY` and `paytheory.BOOK`


- `pill`: (Boolean)
    - Pass `true` to make the payment button a pill shape. Defaults to `false`.


- `height`: (String)
    - The height of the payment button. Defaults to `48px`.
    - min-height is `30px` and max-height is `55px`
