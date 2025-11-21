---
icon: js
---

# Checking for New Rows

Include the following code at the start of your rule to check for new rows. Only works for:

* Export Manipulation
* Text Manipulation (Export)
* Comparison Rules&#x20;
* Display rules

Does not work for&#x20;

* Text Manipulation (Import) because you only have access to a single source

&#x20;

```javascript
const isNewRow = rowData.isMissingInSecondaryDatasource == true && rowData.isFoundInSecondaryDatasource == false;
```
