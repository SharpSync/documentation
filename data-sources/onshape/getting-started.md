---
icon: sparkle
---

# Getting Started

## Onshape Setup

* [Onshape App Store Subscription](getting-started.md#onshape-app-store-subscription)
* Setting up Onshape
* Configure the authentication options
* Mapping values to ERP
* Export Assembly from Onshape
* Export Part Studio Part from Onshape
* Export Release Package Contents (Assemblies & Parts) from Onshape
* Troubleshooting

`Note:` Onshape integration for SharpSync is not currently supported for free versions. For SharpSync to work, you need to create custom properties in a company in Onshape. This is only possible with the Professional or Enterprise versions of Onshape. This is a limitation of Onshape.

### Onshape App Store Subscription

<figure><img src="../../.gitbook/assets/onshape_subscribe.png" alt=""><figcaption></figcaption></figure>

* Find and subscribe to the [SharpSync.net](https://appstore.onshape.com/apps/ERP%20Connector/D375UTR5KWRHUES3LFV56CCHA5VBYDFPXTGPXPQ=/description) OnShape app in the Onshape App Store&#x20;

<figure><img src="../../.gitbook/assets/onshape_app_permissions (1).png" alt=""><figcaption></figcaption></figure>

* For Onshape Enterprise users, make sure to add the related Onshape users to the list of users that have permission to access the Onhsape App Store&#x20;

### Setting up Onshape Data Source in SharpSync

* In the Navigation Bar, select Data Sources
* On the right (in Data Sources), select OnShape as the Data Source and click ADD DATASOURCE
* Change the Server URL to:

```
https://cad.onshape.com
```

* Scroll to the bottom of the page and click UPDATE
* Change the Authentication Type to OAuth2
* [Configure the authentication options](getting-started.md#configure-the-authentication-options)
* Click AUTHENTICATE. If the configuration is successful, the Authentication Status will update and show <mark style="color:green;">Connected</mark>.&#x20;
* Configure the Primary and Secondary Identifiers as follows:
  * Primary Identifier: `partNumber`
  * Secondary Identifier: `name`&#x20;

After successfully authenticating with OnShape, the update should automatically trigger. If it does not, click the _Update_ button.

The custom fields from OnShape have now successfully been pulled into SharpSync.

Please make sure to set up your Secondary Data Source. For more information, refer to the other Data Sources listed in the Navigation Bar.

#### Configure the authentication options

* Click CONFIGURE
* Choose one of the two paths below depending on your version of our product:

{% tabs %}
{% tab title="Professional or Free version" %}
Base API path

<mark style="color:orange;">The Base API we use to connect</mark>

* Leave the default link to the OnShape instance as-is<mark style="color:orange;">:</mark>

> https://cad.onshape.com/api

#### Authentication Types (see below\*\*)

<mark style="color:orange;">The type of authentication method to use when authenticating with OnShape.</mark>&#x20;

We only support:

> OAuth 2.0

* Select OAuth 2.0

Once you have selected the Authentication Type, the remainder of the form should autocomplete to the following:

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1).png" alt="Image of configuration settings"><figcaption><p>Configuration Settings</p></figcaption></figure>

* Click SAVE to save and close the form
{% endtab %}

{% tab title="Enterprise" %}
Base API path

<mark style="color:orange;">The Base API we use to connect</mark>

* Fill in the values for your enterprise name. This means that, in the url, instead of

> https://cad.onshape.com/api

you will have

{% code fullWidth="false" %}
```
https://{enterpriseName}.onshape.com/api 
```
{% endcode %}

#### Authentication Types (see below\*\*)

<mark style="color:orange;">The type of authentication method to use when authenticating with OnShape.</mark>&#x20;

We only support:

> OAuth 2.0

* Select OAuth 2.0

Once you have selected the Authentication Type, the remainder of the form should autocomplete to the following:

<figure><img src="../../.gitbook/assets/image (3).png" alt="Image of configuration settings" width="563"><figcaption><p>Configuration Settings</p></figcaption></figure>

* Click SAVE to save and close the form
{% endtab %}
{% endtabs %}

Please make sure to set up your Secondary Source. For more information, refer to the other Data Sources listed in the Navigation Bar.

Once completed with the setup for both Primary and Secondary Data Sources, proceed to setup [Property Mappings](../../fundamentals/property-mappings/).

### Mapping values to ERP

#### Mapping the URL to a field in the ERP

Onshape is an online Data Source, so you can map the URL of the document to a field in the ERP.

To map an Onshape document URL to a property in the ERP do the following:

* Create a new property mapping
* Onshape property: `(Unmapped)`
* ERP property: `{nameOfField}` e.g. `document_url`
* Rendering Type: `URL`
* Create a new rule: `Text manipulation`
* Rule value: `return rowData.primaryViewHref;`

<figure><img src="../../.gitbook/assets/onshape_update_import_rule.png" alt=""><figcaption></figcaption></figure>



* Click Save
* Reload the BOM to display in the BOM Comparison screen
* Click Submit to update

### Export Assembly from Onshape

*   You can export a Assembly BOM from Onshape to SharpSync via:

    * The Onshape Tree Structure Assembly Context Menu&#x20;

    <figure><img src="../../.gitbook/assets/onshape_export_to_sharpsync.png" alt=""><figcaption></figcaption></figure>

    * The Onshape Assembly Tab Context Menu&#x20;

    <figure><img src="../../.gitbook/assets/onshape_export_assembly_tab.png" alt=""><figcaption></figcaption></figure>

    ### Export Part Studio Part from Onshape
* You can export a Part Studio Part BOM from Onshape to SharpSync via:
  * The Onshape Part Studio Tab Part Context Menu&#x20;

<figure><img src="../../.gitbook/assets/onshape_export_individual_part.png" alt=""><figcaption></figcaption></figure>

### Export Release Package Contents (Assemblies & Parts) from Onshape

* You can export the contents (Assemblies & Parts)  of a Release Package from Onshape to SharpSync via:
  * Selecting the desired Release Package by Left-Clicking on the related version in the Onshape Document Version Tree
  * Clicking on "Export Release Contents" in the Onshape Assembly Tab Context Menu&#x20;
* SharpSync will query the Release Package for Assemblies and Parts and will automatically create a SharpSync BOM per top level Assembly and Part and ignoring their nested children.

<figure><img src="../../.gitbook/assets/Screenshot 2025-07-07 at 3.47.08 PM.png" alt=""><figcaption></figcaption></figure>

### Troubleshooting

#### Could not find data source

The following response in the network tab with a 400 status code

```
Request Method: GET
Status Code: 400 Bad Request

Could not add file lookup to the server
{"message":"Invalid id specified or bad request"}

OR

{"message":"Could not find datasource for organization {uuid} and module Onshape","data":[]}
```

You are trying to sync to an organization in SharpSync that has not yet been created.&#x20;

First, create the organization by following the setup steps in [Data Sources](../../fundamentals/data-sources.md).

#### Cannot get properties

The following response in the network tab with a 500 status code

```json
Request Method: GET
Status Code: 502 Bad Gateway

{
    "message": "Could not find the Onshape company that the user belongs to when loading the BOM"
}
```

This means that you're attempting to use SharpSync with a free version. Free versions are currently not supported due to a limitation in Onshape that you cannot create custom properties (True at the time of writing this)

#### Cannot pull Onshape BOM

The following response in the network tab with a 502 status code

```json
{
    "message": "Could not pull the bom from Onshape - request was Forbidden. Check that the document hostname/origin matches the datasource server hostname"
}
```

Let's say the enterprise name is `starkindustries.onshape.com` .

Check:

* The hostname is configured correctly in the datasource (e.g. cad.onshape.com vs starkindustries.onshape.com)
* The oauth hostnames all match
  * For free version: `oauth.onshape.com` and NOT `cad.onshape.com`
  * For enterprise version `starkindustries.onshape.com` and NOT `oauth.onshape.com`
