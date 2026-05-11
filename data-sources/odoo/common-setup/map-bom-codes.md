---
icon: diagram-subtask
---

# Map Product Template Company Id

### Property Mapping Settings

In Odoo, if your instance is set for multi companies and you want to sync Products and BOMs, you can specify a company id to associate with Products and BOMs creation and update.

From SharpSync, start by adding a [Property Mapping](../../../fundamentals/property-mappings/) as follows:

<table><thead><tr><th width="284">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Primary Accessor</td><td><code>(Unmapped)</code> or any mapping from which you can derive a company id</td></tr><tr><td>Secondary Accessor</td><td><code>product.template.company_id</code></td></tr><tr><td>Prefer Odoo Value</td><td>checked (if you are sure all existing Odoo Pproducts and BOMs are associated with the correct company id)</td></tr><tr><td>Update Odoo on submit</td><td>checked</td></tr><tr><td>Rendering Type</td><td>Free Text</td></tr></tbody></table>

### Property Mapping Rules

In addition to this you'll want to create the following rules:

* A `Set cell value` rule for the Primary Source (CAD source) to assign the desired company id on data import (example: `2` ). Unless you can derive the company id from your business logic and the primary accessor that you have associated with this mapping.
* A `Text not empty` rule. This will prevent errors when submitting the BOM&#x20;
