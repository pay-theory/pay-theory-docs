# Users

Users are accounts that have access to PayTheory Portals. Users can be created with access to the Merchant, System, or Partner Portal.

## The User Object
```js
{
    username: String
    email: String
    user_status: String
    full_name: String
    phone: String
}
```

**`username`: String**  
The cognito username id of the user.

**`email`: String**  
The email address of the user.

**`user_status`: String**  
The status of the user. Likely to be one of the following:
* `CONFIRMED`: User has logged in and set up their password.
* `FORCE_CHANGE_PASSWORD`: User has yet to login with their temporary password.

**`full_name`: String**  
The full name of the user.

**`phone`: String**  
The phone number of the user.


## Query Users
```js
{
    users(user_pool: UserPool, merchant_uid: String) {
        username
        email
        user_status
        full_name
        phone
    }
}
```

**Arguments**

**`user_pool`: UserPool**  
The user pool to query. One of the following:
* `MERCHANT`: Query users in the Merchant Portal.
* `SYSTEM`: Query users in the System Portal.
* `PARTNER`: Query users in the Partner Portal.

**`merchant_uid`: String**  
The merchant uid to query users for. Only used when querying users in the Merchant and System Portal.

**Returns**

```js
{
    "data": {
        "users": [
            {
                "username": "XXXXXX",
                "email": "XXXXXX",
                "user_status": "XXXXXX",
                "full_name": "XXXXXX",
                "phone": "XXXXXX"
            },
            {
                "username": "XXXXXX",
                "email": "XXXXXX",
                "user_status": "XXXXXX",
                "full_name": "XXXXXX",
                "phone": "XXXXXX"
            }
        ]
    }
}
```

**`users`: [User]**  
The list of users that are returned from the query. Objects will include all keys that are passed in with the query.

## Create User
```js
mutation {
  createUser(input: { email: AWSEmail, 
                    first_name: String,
                    last_name: String, 
                    merchant_uid: String, 
                    phone: AWSPhone, 
                    user_pool: UserPool
                }) {
    email
    full_name
    phone
    user_status
    username
  }
}
```

**Arguments**

**`input`: UserInput**  
An object containing the following keys:

**`email`: AWSEmail**  
The email address of the user. Must be a valid email address or the mutation will fail. Can only have one user per email per user pool.

**`first_name`: String**  
The first name of the user.

**`last_name`: String**  
The last name of the user.

**`merchant_uid`: String**  
The `merchant_uid` of the merchant to create the user for. Only used when creating a user for the Merchant or System Portal.

**`phone`: AWSPhone**  
The phone number of the user. Must be a valid phone number or null or the mutation will fail.

**`user_pool`: UserPool**  
The user pool to create the user in. One of the following:
* `MERCHANT`: Create a user in the Merchant Portal.
* `SYSTEM`: Create a user in the System Portal.
* `PARTNER`: Create a user in the Partner Portal.

## Delete User
```js
mutation {
  deleteUser(username: String, user_pool: UserPool) 
}
```

**Arguments**

**`username`: String**  
The cognito username id of the user to delete.

**`user_pool`: UserPool**  
The user pool to delete the user from. One of the following:
* `MERCHANT`: Delete a user from the Merchant Portal.
* `SYSTEM`: Delete a user from the System Portal.
* `PARTNER`: Delete a user from the Partner Portal.

