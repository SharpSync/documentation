---
icon: magnifying-glass
---

# Read BOM Operations

BOM operations are _individual_ operations that are performed on the item. This means that, despite having the same name as other operations on other BOMs, the items shown are unique.&#x20;

So even if you see duplicates in Odoo, they each have their own underlying unique  `operationId` _for that line in the BOM._

{% hint style="info" %}
The best way to manage operations is to be consistent in your naming convention for your operations.&#x20;

In other words, if you are going to assemble something, always give it the exact same name (e.g. `Assembly` or `Assemble`).
{% endhint %}



BOM operations can often include one or more of the following items in the table below:&#x20;

| Destructive operation | Non-Destructive operation |
| --------------------- | ------------------------- |
| Punching              | Painting                  |
| Welding               | Assembling                |
| Galvanizing           | Powder Coating            |
| etc.                  | Wiring                    |



To read these operations from the BOM, we map to the Odoo property&#x20;

> mrp.bom.operation\_id

Create a new [Property Mapping](../../../../fundamentals/property-mappings/) with the following settings:

<table><thead><tr><th width="301">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Property Name / Header</td><td>BOM Operations</td></tr><tr><td>Accessor</td><td>bomOperations</td></tr><tr><td>Primary Property</td><td>(Unmapped)</td></tr><tr><td>Secondary Property</td><td><code>mrp.bom.operation_ids</code></td></tr><tr><td>Update Odoo on Submit</td><td>false</td></tr><tr><td>Rendering Type</td><td><code>Advanced Multi Select List</code></td></tr><tr><td>List Display Selector</td><td>displayName</td></tr><tr><td>List Value Selector</td><td>id</td></tr><tr><td>List items</td><td><p> Here is a sample of what you can insert, but it can be anything, whatever you use most frequently [more on this below]**</p><pre class="language-json5"><code class="lang-json5">[
    {
        "workCenterId": 2,
        "name": "Drill",
        "value": {
            "sequence": 10,
            "name": "Drill",
            "workcenter_id": 2,
            "time_mode": "manual",
            "time_mode_batch": 10,
            "time_cycle_manual": 60
        }
    },
    {
        "workCenterId": 1,
        "name": "Galvanize",
        "value": {
            "sequence": 20,
            "name": "Galvanize",
            "workcenter_id": 1,
            "time_mode": "manual",
            "time_mode_batch": 10,
            "time_cycle_manual": 60
        }
    },
    {
        "workCenterId": 3,
        "name": "Punch",
        "value": {
            "sequence": 30,
            "name": "Punch",
            "workcenter_id": 3,
            "time_mode": "manual",
            "time_mode_batch": 10,
            "time_cycle_manual": 60
        }
    },
]
</code></pre></td></tr><tr><td>Enabled</td><td>true</td></tr><tr><td>Prefer Odoo Value</td><td>true</td></tr></tbody></table>

\*\* When creating this list, and you only want to read the values, you'll want to specify at least the Work center id, and the name of the operation. If you also want to write the values back to Odoo, then you'll have to specify the entire object required to add new operations and the default settings for the operation.&#x20;

{% hint style="info" %}
Writing BOM operations is explained in the next topic [write-bom-operations.md](write-bom-operations.md "mention")&#x20;
{% endhint %}

Some BOM operation properties that are available for reading are listed below. The list is not exhaustive and you may customize your own (See the Odoo Configuration options > Models > `mrp.routing.workcenter`):

<table><thead><tr><th width="198">Property</th><th width="117">Type</th><th width="130">Availability</th><th data-type="checkbox">Required for reading in SharpSync</th></tr></thead><tbody><tr><td>active</td><td>bool</td><td>16, 17, 18</td><td>false</td></tr><tr><td>name</td><td>string</td><td>16, 17, 18</td><td>true</td></tr><tr><td>bom_id</td><td>integer</td><td>16, 17, 18</td><td>false</td></tr><tr><td>workcenter_id</td><td>integer</td><td>16,17,18</td><td>true</td></tr><tr><td>time_mode_batch</td><td>integer</td><td>16,17,18</td><td>false</td></tr><tr><td>time_cycle_manual</td><td>integer</td><td>16,17,18</td><td>false</td></tr><tr><td>note</td><td>string</td><td>16,17,18</td><td>false</td></tr><tr><td>sequence</td><td>integer</td><td>16,17,18</td><td>false</td></tr></tbody></table>

After adding the new Property Mapping, add an import rule

<table><thead><tr><th width="235">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Type</td><td>import</td></tr><tr><td>Name</td><td><code>Text Manipulation</code></td></tr><tr><td>Process for {Primary}</td><td>false</td></tr><tr><td>Process for Odoo</td><td>true</td></tr><tr><td>JavaScript expression</td><td><pre class="language-javascript"><code class="lang-javascript">if (!s) 
  return [];

if (Array.isArray(s)) 
  return s.map((item) => item.workcenter_id["id"]);

if (typeof s === "string") {
  try {
    const parsed = JSON.parse(s);
    if (Array.isArray(parsed)) {
      return parsed.map((item) => item.workcenter_id["id"]);
    } else {
      console.log("Parsed JSON is not an array, it is " + typeof parsed, parsed);
      return [];
    }
  } catch (e) {
    console.log("Invalid JSON string:", s, "Error:", e.message);
    return [];
  }
}
console.log("Input is not an array or valid JSON string, it is " + typeof s, s);
return [];

</code></pre></td></tr></tbody></table>
