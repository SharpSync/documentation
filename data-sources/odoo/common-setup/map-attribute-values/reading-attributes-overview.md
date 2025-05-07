---
icon: magnifying-glass
---

# Reading Attributes - Overview

Before reading the below, make sure to read [.](./ "mention")

{% hint style="warning" %}
The following article describes how to show single value attributes in a column for SharpSync. It does not show how to write the values. Please make sure to mark columns showing single values as Read-Only
{% endhint %}

An Odoo product template can have many attribute mappings. To break these down into individual columns in SharpSync, make one or more of the following changes:

* Map the attribute names
* Map individual attribute values
* Map all attribute values

### But first, an overview of displaying the values from Odoo

When mapping to _product.template_.color

> a value of <mark style="color:red;">Red</mark> **AND** <mark style="color:orange;">Satin Black</mark>. will be displayed on screen.&#x20;

But when mapping to _product.product_.color.

> a value of Red <mark style="color:orange;">**OR**</mark> Satin Black will be displayed on screen

When mapping to _product.template_.finish

> a value of <mark style="color:blue;">Matte</mark> **AND** <mark style="color:purple;">Gloss</mark>. will be displayed on screen.&#x20;

But when mapping to _product.product_.finish.

> a value of Matte <mark style="color:orange;">**OR**</mark> Gloss will be displayed on screen

### &#x20;Querying List Values for all attributes

To view the values of a product template's attributes in SharpSync, use the Property Mapping list `product.attribute` &#x20;

Start by adding a [Property Mapping ](../../../../fundamentals/property-mappings.md)for&#x20;

> product.template.atributes

<table><thead><tr><th width="284">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Primary Accessor</td><td>(Unmapped)</td></tr><tr><td>Secondary Accessor</td><td><code>product.template.attributes</code></td></tr><tr><td>List Name</td><td><code>product.attribute</code></td></tr><tr><td>List Value Selector</td><td>{id}:{name}</td></tr><tr><td>Prefer Odoo Value</td><td>checked</td></tr><tr><td>Update Odoo on submit</td><td><p>! enable only if mapping a single value ! </p><p>[You have been warned!] </p></td></tr></tbody></table>



* Open the Property Mapping > Settings and enter a `List Name` of&#x20;

> product.attribute

Click the `Save` button. The list preview will update with the values of all attributes available in Odoo. If you want to only map the attribute names, move to [display-all-attribute-names.md](display-all-attribute-names.md "mention")

{% hint style="info" %}
### Choose what to display

You'll want to display _one_ of the following:

* The mapped attribute names (e.g. Color, Finish, Legs)
* The mapped attribute values (e.g. Color: Black, Red, Satin Black)



For more complicated mappings, please contact us through the support portal.
{% endhint %}

Read the next articles in line to work with all attributes or single attribute names.
