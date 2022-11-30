# Payment Links

Payment links are a used to create a checkout page without having to write any code. You can create a payment link and send it to your customers to pay for your products or services.

## The Payment Link Object
```js
{
    merchant_uid: String
    link_id: String
    link_url: String
    link_name: String
    payment_name: String
    payment_description: String
    amount: Int
    currency: String
    fee_mode: String
    amount_is_variable: Boolean
    min_amount: Int
    max_amount: Int
    require_phone: Boolean
    call_to_action: String
    accepted_payment_methods: String
    account_code: String
    custom_success_message: String
    redirect_url: String
    is_active: Boolean
    created_date: String
}
```

**`merchant_uid`: String**  
The PayTheory unique identifier assigned to the merchant that the transaction belongs to.

**`link_id`: String**  
The unique id of the payment link.

**`link_url`: String**  
The url of the payment link.

**`link_name`: String**  
The name you give to the payment link for internal tracking purposes.

**`payment_name`: String**  
The name of the payment that will be displayed to the customer. This will be passed in as the `reference` at the time of the transaction.

**`payment_description`: String**  
The description of the payment that will be displayed to the customer.

**`amount`: Int**  
The amount of the payment that the payor will be asked to pay or if `amount_is_variable` is set to the amount that will be the default amount.

**`currency`: String**  
The type of currency for the payment.

**`fee_mode`: String**  
The fee mode of the payments that will be made with the payment link.
* `INTERCHANGE`
* `SERVICE_FEE`

**`amount_is_variable`: Boolean**  
If set to `true` the payor will be able to enter the amount they want to pay.

**`min_amount`: Int**  
The minimum amount the payor can pay if `amount_is_variable` is set to `true`.

**`max_amount`: Int**  
The maximum amount the payor can pay if `amount_is_variable` is set to `true`.

**`require_phone`: Boolean**  
If set to `true` the payor will be required to enter their phone number before making the payment.

**`call_to_action`: String**  
The text that will be displayed on the button on the checkout page.
* `PAY`
* `DONATE`
* `BOOK`

**`accepted_payment_methods`: String**  
The payment methods that will be available to a payor when making a payment.
* `ALL`
* `NOT_CASH`
* `NOT_CARD`
* `NOT_ACH`
* `ONLY_CASH`
* `ONLY_CARD`
* `ONLY_ACH`

**`account_code`: String**  
Account Code that will be passed in to every transaction made with this payment link.

**`custom_success_message`: String**  
The message that will be displayed to the payor after a successful payment.

**`redirect_url`: String**  
The url that the payor will be redirected to after a successful payment.

**`is_active`: Boolean**  
If set to `true` the payment link will be active and available to payors. If set to `false` the payment link will not be available to payors.

**`created_date`: String**  
The date and time the payment link was created.

## Query Payment Links
```js
{
  paymentLinks(direction: FORWARD, limit: 10, offset: "", offset_id: "", query: QueryObject) {
        total_row_count
        items {
            merchant_uid
            link_id
            link_url
            link_name
            payment_name
            payment_description
            amount
            currency
            fee_mode
            amount_is_variable
            min_amount
            max_amount
            require_phone
            call_to_action
            accepted_payment_methods
            account_code
            custom_success_message
            redirect_url
            is_active
            created_date
        }
  }
}
```
**Arguments**

**`limit`: Int**  
The number of payment links to return.

**`direction`: String**  
The direction of the pagination. Makes sure the results are returned in the correct order.
* `FORWARD`
* `BACKWARD`

**`offset`: String**  
The value of the offset item for which the list is being sorted.  
If the direction is `FORWARD`, the offset item is the last item in the previous list.  
If the direction is `BACKWARD`, the offset is the first item in the previous list.

**`offset_id`: String**  
The `link_id` of the offset item. If the direction is `FORWARD`, the offset item is the last item in the list. If the direction is `BACKWARD`, the offset is the first item in the list.

**`query`: QueryObject**  
The query to filter the payment links with based on Pay Theory defined data.  Detailed information about the query object can be found [here](query).

**Returns**

```js
{
    "data": {
        "paymentLinks": {
            "items": [
                {
                    "link_id": "pt_link_XXXXX",
                    ...
                },
                {
                    "link_id": "pt_link_XXXXX",
                    ...
                },
              ...
            ],
            "total_row_count": 256
        }
    }
}
```

**`items`: [Payor]**  
The list of payment links that are returned from the query. Objects will include all keys that are passed in with the query.

**`total_row_count`: Int**  
The total number of payment links that match the query. Used to help with pagination.

## Create a Payment Link

```js
mutation {
    createPaymentLink(input: {
               accepted_payment_methods: AcceptedPaymentMethods, 
               account_code: String, 
               amount: Int,
               amount_is_variable: Boolean,
               call_to_action: CallToAction,
               currency: String,
               custom_success_message: String,
               fee_mode: FeeMode, 
               link_name: String, 
               max_amount: Int, 
               merchant_uid: String, 
               min_amount: Int, 
               payment_description: String, 
               payment_name: String, 
               redirect_url: String, 
               require_phone: Boolean
           }) {
    accepted_payment_methods
    account_code
    amount
    amount_is_variable
    call_to_action
    created_date
    currency
    custom_success_message
    fee_mode
    link_id
    is_active
    link_name
    link_url
    max_amount
    merchant_uid
    min_amount
    payment_description
    payment_name
    redirect_url
    require_phone
  }
}
```

**Arguments**

**`input`: PaymentLinkInput**  
This object contains all the details needed to create a payment link.

**Required Arguments**

**`merchant_uid`: String**  
The merchant uid of the merchant that will be creating the payment link.

