# Configure subsidiary mapping

When using Advanced BOMs subsidiary selection becomes available for manufacturing routes. To setup subsidiary selections, add a new Property Mapping for each item type that you need.

### Subsidiary Property Mapping Settings

| Setting                   | Value                                                                           |
| ------------------------- | ------------------------------------------------------------------------------- |
| Primary accessor          | <p>(Unmapped) </p><p>or mapped to a Primary Source accessor if you have one</p> |
| Secondary accessor        | <p>assemblyitem.subsidiary<br>inventoryitem.subsidiary<br>etc...</p>            |
| Update Primary on Submit  | unchecked                                                                       |
| Update NetSuite on Submit | checked                                                                         |
| Object Value Selector     | `refName`                                                                       |
| List Name                 | `subsidiary`                                                                    |
| List Value Selector       | "id" : "{id}", "displayName" : "{name}"                                         |
| Rendering Type            | Advanced Multi Select List                                                      |
| List Display Selector     | `displayName`                                                                   |
| List Value Selector       | `id`                                                                            |
| List Items                |  \*\*see below                                                                  |
| Prefer NetSuite value     | checked                                                                         |

The list items will depend on the values returned in the `List Values`section after saving the property mapping the first time or clicking the refresh button.&#x20;

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
  { "id": "8", "displayName": "Branch 1, Inc" },
  { "id": "42", "displayName": "My Industries LLC" },
  { "id": "54", "displayName": "Acme Corp" }
]
```

{% hint style="info" %}
Take note: There are 3 objects (with ids 8,42 and 54) in the list above. There should not be a trailing comma after the last object `{ }`in the list above
{% endhint %}

### Subsidiary Property Mapping Rules

Given this input, create the following rules:

* A `Select From Json` import rule for NetSuite (check the checkbox for `Process for NetSuite`) with the following text:

```
items
```

* A `Text Manipulation` import rule for NetSuite (check the checkbox for `Process for NetSuite`) with the following text:

```javascript
if (s !== "") { return s.map((item) => item.id); } else { return []; }
```

* Since the rendering type of this property mapping is a multi-select type, you need to ensure that values are properly manipulated (a multi-select rendering type deals with array type values). One example is to convert empty values from the Primary/CAD source to an empty array; create a `Text Manipulation` import rule for your Primary/CAD source with the following text (or replace with your custom logic that returns an array of subsidiary ids, making sure that the fallback is an array or an empty array):

```javascript
if (s === "") { return [];}
```

