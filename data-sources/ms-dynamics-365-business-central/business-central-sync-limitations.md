---
icon: triangle-exclamation
---

# Business Central Sync Limitations

This page lists what the Business Central sync deliberately does not do, and what fails a row or a load so you can catch it early. Most entries apply to production BOMs. Where a limitation can be caught before you submit, the [last section](business-central-sync-limitations.md#catching-problems-before-you-submit) shows the display rule to add.

### Licence and scope

* **Production BOMs require the Premium licence.** Assembly BOMs work on Essentials.
* **Only the item-level BOM is synced.** Location- or variant-specific BOMs set up through Stockkeeping Units are neither read nor written. A sync succeeds even if a plant builds from a Stockkeeping Unit override that the sync never touched.
* **Routings and resources are not synced yet.** A resource row in production mode fails with a row-level error saying it belongs in a routing. The resource record itself is still created in Business Central, and its mapped fields are still written, so the row is ready for when routing support arrives.
* **Derivative file transfers** (STEP, DXF and similar) are not available for Business Central yet.
* **Item pictures** are uploaded from the CAD thumbnail on every sync.

### Names and numbers

* **A production BOM number and a version code are limited to 20 characters.** SharpSync does not truncate, because two long names truncated to the same value would silently write one item's BOM into another's. A name that is too long fails the row with Business Central's own error, and the item is left without a BOM link. The limit applies to the **whole name your naming scheme produces**, so the room left for the component name is 20 minus whatever fixed text your scheme adds. With the default scheme `{rowData.componentName}_BOM`, for example, the component name must be 16 characters or fewer. The same applies to the version naming scheme.
* **Duplicate components under one parent fail the load.** Two rows with the same component name at the same level are rejected, as in every SharpSync data source.
* **An item and a phantom can collide on a name.** Item numbers and production BOM numbers are separate namespaces in Business Central, so an item `SUBFRAME` and a production BOM `SUBFRAME` can both exist. As sibling rows under one parent they are reported as duplicates, and SharpSync does not decide which one you meant. Rename one of them in Business Central or in CAD.

### Production BOM lines

* **Comment lines are never shown and never modified.** A line with a blank type is skipped when loading, preserved when writing, and not carried into a version that SharpSync creates.
* **A line's type is never changed in place.** A row typed `Production BOM` where Business Central has an `Item` line, or the reverse, fails the row and writes nothing. Changing the type would recreate the line and lose its position, routing link and dates. Fix it in Business Central, or unlink the row and add it back with the type you want.
* **Item-only line fields fail on a phantom row.** A routing link, a variant code, a change of unit of measure or a Fixed Quantity calculation formula can only be set on an `Item` line. Mapping them on a row typed `Production BOM` fails that field with Business Central's own message.
* **Some fields are never written from a mapping.** Status, version code, the record keys and the quantity per are managed by SharpSync. See [Read-Only Fields](configure-mappings/read-only-fields.md).

### Phantoms

* **A phantom's contents are shared.** Its components belong to the phantom's own production BOM, so editing them from one parent changes every BOM that uses that phantom.
* **Two parents in one sync accumulate into the phantom.** When one submit reaches the same phantom from two parents with different contents, the phantom is created once and ends up with the union of both. Field values take the last write. A component only leaves a phantom by being explicitly unlinked from a BOM that shows it.
* **A phantom is matched by production BOM number only.** A CAD sub-assembly must carry that number as its component name to line up with an existing phantom.

### Versions and dates

* **SharpSync resolves the BOM against today's date, in UTC.** A work date set in a Business Central session is not seen. The BOM SharpSync syncs is the one production orders, planning and costing use.
* **A version's starting date is set once**, when SharpSync creates it, and never moved by a later sync unless the field is mapped.
* **Superseded versions are not closed.** Retiring a version stays a manual action in Business Central.
* **A revision whose version is `Closed` fails the row.** Reopen or rename the version in Business Central.

### Certification

* **A sync never certifies a BOM it found `Under Development` or `New`.** The automatic certification setting only applies to BOMs that SharpSync opened or created. See [Certification](production-boms.md#certification).
* **A BOM whose lines did not all write is left uncertified**, whatever the setting says.

### Catching problems before you submit

The failures above surface at sync time, as row-level errors. Display rules set to **block** catch the common ones in the comparison grid, before the BOM is submitted.

**Names that are too long.** On the mapping that holds the component name, add a [Text maximum length](../../fundamentals/rules/display/text-maximum-length.md) rule. The rule fails when the value reaches the number you give, so use `21` minus the number of characters your naming scheme adds around the component name. With the default `{rowData.componentName}_BOM` scheme that is `17`. Recalculate it whenever you change the scheme, and check the version naming scheme the same way if you create versions.

**A type mismatch with Business Central.** On the item type mapping, add the blocking [Text Evaluation](../../fundamentals/rules/display/text-evaluation.md) rule shown under [Catching a type mismatch before you submit](configure-mappings/configure-item-type-mapping.md#catching-a-type-mismatch-before-you-submit).

**A resource row in production mode.** On the item type mapping, add a [Text is not in list](../../fundamentals/rules/display/text-is-not-in-list.md) rule with the value `Resource`, so a row typed `Resource` is blocked until routing support is available.
