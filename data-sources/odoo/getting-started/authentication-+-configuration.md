---
icon: user
---

# Authentication + Configuration

To integrate with an Odoo instance, we have to setup an&#x20;

* In the Navigation Bar, select Data Sources
* On the right (in Data Sources), select Odoo as the Data Source and click the `ADD DATASOURCE`  button&#x20;
*   Change the Server URL to:

    ```
    https://{myEnterpriseInstance}.dev.odoo.com
    ```
* Scroll to the bottom of the page and click UPDATE
* Click CONFIGURE
* On the first tab (Authentication), change the Base API Path to: `https://{myEnterpriseInstance}.dev.odoo.com`
* Leave the Authentication Types drop-down on `Basic Authentication`.
* Enter the Database name.&#x20;

{% hint style="info" %}
To find the database name navigate to [https://{myEnterpriseInstance}/web/database/selector](https://your-odoo-instance/web/database/selector)
{% endhint %}

{% hint style="warning" %}
The database name is case sensitive.
{% endhint %}

* Enter the username and password.
* The configuration should look something like the image below:

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

* On the second tab (BOM Configuration) enter the version number  in the form of 2 digits.
* Click the `SAVE` button to save your connection details and close the form.
* Click `AUTHENTICATE`. If the configuration is successful, the Authentication Status will update and show <mark style="color:green;">Connected</mark>.&#x20;

### Primary and Alternative component identifiers

The primary and alternative identifiers can be used for product.product  or product.template, though if you plan on using configurations, you would probably select a field for `product.product`

Configure the [Primary and Alternative ](../../../fundamentals/data-sources.md)component Identifiers as follows:

* Primary Identifier: `default_code`
* Alternative Identifier:  `name`

{% hint style="info" %}
You can specify any Primary Identifier (field linked to the product.template model) that is used on product templates e.g. name, barcode. You can even use custom field names, as long as the field uniquely identifies the part (it must be a unique identifier)
{% endhint %}

{% hint style="info" %}
An alternative identifier is that field which is used when no value (or a blank value) is found in the Primary Identifier
{% endhint %}



Odoo supports a number of types of authentication. The auth method supported in SharpSync is `Basic`. The configuration values in SharpSync are shown below.

{% tabs %}
{% tab title="Authentication" %}
Base API path

<mark style="color:orange;">This is the full link \[including your customer id] to the Odoo Instance. Typically something like:</mark>

```url
https://odoo.sharpsync.net
```

Authentication Types (see below\*\*)

<mark style="color:orange;">The type of authentication method to use when authenticating with NetSuite. We only support:</mark>

> Basic

Database name

<mark style="color:orange;">The name of the database in Odoo. Take note that it is case sensitive</mark>

```
odoo.app.17.713
```

Basic Auth Username

<mark style="color:orange;">The name of the user connecting to Odoo.</mark>&#x20;

```
yourusername@domain.com
```

Basic Auth Password

<mark style="color:orange;">The password used to connect with the associated username</mark>


{% endtab %}

{% tab title="Configuration" %}
Major version of Odoo that you have (e.g. 17 or 16)

<mark style="color:orange;">The version of Odoo that is installed and will be used. 2 digit number</mark>

```
17
```

If specified, use this custom property to display the name in the hierarchy

<mark style="color:orange;">A property used to show the names of components in SharpSync.</mark>

```
name
```

If specified, loads additional properties from these models (each on new line)

<mark style="color:orange;">Additional models to load custom property mappings for. These are custom per installation (if any)</mark>

```
vehicle.make
vehicle.model
```

Use this scheme to create new BOM revision names

<mark style="color:orange;">This value defines the naming scheme to search for existing BOM names. It is calculated at runtime based on the Primary DataSource's row\*\*</mark>

```
{rowData.componentName}_{rowData.cells.revision}
```
{% endtab %}
{% endtabs %}

\*\* For searching, this is based on the raw data from the Primary Source data before any Property Mapping rules (Import) have run. When a new BOM is created, the final value from the rows cells will be used. So if you manipulate the text in SharpSync to

1. Change from&#x20;
2. `Assembly A1, Rev A` to&#x20;
3. `Assembly A1, Rev B`
4. Then the Bill of Materials Name will be `A1_B`instead of `A1_A`

<details>

<summary>** Authentication Types</summary>

* OAuth 2.0 - not support - contact us for implementation&#x20;
* API Key - not currently supported
* Basic Auth (username / password) - supported

</details>
