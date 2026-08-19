---
icon: magnifying-glass
---

# Write BOM Operations

{% hint style="success" %}
**Summary of article's major steps**

* Create a new Property Mapping
* Add 2 new export rules to format the object and make it ready for Odoo BOM&#x20;
{% endhint %}

BOM operations are _individual_ operations that are performed on the item.  This page follows on the progress from the previous topic [read-bom-operations.md](read-bom-operations.md "mention")

{% hint style="info" %}
The best way to manage operations is to be consistent in your naming convention for your operations.&#x20;

In other words, if you are going to assemble something, always give it the exact same name (e.g. `Assembly` or `Assemble`).
{% endhint %}

{% hint style="warning" %}
**How SharpSync decides what to update**

SharpSync reads the BOM's current Operations from Odoo immediately before writing, and matches each one on **Work Centre plus Operation name**, ignoring case and surrounding spaces. Sequence is not part of the match, so re-ordering a routing updates it in place rather than duplicating it.

* **Renaming an Operation, or pointing it at a different Work Centre, is not an edit.** It no longer matches, so the old Operation is archived and a new one is created. This is why the naming convention above matters.
* **The columns are the routing.** Whatever the BOM Operation columns resolve to is what the BOM will have. A cycle time tuned by hand in Odoo is reset to the value in **List items** on the next sync, and a hand re-ordered routing snaps back to the order of the columns. If you want a different cycle time, set it in **List items**.
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

Reads the Work Centre selected in each BOM Operation column, looks it up in this mapping's **List items**, and returns the matching Operation definitions as the value to write.

This rule does not decide whether an Operation is added or updated, and must never supply an `id`. SharpSync reads the BOM's current Operations from Odoo immediately before writing and matches them on Work Centre and Operation name, so an Operation that already exists is updated in place and only a genuinely new one is created.

#### Rule 2

<table><thead><tr><th width="235">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Type</td><td>export</td></tr><tr><td>Name</td><td><code>Export Manipulation</code></td></tr><tr><td>Process for {Primary}</td><td>false</td></tr><tr><td>Process for Odoo</td><td>true</td></tr><tr><td>JavaScript expression</td><td><pre class="language-javascript"><code class="lang-javascript">if (rowData.modifications.bomOperations &#x26;&#x26; rowData.modifications.bomOperations.length == 0)
  delete s["bomOperations"];

return s;

</code></pre></td></tr></tbody></table>

**What this rule does:**

Removes the `bomOperations` key from the payload when the column resolved to no Operations, so SharpSync skips this row's Operations altogether instead of sending an empty list.

This does **not** clear the Operations on the BOM in Odoo, and no export rule can. An absent or empty value means "this sync has no opinion about Operations", not "this BOM has none". A sync can still _trim_ a routing — clearing one of two BOM Operation columns archives the Operation that is no longer mapped — but it will never empty one. Archived Operations stay visible in Odoo under the **Archived** filter and can be restored there. To remove a BOM's last remaining Operation, do it in Odoo directly.
