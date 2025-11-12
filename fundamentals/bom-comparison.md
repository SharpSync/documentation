---
description: >-
  The view where all data is displayed overlayed on top of one another on
  screen.
icon: table-list
---

# BOM Comparison

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

The BOM comparison screen is where you will ultimately spend most of your time

### BOM row updates

BOM row updates can occur in the primary or secondary Data Source and is influenced by your settings for:

* Property Mappings
* Rules

BOM row updates are also influenced by whether the `PROCESS` checkbox is checked or unchecked. You can use the context menu of any given row's `PROCESS` cell to bulk check/uncheck the `PROCESS` checkbox of related rows and children rows.

#### Primary Source updates

When updating BOM rows the following table illustrates how updates occur at the primary source.

A primary source is typically a CAD or PDM or PLM system. For primary sources, quantity values are typically not updated.

Using the default color scheme for BOM comparison, you can expect the following results:

<table data-full-width="false"><thead><tr><th align="center">Process Row</th><th>Row Background</th><th align="center">Item Creation</th><th width="125">Bom Structure</th><th width="126" align="center">Quantity</th><th width="159" align="center">Item Properties</th><th align="center">Routings</th><th align="center">Derivatives</th></tr></thead><tbody><tr><td align="center">☑️</td><td>⬜ White</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Updated</td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td align="center">☑️</td><td>🟩 Green</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Updated</td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td align="center">☑️</td><td>🟨 Yellow</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Updated</td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td align="center">☑️</td><td>🟥 Red</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Ignored</td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td align="center"></td><td>⬜ White</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Ignored</td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td align="center"></td><td>🟩 Green</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Ignored</td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td align="center"></td><td>🟨 Yellow</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Ignored</td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td align="center"></td><td>🟥 Red</td><td align="center">N/A</td><td>Unchanged</td><td align="center">Unchanged</td><td align="center">Ignored</td><td align="center">N/A</td><td align="center">N/A</td></tr></tbody></table>

#### Secondary Source updates

When updating BOM rows the following table illustrates how updates occur at the secondary source.

A secondary source is typically an ERP or PLM system. For secondary sources, quantity values are typically updated to reflect the quantities of the primary source.

Using the default color scheme for BOM comparison, you can expect the following results:

<table data-full-width="false"><thead><tr><th width="113.8671875" align="center">Process Row</th><th width="141.5625">Row Background</th><th width="113.69921875" align="center">Item Creation</th><th width="338.8203125">Bom Structure</th><th width="104.38671875" align="center">Quantity</th><th width="114.1015625" align="center">Item Properties</th><th width="100.59765625" align="center">Routings</th><th width="108.87109375" align="center">Derivatives</th></tr></thead><tbody><tr><td align="center">☑️</td><td>⬜ White</td><td align="center"></td><td>Unchanged</td><td align="center">Updated</td><td align="center">Updated</td><td align="center">Updated</td><td align="center">Processed</td></tr><tr><td align="center">☑️</td><td>🟩 Green</td><td align="center">Created</td><td>Linked To Parent</td><td align="center">Updated</td><td align="center">Updated</td><td align="center">Updated</td><td align="center">Processed</td></tr><tr><td align="center">☑️</td><td>🟨 Yellow</td><td align="center"></td><td>Linked To Parent</td><td align="center">Updated</td><td align="center">Updated</td><td align="center">Updated</td><td align="center">Processed</td></tr><tr><td align="center">☑️</td><td>🟥 Red</td><td align="center"></td><td>Unlinked From Parent + Children Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td></tr><tr><td align="center"></td><td>⬜ White</td><td align="center"></td><td>Unchanged</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td></tr><tr><td align="center"></td><td>🟩 Green</td><td align="center">Not Created</td><td>Not Linked To Parent</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td></tr><tr><td align="center"></td><td>🟨 Yellow</td><td align="center"></td><td>Not Linked To Parent</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td></tr><tr><td align="center"></td><td>🟥 Red</td><td align="center"></td><td>Link To Parent Kept + Children Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td><td align="center">Ignored</td></tr></tbody></table>

