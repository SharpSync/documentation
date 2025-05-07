---
icon: diagram-subtask
---

# Configure quantity mapping

Exactly one property mapping _must_ be set as a quantity mapping in order for SharpSync to know which mapping value to use when updating the BOM structure in NetSuite.

### Quantity Property Mapping Settings

<table><thead><tr><th width="279">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Primary accessor</td><td>Your Primary Source bom quantity accessor</td></tr><tr><td>Secondary accessor</td><td><p>bom.quantity (for simple BOMs)</p><p>bomrevision.bomQuantity (for advanced BOMs)</p></td></tr><tr><td>Update Primary on Submit</td><td>unchecked</td></tr><tr><td>Update NetSuite on Submit</td><td>checked</td></tr><tr><td>Rendering Type</td><td>Free Text</td></tr><tr><td>Is Quantity Property</td><td>checked</td></tr></tbody></table>
