---
icon: magnifying-glass
---

# Read BOM WorkCenters

Create a new [Property Mapping](../../../../fundamentals/property-mappings/) with the following settings:

<table><thead><tr><th width="301">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Property Name / Header</td><td>Operation WorkCenters</td></tr><tr><td>Accessor</td><td>operationWorkCenters</td></tr><tr><td>Primary Property</td><td>(Unmapped)</td></tr><tr><td>Secondary Property</td><td>mrp.bom.operation_ids</td></tr><tr><td>Update Odoo on Submit</td><td><p>false </p><p>[not here, we'll do it for the individual operations]</p></td></tr><tr><td>List Name</td><td>mrp.workcenter</td></tr><tr><td>List Value Selector</td><td>{id}:{resource_id[1]}</td></tr><tr><td>Rendering Type</td><td>Advanced Multi Select List</td></tr><tr><td>List Display Selector</td><td>name</td></tr><tr><td>List Value Selector</td><td>id</td></tr><tr><td>List items</td><td><p>Copied from the <code>List Value</code> (and formatted as a JSON array) but should look something like this </p><pre class="language-json5"><code class="lang-json5">[
  { "id": 1, "name": "Galvanizing Station" },
  { "id": 2, "name": "Drill Station 1"  }
]
</code></pre></td></tr><tr><td>Enabled</td><td>true</td></tr><tr><td>Prefer Odoo Value</td><td>true</td></tr><tr><td></td><td></td></tr></tbody></table>

After adding the new Property Mapping, add an import rule

<table><thead><tr><th width="235">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Type</td><td>import</td></tr><tr><td>Name</td><td>Text Manipulation</td></tr><tr><td>Process for {Primary}</td><td>false</td></tr><tr><td>Process for Odoo</td><td>true</td></tr><tr><td>JavaScript expression</td><td><pre class="language-javascript"><code class="lang-javascript">
if (!s)
  return [];

if (Array.isArray(s)) 
  return s.map((item) => item.workcenter_id["id"]);

if (typeof s === "string") {
  try {
    const parsed = JSON.parse(s);
    if (Array.isArray(parsed)) {
      return parsed.map((item) => item.workcenter_id["id"]);
    } else {
      console.log("Parsed operationWorkCenters JSON is not an array, it is " + typeof parsed, parsed);
      return [];
    }
  } catch (e) {
    console.log("Invalid operationWorkCenters JSON string:", s, "Error:", e.message);
    return [];
  }
}

console.log("Input string for operationWorkCenters is not an array or valid JSON string, it is " + typeof s, s);
return [];

</code></pre></td></tr></tbody></table>
