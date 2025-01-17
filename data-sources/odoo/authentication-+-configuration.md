# Authentication + Configuration



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
odoo17
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

Use this scheme to create new BOM names

<mark style="color:orange;">This value defines the naming scheme to name BOM names. It is calculated at runtime</mark>

```
{rowData.componentName}_BOM
```

Use this scheme to create new BOM names

<mark style="color:orange;">This value defines the naming scheme to name BOM revisions. It is calculated at runtime</mark>

```
{rowData.componentName}_{rowData.cells.revision}
```
{% endtab %}
{% endtabs %}

<details>

<summary>** Authentication Types</summary>

* OAuth 2.0 - not support - contact us for implementation&#x20;
* API Key - not currently supported
* Basic Auth (username / password) - supported

</details>
