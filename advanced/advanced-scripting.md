---
description: '[Work in progress]'
cover: >-
  https://images.unsplash.com/photo-1518773553398-650c184e0bb3?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwyfHxjb2RlfGVufDB8fHx8MTczNTgxODA2MHww&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Advanced Scripting

Advanced Rules are more difficult to use, but they are much more powerful than the other rules.

User scriptable rules (JavaScript) have access to 4 parameters, instead of just 1. The rules that fall into this category are:

* Text Manipulation (import / export)
* Text Evaluation (display)
* Export Manipulation (export only)

The parameters available to these rules are listed below:

<table><thead><tr><th width="139">Parameter</th><th width="325">Description</th><th>Note</th></tr></thead><tbody><tr><td>s</td><td>The current string value from the source</td><td>Can be modified by previous rule</td></tr><tr><td>rowData</td><td>The entire row's data (all the cell values, modifications, differences, etc.)</td><td>Cannot be modified, read-only</td></tr><tr><td>p</td><td>The pass / block value</td><td>Cannot be modified, read-only</td></tr><tr><td>pm</td><td>The property mapping object</td><td>Contains additional options such as "Is it read-only"</td></tr></tbody></table>

typical rules only have access to the string value `s` which is passed to it. In the table listed above are the data to which special rules listed on this page has access to. It allows for much greater scripting capability.&#x20;

When the rule is an export or import rule, the result of a rule must always be a `string` or a representation of a `string` such as a JavaScript `object` (JSON) which has been serialized. If it does not return a value you may experience unexpected results in the UI, possibly even instability (BOM view not rendering) in attempting to render client side BOMs.



When the rule is an evaluation rule, the result (which must only be returned for errors or warnings), must be an object containing a 'message' key, e.g.

```
{ 'message' : 'if the rule fails show this text in the tooltip' }
```

{% hint style="info" %}
Should you happen to break your BOM view using these scripts, simply disable the rule.&#x20;
{% endhint %}



<table><thead><tr><th width="193">Param</th><th>Description</th></tr></thead><tbody><tr><td>s</td><td>The current string value in the cell (changes with each successive import rule or export rule (if there are any)</td></tr><tr><td>rowData</td><td>The rowData object (more detail below **)</td></tr><tr><td>p</td><td>The pass / block value, does not change</td></tr><tr><td>pm</td><td>The Property Mapping's object. Does not change. (** more detail below)</td></tr></tbody></table>

### `s` Parameter

The `s` parameter is the representation of the cell value. Most of the time this is a value of _type_ `string`  but sometimes it can be a _type_ JSON `object` or a JSON `array` (if using say an import manipulation rule to change the type).

{% hint style="info" %}
If you see a value onscreen shown as \[object Object] this means you have a JSON object or array that needs to be parsed or converted into a different _type_
{% endhint %}



### `rowData` Parameter

&#x20;The `rowData` is the row itself including any cells, differences or modifications. It contains, but is not limited to, the following key/values:

<table><thead><tr><th width="241">Key name</th><th>Description of the value</th></tr></thead><tbody><tr><td><code>isAssemblyRow</code></td><td>A bool value indicating if the current row value is an assembly row (contains children according to the source)</td></tr><tr><td><code>componentName</code></td><td>The primary identifier of each row - typically the name of the component</td></tr><tr><td><code>componentPathArray</code></td><td>The path of each component. So if you have assembly A1, with Part P1, then this value will be [ 'A1', 'P1' ]</td></tr><tr><td><code>cells</code></td><td><p>The row values for the entire row. A typical row object might look something like this (notice the nested `cells` key):</p><pre class="language-json"><code class="lang-json">{
  "isAssemblyRow" : false,
  "componentName" : "Part 1",
  "componentPathArray" : [ "A1", "Part 1"],
  "cells" : {
     "partNumber" : "P1",
     "description" : "Side plate",
     "revision" : "A"
     "material" : "steel",
     "qty" : 1,
  }
} 
</code></pre></td></tr><tr><td><code>modifications</code></td><td><pre class="language-javascript"><code class="lang-javascript">{
  "isAssemblyRow" : false,
  "componentName" : "Part 1",
  "componentPathArray" : [ "A1", "Part 1"],
  "modifications" : {
     "partNumber" : "P1-PN",
     "description" : "Side Plate",
     "material" : "Steel",
  }
} 
</code></pre></td></tr><tr><td><code>primarySourceExportData</code></td><td>object containing the final set of data to send to the Primary Source. This is the final set of values to be sent.</td></tr><tr><td><code>secondarySourceExportData</code></td><td>object containing the final set of data to send to the Secondary Source. This is the final set of values to be sent</td></tr></tbody></table>

### `p` Parameter

The `p`  parameter is the `PassOrBlock`value specified in the Property Mapping Rule options.

### `pm` Parameter

The `pm`value contains values related to the [Property Mapping](broken-reference) used for this column. It cannot be modified, and any modifications to it are ignored. It contains, but is not limited to, the following key/values:

| Key name                           | Description of the value                                           |
| ---------------------------------- | ------------------------------------------------------------------ |
| `isReadOnly`                       | Whether the admin has selected this property as editable           |
| `isVisible`                        | Whether the admin has selected this property as visible by default |
| `shouldUpdatePrimaryDatasource`    | Whether to update the Primary Source                               |
| `shouldUpdateSecondaryDatasource`  | Whether to update the Secondary Source                             |

