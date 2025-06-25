---
icon: sparkle
---

# Getting Started

{% hint style="danger" %}
This document is a work in progress.

Check back here  frequently for updates.
{% endhint %}

### Install SharpSync custom extension for Dynamics 365 Business Central

In SharpSync, when logged in to your organization as an admin user:

* Navigate to [Downloads](https://app.sharpsync.net/admin/downloads)
* Locate and download the `SHARPSYNC APP FOR DYNAMICS 365` custom extension app which should save locally a file ending in `*.app`&#x20;

In MS Dynamics 365 Business Central, when logged in to your tenant as an administrator (appropriate permissions are typically the EXTENSION MGT. - ADMIN permission set):

* Navigate to Extension Management: From the Business Central home page, use the search feature and type "Extension Management." Select the appropriate link.
* Upload the Extension: On the Extension Management page, click Manage and choose Upload Extension. You will be prompted to select the .app file for your extension that you previously downloaded.
* Deploy the Extension:
  * Choose `Target` = `Current version` (unless releasing for next BC upgrade).
  * Choose `Schema Sync Mode` = `Add` (unless the version includes schema changes, you may have to select `Force Sync` )
  * Accept the disclaimer and click Deploy.
* The deployment is processed in the background.
* You can monitor the progress using Installation Status (Click Manage → Installation Status), then refresh once it’s complete.
* Upon successful deployment, the app should now appear in your extensions table with the checkbox for `Is Installed` checked and the `Published As` value as `PTE`
* Additional documentation can be found in the following links:
  * [Customizing Business Central online using apps](https://learn.microsoft.com/en-us/dynamics365/business-central/ui-extensions)
  * [Install and Uninstall Extensions (Apps) in Business Central](https://learn.microsoft.com/en-us/dynamics365/business-central/ui-extensions-install-uninstall)

### Setup Dynamics 365 Business Central Datasource

To configure a Dynamics 365 Business Central datasource instance in SharpSync you need:

* The base API path of Dynamics 365 Business Central cloud which is: `https://api.businesscentral.dynamics.com`
*   Dynamics 365 Business Central uses the OAuth 2.0 protocol to authenticate, therefore, a code grant url, a refresh token url and the OAuth scopes need to be supplied, they are of the form:

    * Code Grant URL:&#x20;

    ```plaintext
    https://login.microsoftonline.com/{{sharpsync-app-tenant-id}}/oauth2/v2.0/authorize
    ```

    * Refresh Token URL:&#x20;

    ```markdown
    https://login.microsoftonline.com/{sharpsync-app-tenant-id}/oauth2/v2.0/token
    ```

    * Scopes:

    ```
    https://api.businesscentral.dynamics.com/.default offline
    ```
* Your Dynamics 365 Business Central instance company id and environment:
  * The company id can be obtained by following these steps from your Dynamics 365 Business Central instance web interface:
    * Log in to your Business Central account and navigate to the Companies page (search for `Companies` and navigate to the page.
    * Select the company you want to use.
    * Click the question mark button on the top right corner and select Help & Support.
    * Under the troubleshooting section, click Inspect pages and data.
    * In the inspector, you can enter Id in the search field and the first result contains your company ID.
    * Id (8000, GUID) or $systemId (2000000000, GUID)
  * The company environment is usually visible on the top right of your Dynamics 365 Business Central instance web interface.
    * The environment is usually `Production` or `Sandbox`
* All the above credentials (except for your instance's company id and environment) will already be set in SharpSync when you are configuring your data source.

### Configure Dynamics 365 Business Central Datasource

* Login on the application
* Navigate to `Data Sources`
* On the right > Select Dynamics365 > Add
* Click the configure button
* On the first tab `Authentication`, select the `OAuth 2.0` authentication type
* On the second tab `Configuration`, enter your Company Id and Environment as described in the previous section
* Click the `Save` button
* Click the `Authenticate` button

### Definitions

Dynamics 365 Business Central API selected definitions:

* Tenant: A tenant is an organization or directory.
* Application: A unique application that belongs to a tenant and has a unique ID (guid)
* Company: A tenant may have multiple companies (e.g. an organization may trade as different company names in different regions)
* Items: An item is created in a company and is queried using the company id
