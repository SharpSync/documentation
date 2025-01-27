# Configure Item Type mapping

The NetSuite REST API using specific paths for mapping item types such as (but not limited to):

* assemblyitem
* inventoryitem
* noninventoryitem
* noninventoryresaleitem
* noninventorypurchaseitem
* bom
* bomrevision
* etc

Exactly one property mapping for NetSuite _must_ be set as an item type mapping in order for SharpSync to know which item type value to use when creating new items in NetSuite. You can leave both the Onshape and the Netsuite mappings as _(Unmapped)_, but the mapping must exist.

### Item Type Property Mapping Settings

| Setting                   | Value                                                                           |
| ------------------------- | ------------------------------------------------------------------------------- |
| Primary accessor          | <p>(Unmapped) </p><p>or mapped to a Primary Source accessor if you have one</p> |
| Secondary accessor        | (Unmapped)                                                                      |
| Update Primary on Submit  | unchecked                                                                       |
| Update NetSuite on Submit | unchecked                                                                       |
| Rendering Type            | Select List                                                                     |
| List Items                | (see below)                                                                     |
| Prefer NetSuite value     | checked                                                                         |
| Is Item Type Property     | checked                                                                         |

When an item is created in NetSuite, it is typically one of the item types mentioned above. As such, add this as a list of options to pick from your `Select List.` You can include any item type in this list that is supported by your installation of NetSuite. A typical `List Items` list is as follows:

```
assemblyitem|inventoryitem|noninventorypurchaseitem|noninventorysaleitem|noninventoryresaleitem
```

### Item Type Property Mapping Rules

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