### BOM Comparison Legend

<table data-full-width="false"><thead><tr><th>Color scheme</th><th>Description</th></tr></thead><tbody><tr><td><img src="../.gitbook/assets/image (20).png" alt="" data-size="original"></td><td>Blue border - DS1 and DS2 values are different. Without user modifications</td></tr><tr><td><img src="../.gitbook/assets/image (11).png" alt="" data-size="original"></td><td>Blue &#x26; green border - DS1 and DS2 values are different. With user modifications</td></tr><tr><td><img src="../.gitbook/assets/image (13).png" alt="" data-size="original"></td><td>Green border - DS1 and DS2 values are identical. With user modifications</td></tr><tr><td><img src="../.gitbook/assets/image (14).png" alt="" data-size="original"></td><td>Orange border - Value did not pass display mapping rule (Warning)</td></tr><tr><td><img src="../.gitbook/assets/image (15).png" alt="" data-size="original"></td><td>Red border - Value did not pass display mapping rule (Error)</td></tr><tr><td><img src="../.gitbook/assets/image (16).png" alt="" data-size="original"></td><td>Red background - Component is missing from the DS1 BOM. Will be unlinked from the DS2 BOM</td></tr><tr><td><img src="../.gitbook/assets/image (17).png" alt="" data-size="original"></td><td>Orange background - Component is present in DS2. Will be linked to the corresponding DS2 BOM</td></tr><tr><td><img src="../.gitbook/assets/image (18).png" alt="" data-size="original"></td><td>Green background - Component is missing from DS2. Will be added to DS2 and linked to the corresponding BOM</td></tr><tr><td><img src="../.gitbook/assets/image (19).png" alt="" data-size="original"></td><td>Blue background- Row line type is drawing used to configure drawing types derivatives</td></tr></tbody></table>

If you ever find yourself in the need to review this information, click the `Legend` button at the bottom of the BOM Comparison screen

{% hint style="success" %}
These colors are modifiable under then User Settings
{% endhint %}

<figure><img src="../.gitbook/assets/bom_legend (1).png" alt=""><figcaption></figcaption></figure>

### BOM Submittal Statuses

When submitting a Bill of Materials (BOM), the BOM runs through a number of stages. You can hover over the dot of the BOM to understand it's status before submittal and after submittal.

<figure><img src="../.gitbook/assets/image (2).png" alt="Hovering over a dot will show the status"><figcaption><p>Hovering over a dot will show the status</p></figcaption></figure>

Below is a table listing common statuses

<table><thead><tr><th width="120">Dot</th><th width="122">Dot color</th><th>Explanation</th></tr></thead><tbody><tr><td><div><figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure></div></td><td>Light Gray</td><td>Bill of Materials has been created, but not submitted. It may or may not have been loaded</td></tr><tr><td><div><figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure></div></td><td>Yellow</td><td>Bill of Materials has been submitted, but it is being processed. The possible processing states depends on whether it is a Primary or Secondary source. A Primary Source may generate derivatives first before the data is ready for the Secondary Source to process**</td></tr><tr><td><div><figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure></div></td><td>Green</td><td>The Bill of Materials has successfully completed all specified operations.</td></tr><tr><td><div><figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure></div></td><td>Orange</td><td>The Bill of Materials completed with warnings. These are typically warnings that you may ignore, but you should have a look to see what happened. This can happen if you send invalid values to a source (e.g. text instead of a number)</td></tr><tr><td><div><figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure></div></td><td>Red</td><td>The Bill of Materials completed with errors. This usually means there was something that happened that prevented the BOM from completing fully. You should look at the errors to see what happened.  </td></tr></tbody></table>

\*\* When a Bill of Materials is submitted, there are sub-process which may run which lengthens the time to process the BOM. You can close your tab and come back later, or wait for the toast notification to show when it's done (the color will change)

Below is a decision tree on how the processing takes place, and this will help you form an understanding of why a Bill of Materials takes time to complete

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>



