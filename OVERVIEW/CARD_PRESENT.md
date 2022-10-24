# Card Present Quick Start

This guide will walk you through the steps to activate a card present device to accept in person card payments with a device.

## Step 1: Find the Device ID

You will need to know the device ID of the device you want to activate. You can find the device ID by querying the GraphQL API.

```graphql
{
  devices(query: { query_list: [
                                {
                                  key: "merchant_uid",
                                  value: "YOUR_MERCHANT_UID",
                                  operator: EQUAL,
                                  conjunctive_operator: AND_NEXT
                                },
                                {
                                  key: "device_description",
                                  value: "DEVICE_DESCRIPTION",
                                  operator: EQUAL
                                }
                              ]
                  }) {
    items {
      device_description
      device_id
      device_name
      is_active
      merchant_uid
    }
  }
}
```

## Step 2: Set Up PayTheory Web SDK

Once you have the device ID you need to set up the PayTheory Web SDK.

```javascript
let payTheory = window.paytheory

payTheory.create("YOUR_API_KEY")
    .then(ptObject => {
        ptObject.mount()
      
        ptObject.cardPresentObserver(message => {
            // Handle card present messages
        })
    })
```

You will need to call the `cardPresentObserver` function to handle messages from the card present iframe.

## Step 3: Activate the Device

Once you have received a `READY` message from the card present iframe you can activate the device to accept a payment.

```javascript
//Amount passed in is in cents
const AMOUNT = 1000

// optionally provide details about the payor
const PAYOR_INFO = {
    "first_name": "Some",
    "last_name": "Body",
    "email": "somebody@gmail.com",
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

// Parameters that you will pass into the transact function. More details below.
const CARD_PRESENT_PARAMS = {
    amount: AMOUNT,
    deviceId: "pt_dev_XXXXXXXXX", // The device ID you received from the GraphQL query in Step 1
    payorInfo: PAYOR_INFO, // optional
}

// Function is available on the ptObject returned from the create function in Step 2
ptObject.activateCardPresentDevice(CARD_PRESENT_PARAMS)
```

## Step 4: Handle responses In the `cardPresentObserver`

You should receive two responses after calling the `activateCardPresentDevice` function.  

The first response will be a `ACTIVATED` message. This will let you know that the device is ready to accept a payment.

The second response will be a `COMPLETE` message. This will let you know that the device has completed the payment session, but it could be either a `SUCCEEDED` or `FAILURE` message. You will need to check the `result` field to see if the payment was successful.



