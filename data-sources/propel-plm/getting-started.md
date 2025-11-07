---
icon: sparkle
---

# Getting Started

_This document is a work in progress_

### Configuration

To configure a Propel instance you need:

* The base API path of your Propel cloud instance which is of the form: `https://{propel-instance}.my.salesforce.com`
* Propel uses the OAuth 2.0 protocol to authenticate, therefore, a code grant url (OAuth Url), a refresh token url (OAuth token Url), a Salesforce Connected App Consumer Key (OAuth Client ClientId) and Consumer Secret (OAuth Client Secret), and the OAuth Scopes need to be supplied, they are of the form:
  * OAuth Url: `https://{propel-instance}.my.salesforce.com/services/oauth2/authorize`
  * OAuth token Url: `https://{propel-instance}.my.salesforce.com/services/oauth2/token`
  * OAuth Client ClientId: The Salesforce Connected App Consumer Key from the [previous](oauth-setup.md) step
  * OAuth Client Secret: The Salesforce Connected App Consumer Secret from the [previous](oauth-setup.md) step
  * OAuth Scopes: `full api web refresh_token offline_access`

### Setup Propel Data Source

* Login on the application
* Navigate to `Data Sources`
* On the right > Select Propel > Add
* Update the Server Url with your instance's url, then click on the `UPDATE` button
* Click on the configure button
* On the first tab `Authentication`, select the `OAuth 2.0` authentication type then enter the OAuth credentials
* On the second tab `BOM Configuration`, select the preferred BOM child revisions selection option
* Click the `Save` button
* Click the `Authenticate` button

### Setup Propel Property Mappings

* Navigate to `Property Mapping`
* After creating/adding your[ Property Mappings ](../../fundamentals/property-mappings/)from your primary/CAD source, select the corresponding Propel property from the drop down selection boxes.

### Notes:

In the current implementation of the SharpSync Propel Module:

* New Propel items are are already created with at least a Name and Category prior to exporting to SharpSync.
* Existing Propel item Names or Part Numbers (Propel property name: `"Name"`) cannot be edited.
* Existing Propel item Categories (Propel property name: `"PDLM__Category__c"`) cannot be edited.
