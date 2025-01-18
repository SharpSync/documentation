# Configure Subsidiary Selection

When using Advanced BOMs subsidiary selection becomes available for manufacturing routes. To setup subsidiary selections, add a new Property Mapping



### Property Mapping Settings

Primary accessor: (unmapped) (or mapped if you have one in the primary)

Secondary accessor: One of {inventoryitem.subsidiary | assemblyitem.subsidiary}&#x20;

Property mapping Settings:

| Setting                   | Value                                   |
| ------------------------- | --------------------------------------- |
| Update NetSuite on Submit | checked                                 |
| List Name                 | `subsidiary`                            |
| List Value Selector       | "id" : "{id}", "displayName" : "{name}" |
| Rendering Type            | Advanced Multi Select List              |
| List Display Selector     | `displayName`                           |
| List Value Selector       | `id`                                    |
| List items                | \*\* see below                          |

The list items will depend on the values returned in the `List Values`section after saving the property mapping the first time or clicking the refresh button.&#x20;

The list values returned needs to be formatted. You can use Chat GPT with this prompt:

Below is a sample that can be generated from this list:

```
Convert the following list of items into a Json array of objects with an "id' and "displayName" keyValue pair
```

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

Given this input, create 2 new import rules (check the checkbox for `Process for NetSuite`):



| Import Rule name  |                                                      |
| ----------------- | ---------------------------------------------------- |
| Select from json  | items                                                |
| Text manipulation | if (!s) return \[]; return s.map((item) => item.id); |





