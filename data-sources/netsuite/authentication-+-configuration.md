---
description: Configuration of a Netsuite Datasource
---

# Authentication + Configuration

NetSuite supports a number of types of authentication. The auth method supported in SharpSync is OAuth 2. The configuration values in SharpSync are shown below.

{% tabs %}
{% tab title="Authentication" %}
Base API path

<mark style="color:orange;">This is the full link \[including your customer id] to the NetSuite Instance. Typically something like:</mark>

```
https://[customerId].suitetalk.api.netsuite.com
```

Authentication Types (see below\*\*)

<mark style="color:orange;">The type of authentication method to use when authenticating with NetSuite. We only support:</mark>

> OAuth 2.0

OAuth Url

<mark style="color:orange;">The URL where authentication initially happens</mark>

```
https://[customerId].app.netsuite.com/app/login/oauth2/authorize.nl
```

OAuth Token Url

<mark style="color:orange;">The URL where tokens are created / renewed. Typically something like</mark>&#x20;

```
https://[customerId].suitetalk.api.netsuite.com/services/rest/auth/oauth2/v1/token
```

OAuth Client Client Id

<mark style="color:orange;">The OAuth Client Id registered for the SharpSync application</mark>

> {some long string of text}

OAuth Client Secret

<mark style="color:orange;">The OAuth Client secret registered for the SharpSync application</mark>

> {some long string of text}

OAuth Scopes

<mark style="color:orange;">Think of this as the permissions needed by your OAuth application. You should set this to:</mark>

```
rest_webservices,restlets
```
{% endtab %}

{% tab title="Configuration" %}

{% endtab %}
{% endtabs %}



<details>

<summary> ** Authentication Types</summary>

* OAuth 2.0 - the latest greatest method which is the most robust and secure
* API Key - not currently supported
* Basic Auth (username / password) - not currently supported

</details>
