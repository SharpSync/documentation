---
description: >-
  The number in the cell value must be between (inclusive) the numbers
  specified.
---

# Number between

Type: Display

<details>

<summary>Example</summary>

* Cell value: 0
* Rule values:
  * Min val: 1
  * Max val: 5
* Result: False, since value is less than min value.

</details>

<details>

<summary>Example</summary>

* Cell value: 1
* Rule values:
  * Min val: 1
  * Max val: 5
* Result: True, since value is between min and max value (inclusive)

</details>

<details>

<summary>Example</summary>

* Cell value: 5
* Rule values:
  * Min val: 1
  * Max val: 5
* Result: True, since value is between min and max value (inclusive)

</details>
