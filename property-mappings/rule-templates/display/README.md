---
icon: sparkles
---

# Display

### Display Rules

Display rules are rules that trigger either warnings or errors _after_ the data has been imported. This plays out as:

* Data is imported
* Import rules are applied to transform data
* Data is displayed onscreen
* Rules are evaluated
* Warnings or errors are displayed based on the rule conditions

A Display Rule can be set to either `pass` or `block`.

* A value of `pass` will show an orange border if it fails. The user is still able to submit the BOM
* A value of `block` will show a red border if it fails. The user is not able to submit the BOM
* (Colors are configurable)

### Rule explanations

Below is a comprehensive list of Display Property Mapping Rules. Click a rule to explore it in depth.&#x20;

Go to [Rule Setup ](./#setting-up-a-rule)for more information about setting up rules.

### List of Display Rules

| Rule name                                           | Type    | Description                                                                          |
| --------------------------------------------------- | ------- | ------------------------------------------------------------------------------------ |
| [Number is between](number-between.md)              | Display | The number in the cell value must be between (inclusive) the numbers specified       |
| [Text contains](text-contains.md)                   | Display | The cell value must contain the specified text                                       |
| [Text ends with](text-ends-with.md)                 | Display | The cell value must end with the specified text                                      |
| [Text evaluation](text-evaluation.md)               | Display | Evaluates the cell value given the JavaScript expression                             |
| [Text is a number](text-is-a-number.md)             | Display | The cell value must not be numeric                                                   |
| [Text is exactly](text-is-exactly.md)               | Display | The cell value must exactly match the specified text                                 |
| [Text is in list](text-is-in-list.md)               | Display | The cell value must be in the list of specified items (Comma separated)              |
| [Text is not a number](text-is-not-a-number.md)     | Display | The cell value must not be numeric                                                   |
| [Text is not empty](text-is-not-empty.md)           | Display | The cell value must contain text (1 or more characters)                              |
| [Text is not in list](text-is-not-in-list.md)       | Display | The cell value must not be in the list of specified items (Comma separated)          |
| [Text length between](text-length-between.md)       | Display | The number of characters in the cell value must be between the lower and upper limit |
| [Text length is exactly](text-length-is-exactly.md) | Display | The number of characters in the cell value must be exactly the length specified      |
| [Text maximum length](text-maximum-length.md)       | Display | Limits the length of the cell value text to a maximum number of characters           |
| [Text minimum length](text-minimum-length.md)       | Display | The number of characters in the cell value must be greater than the specified number |
| [Text not contains](text-not-contains.md)           | Display | The cell value must not contain the specified text                                   |
| [Text not ends with](text-not-ends-with.md)         | Display | The cell value text must not end with the specified text                             |
| [Text not starts with](text-not-starts-with.md)     | Display | The cell value must not start with the specified text                                |
| [Text starts with](text-starts-with.md)             | Display | The cell value text must start with the specified text                               |
