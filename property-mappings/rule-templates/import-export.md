---
icon: sparkles
---

# Import / Export

### Import Rules

Import rules are run when the data is imported from the source. The rule will change the incoming value from the data source. For example, if you have a value from a CSV file that is being imported as 0, you can transform the value using the `Text Manipulation` rule to change from `0` => `0.0`

_Example_ You have a value that is received from the data source as a JSON object, say

```json
 {
   "id": 42,
   "refName" : "Material Name"
 }
```

You can use the

> `Select from Json`

rule to select the key called `refName` from this JSON object. The value displayed onscreen will be `Material Name`

_Example_

```json
[
{
   "id": 41,
   "refName" : "Material Name 1"
},
{
   "id": 42,
   "refName" : "Material Name 2"
 }
]
```

You can use the

> `Select from Json`

rule to select the key called `[1].refName` from this JSON object. The value displayed onscreen will be `Material Name 2`. The selector `[1].refName` uses a text string to select the value from the array of values. In this case select the 2nd element (indexes start at 0, so element 1 is the 2nd element in the array of 2 elements), then select the `refName` key on the element. Nested properties are supported.

### Export Rules

Export rules are run when the data is exported from SharpSync when using the `Submit BOM` button. The rule will change the value sent to the data source. For example, if you have value from a source, say Onshape, that was imported as `0`, the displayed onscreen as `0.0`, you can transform the value using the `Text Manipulation` rule to change from `0.0` => `0` so that the value may be accepted by Onshape.

### Rule explanations

Below is a comprehensive list of Import/Export Property Mapping Rules. Click a rule to explore it in depth.&#x20;

| Rule name                                                         | Type            | Description                                                                                               |
| ----------------------------------------------------------------- | --------------- | --------------------------------------------------------------------------------------------------------- |
| [Append Text](import-export/append-text.md)                       | Import / Export | Adds the specified text to the end of the cell value                                                      |
| [Calculate number](import-export/calculate-number.md)             | Import / Export | Uses the cell value and performs a calculation. The result of the calculation replaces the cell value     |
| [Export manipulation](import-export/export-manipulation.md)       | Export only     | Runs the specified javascript expression when data is exported. Has the ability to remove rowData values  |
| [Format as decimal](import-export/format-as-decimal-number.md)    | Import / Export | Converts the cell value to a number and adds the specified number of decimals. This does round the number |
| [Number between](import-export/number-between.md)                 |                 |                                                                                                           |
| [Prepend text](import-export/prepend-text.md)                     | Import / Export | Adds the specified text to the beginning of the cell value                                                |
| [Remove property](import-export/remove-property.md)               |                 | Removes the specified property when exporting the data                                                    |
| [Replace all instances](import-export/replace-all-instances.md)   | Import / Export | Replaces all instances of the specified text with the new value                                           |
| [Replace first instance](import-export/replace-first-instance.md) | Import / Export | Replaces the first instance of the specified text with the new value                                      |
| [Round to nearest X](import-export/round-to-nearest-x.md)         | Import / Export | Rounds the number to up or down the nearest specified digit                                               |
| [Select from Json](import-export/select-from-json.md)             | Import / Export | Converts the cell value from text to a JSON object and returns the value given by the specified key       |
| [Set cell value](import-export/set-cell-value.md)                 | Import / Export | Sets the cell value to the specified text. Existing text is replaced                                      |
| [Set empty cells](import-export/set-empty-cells.md)               | Import / Export | Set an empty (any cell that has only whitespace or no value) cell value to the specified text             |
| [Text contains](import-export/text-contains.md)                   | Import/Export   | The cell value must contain the specified text.                                                           |
| [Text ends with](import-export/text-ends-with.md)                 | Import/Export   | The cell value must end with the specified string.                                                        |
| [Text evaluation](import-export/text-evaluation.md)               |                 |                                                                                                           |
| [Text is a number](import-export/text-is-a-number.md)             | Import/Export   | The cell value must be a number.                                                                          |
| [Text is exactly](import-export/text-is-exactly.md)               | Import/Export   | The cell value must be an exact match with the specified text.                                            |
| [Text is in list](import-export/text-is-in-list.md)               | Import/Export   | The cell value must match a value in a string list.                                                       |
| [Text is not a number](import-export/text-is-not-a-number.md)     | Import/Export   | The cell value must not be a number.                                                                      |
| [Text is not empty](import-export/text-is-not-empty.md)           | Import/Export   | The cell value must not be empty.                                                                         |
| [Text is not in list](import-export/text-is-not-in-list.md)       | Import/Export   | The cell value must not match a value in a string list.                                                   |
| [Text length between](import-export/text-length-between.md)       |                 |                                                                                                           |
| [Text length is exactly](import-export/text-length-is-exactly.md) | Import/Export   | The number of characters in the cell value must be exactly the length specified.                          |
| [Text manipulation](import-export/text-manipulation.md)           | Import / Export | Manipulates (and returns the result of) the cell value using the given the javascript expression          |
| [Text maximum length](import-export/text-maximum-length.md)       |                 |                                                                                                           |
| [Text minimum length](import-export/text-minimum-length.md)       |                 |                                                                                                           |
| [Text not contains](import-export/text-not-contains.md)           | Import/Export   | The cell value must not contain the specified string.                                                     |
| [Text not ends with](import-export/text-not-ends-with.md)         | Import/Export   | The cell value must not end with the specified string.                                                    |
| [Text not starts with](import-export/text-not-starts-with.md)     | Import/Export   | The cell value must not start with the specified string.                                                  |
| [Text starts with](import-export/text-starts-with.md)             | Import/Export   | The cell value must start with the specified string.                                                      |
