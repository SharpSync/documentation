---
icon: octagon-exclamation
---

# Read-Only Fields

Some Business Central fields identify a record or are managed by SharpSync as part of the BOM structure. Mapping them is fine for **display**, but they must not be written on submit. Disable _Update MS Dynamics 365 On Submit_ on their property mappings.

{% hint style="warning" %}
_Update MS Dynamics 365 On Submit_ is **checked by default** when you create a property mapping. Untick it for every field listed on this page.
{% endhint %}

## Assembly BOM fields

It is recommended to disable the setting for the following fields to prevent any unexpected behaviour.

| Record        | Json Unique Identifier | BC Field Name    |
| ------------- | ---------------------- | ---------------- |
| Item          | `itemNumber`           | No.              |
| Resource      | `resourceNumber`       | No.              |
| BOM Component | `itemNumber`           | No.              |
| BOM Component | `lineNumber`           | Line No.         |
| BOM Component | `parentItemNumber`     | Parent Item No.  |
| BOM Component | `type`                 | Type             |

## Production BOM fields

For production BOMs the rule is **enforced**. A mapping to one of the fields below with _Update MS Dynamics 365 On Submit_ on produces an error on the row at sync time, and the field is not written. The rest of the row's mapped fields are still written, so the error is a reminder to fix the mapping rather than a failed sync. The message reads:

> _'status' is read-only to property mappings: SharpSync manages it as part of the BOM structure and did not write it. Untick 'Update Dynamics365' on that mapping to keep displaying the field without this error._

| Record                 | Json Unique Identifier | BC Field Name      | Why it is read-only                                                              |
| ---------------------- | ---------------------- | ------------------ | -------------------------------------------------------------------------------- |
| Production BOM Header  | `status`               | Status             | Certification is controlled by the data source configuration, not by a mapping.  |
| Production BOM Header  | `number`               | No.                | Identifies the BOM. Writing it would rename it.                                   |
| Production BOM Version | `status`               | Status             | Same as the header status.                                                        |
| Production BOM Version | `versionCode`          | Version Code       | Identifies the version. It is named by the version naming scheme.                 |
| Production BOM Version | `productionBomNumber`  | Production BOM No. | Identifies the BOM the version belongs to.                                        |
| Production BOM Line    | `type`                 | Type               | Set from the item type mapping when the line is created, and never changed after. |
| Production BOM Line    | `number`               | No.                | Identifies the component. Writing it would re-point the line.                     |
| Production BOM Line    | `lineNumber`           | Line No.           | Identifies the line.                                                              |
| Production BOM Line    | `productionBomNumber`  | Production BOM No. | Identifies the BOM the line belongs to.                                           |
| Production BOM Line    | `versionCode`          | Version Code       | Identifies the version the line belongs to.                                       |
| Production BOM Line    | `quantityPer`          | Quantity per       | Written from the quantity mapping, not from a field mapping.                      |

{% hint style="info" %}
`Status` and `Version Code` are worth mapping for **display**. They are the only way to see in the comparison grid whether the BOM you loaded is `Certified` or `Under Development`, and which version it is. See [Production BOMs](../production-boms.md).
{% endhint %}
