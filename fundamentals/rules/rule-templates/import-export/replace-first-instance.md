---
description: Replace the first instance of the specified text with the new value
---

# Replace first instance

Type: Import/Export

Description: Searches the cell value and replaces the first instance of the found string with a new value.

Scope: The rule only applies to the first instance. All following instances will be ignored.&#x20;

Also see [Replace All Instances](replace-all-instances.md)

<details>

<summary>Example</summary>

* Cell value: 1, 234, 567.89
* Rule values:
  * Replace text: ,    (a single comma)
  * With text:     (an empty space)
* Result: 1 234, 567.00

</details>
