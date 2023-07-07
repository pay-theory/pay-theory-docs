# Failed Transactions
When a transaction fails you will be returned a transaction with a status of `FAILURE`. You will also receive a `reason` object with an `error_code` and `error_text` that will give you more information about the failure.

The `error_text` will be a sentence explaining the Failure and is the better option to be displayed to the user without modification. 

The `error_code` a string that you can use if you would rather display your own error message. The error codes are listed below.

| CODE                                | DESCRIPTION                                                                                                        |
|-------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| ADDRESS_VERIFICATION_FAILED_RISK_RULES          | The Risk Rules associated to the Merchant declined the transaction. The cardholder needs to attempt the transaction again using the correct address.                                                                                                                      |
| AUTHORIZATION_EXPIRED_OR_ALREADY_CAPTURED | The authorization is either expired or has already been captured. A new authorization needs to be created.                                                                     |
| BANK_ACCOUNT_CLOSED                | The bank account has been closed. Contact the account owner and get another method of payment that's active.                                                                  |
| BANK_ACCOUNT_FROZEN                | The bank account is frozen. Contact the account owner and get another method of payment.                                                                                      |
| CALL_ISSUER                        | The card was declined for an unknown reason. The cardholder needs to contact their card issuer for more information.                                                          |
| CARDHOLDER_PREVENTED_RECURRING_TRANSACTION  | Cardholder requested that recurring or installment transaction to be stopped.                                                                                                             |
| CARD_ACCOUNT_CLOSED                | The card account has been closed. The cardholder should use an active card.                                                                                                   |
| CARD_NETWORK_ERROR                 | There is a problem with the card network. Contact the network for more information.                                                                                            |
| CARD_NOT_SUPPORTED                 | The card does not support this type of purchase. The cardholder needs to contact their issuing bank to make sure their card can be used to make this type of purchase.       |
| COMMUNICATION_ERROR                | There was a network communication error with the host. Check your network connection and retry the transaction.                                                              |
| CVV_FAILED_RISK_RULES              | The Risk Rules associated with the Merchant declined the transaction. The cardholder needs to attempt the transaction again using the correct CVV.                                                |
| DEVICE_IN_USE                      | The device is currently processing a request. Attempt a request again once the request is complete, or within 6 minutes.                                                       |
| DEVICE_NOT_CONNECTED               | The payment terminal isn't connected to a network. Connect the payment terminal to a network and reattempt the transaction.                                                   |
| DO_NOT_HONOR                       | The card was declined for an unknown reason. The cardholder needs to contact their issuing bank for more information.                                                          |
| DUPLICATE_TRANSACTION              | A transaction with the same amount and card was approved recently and marked as a duplicate. If this duplicate transaction was intentional, contact the processor for support. |
| EXCEEDS_APPROVAL_LIMIT             | The authorization exceeds the approval limit. The cardholder needs to contact their card issuer for more information.                                                          |
| EXCEEDS_WITHDRAWAL_FREQUENCY_LIMIT | The card has exceeded its withdrawal frequency limit. The cardholder needs to use a different card.                                                                            |
| EXPIRED_CARD                       | The card has expired. The cardholder needs to use another card that's not expired.                                                                                            |
| FRAUD_DETECTED                     | The card was declined for fraud by the processor. The customer should contact their card issuer for more information.                                                         |
| GENERIC_DECLINE                     | The transaction was declined for an unknown reason. The account owner needs to contact their issuer for more information.                                                         |
| INACTIVE_CARD                      | The card is inactive. The cardholder needs to activate the card or use an active card.                                                                                       |
| INCOMPLETE_TRANSACTION             | The transaction was not completed by the cardholder. The cardholder needs to reattempt the transaction.                                                                        |
| INSUFFICIENT_FUNDS                 | The account has insufficient funds for the transaction. The account holder needs to use another method of payment.                                                            |
| INVALID_ACCOUNT_NUMBER             | The card number is not valid. The cardholder needs to contact their issuing bank for more information or use another card.                                                     |
| INVALID_AMOUNT                     | The amount exceeds the amount that is allowed on the card. The cardholder needs to check with their issuing bank to see if they can make purchases of that amount.             |
| INVALID_BANK_ACCOUNT_NUMBER        | The bank account number is not valid. The account holder needs to use a valid account number.                                                                                 |
| INVALID_CARD                       | The card is invalid. The cardholder needs to use another card that is not invalid.                                                                                            |
| INVALID_CVV                         | The CVV number is invalid. The cardholder needs to attempt the transaction again using the correct CVV.                                                                         |
| INVALID_ISSUER                      | The card number is not associated with a valid card issuing bank. The cardholder needs to contact their issuing bank.                                                            |
| INVALID_ROUTING_NUMBER             | The bank routing number provided is invalid. The bank account holder needs to provide a valid routing number.                                                                   |
| INVALID_TRANSACTION                 | The transaction is not permitted by the issuing bank. The cardholder needs to contact their issuing bank for more information.                                                  |
| LOST_OR_STOLEN_CARD                | The transaction was declined for an unknown reason. The account owner needs to contact their issuer for more information.                                                       |
| MAX_TRANSACTION_AMOUNT_EXCEEDED    | The transaction was declined because it exceeded the max transaction amount that's associated with the Merchant. The transaction needs to be recreated with a lower amount.      |
| NON_TRANSACTION_ACCOUNT            | This is a non-transaction account that limits or prevents transactions. Contact your customer and request permission to charge a different bank account.                     |
| NO_BANK_ACCOUNT_FOUND              | The account number is valid; however, the number doesn't correspond to the account holder or it's not an open account. The account holder needs to reenter their information with the correct details.              |
| PAYMENT_STOPPED                    | The customer has stopped the payment with their bank. Contact the customer for details and to arrange payment.                                                                |
| PICK_UP_CARD                       | The card has been reported as lost or stolen by the cardholder. The cardholder needs to contact their issuing bank.                                                          |
| RESTRICTED_CARD                    | The card has a restriction preventing approval for this transaction. Please contact the issuing bank for a specific reason.                                                    |
| RESUBMIT_TRANSACTION                | The transaction couldn’t be processed by the issuer for an unknown reason. The cardholder needs to attempt the transaction again. If the transaction is declined again, the cardholder should contact their issuing bank.                     |
| SYSTEM_ERROR                       | There was a system error processing the transaction.                                                                                                                         |
| TIMEOUT                             | There was a timeout while performing the transaction request.                                                                                                                |
| TRANSACTION_NOT_PERMITTED           | The transaction was declined because the card or transaction type is not permitted. The cardholder needs to use a different type of card or attempt a different transaction method. |
| UNKNOWN_ERROR                       | There was an error processing the transaction.                                                                                                                                 |
