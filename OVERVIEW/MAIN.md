## Our Platform

Pay Theory provides three environments for your integration:

- **PayTheory.com**
  - live integration for accepting payments
- **PayTheoryStudy.com** 
  - sandbox integration for testing _new versions of your software_ against Pay Theory
- **PayTheoryLab.com**
  - acceptance integration for testing _new versions of Pay Theory_ against your software
 
We understand how important the stability of your platform is. 

Our platform is designed from the ground up to support yours by providing a flexible infrastructure for deployment of both your changes and ours.

Your instance of our platform lives in your own personal cloud space with its own collection of tables and security policies.

You determine when changes from PayTheoryLab.com are deployed to PayTheory.com and PayTheoryStudy.com.

## Features

### Pay Theory Payments

Provide safe and secure access to payments with hosted inputs for:

- Credit card
- Debit card
- ACH
- Cash

For basic [HTML Examples](https://PARTNER.html.example.STAGE.com/index.html) check here. _make sure you have an API key ready first_


### Pay Theory Sockets

Sockets provide a secure, consistent experience regardless of internet speed

- No public facing payment API endpoints
- End to end encryption of messages using [NaCl](https://nacl.cr.yp.to/) (external link)
- Gated payment process
- Fine-grained rate limiting
- Improved API performance on slow connections


### Pay Theory Detective

Actively detects bots & malicious actors

- Anonymized fingerprinting
- Behavioral analysis
- Device / Website attestation
- Machine learning (coming soon)

## Platforms

-   Pay Theory provides SDKs for the following platforms
    -   [Web / JavaScript](/web)
    -   iOS
    -   Android (coming soon)
    -   WooCommerce (coming soon)


## Partner Application Environment

-  Partner applications run as isolated instances
-  Pay Theory maps upstream processor data feeds providing:
    - Payment summaries
    - Settlement summaries
    - Chargeback / Dispute management

### Merchant Portal
- Merchants can manage SDK in the [merchant portal](https://PARTNER.merchant.dashboard.STAGE.com)
- Account details are on the [settings page](https://PARTNER.merchant.dashboard.STAGE.com/settings)
- Access to Payments, Settlements, and Chargeback details
- Issue refunds on payments

### System Portal
_Not available in start sandbox accounts_
- Systems are merchants who also have other merchants they manage
- Systems manage their merchants in the [system portal](https://PARTNER.system.dashboard.STAGE.com)
    - Assign merchant user accounts
    - View merchant transactions
    - Manage merchant credentials
    - Issue refunds on payments for all system merchants

### Partner Portal
_Not available in start sandbox accounts_
- Partners manage merchants in the [partner portal](https://PARTNER.partner.dashboard.STAGE.com)
    - Assign merchant user accounts
    - View merchant transactions
    - Manage merchant credentials
    - Mobile application registration (coming soon)
    - Website registration (coming soon)
    - Onboard new merchants to live accounts (coming soon) 