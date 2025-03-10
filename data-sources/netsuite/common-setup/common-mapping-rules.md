# Common Mapping Rules

Since most of the NetSuite fields are dependent on the NetSuite `itemType` field, a useful `display`rule to ensure that the value for any mapping for the corresponding item type is not empty is the following:

```javascript
const itemTypeValue =
  "itemType" in rowData.modifications
    ? rowData.modifications.itemType
    : rowData.cells.itemType;
if (itemTypeValue === pm.secondaryAccessor.split(".")[0] && s === "") {
  return { message: "Value must not be empty" };
}
```



