---
icon: paperclip
---

# Derivative Files and Links

SharpSync writes your CAD derivatives to the Business Central **item** they belong to, in the item's **Attachments** FactBox. How derivatives are set up in SharpSync is explained in [Derivatives](../../advanced/derivatives.md). This page explains where they end up in Business Central, and what each sync changes.

* A derivative with **Store File** checked becomes a file under **Documents**.
* A derivative with **Store Url** checked becomes a link under **Links**.
* A derivative with both checked gets both.

You do not select a property mapping for Business Central derivatives: the destination is always the item's Attachments FactBox.

{% hint style="info" %}
Links require version **2.1.0.0** or later of the SharpSync extension in Business Central. Files work with every version.
{% endhint %}

## Which item receives what

* A part or assembly row's derivatives go to its own item.
* A drawing row has no item of its own, so its derivatives (for example a PDF of the drawing) go to the item of the part the drawing documents: its direct parent in the BOM.
* A phantom (`Production BOM`) row and a resource row have no item, and Business Central only takes attachments on items through its API, so their derivatives are skipped with a warning.
* A part used several times in the BOM gets its derivatives once per sync.
* For a `DRAWING` derivative, the link opens the drawing itself in the CAD system.

## Names, revisions and re-syncs

SharpSync recognizes a file or link by its name, **extension included**, ignoring upper and lower case. So `PART_A.pdf` and `PART_A.step` are two different files. The name comes from the derivative template's naming pattern, which defaults to `{rowData.componentName}_{rowData.cells.revision}`.

* **A file with the same name already exists:** its content is replaced in place. Its _Attached Date_ moves to the time of the sync, and anything you set on it in Business Central, such as _Flow to Production Trx_, is kept.
* **A link with the same name already exists:** its URL is updated in place when it changed. An unchanged link is left alone.
* **The name is new**, typically because the revision changed: a new file or link is added, and the previous revision's file or link stays on the item.
* SharpSync never renames or deletes a file or link.

Your naming pattern therefore decides what Business Central keeps:

| Naming pattern                                         | Result on the item                                       |
| ------------------------------------------------------ | -------------------------------------------------------- |
| Includes the revision (the default)                    | One file per revision, so the item keeps the history     |
| Leaves the revision out, e.g. `{rowData.componentName}` | One file per derivative type, always the current one     |

If an item already holds several files or links with the same name, added by hand or by another tool, SharpSync refreshes the most recently added one, leaves the others alone, and shows a warning.

## Getting drawings onto production orders

Business Central copies an item's files onto new production order lines when **Flow to Production Trx** is ticked on the file. You find it in the item's Attachments FactBox: **Documents**, then **Show details**. SharpSync cannot tick this flag, because Business Central does not expose it through its API, but it never clears it either: tick it once, and later syncs keep it.

* Production orders created before a sync keep the copy they were given. Only new production orders get the updated file.
* With the revision in the naming pattern, an older revision stays ticked until you untick it, so new production orders would receive both revisions. If you use this flag, consider a naming pattern without the revision for the files meant for the shop floor.

## Viewing the files

* PDF files open in Business Central's built-in viewer. Other file types, such as STEP, DXF or DWG, are downloaded when you select them.
* A single file can be up to 350 MB.
* Files are stored in your Business Central database and count against your environment's storage capacity. From Business Central 2026 release wave 1 (version 28), an administrator can keep attachments in SharePoint or Azure storage instead. See Microsoft's [External file storage for document attachments](https://learn.microsoft.com/en-us/dynamics365/business-central/across-store-document-attachments-externally).
