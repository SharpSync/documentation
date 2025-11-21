---
description: Removes the specified property when exporting the data.
---

# Remove property

Type: Export only

Description: Removes the property from the `secondaryExportData` object (see [rowData](../advanced-scripting/)).

Implementation: Can only have one Remove Property rule per BOM

<details>

<summary>Example</summary>

* You have created Property Mapping that is unique to certain components (for example, you created a Property Mapping with an accessor of Calculated Volume. You are using it to compare values onscreen, but you do not want to send this property to your Data Source). \
  \
  In this case, you would create a rule to remove the property before exporting the data to your Data Sources.

- Lastly, remember to check for which Data Sources this rule should apply. In the example below the rule will only apply to the Secondary Data Source, in other words, the property not be removed for the Primary Data Source, but it will be removed for the Secondary Data Source.

<img src="../../../.gitbook/assets/image (21).png" alt="" data-size="original">





</details>
