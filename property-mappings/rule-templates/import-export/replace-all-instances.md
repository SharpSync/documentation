---
description: Replace all instances of the specified text with the new value.
---

# Replace all instances

Type:  Import/Export Rule&#x20;

<details>

<summary>Example: Replace all delimiters</summary>

* Cell value: 1, 234, 567.89
* Rule values:
  * Replace text: ,    (a single comma)
  * With text:     (an empty space)
* Result: 1 234 567.00

</details>

<details>

<summary>Example: Replace all &#x3C;p> and &#x3C;/p> tags</summary>

Note: This case cannot be achieved with only one iteration. If the specified text cannot be modified with a single iteration of a rule, additional rules should be created to modify the text.

* Cell value: \<p>value\</p>
* Rule values:
  * Replace text: \<p>  &#x20;
  * With text:     (an empty space)
* Result: value\</p>

After adding this rule, add a second rule to replace all \</p> tags

</details>



