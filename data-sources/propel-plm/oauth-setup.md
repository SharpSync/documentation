---
icon: shield
---

# OAuth Setup

## Propel (Salesforce) OAuth Setup

Salesforce supports industry standard OAuth 2.0 authentication and authorization.

NOTES:

1. The user setting up OAuth in Salesforce needs to have `System Admin` permissions.

### Step: Create a new Salesforce OAuth Connected App&#x20;

* In your salesforce instance, go to `Setup`
* In `Setup` , under `PLATFORM TOOLS` go to `Apps` > `External Client Apps` > `Settings`&#x20;
* Toggle the `Allow creation of connected apps` option on
* Then click on `New Connected App` to start created a new app.

<figure><img src="../../.gitbook/assets/Screenshot 2025-11-07 at 8.24.14 AM (2).png" alt=""><figcaption></figcaption></figure>

* In the `New Connected App` page, fill out the following:
* The Connected App Name
* The Contact Email
* Check the `Enable OAuth Settings` checkbox
* Check the `Enable for Device Flow` checkbox
* Under Callback URL:
  * Keep the default Salesforce callback URL: [https://login.salesforce.com/services/oauth2/success\
    ](https://login.salesforce.com/services/oauth2/success) for production instances and [https://test.salesforce.com/services/oauth2/success](https://test.salesforce.com/services/oauth2/success) for sandbox instances.
  * On a new line, add the SharpSync callback URL: [https://app.sharpsync.net/callback-oauth](https://app.sharpsync.net/callback-oauth)
* Select the following `Selected OAuth Scopes`:
  * Access the identity URL service (id, profile, email, address, phone)
  * Access unique user identifiers (openid)
  * Full access (full)
  * Manage user data via APIs (api)
  * Manage user data via Web browsers (web)
  * Perform requests at any time (refresh\_token, offline\_access)
* Uncheck the `Require Proof Key for Code Exchange (PKCE) Extension for Supported Authorization Flows` checkbox
* Check the `Require Secret for Web Server Flow` checkbox
* Check the `Require Secret for Refresh Token Flow` checkbox
* Keep all other checkboxes unchecked
* Then click on the `SAVE` button
* You will be redirected to the newly created Connected App page.
* Click on the `Manage Consumer Details` button

<figure><img src="../../.gitbook/assets/Screenshot 2025-11-07 at 8.42.35 AM.png" alt=""><figcaption></figcaption></figure>

In the next page, make sure to save the `Consumer Key` and `Consumer Secret` in a secure location because they will be used in the next steps in SharpSync to set up your Propel DataSource connections settings.

You are now ready and you can move on to configuring Propel as a datasource in SharpSync

For more info, you can find links to the Salesforce docs related to the OAuth 2.0 setup below:

[Authorization Through Connected Apps and OAuth 2.0](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_oauth_and_connected_apps.htm)

