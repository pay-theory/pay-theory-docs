# Disputes

Disputes are charges that have been contested by a cardholder to their card issuer. 

They could be an `INQUIRY` which is just a request for information, or an actual chargeback in the status `PENDING` which is a request for a charge to be reversed. 

## The Settlement Object

```js
{
    "merchant": "8404e0bf-a018-4329-a667-9e2aed9cee1e",
    "transaction_id": "pt-austin-paytheorylab-004ah5",
    "dispute_id": "DItr55AS8SwakWj2AUmsxNUo",
    "account_code": "account12345",
    "amount": 888888,
    "card_brand": "VISA",
    "dispute_date": 1648503429,
    "settlement_deposit_batch": 0,
    "descriptor": "Tech Fees",
    "email": "someone@example.com",
    "evidenceLast_send_date": 1649088902,
    "expiration_date": 1649108229,
    "full_name": "Admin Account",
    "last_four": "0006",
    "phone": "1234567890",
    "reason": "FRAUD",
    "reason_message": "",
    "settlement_batch": 32,
    "status": "PENDING",
    "transaction_date": 1647879716,
    "updated_date": 1648503427,
    "withdrawal_settlement_batch": 32
}
```

**`merchant`: String**  
The Pay Theory unique identifier assigned to the merchant that the dispute belongs to.

**`transaction_id`: String**  
The unique transaction identifier of the charge that is being disputed.

**`dispute_id`: String**  
The Pay Theory unique dispute identifier.

**`status`: Status**  
The status of the dispute.
* `PENDING`
* `INQUIRY`
* `WON`
* `LOST`

**`reason`: Reason**  
The reason code for the dispute as passed by the card issuer.

**`amount`: Int**  
The amount of the transaction in dispute.

**`dispute_date`: Int**  
The date the dispute was created.

**`evidence_last_send_date`: Int**  
The last date evidence was sent to the processor. If no evidence was sent, this will be null.

**`updated_date`: Int**  
The date the dispute was last updated.

**`expiration_date`: Int**  
The final date to submit evidence to contest a dispute.

**`account_code`: String**  
Metadata passed in at the time of the transaction under the key `pay-theory-account-code`

**`descriptor`: String**  
Metadata passed in at the time of the transaction under the key `pay-theory-reference`

**`last_four`: String**  
The last four digits of the card number.

**`card_brand`: String**  
The brand of the card used to make the payment.

**`full_name`: String**  
 The `first_name` and `last_name` that was passed into the `customer_info` object at time of transaction.

**`reason_message`: String**  
A more detailed reason provided by the card issuer for the dispute.

**`settlement_batch`: String**  
The settlement batch number that the original payment belongs to.

**`settlement_withdrawal_batch`: String**  
The settlement batch number where funds were withdrawn from the merchants account.

**`settlement_deposit_batch`: String**  
The settlement batch number where funds were deposited into the merchants account if a dispute is `WON`.

**`transaction_date`: Int**  
The date the original transaction was made.

**`phone`: String**  
The phone number that was passed into the `customer_info` object at time of transaction.

**`email`: String**  
The email address that was passed into the `customer_info` object at time of transaction.

## Query Disputes
### Coming Soon
