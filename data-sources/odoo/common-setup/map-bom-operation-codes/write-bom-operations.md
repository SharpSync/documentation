---
icon: magnifying-glass
---

# Write BOM Operations

BOM operations are _individual_ operations that are performed on the item.  This page follows on the progress from the previous topic [read-bom-operations.md](read-bom-operations.md "mention")

{% hint style="info" %}
The best way to manage operations is to be consistent in your naming convention for your operations.&#x20;

In other words, if you are going to assemble something, always give it the exact same name (e.g. `Assembly` or `Assemble`).
{% endhint %}

{% hint style="warning" %}
**How SharpSync decides what to update**

SharpSync reads the BOM's current Operations from Odoo immediately before writing, and matches each one on **Work Centre plus Operation name**, ignoring case and surrounding spaces. Sequence is not part of the match, so re-ordering a routing updates it in place rather than duplicating it.

* **Renaming an Operation, or pointing it at a different Work Centre, is not an edit.** It no longer matches, so the old Operation is archived and a new one is created. This is why the naming convention above matters.
* **The columns are the routing.** Whatever the BOM Operation columns resolve to is what the BOM will have. A cycle time tuned by hand in Odoo is reset to the value in **List items** on the next sync, and a hand re-ordered routing snaps back to the order of the columns. If you want a different cycle time, change the cycle time value in the property mapping's **List items** setting. If you want SharpSync to keep the cycle time/ordering the same, you'll need to set up a custom mapping rule that does so.
{% endhint %}

### Create a new Property Mapping

To write these operations to the Odoo BOM, we map to the Odoo property&#x20;

> mrp.bom.operation\_ids

The first step is to create a Property Mapping in addition to the Property Mapping created in the previous article.

Create a new [Property Mapping](../../../../fundamentals/property-mappings/) with the following settings :

<table><thead><tr><th width="301">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Property Name / Header</td><td>BOM Operations</td></tr><tr><td>Accessor</td><td>bomOperations</td></tr><tr><td>Primary Property</td><td>(Unmapped) (or a related property in the Primary Source)</td></tr><tr><td>Secondary Property</td><td><code>mrp.bom.operation_ids</code></td></tr><tr><td>List Name</td><td>mrp.workcenter</td></tr><tr><td>List Value Selector</td><td>{id}:{name}</td></tr><tr><td>Update Odoo on Submit</td><td>true</td></tr><tr><td>Rendering Type</td><td><code>Advanced List</code></td></tr><tr><td>List Display Selector</td><td><code>operationName</code></td></tr><tr><td>List Value Selector</td><td><code>workCenterId</code></td></tr><tr><td>List items</td><td><p> Here is a sample of what you can insert, but it can be anything, whatever you use most frequently [more on this below]**</p><pre class="language-json5"><code class="lang-json5">[
    {
        "workCenterId": 1,
        "name": "Drill",
        "operationName" : "Drill",
        "value": {
            "sequence": 10,
            "name": "Drill",
            "workcenter_id": 1,
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
            "workcenter_id": 2,
            "time_mode": "manual",
            "time_mode_batch": 10,
            "time_cycle_manual": 60
        }
    },
    // add as many as you like....
]
</code></pre></td></tr><tr><td>Enabled</td><td>true</td></tr><tr><td>Prefer Odoo Value</td><td>true</td></tr></tbody></table>

### Add new Export Rules

After adding the new Property Mapping, add 2 new export rules:

#### Rule 1

<table><thead><tr><th width="235">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Type</td><td>export</td></tr><tr><td>Name</td><td><code>Text Manipulation</code></td></tr><tr><td>Process for {Primary}</td><td>false</td></tr><tr><td>Process for Odoo</td><td>true</td></tr><tr><td>JavaScript expression</td><td><pre class="language-javascript"><code class="lang-javascript">/* Collect the Work Centres chosen in the BOM Operation columns, in column order. */
let selectedWorkCenterIds = [];

if (rowData.modifications.bomOperation1)
  selectedWorkCenterIds = selectedWorkCenterIds.concat(rowData.modifications.bomOperation1);

if (rowData.modifications.bomOperation2)
  selectedWorkCenterIds = selectedWorkCenterIds.concat(rowData.modifications.bomOperation2);

/* Turn each chosen Work Centre into the Operation defined in this mapping's List items. */
const pmObjectListItems = JSON.parse(pm.objectListItems);
const updateValue = [];

selectedWorkCenterIds.forEach((workCenterId) => {
  pmObjectListItems.forEach((item) => {
    if (item.workCenterId === workCenterId)
      updateValue.push(item.value);
  });
});

return updateValue;

</code></pre></td></tr></tbody></table>

**What this rule does:**

Reads the work center selected in the BOM operation columns set up in \[[Read BOM Operations](read-bom-operations.md)], looks it up in this mapping's **List items**, and returns the matching operation definitions as the value to write to Odoo.

This rule does not determine whether an operation is added or updated. SharpSync reads the BOM's current operations from Odoo immediately before writing and matches them by checking the workcenter and operation name values, so an operation that already exists is updated in place and only an operation with no workcenter & operation name match in Odoo is created. An operation `id` value is not necessary for this, and if an operation `id` value is supplied then SharpSync will ignore it.

#### Rule 2

<table><thead><tr><th width="235">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Type</td><td>export</td></tr><tr><td>Name</td><td><code>Export Manipulation</code></td></tr><tr><td>Process for {Primary}</td><td>false</td></tr><tr><td>Process for Odoo</td><td>true</td></tr><tr><td>JavaScript expression</td><td><pre class="language-javascript"><code class="lang-javascript">if (rowData.modifications.bomOperations &#x26;&#x26; rowData.modifications.bomOperations.length == 0)
  delete s["bomOperations"];

return s;

</code></pre></td></tr></tbody></table>

**What this rule does:**

This rule removes the `bomOperations` key from SharpSync's export payload when there are no operations to export, so SharpSync skips performing any actions on the BOM's operations altogether instead of processing an empty list.

Sending an empty operation list to Odoo will **not** clear the operations from the BOM in Odoo, and no export rule can. SharpSync interprets an absent or empty operation list as an instruction to leave the current Odoo operations as-is. SharpSync can trim a BOM's operation list, but it cannot empty it. Removing all operations from a BOM must be done manually in Odoo.

**Note:** Because sending an empty list is the same as sending no list at all, this rule is optional and is just for efficiency purposes.
