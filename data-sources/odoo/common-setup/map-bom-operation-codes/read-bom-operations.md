---
icon: magnifying-glass
---

# Read BOM Operations

{% hint style="danger" %}
This page is a Work in Progress and not completed yet, please check back again soon
{% endhint %}

BOM operations are _individual_ operations that are performed on the item. There are 2 scenarios, reading and writing. BOM operations are never repeated. Even if you see duplicates in Odoo, they each have their own underlying unique _operationId for that line in the BOM._

BOM operations can often include one or more of the following items in the table&#x20;

| Destructive operation | Non-Destructive operation |
| --------------------- | ------------------------- |
| Punching              | Painting                  |
| Welding               | Assembly                  |
| Galvanizing           | Powder Coating            |
| etc                   | Wiring                    |



To read these operations from the BOM, we map to the Odoo property&#x20;

> mrp.bom.operation\_id

{% hint style="info" %}
When writing a value to Odoo, you have to provide the full spec of the object in the form.

<pre class="language-json5"><code class="lang-json5"><strong>"value":  
</strong><strong>{ 
</strong><strong>  "sequence": 30,
</strong><strong>  "name": "Punch",
</strong><strong>  "workcenter_id": 3,
</strong><strong>  "time_mode": "manual",
</strong><strong>  "time_mode_batch": 10,
</strong><strong>  "time_cycle_manual": 60
</strong><strong>} 
</strong>           
</code></pre>

This is shown the example below
{% endhint %}

Create a new [Property Mapping](../../../../fundamentals/property-mappings/) with the following settings:

<table><thead><tr><th width="301">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Property Name / Header</td><td>BOM Operations</td></tr><tr><td>Accessor</td><td>bomOperations</td></tr><tr><td>Primary Property</td><td>(Unmapped)</td></tr><tr><td>Secondary Property</td><td><code>mrp.bom.operation_ids</code></td></tr><tr><td>Update Odoo on Submit</td><td>true</td></tr><tr><td>Rendering Type</td><td><code>Advanced Multi Select List</code></td></tr><tr><td>List Display Selector</td><td>displayName</td></tr><tr><td>List Value Selector</td><td>id</td></tr><tr><td>List items</td><td><p> Here is a sample of what you can insert, but it can be anything, whatever you use most frequently [more on this below]**</p><pre class="language-json5"><code class="lang-json5">[
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

\*\* When creating this list, and you only want to read the values, you'll want to specify at least the Work center id, and the name of the operation. If you also want to write the values back to Odoo, then you'll have to specify the entire object required to add new operations and the default settings for the operation. Adding new work centers are not supported at the moment.&#x20;

Some default BOM operation properties that are available are listed below. The list is not exhaustive and you may customize your own (See the Odoo Configuration options > Models > `mrp.routing.workcenter`):

<table><thead><tr><th>Property</th><th>Type</th><th>Availability</th><th data-type="checkbox">Required for reading</th></tr></thead><tbody><tr><td>active</td><td>bool</td><td>16, 17, 18</td><td>false</td></tr><tr><td>name</td><td>string</td><td>16, 17, 18</td><td>true</td></tr><tr><td>bom_id</td><td>integer</td><td>16, 17, 18</td><td>false</td></tr><tr><td>workcenter_id</td><td>integer</td><td>16,17,18</td><td>true</td></tr><tr><td>time_mode_batch</td><td>integer</td><td>16,17,18</td><td>false</td></tr><tr><td>time_cycle_manual</td><td>integer</td><td>16,17,18</td><td>false</td></tr><tr><td>note</td><td>string</td><td>16,17,18</td><td>false</td></tr><tr><td>sequence</td><td>integer</td><td>16,17,18</td><td>false</td></tr></tbody></table>

After adding the new Property Mapping, add an import rule

<table><thead><tr><th width="235">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Type</td><td>import</td></tr><tr><td>Name</td><td><code>Text Manipulation</code></td></tr><tr><td>Process for {Primary}</td><td>false</td></tr><tr><td>Process for Odoo</td><td>true</td></tr><tr><td>JavaScript expression</td><td>// TODO</td></tr></tbody></table>
