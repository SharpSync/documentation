---
icon: magnifying-glass
---

# Read BOM Operations

{% hint style="danger" %}
This page is a work in progress and not completed yet
{% endhint %}

Create a new [Property Mapping](../../../../fundamentals/property-mappings/) with the following settings:

<table><thead><tr><th width="301">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Property Name / Header</td><td>BOM Operations</td></tr><tr><td>Accessor</td><td>bomOperations</td></tr><tr><td>Primary Property</td><td>(Unmapped)</td></tr><tr><td>Secondary Property</td><td>mrp.bom.operation_ids</td></tr><tr><td>Update Odoo on Submit</td><td><p>true</p><p></p></td></tr><tr><td>List Name</td><td>mrp.workcenter</td></tr><tr><td>List Value Selector</td><td>{id}:{resource_id[1]}</td></tr><tr><td>Rendering Type</td><td>Advanced Multi Select List</td></tr><tr><td>List Display Selector</td><td>name</td></tr><tr><td>List Value Selector</td><td>id</td></tr><tr><td>List items</td><td> // TODO</td></tr><tr><td>Enabled</td><td>true</td></tr><tr><td>Prefer Odoo Value</td><td>true</td></tr></tbody></table>

After adding the new Property Mapping, add an import rule

<table><thead><tr><th width="235">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Type</td><td>import</td></tr><tr><td>Name</td><td>Text Manipulation</td></tr><tr><td>Process for {Primary}</td><td>false</td></tr><tr><td>Process for Odoo</td><td>true</td></tr><tr><td>JavaScript expression</td><td>// TODO</td></tr></tbody></table>
