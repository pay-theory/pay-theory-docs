# Metadata 

Metadata is used to pass customer data to track items in Pay Theory Systems.

These calls allow you to update or delete metadata for an item you created with metadata.

## Update Metadata

```js
mutation {
  updateMetadata(id: String, merchant_uid: String, metadata: JSON, metadata_associate: MetadataAssociate)
}
```

**Arguments**

**`id`: String**  
The id of the item you want to update metadata for.

**`merchant_uid`: String**  
The Pay Theory merchant_uid of the item you want to update metadata for.

**`metadata`: JSON**  
The metadata you want to update the item with. Any keys passed in will overwrite the existing metadata.

**`metadata_associate`: MetadataAssociate**  
The type of item you want to update metadata for. Options are:

* `TRANSACTION`
* `RECURRING`

**Returns**

Returns true if it was successful or an error if it was not.

## Delete Metadata

```js
mutation {
  deleteMetadata(id: String, merchant_uid: String, metadata_associate: MetadataAssociate, metadata_keys: [String]!)
}
```

**Arguments**

**`id`: String**  
The id of the item you want to delete metadata for.

**`merchant_uid`: String**  
The Pay Theory merchant_uid of the item you want to delete metadata for.

**`metadata_associate`: MetadataAssociate**  
The type of item you want to delete metadata for. Options are:
* `TRANSACTION`
* `RECURRING`

**`metadata_keys`: [String]!**  
The keys of the metadata you want to delete.

**Returns**

Returns true if it was successful or an error if it was not.
