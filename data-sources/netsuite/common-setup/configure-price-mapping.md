# Configure price mapping

### Price Property Mapping Settings

| Setting                   | Value                                                                                       |
| ------------------------- | ------------------------------------------------------------------------------------------- |
| Primary accessor          | <p>(unmapped) </p><p>or mapped to a Primary Source accessor if you have one</p>             |
| Secondary accessor        | <p>One of: </p><ul><li>assemblyitem.price</li><li>inventoryitem.price</li><li>etc</li></ul> |
| Update Primary on Submit  | unchecked                                                                                   |
| Update NetSuite on Submit | checked                                                                                     |
| Object Value Selector     |                                                                                             |
| List Name                 |                                                                                             |
| List Value Selector       |                                                                                             |
| Rendering Type            | `Free Text`                                                                                 |
| Prefer NetSuite value     | checked (if existing NetSuite values are the master values)                                 |

### Price Property Mapping Rules

Given this input, create the following rules:

* `Text Manipulation` import rule for NetSuite (check the checkbox for `Process for NetSuite`) with the following text:

{% code overflow="wrap" %}
```javascript
if (s && "items" in JSON.parse(s) && JSON.parse(s).items.length > 0) {   return JSON.parse(s).items[0].price; } return "";
```
{% endcode %}

