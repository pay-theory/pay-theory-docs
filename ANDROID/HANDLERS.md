# Response Handlers

Pay Theory Android SDK includes functions that are implemented via `Payable` interface

Using the Payable interface on your app's activity will allow you to implement its function members.

These function members act as message handlers for your payment, barcode, and tokenization requests.

```Kotlin
class MainActivity : AppCompatActivity() , Payable {
    
    fun handleSuccess(successfulTransactionResult: SuccessfulTransactionResult){
        println(successfulTransactionResult)
    }

    fun confirmation(confirmationMessage: ConfirmationMessage, transaction: Transaction){
        println(confirmationMessage)
    }

    fun handleTokenizeSuccess(paymentMethodToken: PaymentMethodTokenResults){
        println(paymentMethodToken)
    }

    fun handleBarcodeSuccess(barcodeResult: BarcodeResult){
        println(barcodeResult)
    }
    
    fun handleFailure(failedTransactionResult: FailedTransactionResult){
        println(failedTransactionResult)
    }

    fun handleError(error: Error){
        println(error)
    }
}
```

## handleSuccess

The response object received from a successful card or bank payment.

Payment requested via `payTheoryFragment.configure()`

```kotlin
data class SuccessfulTransactionResult (
    @SerializedName("state") val state: String,
    @SerializedName("amount") val amount: String,
    @SerializedName("brand") val brand: String,
    @SerializedName("last_four") val lastFour: String,
    @SerializedName("service_fee") val serviceFee: String,
    @SerializedName("currency") val currency: String,
    @SerializedName("metadata") val metadata: HashMap<Any, Any>?,
    @SerializedName("receipt_number") val receiptNumber: String,
    @SerializedName("created_at") val createdAt: String,
    @SerializedName("payment_method_id") val paymentMethodId: String,
    @SerializedName("payor_id") val payorId: String,
)
```

## confirmation

The confirmation details object received when setting `confirmation=true` in `PayTheoryFragment`.

*Note that the service fee is included in amount*

```kotlin
data class ConfirmationMessage (
        @SerializedName("payment_token") val paymentToken: String,
        @SerializedName("payer_id") val payerId: String?,
        @SerializedName("processor_payment_method_id") val processorPaymentMethodId: String?,
        @SerializedName("merchant_uid") val merchantUid: String?,
        @SerializedName("last_four") val lastFour: String?,
        @SerializedName("first_six") val firstSix: String?,
        @SerializedName("brand") val brand: String?,
        @SerializedName("session_key") val sessionKey: String,
        @SerializedName("processor") val processor: String,
        @SerializedName("expiration") var expiration: String?,
        @SerializedName("idempotency") val idempotency: String,
        @SerializedName("billing_name") val billingName: String?,
        @SerializedName("billing_address") val billingAddress: Address?,
        @SerializedName("amount") val amount: String,
        @SerializedName("currency") val currency: String,
        @SerializedName("fee_mode") val fee_mode: String,
        @SerializedName("fee") var fee: String,
        @SerializedName("processor_merchant_id") val processor_merchant_id: String?,
        @SerializedName("payment_method") val payment_method: String,
        @SerializedName("metadata") val metadata: HashMap<Any, Any>?,
        @SerializedName("pay_theory_data") val pay_theory_data: HashMap<Any, Any>?,
        @SerializedName("payor_info") val payorInfo: PayorInfo?,
        @SerializedName("payor_id") var payor_id: String?,
        @SerializedName("invoice_id") val invoice_id: String?,
        @SerializedName("payment_intent_id") val paymentIntentId: String?,
)
```

## handleTokenizeSuccess

The response object received from a successful payment method tokenization.

Tokenization requested via `payTheoryFragment.tokenize()`

```kotlin
data class PaymentMethodTokenResults (
    @SerializedName("payment_method_id") val paymentMethodId: String,
    @SerializedName("metadata") val metadata: HashMap<Any, Any>?,
    @SerializedName("payor_id") var payor_id: String?,
    @SerializedName("last_four") val lastFour: String?,
    @SerializedName("first_six") val firstSix: String?,
    @SerializedName("brand") val brand: String?,
    @SerializedName("expiration") val expiration: String?,
    @SerializedName("payment_type") val paymentType: String
)
```

## handleBarcodeSuccess

The response object received from a successful barcode request.

Barcode requested via `payTheoryFragment.configure()` and `transactionType = TransactionType.CASH`

```kotlin
data class BarcodeResult (
        @SerializedName("BarcodeUid") val barcodeUid: String,
        @SerializedName("barcodeUrl") val barcodeUrl: String,
        @SerializedName("barcode") val barcode: String,
        @SerializedName("barcodeFee") val barcodeFee: String,
        @SerializedName("Merchant") val merchant: String,
        @SerializedName("mapUrl") val mapUrl : String
)
```

## handleFailure

The response object received from a declined payment.

```kotlin
data class FailedTransactionResult (
        @SerializedName("state") val state: String,
        @SerializedName("brand") val brand: String,
        @SerializedName("last_four") val lastFour: String,
        @SerializedName("receipt_number") val receiptNumber: String,
        @SerializedName("payment_method_id") val paymentMethodId: String,
        @SerializedName("payor_id") val payorId: String,
)
```

## handleError

The response object received from a system error.

```kotlin
data class Error (
        @SerializedName("reason") val reason: String
)
```
