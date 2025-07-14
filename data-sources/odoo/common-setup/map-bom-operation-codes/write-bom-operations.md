---
icon: magnifying-glass
---

# Write BOM Operations

{% hint style="danger" %}
This page is a Work in Progress and not completed yet, please check back again soon
{% endhint %}

{% hint style="success" %}
**Summary of article's major steps**

* Add an export rule to format the object and make it ready for the Odoo BOM object
* Mark the property Mapping as Manufacturing Step
* Each object in the list is sent to Odoo
{% endhint %}

BOM operations are _individual_ operations that are performed on the item.  This page follows on the progress from the previous topic [read-bom-operations.md](read-bom-operations.md "mention")

{% hint style="info" %}
The best way to manage operations is to be consistent in your naming convention for your operations.&#x20;

In other words, if you are going to assemble something, always give it the exact same name (e.g. `Assembly` or `Assemble`).
{% endhint %}

To write these operations to the Odoo BOM, we map to the Odoo property&#x20;

> mrp.bom.operation\_id

The first step is to edit the Property Mapping created in the previous article.

| Property Mapping Setting | Old value | New value                                                  |
| ------------------------ | --------- | ---------------------------------------------------------- |
| Is Manufacturing Step    | false     | <code class="expression">space.vars.bool_value_true</code> |
| Update Odoo On Submit?   | false     | true                                                       |

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
</strong></code></pre>
{% endhint %}

After adding the new Property Mapping, add an export rule in addition to the import rule from the previous topic [read-bom-operations.md](read-bom-operations.md "mention")

<table><thead><tr><th width="235">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Type</td><td>export</td></tr><tr><td>Name</td><td><code>Text Manipulation</code></td></tr><tr><td>Process for {Primary}</td><td>false</td></tr><tr><td>Process for Odoo</td><td>true</td></tr><tr><td>JavaScript expression</td><td><pre class="language-javascript"><code class="lang-javascript">return s
  .map(
    (workCenterId) => JSON.parse(pm.objectListItems).find((item) => item.workCenterId === workCenterId)?.value || null
  )
  .filter((n) => n != null);
</code></pre></td></tr></tbody></table>

#### What this rule does

We specified a list of standard items in the Property Mapping settings.

This list is the list that we want to pick from, so that we can add this to the BOM operations
