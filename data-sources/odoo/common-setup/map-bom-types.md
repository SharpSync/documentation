---
icon: diagram-subtask
---

# Map BOM Types

In Odoo the default values for BOM types can be in the range

* Manufacture this Product
* Kit
* Subcontracting

\[These can depend on your installation, but it is a common theme]

To update this from SharpSync, start by adding a [Property Mapping ](../../../fundamentals/property-mappings/)for&#x20;

> mrp.bom.type

<table><thead><tr><th width="284">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Primary Accessor</td><td>(Unmapped)</td></tr><tr><td>Secondary Accessor</td><td><code>mrp.bom.type</code></td></tr><tr><td>Prefer Odoo Value</td><td>checked</td></tr><tr><td>Update Odoo on submit</td><td>checked</td></tr><tr><td>Rendering Type</td><td>Advanced List <sup>**see setup below</sup></td></tr></tbody></table>



The type of the Odoo property will be `array` . This is an indication that you can typically select from a list of values in Odoo. In my example the value of this array looks like this&#x20;

> bundle (Bundle)|normal (Manufacture this product)|phantom (Kit)

This means there are 3 internal values in Odoo:

| Value a user may select | User sees in Odoo        |
| ----------------------- | ------------------------ |
| bundle                  | Bundle                   |
| normal                  | Manufacture this product |
| phantom                 | Kit                      |

{% hint style="info" %}
To view these values in Odoo, navigate to&#x20;

Settings > Technical > Database > Fields Selection&#x20;

Search for `BOM Type`&#x20;
{% endhint %}

Let's setup an Advanced Selection list to present this to the user in SharpSync.

* Copy the values in the Array Enum Values field
* Using ChatGPT, Claude, Copilot or similar, convert it to a text json list using the prompt

> Convert this to a json list with string kv pairs using the value before parenthesis as 'name' and the value in the parenthesis as 'value'

* Copy the output

Back in SharpSync, make the following edits to the property mapping for Bom Type:

| Setting               | Value         |
| --------------------- | ------------- |
| Rendering Type        | Advanced List |
| List Display Selector | value         |
| List Value Selector   | name          |
| Prefer Odoo Value     | checked       |

For the list items, paste the value in your clipboard specified in the previous step. It should look like the following:

```json5
[
  { "name": "bundle",  "value": "Bundle" },
  { "name": "normal",  "value": "Manufacture this product" },
  { "name": "phantom", "value": "Kit" }
]
```

I'm now able to display + update the values from Odoo
