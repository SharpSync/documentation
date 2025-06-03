---
icon: diagram-subtask
---

# Map BOM Codes

In Odoo the BOM code is a unique reference to the version of the BOM at the time of submitting it via Onshape.

Multiple BOMs are supported per product template / product variant. To use this feature, different variations of a BOM may be completed, so we use a unique reference for each of these.

To manage this, we set a BOM code to that of the component + the revision

To update this from SharpSync, start by adding a [Property Mapping ](../../../fundamentals/property-mappings/)for&#x20;

> mrp.bom.code

<table><thead><tr><th width="284">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Primary Accessor</td><td>(Unmapped)</td></tr><tr><td>Secondary Accessor</td><td><code>mrp.bom.code</code></td></tr><tr><td>Prefer Odoo Value</td><td>checked</td></tr><tr><td>Update Odoo on submit</td><td>checked</td></tr><tr><td>Rendering Type</td><td>Free Text</td></tr></tbody></table>

When displaying values in Odoo, the value from Odoo may be edited with rules in SharpSync.

When the BOM is created or updated, the new code will be written to the BOM's code field.
