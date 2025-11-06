---
icon: magnifying-glass
---

# Write BOM Operations

{% hint style="danger" %}
This page is a Work in Progress and not completed yet, please check back again soon
{% endhint %}

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

### Create a new Property Mapping

To write these operations to the Odoo BOM, we map to the Odoo property&#x20;

> mrp.bom.operation\_ids

The first step is to create a Property Mapping in addition to the Property Mapping created in the previous article.

Create a new [Property Mapping](../../../../fundamentals/property-mappings/) with the following settings :

<table><thead><tr><th width="301">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Property Name / Header</td><td>BOM Operations</td></tr><tr><td>Accessor</td><td>bomOperations</td></tr><tr><td>Primary Property</td><td>(Unmapped) (or a related property in the Primary Source)</td></tr><tr><td>Secondary Property</td><td><code>mrp.bom.operation_ids</code></td></tr><tr><td>List Name</td><td>mrp.workcenter</td></tr><tr><td>List Value Selector</td><td>{id}:{name}</td></tr><tr><td>Update Odoo on Submit</td><td>false</td></tr><tr><td>Rendering Type</td><td><code>Advanced List</code></td></tr><tr><td>List Display Selector</td><td><code>operationName</code></td></tr><tr><td>List Value Selector</td><td><code>workCenterId</code></td></tr><tr><td>List items</td><td><p> Here is a sample of what you can insert, but it can be anything, whatever you use most frequently [more on this below]**</p><pre class="language-json5"><code class="lang-json5">[
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

| Property Mapping Setting | Old value | New value |
| ------------------------ | --------- | --------- |
| Update Odoo On Submit?   | false     | true      |

### Add new Export Rules

After adding the new Property Mapping, add 2 new export rules

<table><thead><tr><th width="235">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Type</td><td>export</td></tr><tr><td>Name</td><td><code>Text Manipulation</code></td></tr><tr><td>Process for {Primary}</td><td>false</td></tr><tr><td>Process for Odoo</td><td>true</td></tr><tr><td>JavaScript expression</td><td><pre class="language-javascript"><code class="lang-javascript">let bomOperations = [];
let updateValue = [];
let existingOperationIds = [
  { id: 0, operationId: 0 },
  { id: 0, operationId: 0 },
];

/* bopOperation1 */
if (rowData.modifications.bomOperation1 !== "") 
{
  if (rowData.differences.bomOperation1 &#x26;&#x26; rowData.differences.bomOperation1 !== "") 
  {
    existingOperationIds[0].id = JSON.parse(rowData.differences.bomOperations)[0]?.id || 0;
    existingOperationIds[0].operationId = rowData.modifications.bomOperation1;
  }
  if (rowData.modifications.bomOperation1 &#x26;&#x26; rowData.modifications.bomOperation1 !== "") 
  {
    existingOperationIds[0].id = JSON.parse(rowData.differences.bomOperations)[0]?.id || 0;
    existingOperationIds[0].operationId = rowData.modifications.bomOperation1;
  }
  bomOperations = bomOperations.concat(rowData.modifications.bomOperation1);
} 
else 
  return updateValue;
  
/* bopOperation2 */
if (rowData.modifications.bomOperation2 !== "") 
{
  if (rowData.differences.bomOperation2 &#x26;&#x26; rowData.differences.bomOperation2 !== "") {
    existingOperationIds[1].id = JSON.parse(rowData.differences.bomOperations)[1]?.id || 0;
    existingOperationIds[1].operationId = rowData.modifications.bomOperation2;
  }
  
  if (rowData.modifications.bomOperation2 &#x26;&#x26; rowData.modifications.bomOperation2 !== "") {
    existingOperationIds[1].id = JSON.parse(rowData.differences.bomOperations)[1]?.id || 0;
    existingOperationIds[1].operationId = rowData.modifications.bomOperation2;
  }
  
  bomOperations = bomOperations.concat(rowData.modifications.bomOperation2);
}

const pmObjectListItems = JSON.parse(pm.objectListItems);

for (let i = 0; i &#x3C; bomOperations.length; i++) 
{
  const bomOp = bomOperations[i];  
  pmObjectListItems.forEach((item) => {
    if (item.workCenterId === bomOp) {
      item.value.id = JSON.parse(rowData.differences.bomOperations)[i]?.id || 0;
      item.value.id = JSON.parse(rowData.differences.bomOperations)[i]?.id || 0;
      updateValue.push(item.value);
    }
  });
}

return updateValue;

</code></pre></td></tr></tbody></table>

#### What this rule does

For the mapped BOM operations, get the id of the existing BOM operation line. Use this to determine if the BOM operation should be added or updated

<table><thead><tr><th width="235">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Type</td><td>export</td></tr><tr><td>Name</td><td><code>Export Manipulation</code></td></tr><tr><td>Process for {Primary}</td><td>false</td></tr><tr><td>Process for Odoo</td><td>true</td></tr><tr><td>JavaScript expression</td><td><pre class="language-javascript"><code class="lang-javascript">if (rowData.modifications.bomOperations &#x26;&#x26; rowData.modifications.bomOperations.length == 0)
  delete s["bomOperations"];

return s;

</code></pre></td></tr></tbody></table>

#### What this rule does

If there is no value for the `bomOperations` value, then remove it, clearing the operations from Odoo
