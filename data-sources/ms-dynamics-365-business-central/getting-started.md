---
icon: sparkle
---

# Getting Started

{% hint style="danger" %}
This document is a work in progress.

Check back here  frequently for updates.
{% endhint %}

### Install SharpSync custom extension for Dynamics 365 Business Central

When logged in to your SharpSync organization as an admin user:

* Navigate to [Downloads](https://app.sharpsync.net/admin/downloads)
* Locate and download the current version of the custom extension app which should look like `CADSharp LLC_sharpsync-dynamics-365-api-extension_#.#.#.#.app` `(IN PROGRESS)`
* Navigate to your Dynamics 365 Business Central web instance as an administrator
* Navigate to Extension Management: From the Business Central home page, use the search feature and type "Extension Management." Select the appropriate link.
* Upload the Extension: On the Extension Management page, click Manage and choose Upload Extension. You will be prompted to select the .app file for your extension.
* Deploy the Extension: After selecting the file, click Accept and then Deploy. If your extension includes schema changes, you may have to select a schema sync mode, such as Force Sync for development environments.
* Manage Permissions: Make sure that the user installing the extension has appropriate permissions, typically the EXTENSION MGT. - ADMIN permission set.
* Additional documentation can be found in the following links:
  * [Customizing Business Central online using apps](https://learn.microsoft.com/en-us/dynamics365/business-central/ui-extensions)
  * [Install and Uninstall Extensions (Apps) in Business Central](https://learn.microsoft.com/en-us/dynamics365/business-central/ui-extensions-install-uninstall)

### Setup Dynamics 365 Business Central Datasource

To configure a Dynamics 365 Business Central datasource instance in SharpSync you need:

* The base API path of Dynamics 365 Business Central cloud which is: `https://api.businesscentral.dynamics.com`
*   Dynamics 365 Business Central uses the OAuth 2.0 protocol to authenticate, therefore, a code grant url, a refresh token url and the oauth scopes need to be supplied, they are of the form:

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
    * Log in to your Business Central account and navigate to the Companies tab.
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
