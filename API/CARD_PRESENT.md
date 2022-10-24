# Card Present Device

Card Present Devices 

## The Device Object

```js
{
  merchant_uid: String
  device_id: String
  device_name: String
  device_description: String
  is_active: Boolean
}
```

**`merchant_uid`: String**  
The PayTheory unique identifier assigned to the merchant that the device belongs to.

**`device_id`: String**  
The unique id that represents the device in the PayTheory system.

**`device_name`: String**  
The name of the type of device that is being used.

**`device_description`: String**  
Custom description of the device that can be used to identify the device.

**`is_active`: Boolean**  
Whether the device is active and can be used to process transactions.

## Query Devices
```javascript
{
    devices(direction: FORWARD, limit: 10, offset: "", offset_id: "", query: QueryObject) {
        items {
            device_description
            device_id
            device_name
            is_active
            merchant_uid
        }
        total_row_count
    }
}
```

**Arguments**

**`limit`: Int**  
The number of devices to return.

**`direction`: String**  
The direction of the pagination. Makes sure the results are returned in the correct order.
* `FORWARD`
* `BACKWARD`

**`offset`: String**  
The value of the offset item for which the list is being sorted.  
If the direction is `FORWARD`, the offset item is the last item in the previous list.  
If the direction is `BACKWARD`, the offset is the first item in the previous list.

**`offset_id`: String**  
The `device_id` of the offset item. If the direction is `FORWARD`, the offset item is the last item in the list. If the direction is `BACKWARD`, the offset is the first item in the list.

**`query`: QueryObject**  
The query to filter the devices with based on PayTheory defined data. Detailed information about the query object can be found [here](query).

**Returns**

```js
{
    "data": {
        "devices": {
            "items": [
                {
                    "device_id": "ptl_dev_XXXXXXXXX",
                },
                {
                    "device_id": "ptl_dev_XXXXXXXXX",
                },
              ...
            ],
            "total_row_count": 2
        }
    }
}
```

**`items`: [Settlement]**  
The list of devices that are returned from the query.

**`total_row_count`: Int**  
The total number of devices that match the query. Used to help with pagination.


## Update Device

```js
mutation {
    updateDevice(device_description: "", device_id: "", merchant_uid: "")
}

```

**Arguments**

**Required Arguments**

**`device_id`: String**  
The unique id that represents the device in the PayTheory system.

**`merchant_uid`: String**  
The PayTheory unique identifier assigned to the merchant that the device belongs to.

**`device_description`: String**  
Custom description of the device that can be used to identify the device.

**Returns**  

Returns `true` if the device was successfully updated.
