---
description: >-
  Converts the cell value from text/string to a JSON object and returns the
  value given by the specified JSON selector.
---

# Select from JSON

Type: Import/Export

Description: Converts the cell value to a JSON object and selects the value specified by the selector.

Supports nested key/values and arrays. You can use `key.value[index].key` to retrieve value for a given key.

<details>

<summary>Example</summary>

* Cell value

{&#x20;

&#x20; "parts" : \[

&#x20;    { "name" : "coil", "quantity" : 2 },&#x20;

&#x20;    {"name" : "fan", "quantity" : 3 }

&#x20; ]

}

* Rule values:
  * Select value: parts\[1].name
* Result: fan

</details>
