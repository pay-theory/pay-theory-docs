# Pay Theory Android SDK

Pay Theory Android SDK used to help you integrate Pay Theory into your app.

## Importing the SDK

The Pay Theory Android SDK is now available at [Maven Repository](https://repo1.maven.org/maven2/com/paytheory/)

- Add mavenCentral() to your project level `build.gradle` file

```kotlin
buildscript {
    repositories {
        ...
        mavenCentral()
    }
}
```

```kotlin
dependencies {
  implementation 'com.paytheory:pay-theory-android:2.7.2'
}
```

### Update your app level `build.gradle`

- change minSdk version to 26 or higher

- change targetSdk and compileSdk to 33 or higher

```Kotlin
android {
    ...
    compileSdk 33
    ...
    defaultConfig {
        applicationId "com.example.pay_theory_android_sdk_kotlin_demo"
        minSdk 26
        targetSdk 33
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }
```

## Step 1: Add PayTheoryFragment

Add PayTheoryFragment as a fragment in your activity's xml layout file.

```xml
    <fragment
    android:id="@+id/payTheoryFragment"
    android:name="com.paytheory.android.sdk.fragments.PayTheoryFragment"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:layout_constraintBottom_toBottomOf="parent"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toTopOf="parent" />
```

## Step 2: Add PayTheoryFragment configurations to your app's activity

Update your configurations in your activity with PayTheoryFragment.

Set your api key.

```Kotlin
private val apiKey = "API_KEY"
```

Set PayTheoryFragment inside your `onCreate` method.

```Kotlin
val payTheoryFragment = this.supportFragmentManager.findFragmentById(R.id.payTheoryFragment) as PayTheoryFragment
```

Configure PayTheoryFragment inside your `onCreate` method.

```Kotlin
payTheoryFragment.configure(apiKey = apiKey, amount = 1000)
```

## Step 3: Handle Response

Add Payable interface along with its functions to your activity.

```Kotlin
class MainActivity : AppCompatActivity() , Payable {
    
    fun handleSuccess(successfulTransactionResult: SuccessfulTransactionResult){
        println(successfulTransactionResult)
    }

    fun handleFailure(failedTransactionResult: FailedTransactionResult){
        println(failedTransactionResult)
    }
    
    fun confirmation(confirmationMessage: ConfirmationMessage, transaction: Transaction){
        println(confirmationMessage)
    }

    fun handleError(error: Error){
        println(error)
    }
    
    fun handleBarcodeSuccess(barcodeResult: BarcodeResult){
        println(barcodeResult)
    }

    fun handleTokenizeSuccess(paymentMethodToken: PaymentMethodTokenResults){
        println(paymentMethodToken)
    }
}
```
