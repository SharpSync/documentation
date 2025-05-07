---
icon: diagram-subtask
---

# Configure Where Used Link mapping

In NetSuite it is often useful to know which assemblies or BOM revisions are using a specific component. We'll make use of the `rowData` object + `secondarySourceComponentId` (The id of the item in NetSuite) to generate the link.

This link is displayed in the [BOM comparison ](../../../fundamentals/bom-comparison.md)page which you can click in the BOM. This link will open a new tab showing all the inventory records making use of this component / assembly.

To setup Where Used, add a Property Mapping with accessor value `whereUsed`:

### Where Used Property Mapping Settings

| Setting                   | Value      |
| ------------------------- | ---------- |
| Primary accessor          | (Unmapped) |
| Secondary accessor        | (Unmapped) |
| Update Primary on Submit  | unchecked  |
| Update NetSuite on Submit | unchecked  |
| Rendering Type            | Url        |
| Prefer NetSuite value     | checked    |

### Where Used Property Mapping Rules

* A `Text Manipulation` import rule for NetSuite (check the checkbox for `Process for NetSuite`) with the following text:

```javascript
return 'https://[companyId].app.netsuite.com/core/pages/itemchildrecords.nl?id=' + rowData.secondarySourceComponentId + '&t=InvtItem%05ProjectCostCategory&rectype=-10';
```