**`link_name`: String**  
The name you give to the payment link for internal tracking purposes.

**`payment_name`: String**  
The name of the payment that will be displayed to the customer. This will be passed in as the `reference` at the time of the transaction.

**`amount`: Int**  
The amount of the payment that the payor will be asked to pay or if `amount_is_variable` is set to the amount that will be the default amount.  
This is only required if `amount_is_variable` is set to `false`

**Optional Arguments**

**`payment_description`: String**  
The description of the payment that will be displayed to the customer. This will be passed in as the `reference` at the time of the transaction.

**`currency`: String**  
The currency of the payment.

**`fee_mode`: FeeMode**  
The fee mode of the payments that will be made with the payment link.
* `INTERCHANGE`
* `SERVICE_FEE`

**`amount_is_variable`: Boolean**  
If set to `true` the payor will be able to enter the amount they want to pay. If set to `false` the payor will be asked to pay the amount specified in `amount`.

**`min_amount`: Int**  
The minimum amount that the payor will be able to pay if `amount_is_variable` is set to `true`.

**`max_amount`: Int**  
The maximum amount that the payor will be able to pay if `amount_is_variable` is set to `true`.

**`require_phone`: Boolean**  
If set to `true` the payor will be required to enter their phone number before they can pay.

**`call_to_action`: CallToAction**  
The call to action that will be displayed on the button at the time of checkout.
* `PAY`
* `DONATE`
* `BOOK`

**`accepted_payment_methods`: AcceptedPaymentMethods**
The payment methods that will be available to a payor when making a payment.
* `ALL`
* `NOT_CASH`
* `NOT_CARD`
* `NOT_ACH`
* `ONLY_CASH`
* `ONLY_CARD`
* `ONLY_ACH`

**`account_code`: String**  
Account Code that will be passed in to every transaction made with this payment link.

**`custom_success_message`: String**  
The message that will be displayed to the payor after they have successfully paid. Cannot be used with `redirect_url`.

**`redirect_url`: String**  
The url that the payor will be redirected to after they have successfully paid. Cannot be used with `custom_success_message`.

**Returns**

```js
{
    "data": {
        "createPaymentLink": {
            ... // Payment Link Object
        }
    }
}
```


## Update a Payment Link

```js
mutation {
    updatePaymentLink(input: {
               accepted_payment_methods: AcceptedPaymentMethods, 
               account_code: String, 
               amount: Int,
               amount_is_variable: Boolean,
               call_to_action: CallToAction,
               currency: String,
               custom_success_message: String,
               fee_mode: FeeMode, 
               link_id: String,
               link_name: String, 
               max_amount: Int, 
               merchant_uid: String, 
               min_amount: Int, 
               payment_description: String, 
               payment_name: String, 
               redirect_url: String, 
               require_phone: Boolean
           }) {
    accepted_payment_methods
    account_code
    amount
    amount_is_variable
    call_to_action
    created_date
    currency
    custom_success_message
    fee_mode
    link_id
    is_active
    link_name
    link_url
    max_amount
    merchant_uid
    min_amount
    payment_description
    payment_name
    redirect_url
    require_phone
  }
}
```

**Arguments**

**`input`: UpdatePaymentLinkInput**  
This object contains all the details needed to create a payment link.

**Required Arguments**

**`merchant_uid`: String**  
The merchant uid of the merchant that will be creating the payment link.

**`link_id`: String**  
The id of the payment link that will be updated.

**Optional Arguments**

**`link_name`: String**  
The name you give to the payment link for internal tracking purposes.

**`payment_name`: String**  
The name of the payment that will be displayed to the customer. This will be passed in as the `reference` at the time of the transaction.

**`amount`: Int**  
The amount of the payment that the payor will be asked to pay or if `amount_is_variable` is set to the amount that will be the default amount.  
This is required if `amount_is_variable` is set to `false`

**`payment_description`: String**  
The description of the payment that will be displayed to the customer. This will be passed in as the `reference` at the time of the transaction.

**`currency`: String**  
The currency of the payment.

**`fee_mode`: FeeMode**  
The fee mode of the payments that will be made with the payment link.
* `INTERCHANGE`
* `SERVICE_FEE`

**`amount_is_variable`: Boolean**  
If set to `true` the payor will be able to enter the amount they want to pay. If set to `false` the payor will be asked to pay the amount specified in `amount`.

**`min_amount`: Int**  
The minimum amount that the payor will be able to pay if `amount_is_variable` is set to `true`.

**`max_amount`: Int**  
The maximum amount that the payor will be able to pay if `amount_is_variable` is set to `true`.

**`require_phone`: Boolean**  
If set to `true` the payor will be required to enter their phone number before they can pay.

**`call_to_action`: CallToAction**  
The call to action that will be displayed on the button at the time of checkout.
* `PAY`
* `DONATE`
* `BOOK`

**`accepted_payment_methods`: AcceptedPaymentMethods**
The payment methods that will be available to a payor when making a payment.
* `ALL`
* `NOT_CASH`
* `NOT_CARD`
* `NOT_ACH`
* `ONLY_CASH`
* `ONLY_CARD`
* `ONLY_ACH`

**`account_code`: String**  
Account Code that will be passed in to every transaction made with this payment link.

**`custom_success_message`: String**  
The message that will be displayed to the payor after they have successfully paid. Cannot be used with `redirect_url`.

**`redirect_url`: String**  
The url that the payor will be redirected to after they have successfully paid. Cannot be used with `custom_success_message`.

**Returns**

```js
{
    "data": {
        "updatePaymentLink": {
            ... // Payment Link Object
        }
    }
}
```