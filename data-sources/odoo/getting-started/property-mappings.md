---
description: The quickest way to get started with Odoo is to add some property mappings
---

# Property Mappings

### Add Property Mappings

After [authenticating ](authentication-+-configuration.md)with Odoo, add the following [Property Mappings](../../../fundamentals/property-mappings.md)

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
Description (mapped to `product.template.description`) in Odoo is an HTML field. So it comes with some HTML tags. It can be desireable to remove these, so the following rule accounts for this by removing \<p>\</p> paragraph tags
{% endhint %}

| Rule                  | Value                                                      | Process for |
| --------------------- | ---------------------------------------------------------- | ----------- |
| Replace all instances | <p>Match value &#x3C;p><br>Replace with (leave empty)</p>  | Odoo        |
| Replace all instances | <p>Match value &#x3C;/p><br>Replace with (leave empty)</p> | Odoo        |

#### Quantity

{% hint style="info" %}
Quantity (mapped to `mrp.bom.product_qty`) in Odoo is an numeric field. We want to format it so that it displays as the same number of decimal digits as those coming from our primary source
{% endhint %}



| Rule                     | Value                   | Process for             |
| ------------------------ | ----------------------- | ----------------------- |
| Format as decimal number | Number of decimals: `0` | Odoo + Primary          |
| Text is not empty        | block                   | (Prevents empty values) |



You are now ready to submit a new [Bill of Materials](../../../fundamentals/bom-comparison.md) and view it in SharpSync.

