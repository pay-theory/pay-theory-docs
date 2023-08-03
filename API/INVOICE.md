# Invoice
 
An invoice is a request for payment. It can be used to track pending payments and to send reminders to payors.  
Invoice emails can also be sent on your behalf to your payors with a custom invoice checkout link.

## Invoice Object

```graphql
{
    invoice_id: String
    merchant_uid: String
    merchant_invoice_number: String
    payor: Payor
    invoice_amount: Int
    currency: String
    fee_mode: FeeMode
    total_paid_amount: Int
    status: InvoiceStatus
    invoice_name: String
    invoice_description: String
    is_secure: Boolean
    security_pin: String
    due_by: AWSDate
    settings: InvoiceSettings
    created_date: AWSDateTime
    invoice_date: AWSDate
    offline_transactions: [OfflineTransaction]
    api_key: String
    account_code: String
    reference: String
    require_payor_address: Boolean
}
```

**invoice_id**: String  
The Pay Theory unique identifier for the invoice.

**merchant_uid**: String  
The Pay Theory unique identifier assigned to the merchant that the invoice belongs to.

**merchant_invoice_number**: String  
The merchant's unique identifier for the invoice. This is optional and defined by the merchant.

**payor**: Payor  
The payor object for the invoice. More information on the payor object can be found [here](payor).

**invoice_amount**: Int  
The total amount of the invoice.

**currency**: String  
The currency of the invoice. 

**fee_mode**: FeeMode  
The fee mode for the invoice. It can be one of the following:
- MERCHANT_FEE
- SERVICE_FEE

**total_paid_amount**: Int  
The total amount paid on the invoice.

**status**: InvoiceStatus  
The status of the invoice. It can be one of the following:
- NOT_PAID
- PARTIALLY_PAID
- PAID

**invoice_name**: String  
The name of the invoice.

**invoice_description**: String  
The description of the invoice.

**is_secure**: Boolean  
Whether or not the invoice is secure. If the invoice is secure, the payor will be required to enter a security pin to view the invoice.

**security_pin**: String  
The security pin for the invoice. This is what would be used to access the invoices checkout page.

**due_by**: String  
The date the invoice is due.

**settings**: InvoiceSettings  
The settings for the invoice. This is a JSON object that contains the following fields:
- `accepted_payment_methods`: Object
  - card: Boolean
  - ach: Boolean
  - cash: Boolean

**created_date**: String  
The date the invoice was created.

**invoice_date**: String  
The date the invoice was sent.

**offline_transactions**: [OfflineTransaction]  
A list of offline transactions associated with the invoice. More information on the offline transaction object can be found [here](#offline-transaction).

**api_key**: String  
The API key used to create the invoice.