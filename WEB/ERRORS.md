# SDK Errors
You can be returned a number of errors from the SDK. They can be returned to the `errorObserver` or potentially in a response of type `ERROR` from a function call.  
Below is a list of errors that can be returned from the SDK.

| CODE                    | DESCRIPTION                                                                                                                     |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| ACTION_COMPLETE         | Indicates that an action has been successfully completed and cannot be run again. The fields must be cleared and mounted again. |
| ACTION_IN_PROGRESS      | Indicates that an action is currently in progress. The action cannot be run again until the current action is complete.         |
| CANCEL_FAILED           | Indicates that a cancellation operation failed.                                                                                 |
| FIELD_ERROR             | Indicates an error with mounting the PayTheory Fields.                                                                          |
| INVALID_PARAM           | Indicates that an invalid parameter was provided.                                                                               |
| NO_FIELDS               | Indicates that no fields were found on the DOM to mount.                                                                        |
| NO_TOKEN                | Indicates that no session token was found. This is typically indicative of a network failure.                                   |
| NOT_READY               | Indicates that the SDK is not yet connected and ready to perform the action of a function.                                      |
| NOT_VALID               | Indicates that the transacting field is not valid at the time of running the function.                                          |
| SESSION_EXPIRED         | Indicates that the socket session has expired.                                                                                  |
| SOCKET_ERROR            | Indicates an error with the call to the socket.                                                                                 |
| TRANSACTING_FIELD_ERROR | Indicates either multiple transacting fields or no transacting fields were found visible on the DOM when running a function.    |


ndicates that an action is currently in progress. The action cannot be run again until the current action is complete. |