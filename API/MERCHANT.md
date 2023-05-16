# Merchant

Merchants are the entities that are using Pay Theory to accept payments. 

## The Merchant Object
```js
{
    merchant_uid: String
    merchant_name: String
    api_key: String
    submitted_onboarding: Boolean
    card_active: Boolean
    ach_active: Boolean
    cash_active: Boolean
    fee_model: FeeModel
    is_system: Boolean
    parent_merchant_uid: String
}
```

**`merchant_uid`: String**  
The Pay Theory unique identifier assigned to the merchant.

**`merchant_name`: String**  
The name of the merchant.

**`api_key`: String**  
The API key of the merchant. This is used to authenticate use of the PayTheory Web and Native SDKs.

**`submitted_onboarding`: Boolean**  
Whether the merchant has submitted their onboarding information.

**`card_active`: Boolean**  
If the merchant has successfully completed onboarding and has a card processor active.

**`ach_active`: Boolean**  
If the merchant has successfully completed onboarding and has an ACH processor active.

**`cash_active`: Boolean**  
If the merchant has successfully completed onboarding and has a cash processor active.

**`fee_model`: FeeModel**  
The fee model that the merchant is using.  This is used to calculate the fees that are charged to the payor.  Detailed information about the fee model can be found [here](feemodel).

**`is_system`: Boolean**  
Returns `true` if the merchant has children merchants.

**`parent_merchant_uid`: String**  
The `merchant_uid` of the parent merchant.  This is only set if the merchant is a sub merchant of a system merchant.

## Query Merchant
```js
{
    merchant(merchant_name: String, merchant_uid: String) {
        ach_active
        api_key
        card_active
        cash_active
        fee_model {
            merchant_fee {
                ach_basis
                ach_fixed
                card_basis
                card_fixed
            }
            service_fee {
                ach_basis
                ach_fixed
                card_basis
                card_fixed
            }
            service_fee_min {
                ach_basis
                ach_fixed
                card_basis
                card_fixed
            }
        }
        is_system
        merchant_name
        merchant_uid
        parent_merchant_uid
        submitted_onboarding
    }
}

```

**Arguments**

**`merchant_name`: String**  
The name of the merchant to query.

**`merchant_uid`: String**  
The `merchant_uid` of the merchant to query.

**Returns**

```js
{
    "data": {
        "merchant": {
            "merchant_uid": String,
            ...
        }
    }
}
```

**`merchant`: Merchant**  
This call returns a single merchant object with the specified `merchant_uid` or `merchant_name`.

## The List Merchant Object
This is a limited merchant object that is returned when you want to query a list of merchants.

```js
{
    merchant_uid: String
    merchant_name: String
    is_system: Boolean
    parent_merchant_uid: String
    submitted_onboarding: Boolean
    card_active: Boolean
    ach_active: Boolean
    cash_active: Boolean
}
```

**`merchant_uid`: String**  
The Pay Theory unique identifier assigned to the merchant.

**`merchant_name`: String**  
The name of the merchant.

**`is_system`: Boolean**  
If the merchant is a system merchant.  System merchants are merchants that also have sub merchants.

**`parent_merchant_uid`: String**  
The `merchant_uid` of the parent merchant.  This is only set if the merchant is a sub merchant of a system merchant.

**`submitted_onboarding`: Boolean**  
Whether the merchant has submitted their onboarding information.

**`card_active`: Boolean**  
If the merchant has successfully completed onboarding and has a card processor active.

**`ach_active`: Boolean**  
If the merchant has successfully completed onboarding and has an ACH processor active.

**`cash_active`: Boolean**  
If the merchant has successfully completed onboarding and has a cash processor active.


## Query Merchants
```js
{
  merchants(direction: FORWARD, limit: 10, offset: "", offset_id: "", query: QueryObject) {
    items {
        ach_active
        card_active
        cash_active
        is_system
        merchant_name
        merchant_uid
        parent_merchant_uid
        submitted_onboarding
    }
    total_row_count
  }
}
```

**Arguments**

**`limit`: Int**  
The number of merchants to return.

**`direction`: String**  
The direction of the pagination. Makes sure the results are returned in the correct order.
* `FORWARD`
* `BACKWARD`

**`offset`: String**  
The value of the offset item for which the list is being sorted.  
If the direction is `FORWARD`, the offset item is the last item in the previous list.  
If the direction is `BACKWARD`, the offset is the first item in the previous list.

**`offset_id`: String**  
The `merchant_uid` of the offset item. If the direction is `FORWARD`, the offset item is the last item in the list. If the direction is `BACKWARD`, the offset is the first item in the list.

**`query`: QueryObject**  
The query to filter the merchants with based on Pay Theory defined data.  Detailed information about the query object can be found [here](query).

**Returns**

```js
{
    "data": {
        "merchants": {
            "items": [
                {
                    "merchant_uid": "XXXXXX",
                },
                {
                    "merchant_uid": "XXXXXX",
                },
              ...
            ],
            "total_row_count": 24
        }
    }
}
```

**`items`: [ListMerchant]**  
The list of merchants that are returned from the query. Objects will include all keys that are passed in with the query.

**`total_row_count`: Int**  
The total number of merchants that match the query. Used to help with pagination.

## Create Merchant
```js
mutation {
  createMerchant(merchant_name: String, 
                parent_merchant_uid: String, 
                user: {
                    email: AWSEmail, 
                    first_name: String, 
                    last_name: String, 
                    phone: AWSPhone
                }) {
    ach_active
    card_active
    cash_active
    merchant_name
    merchant_uid
    parent_merchant_uid
    submitted_onboarding
  }
}
```

**Arguments**

**`merchant_name`: String**  
The name of the merchant to create.

**`parent_merchant_uid`: String**  
The `merchant_uid` of the parent merchant.  This is only set if the merchant belongs to another merchant account.

**`user`: User**  
The user that will be created for the merchant.  This user will be given access to the onboarding form on Merchant creation. 

**User Arguments**

**`email`: AWSEmail**  
The email address of the user. Must be a valid email address or the mutation will fail.

**`first_name`: String**  
The first name of the user.

**`last_name`: String**  
The last name of the user.

**`phone`: AWSPhone**  
The phone number of the user. Must be a valid phone number or null or the mutation will fail.

**Returns**

```js
{
    "data": {
        "createMerchant": {
            "merchant_uid": "XXXXXX",
            ...
        }
    }
}
```

**`createMerchant`: ListMerchant**  
This will return the newly created merchant object.