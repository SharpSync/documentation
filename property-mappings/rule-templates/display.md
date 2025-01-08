---
icon: sparkles
description: '[docs in progress]'
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

Go to [Rule Setup ](display.md#setting-up-a-rule)for more information about setting up rules.

### List of Display Rules

| Rule name              | Type    | Description                                                                          |
| ---------------------- | ------- | ------------------------------------------------------------------------------------ |
| Number between         | Display | The number in the cell value must be between (inclusive) the numbers specified       |
| Text contains          | Display | The cell value must contain the specified text                                       |
| Text ends with         | Display | The cell value must end with the specified text                                      |
| Text evaluation        | Display | Evaluates the cell value given the JavaScript expression                             |
| Text is in list        | Display | The cell value must be in the list of specified items (Comma separated)              |
| Text is not in list    | Display | The cell value must not be in the list of specified items (Comma separated)          |
| Text is a number       | Display | The cell value must not be numeric                                                   |
| Text is exactly        | Display | The cell value must exactly match the specified text                                 |
| Text is not a number   | Display | The cell value must not be numeric                                                   |
| Text is not empty      | Display | The cell value must contain text (1 or more characters)                              |
| Text length between    | Display | The number of characters in the cell value must be between the lower and upper limit |
| Text length is exactly | Display | The number of characters in the cell value must be exactly the length specified      |
| Text maximum length    | Display | Limits the length of the cell value text to a maximum number of characters           |
| Text minimum length    | Display | The number of characters in the cell value must be greater than the specified number |
| Text not contains      | Display | The cell value must not contain the specified text                                   |
| Text not ends with     | Display | The cell value text must not end with the specified text                             |
| Text not starts with   | Display | The cell value must not start with the specified text                                |
| Text starts with       | Display | The cell value text must start with the specified text                               |
