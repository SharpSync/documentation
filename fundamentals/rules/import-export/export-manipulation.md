---
description: Modifies the outgoing data before it is sent to the secondary source.
---

# Export manipulation

Type: Export only

Description: Modifies the `secondaryExportData`object by removing, adding or altering the data.

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
