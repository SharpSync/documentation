---
icon: route
---

# Routings

In [production BOM mode](configure-bom-mode.md), SharpSync can **assign** each item its routing, and **show** the routing's fields on the item's row in the comparison grid. It does not create routings or change their operations: those stay in Business Central, where production engineering maintains them.

{% hint style="info" %}
Routings are part of Business Central's **Manufacturing** functionality, which requires the **Premium** licence. Routing fields need version **2.2.0.0** or later of the SharpSync extension.
{% endhint %}

| What                                                          | Load | Sync                                  |
| ------------------------------------------------------------- | :--: | ------------------------------------- |
| Which routing an item uses (the item's _Routing No._)         |  ✅  | ✅ to a routing that already exists    |
| The routing's own fields (header and version), e.g. Description |  ✅  | Display only                          |
| The routing's operations                                      |  —   | —                                     |

## Assign a routing to an item

Map the item field **Routing No.** (Json `routingNumber`) and give it a list of the routings in Business Central:

| Setting                      | Value                                                                         |
| ---------------------------- | ----------------------------------------------------------------------------- |
| Property Name                | Routing No. (or any name you like)                                            |
| Primary accessor             | (Unmapped)                                                                    |
| Secondary accessor           | Routing No. (Item Field)                                                      |
| List Name                    | `No.` (or `Description` to pick by description)                               |
| Prefer Dynamics365 Value     | checked                                                                       |
| Update Dynamics365 on Submit | checked                                                                       |

Click _Refresh List Values_ to fetch the routing numbers. See [List Names For nestedObject Mappings](configure-mappings/list-names-for-nestedobject-mappings.md). Users then pick a routing for an item from the dropdown, and the sync writes it to the item.

* **The routing must already exist in Business Central.** Business Central refuses a routing number it doesn't have, so create new routings in Business Central first.
* **The item must be of type `Inventory`.** Business Central refuses a routing on any other item type.
* **A blank Routing No. is never sent.** SharpSync never removes a routing from an item, whether the cell was cleared or simply left empty. To remove an item's routing, clear it in Business Central.
* **Why _Prefer Dynamics365 Value_:** CAD files rarely carry a routing number. With the setting checked, each row starts from the routing the item already has in Business Central, so the column shows what is there and an untouched row changes nothing.
* **After changing an item's routing, reload the BOM** to see the new routing's fields. Until then, the row still shows the fields of the routing it was loaded with.

{% hint style="info" %}
**Deriving the routing from CAD.** If your parts carry a property that decides their routing, such as a process or material, a rule on the mapping can turn it into a routing number, for example every laser-cut part to `R-LASER`. Map that property as the primary accessor and leave _Prefer Dynamics365 Value_ unchecked, so the derived value wins. A [Text Maximum Length](../../fundamentals/rules/display/text-maximum-length.md) rule of 20 catches a number Business Central can't store before you submit.
{% endhint %}

## Show the routing's fields

After a _Refresh Secondary Accessors_ in production BOM mode, the secondary accessors include the routing's header fields, named like `Description (Routing Header Field)`, and its version fields, named like `Starting Date (Routing Version Field)`. Map any of them for display:

| Setting                      | Value         |
| ---------------------------- | ------------- |
| Prefer Dynamics365 Value     | checked       |
| Update Dynamics365 on Submit | **unchecked** |

* Each item row shows the fields of its own routing. A part with no routing, and a phantom row, show nothing.
* **The version shown is the one in effect today:** the latest version that isn't `Closed` and starts today or earlier. `Under Development` and `New` versions count. When no version applies, Business Central uses the routing's own lines, and the version fields are blank.
* Business Central's routing card shows only a `Certified` version as its _Active Version_, so it can name a different version than SharpSync until that version is certified.
* An item whose Routing No. names a routing that no longer exists gets a load warning, and its routing fields stay blank.

These fields are **display only**. A routing column with _Update Dynamics365 on Submit_ checked gets a warning at sync, and its value is not sent. The rest of the row is still synced. The message reads:

> _'Description (Routing Header Field)' is display-only: SharpSync assigns an item's routing through its Routing No. but does not change the routing itself, so this value was not sent to Dynamics365. Untick 'Update Dynamics365' on that mapping to keep displaying the field without this warning._

## Not supported

* Creating routings, and changing a routing's operations, work centers, times or status. Maintain these in Business Central.
* A routing version per CAD revision.
* Resource rows in production BOM mode. See [Business Central Sync Limitations](business-central-sync-limitations.md).
