---
icon: diagram-subtask
---

# Configure Item Type Mapping

{% hint style="danger" %}
This document is a work in progress.

Check back here  frequently for updates.
{% endhint %}

The Dynamics 365 Business Central API supports the following default BOM component record types:

* `item`
* `resource`

Exactly one property mapping for _must_ be set as an item type mapping in order for SharpSync to know which item type value to use when creating new items in Dynamics 365 Business Central. You can leave both the primary and secondary accessors as _(Unmapped)_, but the mapping must exist.

### Item Type Property Mapping Settings

| Setting                   | Value                                                                           |
| ------------------------- | ------------------------------------------------------------------------------- |
| Accessor                  | `itemType`                                                                      |
| Primary accessor          | <p>(Unmapped) </p><p>or mapped to a Primary Source accessor if you have one</p> |
| Secondary accessor        | (Unmapped)                                                                      |
| Update Primary on Submit  | unchecked                                                                       |
| Update NetSuite on Submit | checked (IMPORTANT)                                                             |
| Rendering Type            | Select List                                                                     |
| List Items                | `item\|resource`                                                                |
| Is Item Type Property     | checked                                                                         |

When an item is created, it is typically one of the item types mentioned above. As such, add this as a list of options to pick from your `Select List`&#x20;

### Item Type Property Mapping Rules

In addition you'll want to add rules to match your business record creation logic

* At least a rule that sets the item type based on the values coming from your primary source.
* A `Text not empty` rule. This will prevent errors when submitting the BOM with no item type specified.
