---
icon: magnifying-glass
---

# Read BOM WorkCenters

{% hint style="info" %}
### Choose what to display

The work centers and operations both depend on `mpr.bom.operation_ids`. You'll want to display _one_ of the following:

* The mapped work center or stations, read-only (e.g. Drill Station 1, Galvanization, Powder Coating)
* The mapped operation names (e.g. Drill, Galvanize, Powder Coat)



For more complicated mappings, please contact us through the support portal.
{% endhint %}

Create a new [Property Mapping](../../../../fundamentals/property-mappings/) with the following settings:

<table><thead><tr><th width="301">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Property Name / Header</td><td>Operation WorkCenters</td></tr><tr><td>Accessor</td><td>operationWorkCenters</td></tr><tr><td>Primary Property</td><td>(Unmapped)</td></tr><tr><td>Secondary Property</td><td><code>mrp.bom.operation_ids</code></td></tr><tr><td>Update Odoo on Submit</td><td><p>false </p><p>[not here, we'll do it for the individual operations]</p></td></tr><tr><td>List Name</td><td>mrp.workcenter</td></tr><tr><td>List Value Selector</td><td><code>{id}:{resource_id[1]}</code></td></tr><tr><td>Rendering Type</td><td><code>Advanced Multi Select List</code></td></tr><tr><td>List Display Selector</td><td>name</td></tr><tr><td>List Value Selector</td><td>id</td></tr><tr><td>List items</td><td><p>Copied from the <code>List Value</code> (and formatted as a JSON array) but should look something like this </p><pre class="language-json5"><code class="lang-json5">[
  { "id": 1, "name": "Galvanizing Station" },
  { "id": 2, "name": "Drill Station 1"  }
]
</code></pre></td></tr><tr><td>Enabled</td><td>true</td></tr><tr><td>Prefer Odoo Value</td><td>true</td></tr></tbody></table>

{% hint style="success" %}
Productivity Tip

***

To easily convert the list into a list of values, go to your favourite GPT and type the following prompt, pasting the values with it:

{% code overflow="wrap" %}
```
Convert the following text into a list of json objects the number as the "id" and the text as the "name"
```
{% endcode %}
{% endhint %}

After adding the new Property Mapping, add an import rule

<table><thead><tr><th width="235">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Type</td><td>import</td></tr><tr><td>Name</td><td><code>Text Manipulation</code></td></tr><tr><td>Process for {Primary}</td><td>false</td></tr><tr><td>Process for Odoo</td><td>true</td></tr><tr><td>JavaScript expression</td><td><pre class="language-javascript"><code class="lang-javascript">
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
