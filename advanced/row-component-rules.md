---
icon: list-check
---

# Row Component Rules

{% hint style="danger" %}
This feature is in early beta and may be tested on request
{% endhint %}

When importing rows, you may want materials to automatically be listed as children of part components. The row component rules are meant to assist with this.

The current implementation focuses only on Material Rows generated. Future support is planned for non-material rows.

{% hint style="success" %}
You may think of row component rules as rules that affect the _structure_ of a BOM. The structure of the BOM is affected by renaming components or removing certain components
{% endhint %}

| Supported Row Types         | Supported            |
| --------------------------- | -------------------- |
| Component                   | \[future release]    |
| Generated Material Children | :white\_check\_mark: |
| Drawing                     | \[future release]    |
| Derivatives                 | not supported, use   |

| Primary Source    | Supported            |
| ----------------- | -------------------- |
| Autodesk Inventor | \[future release]    |
| Autodesk Fusion   | \[future release]    |
| CSV               | \[future release]    |
| Onshape           | :white\_check\_mark: |
| SolidWorks        | \[future release]    |
| SolidWorks PDM    | \[future release]    |

<figure><img src="../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

Material rows are rows that are generated for part documents that have a material assigned, and where a property mapping exists with the 'Is Material Mapping' enabled in the property mapping settings.

* Each rule is run in order
* Rules are additive, so if the first rule causes the 2nd rule to match that will trigger a multi-rule run

**Structure of a rule**&#x20;

The structure of a rule depends on keywords to manipulate the name. An template is provided below

```
[
  {  
    "searchTerm"    : "Single value. What to search for (e.g. Stainless Steel 304)",,
    "searchTerms"   : [ "Optional", "list", "of", "terms", "Use with isInList"],
    "searchMatch"   : "How it is matched {exact|contains|startsWith|endsWith|isInList}",
    "returnValue"   : "Value to return when a match is found e.g. SS-304",
    "replaceMatch"  : "what is replaced {everything|firstMatch|lastMatch}" 
  }
]
```

Where the following keywords are used

| Type                  | Keyword         | Meaning                                                                                                                                                         |
| --------------------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **parameter**         | `searchTerm`    | What to search for in the component name                                                                                                                        |
| **parameter**         | `searchMatch`   | How the text that is being searched for is matched                                                                                                              |
| **parameter**         | `searchTerms`   | Optional: specify a list of terms to search                                                                                                                     |
| `searchMatch` option  | `exact`         | Matches the text exactly, the whole string                                                                                                                      |
| `searchMatch` option  | `contains`      | Matches a part of the name. Can be anywhere in the string. So `ss` will match stainle`ss` steel and `ss`teel                                                    |
| `searchMatch` option  | `startsWith`    | Matches only if the string starts with the specified text                                                                                                       |
| `searchMatch` option  | `endsWith`      | Matches only if the string ends with the specified text                                                                                                         |
| `searchMatch` option  | `isInList`      | Matches if a the given list of values contains the text                                                                                                         |
| **parameter**         | `returnValue`   | When a match is found, what is returned or used as a replacement value. This must be a string                                                                   |
| **parameter**         | `replaceMatch`  | When replacing the oldValue with the newValue, how is replacement handled                                                                                       |
| `replaceMatch` option | `everything`    | Replaces the entire string. This option replaces the entire string with the newValue. It does not keep any text other than the new value.                       |
| `replaceMatch` option | `firstMatch`    | Replaces only the first instance found. Replacing s with S in the string `stainless steel` will result in `Stainless steel`.                                    |
| `replaceMatch` option | `lastMatch`     | <p>Replaces only the last instance found.<br><br>Replacing s with S in the string <code>stainless steel</code> will result in <code>stainless Steel</code>.</p> |
| `replaceMatch` option | `includeRow`    | Used with `isInList` to determine if the row should remain                                                                                                      |
| `replaceMatch` option | `excludeRow`    | Used with `isInList` to determine if the row should remain                                                                                                      |

**Example: Substitute material name**&#x20;

In the example below, we'll

{% hint style="danger" %}
Do not include the comments // in the rules. They're only for illustration purposes.
{% endhint %}

{% hint style="success" %}
If you're unfamiliar with JSON editors, I've found the easiest editor for JSON is VSCODE&#x20;
{% endhint %}

```json5
[
  // rule 1 - sample
  {  
    "searchTerm"    : "componentNameValueToSearch e.g.1.2. Gavl",
    "searchMatch"   : "contains",
    "returnValue"   : "valueToReturn e.g. Galvanized Steel sheet",
    "replaceMatch"  : "Everything" 
  },
  // rule 2 - actual
  {  
    "searchTerm"    : "tainless", // no preceding 's'
    "searchMatch"   : "contains",
    "returnValue"   : "SS-304",
    "replaceMatch"  : "Everything" // always returns the same material code
  },
  // rule 3 - actual
  {  
    "searchTerms"    : [ "Silicone-Rubber", "Neoprene", "1.5mm AL 5004", "2.5mm AL 5005"],
    "searchMatch"   : "isInList",
    "returnValue"   : "99-AS0002",
    "replaceMatch"  : "Everything" 
  } 
  // rule n ... 
]

```
