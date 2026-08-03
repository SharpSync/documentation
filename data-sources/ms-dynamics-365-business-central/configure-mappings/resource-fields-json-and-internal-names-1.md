---
icon: code
---

# BOM Component Fields Json & BC Field Names

Each SharpSync BOM Component accessor uses a Json unique identifier; the table below shows the Business Central field it corresponds to. The same pairing is returned by the `bomComponentFields` catalog endpoint as `fieldJsonName` / `fieldName`.

| BOM Component Fields Json Unique Identifiers | BOM Component BC Field Names |
| -------------------------------------------- | ---------------------------- |
| parentItemNumber                             | Parent Item No.              |
| lineNumber                                   | Line No.                     |
| type                                         | Type                         |
| itemNumber                                   | No.                          |
| assemblyBom                                  | Assembly BOM                 |
| description                                  | Description                  |
| unitOfMeasureCode                            | Unit of Measure Code         |
| quantityPerParent                            | Quantity per                 |
| position                                     | Position                     |
| position2                                    | Position 2                   |
| position3                                    | Position 3                   |
| machineNumber                                | Machine No.                  |
| leadTimeOffset                               | Lead-Time Offset             |
| bomDescription                               | BOM Description              |
| resourceUsageType                            | Resource Usage Type          |
| variantCode                                  | Variant Code                 |
| installedInLineNumber                        | Installed in Line No.        |
| installedInItemNumber                        | Installed in Item No.        |

