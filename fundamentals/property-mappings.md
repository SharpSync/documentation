---
description: Property mappings are 1:1 or 1:N mappings from the source to the destination.
icon: list-tree
---

# Property Mappings

Property Mappings define the BOM columns displayed in SharpSync. You can think of them as the mapping of meta data from one source to another source, or as the mapping of custom properties from one source to another source.

For each mapping you create, a column will be displayed on the screen. Property mappings are expanded upon in detail here.

After setting up your Property Mappings and Rules, open one of the Primary [Data Sources](data-sources.md) to export the Bill Of Materials (BOM) to SharpSync. For more details on exporting a BOM, see the "Export Assembly Or Part" section for each Data Source.

After setting up your Property Mappings and Rules, open one of the Primary [Data Sources](https://sharpsync.gitbook.io/sharpsync/~/changes/zcOMaRcqMhiRElzWzdJZ/fundamentals/data-sources) to export the Bill Of Materials (BOM) to SharpSync. For more details on exporting a BOM, see the "Export Assembly Or Part" section for each Data Source.

<table data-view="cards"><thead><tr><th></th><th data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td>Property Mapping Settings</td><td><a href="../property-mappings/property-mapping-settings/">property-mapping-settings</a></td><td><a href="../.gitbook/assets/icon_property_mapping.png">icon_property_mapping.png</a></td></tr><tr><td>Rule Template Settings</td><td><a href="../property-mappings/rule-templates/">rule-templates</a></td><td><a href="../.gitbook/assets/icon_rule_mapping.png">icon_rule_mapping.png</a></td></tr></tbody></table>

### Property Mappings Table <a href="#default-property-mappings" id="default-property-mappings"></a>

The following Property Mappings columns are visible by default:

<table><thead><tr><th width="248">Name</th><th>Description</th></tr></thead><tbody><tr><td>Order</td><td>The order in which the columns appear in the Bill of Materials (double-click the cell to change the order)</td></tr><tr><td>Property Name</td><td>The name of the column as it appears in the Bill of Materials</td></tr><tr><td>Accessor</td><td>The unique internal name as it is used in SharpSync</td></tr><tr><td>Primary Data Source  Property</td><td>The property as it is known at the source</td></tr><tr><td>Primary Data Source Type</td><td>The approximated type as it is known at the source</td></tr><tr><td>Update Primary Data Source</td><td>When checked, the source will be updated</td></tr><tr><td>Secondary Data Source Property</td><td>The property as it is known at the source</td></tr><tr><td>Secondary Data Source Type</td><td>The approximated type as it is known at the source</td></tr><tr><td>Update Secondary Data Source</td><td>When checked, the source will be updated</td></tr><tr><td>Visible</td><td>Toggle Visibility Status</td></tr><tr><td>Read Only</td><td>Toggle Read Only Status</td></tr><tr><td>Settings</td><td>Click to edit the settings</td></tr><tr><td>Delete</td><td>Click to delete Property Mapping (Cannot Undo)</td></tr></tbody></table>

Depending on your screen size, columns related to your Primary Data Source and Secondary Data Source might not all fit in the table. Use the scrollbar at the bottom, as shown in the screenshot below, to scroll other property mappings into view.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

\*\* Data Source 2 refers to the [Secondary Data Source](data-sources.md) that you had added.



{% hint style="info" %}
Important: You have to add at least 2 Data Sources before being able to successfully add Property Mappings. If any of your sources have changes applied (new properties added), come back to this screen and click the `UPDATE`  button before you can select the new property.
{% endhint %}

