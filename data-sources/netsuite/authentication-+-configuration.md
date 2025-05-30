---
description: Configuration of a Netsuite Datasource
icon: shield
---

# Authentication + Configuration

NetSuite supports a number of types of authentication. The auth method supported in SharpSync is OAuth 2. The configuration values in SharpSync are shown below.

**Authentication**

* In SharpSync, navigate to `Data Sources`&#x20;
* Select the NetSuite Data Source > `Add`
* Set the `Server Url`

```
https://[customerId].app.netsuite.com
```

* Click `Update`
* Click the Ping button to test the connection. This is optional and will not work if ICMP is disabled on the server
* Click the 'Configure' button
* You can now configure Authentication & Configuration

{% tabs %}
{% tab title="Authentication" %}
Base API path

<mark style="color:orange;">This is the full link \[including your customer id] to the NetSuite Instance. Typically something like:</mark>

```
https://[customerId].suitetalk.api.netsuite.com
```

Authentication Types

<mark style="color:orange;">The type of authentication method to use when authenticating with NetSuite. We only support:</mark>

```
OAuth 2.0
```

OAuth Url

<mark style="color:orange;">The URL where authentication initially happens:</mark>

```
https://[customerId].app.netsuite.com/app/login/oauth2/authorize.nl
```

OAuth Token Url

<mark style="color:orange;">The URL where tokens are created / renewed. Typically something like:</mark>

```
https://[customerId].suitetalk.api.netsuite.com/services/rest/auth/oauth2/v1/token
```

OAuth Client Client Id

<mark style="color:orange;">The OAuth</mark> <mark style="color:orange;"></mark><mark style="color:orange;">`clientId`</mark> <mark style="color:orange;"></mark><mark style="color:orange;">registered for the SharpSync application. Usually generated  by the integration record created:</mark>

> {some long string of text}

OAuth Client Secret

<mark style="color:orange;">The OAuth</mark> <mark style="color:orange;"></mark><mark style="color:orange;">`clientSecret`</mark> <mark style="color:orange;"></mark><mark style="color:orange;">registered for the SharpSync application. Usually generated  the first time the integration record is created:</mark>

> {some long string of text}

OAuth Scopes

<mark style="color:orange;">Think of this as the permissions needed by your OAuth application  to send and receive data. The</mark> <mark style="color:orange;"></mark><mark style="color:orange;">`restlets`</mark> <mark style="color:orange;"></mark><mark style="color:orange;">scope is optional but highly recommended. Without it you will not be able to upload thumbnails. You should set this to:</mark>

```
rest_webservices,restlets
```
{% endtab %}

{% tab title="Configuration" %}
Default item type to use when creating BOM item

<mark style="color:orange;">When creating a new item, if the item cannot be reliably determined from SharpSync, this is the fallback type to be used (this is currently superseded by the values in the</mark> [<mark style="color:orange;">itemType property mapping</mark>](common-setup/item-type-mapping.md)<mark style="color:orange;">), example:</mark>

```
inventoryitem
```

Item types to load properties for

<mark style="color:orange;">When loading accessors (NetSuite Fields) and property mappings, these are the item types searched for:</mark>

<mark style="color:orange;">Simple Boms:</mark>

{% code overflow="wrap" %}
```
assemblyitem|inventoryitem|noninventoryresaleitem|noninventorysaleitem|noninventorypurchaseitem|assemblyitemmember
```
{% endcode %}

<mark style="color:orange;">Advanced BOMs:</mark>

{% code overflow="wrap" %}
```
assemblyitem|inventoryitem|noninventoryresaleitem|noninventorysaleitem|noninventorypurchaseitem|bom|bomrevision|bomrevisioncomponent|manufacturingrouting
```
{% endcode %}

Servlet URL

<mark style="color:orange;">The url of the servlet where thumbnails will be uploaded and advanced BOMs item created.</mark>

<mark style="color:orange;">Simple Boms:</mark>

{% code overflow="wrap" %}
```
https://{companyId}.restlets.api.netsuite.com/app/site/hosting/restlet.nl?script={scriptId}&deploy={deploymentId}&folderId={folderId}
```
{% endcode %}

<mark style="color:orange;">Advanced BOMs (use the same value for</mark> <mark style="color:orange;"></mark><mark style="color:orange;">`script`</mark>  <mark style="color:orange;"></mark><mark style="color:orange;">and</mark> <mark style="color:orange;"></mark><mark style="color:orange;">`itemInjectionScriptId`</mark> <mark style="color:orange;"></mark><mark style="color:orange;">):</mark>

{% code overflow="wrap" %}
```
https://{companyId}.restlets.api.netsuite.com/app/site/hosting/restlet.nl?script={scriptId}&itemInjectionScriptId={scriptId}&deploy={deploymentId}&folderId={folderId}
```
{% endcode %}

Use server side scripting for items

<mark style="color:orange;">Use the server to update custom fields for items which are not surfaced in the NetSuite api. (check this if using advanced BOMs)</mark>

Use advanced BOMs

<mark style="color:orange;">Check to let SharpSync know that you use advanced BOMs in NetSuite</mark>

BOM Naming Scheme

<mark style="color:orange;">The scheme that SharpSync uses to search for and generate BOM names</mark>

BOM Revision Naming Scheme

<mark style="color:orange;">The scheme that SharpSync uses to search for and generate BOM revision names</mark>
{% endtab %}
{% endtabs %}
