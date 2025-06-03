---
icon: sparkle
---

# Getting Started

See [Authentication + Configuration](authentication-+-configuration.md) before continuing with the below.

{% hint style="warning" %}
It is important to start with something simple like Description and Quantity . Once you've confirmed that a Bill of Materials is being created according to your specifications and that is working to your liking, work your way up from there to more complicated mappings like routings, unique values, and generated values.
{% endhint %}

### Add Property Mappings

After [authenticating ](authentication-+-configuration.md)with Odoo, add the following [Property Mappings](../../../fundamentals/property-mappings/)

| Name        | Primary Accessor | Odoo accessor                |
| ----------- | ---------------- | ---------------------------- |
| Description | Description      | product.template.description |
| Quantity    | Quantity         | mrp.bom.product\_qty         |



### Property Mapping settings

#### Description

Recommended settings for description

| Setting name          | Value                                                       |
| --------------------- | ----------------------------------------------------------- |
| Update Odoo on Submit | Checked                                                     |
| Rendering Type        | Free Text                                                   |
| Enabled               | Checked                                                     |
| Prefer Odoo Value     | <p>(your choice)</p><p>Recommended setting is unchecked</p> |
| Visible               | Checked                                                     |

Leave all other fields as default

#### Quantity

Recommended settings for Quantity&#x20;

| Setting name          | Value     |
| --------------------- | --------- |
| Update Odoo on Submit | Checked   |
| Rendering Type        | Free Text |
| Enabled               | Checked   |
| Prefer Odoo Value     | Unchecked |
| Is Read Only          | Checked   |
| Visible               | Checked   |

Leave all other fields as default

### Property mapping rules

To format the data coming from your[ Primary Source](../../../fundamentals/data-sources.md) and [Secondary Source](../../../fundamentals/data-sources.md) (Odoo), we use import rules to massage the data.

Add the following rules, then click the save button for each rule.

#### Description

{% hint style="info" %}
Description (mapped to `product.template.description`) in Odoo is an HTML field. So it comes with some HTML tags. It can be desirable to remove these tags, so the following import rule accounts for this by removing \<p>\</p> paragraph tags
{% endhint %}

| Rule                  | Value                                                      | Process for |
| --------------------- | ---------------------------------------------------------- | ----------- |
| Replace all instances | <p>Match value &#x3C;p><br>Replace with (leave empty)</p>  | Odoo        |
| Replace all instances | <p>Match value &#x3C;/p><br>Replace with (leave empty)</p> | Odoo        |

#### Quantity

{% hint style="info" %}
Quantity (mapped to `mrp.bom.product_qty`) in Odoo is an numeric field. We want to format it so that it displays as the same number of decimal digits as those coming from our Primary Data Source. This is an optional rule mapping.
{% endhint %}



| Rule                     | Value                   | Process for             |
| ------------------------ | ----------------------- | ----------------------- |
| Format as decimal number | Number of decimals: `0` | Odoo + Primary          |
| Text is not empty        | block                   | (Prevents empty values) |



You are now ready to submit a new [Bill of Materials](../../../fundamentals/bom-comparison.md) and view it in SharpSync.

Below are some property mappings that can be imported when working with Odoo as a Secondary Source (right click > Save link as...)

<table><thead><tr><th width="139">CAD Source</th><th width="128">Odoo version</th><th>Comment</th></tr></thead><tbody><tr><td>Onshape</td><td>Odoo v17</td><td>download <a href="https://2811874215-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FcO2KdHJXVWdQ1ou1L85s%2Fuploads%2Fda188S8fq2t3Aeq1qoz9%2Fodoo-17-default-mapping-set.json?alt=media&#x26;token=efa400ee-33f4-4162-a0af-b1e250344e07">odoo-17-default-mapping-set.json</a></td></tr></tbody></table>



