---
icon: js
---

# rowData parameter

The `rowData` parameter represents the entire row's data (all the cell values, modifications, differences, etc.).

The rowData parameter is read-only and cannot be modified with import, export or display rules. There are two specific rules which _does_ let you modify the rowData:

* Export Manipulation
* Comparison Rule

More on this later



The rowData object has some  properties that can be considered stable (will never change, be renamed or removed by developers):

<table><thead><tr><th width="253">Property Name</th><th>Description</th></tr></thead><tbody><tr><td><code>cells</code></td><td>The primary source cells as it arrives from the source</td></tr><tr><td><code>modifications</code></td><td>Any changes made to a row on screen of through import rules. The cell will have a green rectangle.</td></tr><tr><td><code>differences</code></td><td>Any values that are different to the primary source values. If Qty has a value of 5 in the primary, it is not the same as 5.0 from secondary. In the differences there will be a "Qty" : "5.0" value. The cell will have a blue rectangle.</td></tr><tr><td><code>primarySourceExportData</code></td><td>The data, after export rules have run, that will be sent to the Primary Data Source. You can see the exported value by hovering over a cell or opening the Cell Rules Panel</td></tr><tr><td><code>secondarySourceExportData</code></td><td>The data, after export rules have run, that will be sent to the Secondary Data Source. You can see the exported value by hovering over a cell or opening the Cell Rules Panel</td></tr></tbody></table>

#### Example&#x20;

To access the `rowData` 's differences parameter in a script, use&#x20;

```javascript
if ('material' in rowData.differences)
{
  // do something
}
```
