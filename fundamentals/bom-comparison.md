---
description: The view where all data is displayed overlayed on top of one another on screen
icon: table-list
---

# BOM Comparison

The BOM comparison screen is where you will ultimately spend most of your time

### BOM row updates

BOM row updates can occur in the primary or secondary Data Source and is influenced by your settings for:

* Property Mappings
* Rules

BOM row updates are also influenced by whether the `PROCESS` checkbox is checked or unchecked. You can use the context menu of any given row's `PROCESS` cell to bulk check/uncheck the `PROCESS` checkbox of related rows and children rows.

#### Primary Source updates

When updating BOM rows the following table illustrates how updates occur at the primary source.

A primary source is typically a CAD or PDM or PLM system. For primary sources, quantity values are typically not updated.

Using the default color scheme for BOM comparison, you can expect the following results:

<table data-full-width="true"><thead><tr><th align="center">Process Row</th><th>Row Background</th><th align="center">Item Creation</th><th>Bom Structure</th><th align="center">Quantity</th><th align="center">Item Properties</th><th align="center">Routings</th><th align="center">Derivatives</th></tr></thead><tbody><tr><td align="center">☑️</td><td>⬜ White</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Updated</td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td align="center">☑️</td><td>🟩 Green</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Updated</td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td align="center">☑️</td><td>🟨 Yellow</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Updated</td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td align="center">☑️</td><td>🟥 Red</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Ignored</td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td align="center"></td><td>⬜ White</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Ignored</td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td align="center"></td><td>🟩 Green</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Ignored</td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td align="center"></td><td>🟨 Yellow</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Ignored</td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td align="center"></td><td>🟥 Red</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Ignored</td><td align="center">N/A</td><td align="center">N/A</td></tr></tbody></table>

#### Secondary Source updates

When updating BOM rows the following table illustrates how updates occur at the secondary source.

A secondary source is typically an ERP or PLM system. For secondary sources, quantity values are typically updated to reflect the quantities of the primary source.

Using the default color scheme for BOM comparison, you can expect the following results:

<table data-full-width="true"><thead><tr><th width="162" align="center">Process Row</th><th>Row Background</th><th align="center">Item Creation</th><th>Bom Structure</th><th align="center">Quantity</th><th align="center">Item Properties</th><th align="center">Routings</th><th align="center">Derivatives</th></tr></thead><tbody><tr><td align="center">☑️</td><td>⬜ White</td><td align="center"></td><td>Unchanged</td><td align="center">Updated</td><td align="center">Updated</td><td align="center">Updated</td><td align="center">Processed</td></tr><tr><td align="center">☑️</td><td>🟩 Green</td><td align="center">Created</td><td>Linked To Parent</td><td align="center">Updated</td><td align="center">Updated</td><td align="center">Updated</td><td align="center">Processed</td></tr><tr><td align="center">☑️</td><td>🟨 Yellow</td><td align="center"></td><td>Linked To Parent</td><td align="center">Updated</td><td align="center">Updated</td><td align="center">Updated</td><td align="center">Processed</td></tr><tr><td align="center">☑️</td><td>🟥 Red</td><td align="center"></td><td>Unlinked From Parent + Children Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td></tr><tr><td align="center"></td><td>⬜ White</td><td align="center"></td><td>Unchanged</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td></tr><tr><td align="center"></td><td>🟩 Green</td><td align="center">Not Created</td><td>Not Linked To Parent</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td></tr><tr><td align="center"></td><td>🟨 Yellow</td><td align="center"></td><td>Not Linked To Parent</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td></tr><tr><td align="center"></td><td>🟥 Red</td><td align="center"></td><td>Link To Parent Kept + Children Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td></tr></tbody></table>
