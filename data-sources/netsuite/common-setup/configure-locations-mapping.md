---
icon: diagram-subtask
---

# Configure locations mapping

`locations` is a `multi-value`  NetSuite field that accepts an array of complex objects

### Locations Property Mapping Settings

| Setting                   | Value                                                                                               |
| ------------------------- | --------------------------------------------------------------------------------------------------- |
| Primary accessor          | <p>(unmapped) </p><p>or mapped to a Primary Source accessor if you have one</p>                     |
| Secondary accessor        | <p>One of: </p><ul><li>assemblyitem.locations</li><li>inventoryitem.locations</li><li>etc</li></ul> |
| Update Primary on Submit  | unchecked                                                                                           |
| Update NetSuite on Submit | checked                                                                                             |
| Object Value Selector     | `refName`                                                                                           |
| List Name                 | `location`                                                                                          |
| List Value Selector       | `"id" : "{id}", "displayName" : "{name}"`                                                           |
| Rendering Type            | Advanced Multi Select List                                                                          |
| List Display Selector     | `displayName`                                                                                       |
| List Value Selector       | `id`                                                                                                |
| List Items                |  \*\*see below                                                                                      |
| Prefer NetSuite value     | checked (if existing NetSuite values are the master values)                                         |

The list items will depend on the values returned in the `List Values` section after saving the property mapping the first time or clicking the refresh button.&#x20;

The list values returned needs to be formatted. You can use Chat GPT with this prompt:

Below is a sample that can be generated from this list:

{% code overflow="wrap" %}
```
Convert the following string into a JSON array with "id" and "displayName" key value pair objects. The keys must be strings
```
{% endcode %}

Once done, it will produce a Json array which can be pasted in the `List Items`field in the Property Mapping settings.

```
[
  { "id": "1", "displayName": "Location 1" },
  { "id": "2", "displayName": "Location 2" },
  { "id": "3", "displayName": "Location 3" }
]
```

{% hint style="info" %}
Take note: There are 3 objects (with ids 1, 2 and 3) in the list above. There should not be a trailing comma after the last object `{ }`in the list above
{% endhint %}

### Price Property Mapping Rules

Given this input, create the following rules:

*   `Select From Json` import rule for NetSuite (check the checkbox for `Process for NetSuite`) with the following text:

    {% code overflow="wrap" %}
    ```javascript
    items
    ```
    {% endcode %}
* `Text Manipulation` import rule for NetSuite (check the checkbox for `Process for NetSuite`) with the following text:

{% code overflow="wrap" %}
```javascript
if (s !== "") { return s.map((item) => item.location.id); } else { return []; }
```
{% endcode %}

* `Text Manipulation` export rule for NetSuite (check the checkbox for `Process for NetSuite`) with the following text (make sure you are using the appropriate parameters for your case) :

```javascript
return s.map((id) => {
  return {
    locationId: Number(id),
    supplyLotSizingMethod: "FIXED_LOT_SIZE",
    fixedLotSize: 300,
    isWip: true,
    supplyType: "PURCHASE"
  };
});
```
