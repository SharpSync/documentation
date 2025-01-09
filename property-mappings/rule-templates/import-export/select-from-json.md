---
description: >-
  Converts the cell value from text/string to a JSON object and returns the
  value given by the specified JSON selector.
---

# Select from JSON

Type: Import/Export

Supports nested key/values and arrays. You can use key.value\[index].key to retrieve value for a given key.

<details>

<summary>Example</summary>

* Cell value: {"parts":\[{"name":"coil","quantity":2},{"name":"fan","quantity":3}]}
* Rule values:
  * Select value: parts\[1].name
* Result: fan

</details>
