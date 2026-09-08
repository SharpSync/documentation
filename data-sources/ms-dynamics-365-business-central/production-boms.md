---
icon: industry
---

# Production BOMs

SharpSync can sync your CAD structure to Business Central as either an **Assembly BOM** or a **Production BOM**. You choose which one in the data source configuration, with the setting [_BOM type to sync_](configure-bom-mode.md#bom-type-to-sync). This page explains how SharpSync works with production BOMs, and what you should expect to see in Business Central after a sync.

{% hint style="info" %}
Production BOMs are part of Business Central's **Manufacturing** functionality, which requires the **Premium** licence. Assembly BOMs work on Essentials.
{% endhint %}

## Which BOM SharpSync reads and writes

A production BOM in Business Central is a **header** (the BOM itself, with its own number and lines) that can optionally hold several **versions**, each with a starting date and a status. An item points at one header through its _Production BOM No._, and Business Central decides at build time which version applies: the latest version whose starting date is not in the future, or the header's own lines when no version qualifies.

SharpSync follows exactly this chain. When you load a BOM, each item's rows come from the production BOM Business Central would build **today**, and when you submit, SharpSync writes back to that same header or version. Once a BOM has a qualifying version, Business Central ignores the header's own lines, so SharpSync never writes to them either: writing there would succeed and change nothing on the shop floor. You can verify what SharpSync is looking at with one click in Business Central: open the item card and use **Prod. Active BOM Version**.

Two details are worth knowing:

* SharpSync always syncs the **item-level** production BOM. Location- or variant-specific BOMs set up through Stockkeeping Units are never read or written.
* SharpSync resolves against today's date (in UTC), not against a work date a user may have set in their Business Central session. This is the same BOM that production orders, planning and costing use.

## Certification

Business Central only builds from a **Certified** BOM. Writing to one means opening it first, and the setting [_Certify BOMs automatically after syncing them_](configure-bom-mode.md#certify-boms-automatically-after-syncing-them) decides what SharpSync does once it has finished writing. The rule behind it is simple: **the setting applies to BOMs that SharpSync itself opened or created. It never certifies a BOM that was already open when the sync found it.**

| Status found in Business Central | What a sync does                                                                                                                                                                                                                                        |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Certified`                      | SharpSync sets it to `Under Development`, writes the changes, and then closes it again. With the setting **on** it is **re-certified**. With the setting **off** it is left `Under Development` for someone to review and certify in Business Central. |
| `Under Development`              | Already open, so SharpSync writes the changes and leaves the status exactly as it found it. **The setting does not certify it**, even when on: someone put this BOM under review on purpose, and only they should decide when it is ready.               |
| `New`                            | Same as `Under Development` when the BOM already existed. A BOM that SharpSync **creates** during the sync starts as `New`, and that one does follow the setting: certified at the end of the sync when on, left `New` when off.                        |
| `Closed`                         | Never selected. A closed version is skipped, and a closed version that a revision points to fails the row (see below).                                                                                                                                  |

{% hint style="warning" %}
With the certification setting **off** (the default), a sync leaves the BOM `Under Development`. Business Central then keeps building from the **previously certified** structure until someone reviews and certifies the new one. This is the intended engineering-change workflow, but it means a successful sync is not yet a change on the shop floor.
{% endhint %}

A BOM whose lines did not all write successfully is always left `Under Development` (or `New` if SharpSync created it), whatever the certification setting says. Certification only ever applies to a structure that was written in full. This is decided per BOM: a sub-assembly that failed does not stop its parent from being certified, because Business Central's certification does not look into item sub-assemblies.

## Seeing what you loaded

The comparison grid does not show a BOM's status by itself. To see which state was loaded, map the fields **Status** and **Version Code** from the production BOM header and version accessors. Both are display-only: leave _Update MS Dynamics 365 On Submit_ **unchecked** on those mappings, or the rows will fail. See [Read-Only Fields](configure-mappings/read-only-fields.md).

## One version per CAD revision

By default, a sync updates the production BOM that Business Central builds today, in place. If you turn on [_Create a new BOM version for each revision_](configure-bom-mode.md#create-a-new-bom-version-for-each-revision), SharpSync keeps one **production BOM version per CAD revision** instead. The version code comes from your version naming scheme, set in [_Use this scheme to name new BOM versions_](configure-bom-mode.md#use-this-scheme-to-name-new-bom-versions), which defaults to:

```
{rowData.componentName}_{rowData.cells.revision}
```

For this to work, the revision must reach SharpSync as a property of the CAD row: add a property mapping whose accessor is exactly `revision`, mapped to the revision property of your CAD system. A revision produced only by a display rule is not visible when the BOM is loaded, so the version cannot be resolved from it.

With versioning on, this is what happens:

* **The comparison shows the revision's version**, whatever its starting date, as long as it exists and is not `Closed`. Submitting edits that version in place.
* **When the revision's version does not exist yet**, the load shows the BOM Business Central builds today and attaches a warning to the row saying so. Submitting creates the new version from what you see in the comparison, with your changes applied on top. It starts on the sync date unless you map _Starting Date_.
* **Re-syncing the same revision** updates its version in place. It never creates a second one.
* **A version's starting date is set once**, when it is created. Later syncs do not move it, unless the field is mapped, because moving it changes which version Business Central picks for backdated and in-flight orders.
* **Older versions are never closed.** Business Central's date ordering supersedes them naturally, and closing them would remove them from resolution for orders that still depend on them. Retiring a version stays a manual action in Business Central.
* **A revision whose version is `Closed`** fails the row. Reopen or rename the version in Business Central first.
* **Comment lines are not carried** into a version SharpSync creates.

### Turning the option on or off

The option only decides whether SharpSync **creates** versions. It does not change how the BOM to edit is chosen once versions exist, and it never deletes or closes anything.

* **Turning it on** starts creating one version per CAD revision from the next sync, for revisions that do not have one yet. Existing versions are kept and edited when their code matches.
* **Turning it off** stops creating versions. Each sync goes back to editing the BOM Business Central builds today, which is the latest qualifying version if the BOM has any, or the header's own lines if it has none. Versions created earlier stay in place and keep resolving by date.

Because nothing is removed either way, you can switch the option in both directions at any time without cleaning up in Business Central.

{% hint style="warning" %}
Keep the revision in the version naming scheme. If the scheme produces the same code for every revision, every sync targets the same version and versioning gives you nothing. A blank revision is not an error: the scheme is applied with an empty value, so with the default scheme the code ends in an underscore, such as `SSY-A1_`.
{% endhint %}

## Phantom BOMs

A production BOM line can point at another production BOM instead of an item. Business Central calls this line type `Production BOM`, and it is commonly used for **phantoms**: a bundle of components that is consumed by several parent BOMs without being an item itself.

SharpSync shows a phantom as **its own row, with the phantom's components beneath it**, exactly like a sub-assembly. The row is identified by the **production BOM number** of the phantom, not by an item number, so a CAD sub-assembly must carry that number as its component name to line up with it. Where it does not, the phantom shows as existing in Business Central only. It can be unlinked like any other row, which removes that one line and leaves the phantom's own BOM untouched.

Whether a row is written as an item or as a phantom is decided by the row's **item type** value, which must be `Item` or `Production BOM`. See [Configure Item Type Mapping](configure-mappings/configure-item-type-mapping.md).

{% hint style="warning" %}
**A phantom is shared.** Its components belong to the phantom's own production BOM, not to the BOM you are syncing. Editing them from one parent changes every BOM that uses the phantom. When one sync reaches the same phantom from two parents, the phantom is created once and its components are the union of both, with the last write winning on field values. This is the same behaviour a shared sub-assembly already has.
{% endhint %}

When a row typed `Production BOM` does not exist in Business Central yet, SharpSync creates the phantom under the row's own component name, together with its components. Because a phantom has no item to take defaults from:

* Its **Unit of Measure Code** (production BOM header field) must be mapped with _Update MS Dynamics 365 On Submit_ on and carry a valid unit code, or the row fails and nothing is created.
* Its **Description** (production BOM header field) should be mapped too. Without it the phantom is created with a blank description and a warning.

A phantom row is a production BOM, not an item. Line fields that only make sense for items, such as a routing link, a variant, a unit of measure change or a fixed quantity formula, fail on a phantom row with Business Central's own message. SharpSync also never changes an existing line from `Item` to `Production BOM` or back: a mismatch fails the row, and you fix it in Business Central or by unlinking the row and adding it back with the type you want.

## BOMs SharpSync creates for items

When an item in your CAD structure has components but no _Production BOM No._ in Business Central, SharpSync creates a production BOM for it, links the item to it, and writes the lines. The header is named by your naming scheme, set in [_Use this scheme to name new BOMs_](configure-bom-mode.md#use-this-scheme-to-name-new-boms), which defaults to:

```
{rowData.componentName}_BOM
```

The new header takes the item's base unit of measure and description, unless you map the header fields yourself. If a header of that name already exists, SharpSync adopts it only when no item claims it and no phantom line references it. Otherwise the row fails and asks you to give the item a _Production BOM No._ or change the scheme.

Business Central's own rules apply to the link and are shown as the row's error: the item must be an **Inventory** item and the header's unit of measure must be one of the item's units of measure.

{% hint style="info" %}
In production mode, a **new item** needs its base unit of measure at creation. Map a column to `Base Unit of Measure` (item field) with _Update MS Dynamics 365 On Submit_ on, and make sure the value is one of your Business Central unit of measure codes, such as `EACH`. Rows for new items fail without it. Assembly mode is not affected.
{% endhint %}

## Shared and broken links

* **A production BOM claimed by more than one item** is read-only, and the row fails asking you to give the item its own production BOM. This most often happens after **Copy Item** in Business Central, which silently copies the _Production BOM No._ to the new item. A header used as a phantom by other BOMs is not this case, and syncs normally.
* **An item whose production BOM no longer exists** loads as a plain part with a warning, so one broken link never blocks the rest of the load. Submitting that row fails until the link is fixed in Business Central.
* **Comment lines** in a production BOM are never shown in SharpSync and never modified by a sync.
* **Resource lines** are not supported in production mode yet. A resource row fails with a message saying it belongs in a routing.
