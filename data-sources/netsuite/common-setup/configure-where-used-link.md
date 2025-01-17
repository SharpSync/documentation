# Configure Where Used Link

### Setup a 'where used' link

In NetSuite it is often useful to know which assemblies or BOM revisions are using a specific component. We'll make use of the `rowData` object + `secondarySourceComponentId` (The id of the item in NetSuite) to generate the link.

Create a new import rule

#### New Rule

Rule: `Text manipulation` (Import Rule)

Value: `Javascript`

```javascript
return 'https://[companyId].app.netsuite.com/core/pages/itemchildrecords.nl?id=' + rowData.secondarySourceComponentId + '&t=InvtItem%05ProjectCostCategory&rectype=-10';
```

Property mapping render type: Url Enable: Prefer NetSuite value (checked)

#### Result

&#x20;A new link is displayed in the [BOM comparison ](../../../fundamentals/bom-comparison.md)page which you can click in the BOM. This link will open a new tab showing all the inventory records making use of this component / assembly.
