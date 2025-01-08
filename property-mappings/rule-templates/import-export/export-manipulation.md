---
description: Modifies the outgoing data before it is sent to the secondary source.
---

# Export manipulation

Type: Export only

It is important to note two things:

1. There can be only one of these rules as it modifies all outgoing data
2. The return variable must always be named`s`. `s` represents the `rowData.sourceExportData` to be modified. Failing to return it will result in corrupted data in the BOM.

e.g. given this Javascript for an isPhantom setting

```javascript
const isNewAssemblyRow = 
rowData.isAssemblyRow && 
rowData.isMissingInSecondaryDatasource == true && 
rowData.isFoundInSecondaryDatasource == false; 

if (!isNewAssemblyRow) 
{ delete s['phantomYN']; }  

if (rowData.isAssemblyRow === true)  
{ delete s['material']; }   

return s;

```

<details>

<summary>Example</summary>

* Cell value: 12.53 m
* Rule values:
  * Number of decimals: 4
  * Remove text: kg|Kg|g|mg|m|mm|each|L|ml|oz|fl
* Result: 12.5300

</details>
