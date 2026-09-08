---
icon: sliders
---

# Configure BOM Mode

The Business Central data source configuration has five settings that decide which kind of BOM SharpSync syncs, and how production BOMs are named and certified. They are on the `Configuration` tab of the data source, below the Company Id and Environment described in [Getting Started](getting-started.md).

| Setting                                        | Default                                       | Applies to      |
| ---------------------------------------------- | --------------------------------------------- | --------------- |
| BOM type to sync                               | Assembly BOMs                                 | All             |
| Create a new BOM version for each revision     | unchecked                                     | Production BOMs |
| Certify BOMs automatically after syncing them  | unchecked                                     | Production BOMs |
| Use this scheme to name new BOMs               | `{rowData.componentName}_BOM`                 | Production BOMs |
| Use this scheme to name new BOM versions       | `{rowData.componentName}_{rowData.cells.revision}` | Production BOMs |

The four production settings are only shown when _BOM type to sync_ is set to Production BOMs.

### BOM type to sync

Business Central has two kinds of BOM, and SharpSync syncs one of them per data source:

* **Assembly BOMs** are the BOM Components of an item, used by assembly orders. They are available on every Business Central licence.
* **Production BOMs** are the manufacturing BOMs, with versions, certification and phantoms. They require the Premium licence. See [Production BOMs](production-boms.md) for how SharpSync works with them.

{% hint style="warning" %}
**Switching the BOM type is a clean break for your property mappings.** The BOM structure accessors differ between the two types: Assembly BOMs expose BOM Component fields, Production BOMs expose Production BOM Header, Version and Line fields. After changing the setting, re-fetch the Business Central accessors and re-map every BOM structure mapping. Item, resource and item attribute mappings are not affected.

Also review the [item type mapping](configure-mappings/configure-item-type-mapping.md): its allowed values change with the BOM type.
{% endhint %}

### Create a new BOM version for each revision

Off by default. When off, a sync updates the production BOM that Business Central builds today, in place. When on, SharpSync keeps one production BOM version per CAD revision, named by the version naming scheme below. The revision must reach SharpSync through a property mapping whose accessor is `revision`. The full behaviour is described under [One version per CAD revision](production-boms.md#one-version-per-cad-revision).

### Certify BOMs automatically after syncing them

Off by default. This setting decides how a production BOM that SharpSync opened for editing, or created, ends up after the sync:

* **Off**: the BOM is left `Under Development` (or `New` if SharpSync created it) for someone to review and certify in Business Central. Until then Business Central keeps building from the previously certified structure.
* **On**: the BOM is certified at the end of the sync, provided every one of its lines was written successfully.

The setting never certifies a BOM that was already `Under Development` or `New` when the sync found it. See [Certification](production-boms.md#certification).

### Use this scheme to name new BOMs

When an item has components but no _Production BOM No._ in Business Central, SharpSync creates a production BOM for it and links the item to it. This scheme names the new BOM, and you can change it to fit your numbering convention. The default appends `_BOM` to the component name, so with the default, item `SSY-A1` gets production BOM `SSY-A1_BOM`.

Business Central limits a production BOM number to **20 characters**, and the limit applies to the whole name the scheme produces. Whatever fixed text your scheme adds reduces the room left for the component name: with the default `_BOM` suffix, a component name longer than 16 characters cannot be used to create a BOM, and the row fails with Business Central's own error. If your part numbers are long, use a scheme that adds less, or nothing at all.

The scheme is only used to **create**. An item that already has a _Production BOM No._ is synced to that BOM, whatever its name. A phantom created by SharpSync is not named by this scheme either: it takes the row's component name as-is.

### Use this scheme to name new BOM versions

Names the version SharpSync creates when _Create a new BOM version for each revision_ is on and the CAD revision's version does not exist yet. You can change this scheme as well. The default combines the component name and the revision, so with the default, revision `B` of `SSY-A1` becomes version `SSY-A1_B`. A version code is also limited to 20 characters, counted on the whole name the scheme produces.

Keep the revision in the scheme. If the scheme produces the same code for every revision, every sync targets one version and versioning does nothing.

### Naming scheme tokens

Both schemes are text with placeholders in braces. Any row value is available:

* `{rowData.componentName}`: the component name of the row.
* `{rowData.cells.<accessor>}`: any mapped cell of the row, by its accessor name, for example `{rowData.cells.revision}`.

### Saving the configuration

Click `Save` after changing any of these settings. If SharpSync asks you to authenticate with Business Central again afterwards, click `Authenticate` as you did during setup.
