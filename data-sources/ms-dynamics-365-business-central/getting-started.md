---
icon: sparkle
---

# Getting Started

{% hint style="danger" %}
This document is a work in progress.

Check back here  frequently for updates.
{% endhint %}

### Install the SharpSync app for Dynamics 365 Business Central

SharpSync connects to Business Central through the **SharpSync for Dynamics 365 Business Central** app, published by CADSharp on [Microsoft Marketplace](https://marketplace.microsoft.com/en-us/product/PUBID.cadsharp%7CAID.sharpsync-for-dynamics-365-bc%7CPAPPID.9ab23efc-f2ae-4a3e-8514-3a6cb62a713e). The app is free, but it needs a SharpSync subscription. It runs on Business Central online, version 26 or later.

Install it while signed in to Business Central as an administrator. The permission set you need is usually `EXTENSION MGT. - ADMIN`.

To install the app from inside Business Central:

1. Search for **Microsoft AppSource apps** and open the page.
2. Find **SharpSync for Dynamics 365 Business Central** (publisher CADSharp) and open it.
3. On the **App overview** page, choose **Install App**.
4. On the **Install extension** page, choose the language and then **Install**. Installing during business hours may disrupt other users. Your session refreshes when the installation is done.

You can also start from the [Marketplace listing](https://marketplace.microsoft.com/en-us/product/PUBID.cadsharp%7CAID.sharpsync-for-dynamics-365-bc%7CPAPPID.9ab23efc-f2ae-4a3e-8514-3a6cb62a713e): choose **Get it now**, sign in with the account you use for Business Central, and follow the prompts.

To check the installation, search for **Extension Management**. The app is listed with **Is Installed** checked and **Published As** set to `Global`.

{% hint style="warning" %}
**Installed an earlier version from a `.app` file?** Earlier versions were uploaded by hand, and Extension Management shows them with **Published As** set to `PTE`. That copy has the same app ID as the AppSource app, so Business Central won't install the AppSource app next to it. On the **Extension Management** page, select the old copy, choose **Uninstall**, then **Unpublish**, and install from AppSource as described above. Afterwards, check that the SharpSync user still has the `SSY SHARPSYNC` permission set (see the next section).
{% endhint %}

For more information, see Microsoft's [Install and Uninstall Extensions (Apps) in Business Central](https://learn.microsoft.com/en-us/dynamics365/business-central/ui-extensions-install-uninstall).

### Give the SharpSync user access in Business Central

SharpSync calls Business Central as the user who signs in when you connect the datasource. Unless that user has the `SUPER` permission set, assign them:

* the permission set `SSY SHARPSYNC` (name *SharpSync*), which the extension installs and which gives access to SharpSync's API pages;
* permissions to read and change the records SharpSync syncs (items, resources, BOMs, production BOMs and routings), usually through the permission sets the user's role already has.

To assign the permission set, search for **Users**, open the user, and add `SSY SHARPSYNC` under **User Permission Sets**.

### Setup Dynamics 365 Business Central Datasource

To configure a Dynamics 365 Business Central datasource instance in SharpSync you need:

* The base API path of Dynamics 365 Business Central cloud which is: `https://api.businesscentral.dynamics.com`
*   Dynamics 365 Business Central uses the OAuth 2.0 protocol to authenticate, therefore, a code grant url, a refresh token url and the OAuth scopes need to be supplied. **Both URLs must contain your organization's own Microsoft Entra tenant id.** You can find it in the [Microsoft Entra admin center](https://entra.microsoft.com) under Overview, as `Tenant ID` (also called the Directory ID). The URLs are of the form:

    * Code Grant URL:&#x20;

    ```plaintext
    https://login.microsoftonline.com/<your-tenant-id>/oauth2/v2.0/authorize
    ```

    * Code Grant URL (alternate for users belonging to multiple tenants):

    <pre data-overflow="wrap"><code>https://login.microsoftonline.com/&#x3C;your-tenant-id>/oauth2/v2.0/authorize?prompt=select_account
    </code></pre>

    * Refresh Token URL:&#x20;

    ```plaintext
    https://login.microsoftonline.com/<your-tenant-id>/oauth2/v2.0/token
    ```

    {% hint style="danger" %}
    SharpSync pre-fills both URLs with a tenant id that is **not yours**. If you leave them unchanged, sign-in appears to succeed but you are authenticated against the wrong directory, and the first request to Business Central fails with `Internal_CompanyNotFound`. The tenant id cannot go in the base API path instead: Business Central reads the tenant from the access token.
    {% endhint %}

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
  * The environment is the **name** of your Business Central environment, not its type. The default production environment is named `Production` and a default sandbox `Sandbox`, but an environment created with its own name, such as `SB-01072026`, must be entered with that exact name.
    * You can read the name in the [Business Central admin center](https://businesscentral.dynamics.com/admin) under Environments, or from the segment after `businesscentral.dynamics.com/` in the web client URL.
* The base API path and the OAuth scopes are already set in SharpSync when you configure your data source. The two OAuth URLs are pre-filled but must be edited with your tenant id, and the company id and environment must be entered.

### Configure Dynamics 365 Business Central Datasource

* Login to SharpSync
* Navigate to `Data Sources`
* On the right > Select Dynamics365 > Add
* Change the `Server url` value to: `https://businesscentral.dynamics.com`
* Change the `Primary Identifier` value to: `itemNumber`
* Change the `Alternative Identifier`  value to: `resourceNumber`
* Click on the `UPDATE` button
* Next, click the `CONFIGURE` button
* On the first tab `Authentication`, select the `OAuth 2.0` authentication type, then replace the tenant id in both `OAuth Url` and `OAuth Token Url` with your own, as described in the previous section
* On the second tab `Configuration`, enter your Company Id and Environment as described in the previous section
* Click the `Save` button
* Click the `Authenticate` button and sign in with a Business Central user of your organization

{% hint style="info" %}
Complete the configuration before you authenticate. Saving the configuration again later requires authenticating again.
{% endhint %}

### Troubleshooting

**Sign-in completes without any prompt, and the first request fails with `Internal_CompanyNotFound`.** You are authenticated against the wrong tenant. Check that both OAuth URLs on the `Authentication` tab carry your own tenant id, save, and authenticate again. If your browser is signed in to several Microsoft accounts, use the alternate Code Grant URL with `?prompt=select_account` to get the account picker.

**The data source connects, but fetching accessors fails with `NoEnvironment` or "Environment does not exist".** Authentication is fine, but the `Environment` value is not the name of your environment. Enter the exact environment name, as shown in the Business Central admin center, rather than the environment type.

**Sign-in ends with "access request received" or a status of "submitted".** Your tenant requires an administrator to approve third-party applications before users can consent to them. This is a one-time action per tenant, not a SharpSync error.

* Who can approve: a Global Administrator, Privileged Role Administrator, Cloud Application Administrator or Application Administrator in Microsoft Entra. Business Central administrator rights are not enough, as they belong to a different system.
* How to approve: either act on the pending request under Microsoft Entra admin center > Enterprise applications > Admin consent requests, or have an administrator open the following URL, which grants consent for the whole organization at once:

```plaintext
https://login.microsoftonline.com/<your-tenant-id>/adminconsent?client_id=d811abe5-64d6-49ce-bc9e-4c0a64935d53
```

* What is approved: delegated permissions only, that is _Access Dynamics 365 Business Central as the signed-in user_ and _Sign in and read user profile_. SharpSync only ever acts as the user who signed in, and its access ends when that account is disabled.
* After clicking Accept, the administrator is redirected to a SharpSync page that may show an error. The consent is granted regardless. You can verify it under Enterprise applications > SharpSync > Permissions.

### Definitions

Dynamics 365 Business Central API selected definitions:

* Tenant: A tenant is an organization or directory.
* Application: A unique application that belongs to a tenant and has a unique ID (guid)
* Company: A tenant may have multiple companies (e.g. an organization may trade as different company names in different regions)
* Items: An item is created in a company and is queried using the company id
