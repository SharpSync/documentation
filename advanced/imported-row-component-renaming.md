# Imported Row Component Renaming

{% hint style="danger" %}
This feature is in beta and may be tested on request
{% endhint %}

The import row renaming rules are meant to assist with naming conventions when rows are imported.

| Supported Row Types         | Supported            |
| --------------------------- | -------------------- |
| Component                   | \[future release]    |
| Generated Material Children | :white\_check\_mark: |
| Drawing                     | \[future release]    |
| Derivatives                 | not supported, use   |

Material rows are rows that are generated for part documents that have a material assigned, and where a property mapping exists with the 'Is Material Mapping' enabled in the property mapping settings.



* Each rule is run in order
* Rules are additive, so if the first rule causes the 2nd rule to match that will trigger a multi-rule run

**Structure of a rule**&#x20;

The structure of a rule depends on keywords to manipulate the name. An template is provided below

```
[
  {  
    "searchTerm"    : "What to search for (e.g. Stainless Steel 304)",
    "searchMatch"   : "How it is matched {exact|contains|startsWith|endsWith}",
    "returnValue"   : "Value to return when a match is found e.g. SS-304",
    "replaceMatch"  : "what is replaced {everything|firstMatch|lastMatch}" 
  }
]
```

Where the following keywords are used

| Keyword                      | Option       | Meaning                                                                                                                                                         |
| ---------------------------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `searchTerm` **parameter**   |              | What to search for in the component name                                                                                                                        |
| `searchMatch` **parameter**  |              | How the text that is being searched for is matched                                                                                                              |
| parameter option             |  `exact`     | Matches the text exactly, the whole string                                                                                                                      |
| parameter option             | `contains`   | Matches a part of the name. Can be anywhere in the string. So `ss` will match stainle`ss` steel and `ss`teel                                                    |
| parameter option             | `startsWith` | Matches only if the string starts with the specified text                                                                                                       |
| parameter option             | `endsWith`   | Matches only if the string ends with the specified text                                                                                                         |
| `returnValue` **parameter**  |              | When a match is found, what is returned or used as a replacement value. This must be a string                                                                   |
| `replaceMatch` **parameter** |              | When replacing the oldValue with the newValue, how is replacement handled                                                                                       |
| parameter option             | `everything` | Replaces the entire string. This option replaces the entire string with the newValue. It does not keep any text other than the new value.                       |
| parameter option             | `firstMatch` | Replaces only the first instance found. Replacing s with S in the string `stainless steel` will result in `Stainless steel`.                                    |
| parameter option             | `lastMatch`  | <p>Replaces only the last instance found.<br><br>Replacing s with S in the string <code>stainless steel</code> will result in <code>stainless Steel</code>.</p> |

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
  // rule n ... 
]

```
