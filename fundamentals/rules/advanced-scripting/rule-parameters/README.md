---
icon: js
cover: >-
  https://images.unsplash.com/photo-1518773553398-650c184e0bb3?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwyfHxjb2RlfGVufDB8fHx8MTczNTgxODA2MHww&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Rule Parameters

### Rule parameter context

The parameters available to these rules have a context associated with them. 'Context' here means that, when the data comes from a source, only the data in that source is available to the rule. This is applicable to Import Data Transformation rules

<table><thead><tr><th width="139">Parameter</th><th width="325">Description</th><th>Documentation</th></tr></thead><tbody><tr><td>s</td><td>The current string value from the source</td><td>Can be modified by previous rule</td></tr><tr><td>rowData</td><td>The entire row's data (all the cell values, modifications, differences, etc.)</td><td>Cannot be modified, read-only</td></tr><tr><td>p</td><td>The pass / block value</td><td>Cannot be modified, read-only</td></tr><tr><td>pm</td><td>The property mapping object</td><td>Contains additional options such as "Is it read-only"</td></tr></tbody></table>

Typical rules only manipulate or use the string value `s` which is passed to it. The table above lists the data to which special rules have access to. It allows for much greater scripting capability.&#x20;
