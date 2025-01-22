---
icon: list-check
description: Detail about what data rules have access to
---

# Rule Template Parameters

User scriptable rules (JavaScript) have access to 3 parameters, not just 1. The rules that fall into this category are:

* Text Manipulation (import / export)
* Text Evaluation (display)
* Export Manipulation (export only)

The parameters available to these rules are listed below:

<table><thead><tr><th width="139">Parameter</th><th width="325">Description</th><th>Note</th></tr></thead><tbody><tr><td>s</td><td>The current string value from the source</td><td>Can be modified by previous rule</td></tr><tr><td>rowData</td><td>The entire row's data (all the cell values, modifications, differences, etc.)</td><td>Cannot be modified, read-only</td></tr><tr><td>p</td><td>The pass / block value</td><td>Cannot be modified, read-only</td></tr><tr><td>pm</td><td>The property mapping object</td><td>Contains additional options such as "Is it read-only"</td></tr></tbody></table>



In the table listed above, typical rules only have access to the string value `s` which is passed to it. The special rules listed above the table has access to more data which allows for much greater scripting capability.&#x20;

The result of a rule must always be a `string` or a representation of a `string` such as a JavaScript `object` (JSON) which has been serialized. If it does not return a value you may experience unexpected results in the UI, possibly even instability (BOM view not rendering) in attempting to render client side BOMs.

{% hint style="info" %}
Should you happen to break your BOM view using these scripts, simply disable the rule.&#x20;
{% endhint %}



<table><thead><tr><th width="193">Param</th><th>Description</th></tr></thead><tbody><tr><td>s</td><td>The current string value in the cell (changes with each successive import rule or export rule (if there are any)</td></tr><tr><td>rowData</td><td>The rowData object (more detail below **)</td></tr><tr><td>p</td><td>The pass / block value, does not change</td></tr><tr><td>pm</td><td>The property mapping's settings. Does not change. (** more detail below)</td></tr></tbody></table>

\*\* The `rowData` value is a special value. It contains, but is not limited to, the following key/values:

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
</code></pre></td></tr><tr><td><code>primaryExportData</code></td><td>object containing the final set of data to send to the Primary Source. This is the final set of values to be sent.</td></tr><tr><td><code>secondaryExportData</code></td><td>object containing the final set of data to send to the Secondary Source. This is the final set of values to be sent</td></tr></tbody></table>

\*\* The `pm`value is a special value. It contains, but is not limited to, the following key/values:

| Key name   | Description of the value                                           |
| ---------- | ------------------------------------------------------------------ |
| isReadOnly | Whether the admin has selected this property as editable           |
| isVisible  | Whether the admin has selected this property as visible by default |

