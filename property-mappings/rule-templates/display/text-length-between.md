---
description: >-
  The number of characters in the cell value must be between the lower and upper
  limit (both inclusive).
---

# Text length between

Type: Display

<details>

<summary>Example: </summary>

* Cell value: Description
* Rule values:
  * Min length: 1
  * Max length: 11
* Result: ![](<../../../.gitbook/assets/image (22).png>)



Observations:&#x20;

* 'Description' is 11 characters long and falls within the ranges.
* No orange border, indicating valid data.

</details>

<details>

<summary>Example: </summary>

* Cell value: D
* Rule values:
  * Min length: 1
  * Max length: 11
* Result: ![](<../../../.gitbook/assets/image (23).png>)



Observations:&#x20;

* 'D' is 1 character long and falls within the ranges.
* No orange border, indicating valid data.

</details>

<details>

<summary>Example: </summary>

* Cell value: Descriptions
* Rule values:
  * Min length: 1
  * Max length: 11
* Result: ![](<../../../.gitbook/assets/image (25).png>)



Observations:&#x20;

* 'Descriptions' is 12 characters long and falls outside the ranges.
* Orange border, indicating invalid data.

</details>
