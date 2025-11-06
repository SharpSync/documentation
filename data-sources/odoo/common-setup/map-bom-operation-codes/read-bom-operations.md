---
icon: magnifying-glass
---

# Read BOM Operations

BOM operations are _individual_ operations that are performed on the item. This means that, despite having the same name as other operations on other BOMs, the items shown are unique.&#x20;

So even if you see duplicates in Odoo, they each have their own underlying unique  `operationId` _for that line in the BOM._

{% hint style="success" %}
**Summary of article's major steps**

* Create a new Property Mapping&#x20;
* Add an import rule to format the object and make it ready for SharpSync
{% endhint %}

<figure><img src="../../../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
The best way to manage operations is to be consistent in your naming convention for your operations.&#x20;

In other words, if you are going to assemble something, always give it the exact same name (e.g. `Assembly` or `Assemble`).
{% endhint %}

{% hint style="info" %}
### Choose what to display

The work centers and operations both depend on `mpr.bom.operation_ids`. You'll want to display _one_ of the following:

* The mapped work center or stations, read-only (e.g. Drill Station 1, Galvanization, Powder Coating)
* The mapped operation names (e.g. Drill, Galvanize, Powder Coat)



For more complicated mappings, please contact us through the support portal.
{% endhint %}

BOM operations can often include one or more of the following items in the table below:&#x20;

| Destructive operation | Non-Destructive operation |
| --------------------- | ------------------------- |
| Punching              | Painting                  |
| Welding               | Assembling                |
| Galvanizing           | Powder Coating            |
| etc.                  | Wiring                    |

To read these operations from the BOM, we map to the Odoo property&#x20;

> mrp.bom.operation\_ids

### Create a new Property Mapping

Create a new [Property Mapping](../../../../fundamentals/property-mappings/) with the following settings (create one for each one you would like to map. So if the first is `bomOperation1`, then the last would be `bomOperationN`) :

<table><thead><tr><th width="301">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Property Name / Header</td><td>BOM Operation 1 (or 2, 3..n)</td></tr><tr><td>Accessor</td><td>bomOperation1</td></tr><tr><td>Primary Property</td><td>(Unmapped) (or a related property in the Primary Source)</td></tr><tr><td>Secondary Property</td><td><code>mrp.bom.operation_ids</code></td></tr><tr><td>List Name</td><td>mrp.workcenter</td></tr><tr><td>List Value Selector</td><td>{id}:{name}</td></tr><tr><td>Update Odoo on Submit</td><td>false</td></tr><tr><td>Rendering Type</td><td><code>Advanced List</code></td></tr><tr><td>List Display Selector</td><td><code>operationName</code></td></tr><tr><td>List Value Selector</td><td><code>workCenterId</code></td></tr><tr><td>List items</td><td><p> Here is a sample of what you can insert, but it can be anything, whatever you use most frequently [more on this below]**</p><pre class="language-json5"><code class="lang-json5">[
    {
        "workCenterId": 1,
        "name": "Drill",
        "operationName" : "Drill",
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
        "workCenterId": 2,
        "name": "Galvanize",
        "operationName" : "Galvanize",
        "value": {
            "sequence": 20,
            "name": "Galvanize",
            "workcenter_id": 1,
            "time_mode": "manual",
            "time_mode_batch": 10,
            "time_cycle_manual": 60
        }
    },
    // add as many as you like....
]
</code></pre></td></tr><tr><td>Enabled</td><td>true</td></tr><tr><td>Prefer Odoo Value</td><td>true</td></tr></tbody></table>

Once you have the property mapping set up, create a 2nd, 3rd and 4th Property Mapping for each additional operation you would like to map (you don't _have_ to create this many, they just serve as place holders).

### Add an import rule

For each of these Property Mappings, add the following `Text Manipulation` (import) rule which parses the value of `mrp.bom.operation_ids` and converts it into a value in the column

{% hint style="info" %}
Writing BOM operations is explained in the next topic [write-bom-operations.md](write-bom-operations.md "mention")&#x20;
{% endhint %}

<table><thead><tr><th width="235">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Type</td><td>import</td></tr><tr><td>Name</td><td><code>Text Manipulation</code></td></tr><tr><td>Process for {Primary}</td><td>false</td></tr><tr><td>Process for Odoo</td><td>true</td></tr><tr><td>JavaScript expression</td><td><pre class="language-javascript"><code class="lang-javascript">if (!s || s.length === 0) 
   return "";
   
const operationToSelect = 1; /* change this to 1, 2,3,4... based on the number of operations*/

if (Array.isArray(s)) {
  if (s.length == 0) return "";
  if (
    s.length > operationToSelect &#x26;&#x26;
    s[operationToSelect].workcenter_id &#x26;&#x26;
    s[operationToSelect].workcenter_id.length > 0
  ) {
    return Number(s[operationToSelect].workcenter_id[0]);
  }
  return "";
}

if (typeof s === "string") {
  try {
    console.log("Parsing operationWorkCenters JSON string:", s);
    const parsed = JSON.parse(s);
    if (Array.isArray(parsed)) {
      /* If the parsed string is an array, return the workcenter ID of the first element */ if (
        parsed.length > operationToSelect &#x26;&#x26;
        parsed[operationToSelect].workcenter_id &#x26;&#x26;
        parsed[operationToSelect].workcenter_id.length > 0
      ) {
        return Number(parsed[operationToSelect].workcenter_id[0]);
      }
      return "";
    } else {
      console.log("Parsed operationWorkCenters JSON is not an array, it is " + typeof parsed, parsed);
      return "";
    }
  } catch (e) {
    console.log("Invalid operationWorkCenters JSON string:", s, "Error:", e.message);
    return "";
  }
}
return "";

</code></pre></td></tr></tbody></table>

After adding the new Property Mapping, add an import rule
