---
description: .
---

# Text is in list

Type: Display

Description: Entries are separated by a pipe symbol '|'. If the cell value matches an entry, the rule succeeds.

<details>

<summary>Example: Fail</summary>

* Cell value: Desc
* Rule value:&#x20;

`abc|def|ghi`

* Result: Rule Fails - Cell value "Desc" does not match any list value

</details>

<details>

<summary>Example: Success</summary>

* Cell value: Stainless Steel
* Rule value:&#x20;

`Stainless Steel|Bronze|Metal`&#x20;

* Result: Rule Succeeds - Cell value "Desc" does not match any list value

</details>

