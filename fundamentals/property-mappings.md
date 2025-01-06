---
icon: list-tree
description: Property mappings are 1:1 or 1:N mappings from the source to the destination.
---

# Property Mappings

Property Mappings defines the BOM columns displayed in SharpSync. You can think of them as the mapping of meta data from one source to another source, or as the mapping of custom properties from one source to another source.

For each mapping you create, a column will be displayed on the screen. Property mappings are expanded upon in detail here.

After setting up your Property Mappings and Rules, open one of the Primary [Data Sources](https://sharpsync.gitbook.io/sharpsync/~/changes/zcOMaRcqMhiRElzWzdJZ/fundamentals/data-sources) to export the Bill Of Materials (BOM) to SharpSync. For more details on exporting a BOM, see the "Export Assembly Or Part" section for each Data Source.

<table data-view="cards"><thead><tr><th></th><th data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td>Property Mapping Settings</td><td><a href="../property-mappings/settings.md">settings.md</a></td><td><a href="../.gitbook/assets/automated-data.svg">automated-data.svg</a></td></tr><tr><td>Rule Template Settings</td><td><a href="../property-mappings/rule-templates/">rule-templates</a></td><td><a href="../.gitbook/assets/automated-data.svg">automated-data.svg</a></td></tr></tbody></table>

#### &#x20;<a href="#default-property-mappings" id="default-property-mappings"></a>

#### Property Mappings Table <a href="#default-property-mappings" id="default-property-mappings"></a>

The following Property Mappings columns are available:

| Name                         | Description                                                                                                |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------- |
| Order                        | The order in which the columns appear in the Bill of Materials (double-click the cell to change the order) |
| Property Name                | The name of the column as it appears in the Bill of Materials                                              |
| Accessor                     | The unique internal name as it is used in SharpSync                                                        |
| \*{Data Source 1} Property   | The property as it is known at the source                                                                  |
| \*{Data Source 1} Type       | The approximated type as it is known at the source                                                         |
| Update \*{Data Source 1}     | When checked, the source will be updated                                                                   |
| \*\*{Data Source 2} Property | The property as it is known at the source                                                                  |
| \*\*{Data Source 2} Type     | The approximated type as it is known at the source                                                         |
| Update \*\*{Data Source 2}   | When checked, the source will be updated                                                                   |
| Visible                      | Toggle Visibility Status                                                                                   |
| Read Only                    | Toggle Read Only Status                                                                                    |
| Settings                     | Click to edit the settings                                                                                 |
| Delete                       | Click to delete Property Mapping (Cannot Undo)                                                             |

Depending on your screen size, columns related to \*{Data Source 1} and \*\*{Data Source 2} might not all fit in the table. Use the scrollbar at the bottom, as shown in the screenshot below, to scroll other property mappings into view.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

\* Data Source 1 refers to the Primary Data Source that you had created.

\*\* Data Source 2 refers to the Secondary Data Source that you had created.



Data related to \*\*{Data Source 2}
