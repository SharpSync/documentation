---
icon: diagram-subtask
---

# Configure restlet.createRecord mapping

When using Advanced BOMs , and creating item related records that are inaccessible from the standard NetSuite REST api is required, the `restlet.createRecord` field can be mapped (but make sure you are using the [SharpSync RESTLet script version 2.0.0](../restlet-script-setup/sharpsync-restlet-script.md) or later in your NetSuite instance). To create different item related records,  add a new Property Mapping for each record type that you need to create.

Below is an sample setup to create the item related record `itemlocationconfiguration`

### Item Location Configuration Property Mapping Settings

<table><thead><tr><th width="279">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Primary accessor</td><td><p>(Unmapped) </p><p>or mapped to a Primary Source accessor if you need one</p></td></tr><tr><td>Secondary accessor</td><td><code>restlet.createRecord</code></td></tr><tr><td>Update Primary on Submit</td><td>unchecked</td></tr><tr><td>Update NetSuite on Submit</td><td>checked</td></tr><tr><td>Rendering Type</td><td>Free Text</td></tr></tbody></table>

### Item Location Configuration Property Mapping Rules

Given this input, create the following rule:

* A `Text Manipulation` export rule for NetSuite (check the checkbox for `Process for NetSuite`) with the following sample text (feel free to update the rule script to suit your business rules):

```javascript
return {
  itemlocationconfiguration: {
    subsidiary: "2",
    location: "4",
    atpleadtime: 90,
    minimumrescheduledays: 1,
    reschedulehorizon: 90,
    rescheduleindays: 7,
    rescheduleoutdays: 30,
    supplylotsizingmethod: "FIXED_LOT_SIZE",
    fixedlotsize: 1,
    supplytype: rowData.isAssemblyRow === true ? "BUILD" : "PURCHASE",
    iswip: true,
  },
};
```

The main key  `itemlocationconfiguration` of the returned object represent the NetSuite record type to be created, while the other key-value pairs (`subsidiary` , `location` , etc... ) represent the new record's field values to be set upon creation.
