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

| Rule name                                           | Type            | Description                                                                                               |
| --------------------------------------------------- | --------------- | --------------------------------------------------------------------------------------------------------- |
| Append Text                                         | Import / Export | Adds the specified text to the end of the cell value                                                      |
| [Calculate number](calculate-number.md)             | Import / Export | Uses the cell value and performs a calculation. The result of the calculation replaces the cell value     |
| [Export manipulatio](export-manipulation.md)n       | Export only     | Runs the specified javascript expression when data is exported. Has the ability to remove rowData values  |
| [Format as decimal](format-as-decimal-number.md)    | Import / Export | Converts the cell value to a number and adds the specified number of decimals. This does round the number |
| [Prepend text](prepend-text.md)                     | Import / Export | Adds the specified text to the beginning of the cell value                                                |
| [Replace all instances](replace-all-instances.md)   | Import / Export | Replaces all instances of the specified text with the new value                                           |
| [Replace first instance](replace-first-instance.md) | Import / Export | Replaces the first instance of the specified text with the new value                                      |
| [Round to neares](round-to-nearest-x.md)t X         | Import / Export | Rounds the number to up or down the nearest specified digit                                               |
| [Select from Json](select-from-json.md)             | Import / Export | Converts the cell value from text to a JSON object and returns the value given by the specified key       |
| Set cell value                                      | Import / Export | Sets the cell value to the specified text. Existing text is replaced                                      |
| Set empty cells                                     | Import / Export | Set an empty (any cell that has only whitespace or no value) cell value to the specified text             |
| Text manipulation                                   | Import / Export | Manipulates (and returns the result of) the cell value using the given the javascript expression          |
| Remove property                                     | Import / Export | Removes the specified property when exporting the data                                                    |
