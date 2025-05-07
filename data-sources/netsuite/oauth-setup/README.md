---
icon: shield
---

# OAuth Setup

## NetSuite OAuth Setup

NetSuite supports industry standard OAuth authentication and authorization. The implementation in SharpSync uses OAuth2 exclusively at this point. Should you require a different implementation such as API keys or OAuth1.0, please engage us

* Create a new integration record
* Scopes
* Permissions
* Testing the OAuth setup

NOTES:

1. In the setup steps below, the value of `{{netsuite-api}}` takes the form

> `https://{companyId}.suitetalk.api.netsuite.com`

2. In the setup steps below, the value of `{companyId}` is the numerical value of your company id
3. You need the permission `Permissions > Setup > Access Token Management` to make `Integration Record` changes

### Step: Create a new integration record

For this to happen you need the permissions

* Lists > Integration application and
* Setup > Integration Application

To create a new integration record do the following:

* Click Setup > Other Setup > Manage Integrations > New
* Enter a name for the integration
* Check the boxes for:
  * Authorization Code Grant
  * Restlets
  * Rest Web Services
* Uncheck the boxes for:
  * TBA: Authorization Flow
* Paste the following OAuth 2.0 Redirect URI: `https://app.sharpsync.net/callback-oauth`
* Get the consumer key (clientId) and consumer secret (client secret) - this will be used later.

Make sure to tick `Ask first time` under `OAUTH 2.0 CONSENT POLICY`

### Step: Create new role

Create a new Role to assign to users that will login.

* Click Setup > Users/Roles > Manage Roles
* Click New Role
* Set the [permissions](permissions.md)

{% hint style="danger" %}
If setting the `Accessible Subsidiaries`to `User Subsidiary`this may cause problems with viewing items that are not in the user's subsidiary.

You may then also want to enable `Allow Cross-Subsidiary Record Viewing`
{% endhint %}

### Step: Scopes

NetSuite requires that the OAuth flow uses the following scopes. Ensure they are enabled for the OAuth connection to work:

* rest\_webservices
* restlets

This forms part of the URL when performing the initial browser redirect

There are 3 steps to authenticating with NetSuite using OAuth:

* A browser redirect
* A call to the token endpoint to get the token
* A call to refresh the token

For the first 2 steps the OAuth client id and secret is required. If successful, you'll be issued an access token. For the last step the refresh token is required to renew your access token

### Step: Permissions

Permissions Required for NetSuite Integration role. These permissions are quired for the successful setup of SharpSync. Some of the permissions are not required for your users (the role your users will be logging in with), and are marked as `Setup Only` below. Once all the permissions has been setup, make sure to test the setup

#### Under Setup -> Records Catalogue

Search for a record you need, then click on the "SuiteScript and REST Query API"

Navigate to the role you want to assign the permissions to, and add the following permissions:

> Setup > Roles > Manage Roles > Developer (Or whichever role you want to assign the permissions to)

For a comprehensive list of permissions required for users and for setting up the integration, refer to the [Permissions](permissions.md) page

### Testing the OAuth setup

#### Step: Browser redirect

Execute the following /GET request in a browser window.

> https://`{companyId}`.app.netsuite.com/app/login/oauth2/authorize.nl?\
> &#x20;`response_type`=code&\
> &#x20;`client_id`={clientId}&\
> &#x20;`redirect_uri`=https://sharpsync.net/callback-oauth&\
> &#x20;`scope`=rest\_webservices,restlets&\
> &#x20;`state`={someValue must be between 22 and 1024 charachters}

This will redirect to the login page for the NetSuite account. Once logged in, the user will be redirected to the redirect\_uri (https://sharpsync.net) with the included state value.

> NOTE: The redirect\_uri must be https://sharpsync.net/callback-oauth and must be url encoded (https%3A%2F%2Fsharpsync.net%2Fcallback-oauth)

If this step is successful, you'll see the login window and a code at the end of the URL displayed in the browser.

#### Step: Get the refresh token

When the previous /GET request is executed, the user will be redirected to the `redirect_uri` with a code value. This code value is used to get the refresh token as well as a new access token.

Some terminology:

* `code` - issued by the OAuth 2 authentication cycle. Used to get the initial `access_token` and `refresh_token`
* `access_token` - let's you access resources in NetSuite. Expires after about an hour
* `refresh_token` - let's you get a new `access_token` repeatedly

You should treat an access token and a refresh token the same as a username and password! It is sensitive information

Execute the following /POST request to the token endpoint. This should be made within 30 seconds of the previous step. (Note that we're now using the `.suitetalk.api` subdomain)

> https://`{companyId}`.suitetalk.api.netsuite.com/services/rest/auth/oauth2/v1/token

The body of the request should be:

* x-www-form-urlencoded
* include the following values:

> `grant_type`=authorization\_code&\
> `redirect_uri`=https%3A%2F%2Fsharpsync.net%2Fcallback-oauth&\
> `code`={code}

The result of this request will look like this (`refresh_token` AND `access_token`)

```json
{
    "access_token": "",
    "refresh_token": "",
    "expires_in": "3600",
    "token_type": "Bearer"
}
```

The important bit here is the `refresh_token`. We'll use it to get us a new `access_token` later.

#### Step: Refresh the access token

Execute the following /POST request to the token endpoint. This should be made within 30 seconds of the previous step.

> https://`{companyId}`.suitetalk.api.netsuite.com/services/rest/auth/oauth2/v1/token

The body of the request should be:

* x-www-form-urlencoded
* include the following values:

> `grant_type`=authorization\_code&\
> `redirect_uri`=https%3A%2F%2Fsharpsync.net%2Fcallback-oauth&\
> `refresh_token`={refresh-token}

The result will look like this (no `refresh_token`, only the `access_token`)

```json
{
    "access_token": "",
    "expires_in": "3600",
    "token_type": "Bearer"
}
```

The important bit here is the `access_token` which gives us access to NetSuite.

Your preparation is now ready and you can move on to configuring NetSuite as a datasource

For more info, you can find links to the NetSuite docs related to the OAuth 2.0 Authorization Code Grant below:

[NetSuite OAuth 2.0 Authorization Code Grant Flow](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_158074210415.html)

[NetSuite Step One GET Request to the Authorization Endpoint](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_158081944642.html)

[NetSuite Step Two POST Request to the Token Endpoint](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_158081952044.html)

[NetSuite Step Three Refresh Token POST Request to the Token Endpoint](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section_158082518856.html)

[NetSuite Response Errors in Step Two and in the Refresh Token Response](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/subsect_158521832417.html)

