# Item Type mapping

The NetSuite REST API using specific paths for mapping item types such as (but not limited to):

* assemblyitem
* inventoryitem
* noninventoryitem
* noninventoryresaleitem
* noninventorypurchaseitem
* bom
* bomrevision
* etc

At least one property mapping for NetSuite _must_ be set as an item type mapping. You can leave the Netsuite mapping as _(Unmapped)_ and invisible, but the mapping must exist. Typically you'd want to set this as 'prefer NetSuite value' as well, but that's optional.

When an item is created in NetSuite, it is typically one of the items mentioned above. As such, add this as a list of options to pick from. You can include any items in this list that is supported by your installation of NetSuite.



In addition to this you'll want to create 2 rules:

1. A text manipulation rule for NetSuite / Primary CAD system that runs the following script (or similar based on your setup) on data import:

```javascript
if (rowData.isAssemblyRow) 
    return 'assemblyitem'; 

return 'noninventorypurchaseitem'; /* or whatever the default item type is */
```

2. A `Text not empty` rule. This will prevent errors when submitting the BOM

On a more technical note: These items are derived by calling the endpoint

> &#x20;/GET `{{netsuite-api}}/services/rest/record/v1/metadata-catalog` with an empty body

### &#x20;
