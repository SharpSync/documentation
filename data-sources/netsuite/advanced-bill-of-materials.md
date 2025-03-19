# Advanced Bill of Materials

Advanced BOMs are NetSuite's way of keeping track of Bill of Materials (BOM) snapshots. You can think of an advanced BOM as the revision of a Bill of Materials at a certain point in time.

Advanced BOMs use the following notable features:

* A parent BOM record (NetSuite itemtype --> `bom`), which is associated to a Assembly (NetSuite itemtype --> `assemblyitem)`
* BOM Revision records (NetSuite itemtype --> `bomrevision`), which are associated to the BOM record and which contain a revision of a set of BOM Components.
* Activation dates
* Routings
* Manufacturing steps

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### Using Advanced BOMs

There are some limitations with the use of advanced BOMs via the NetSuite API. For this reason (at the time of writing), we make use of a custom server side script to create advanced BOMS.

Server side scripting uses a script in NetSuite to set default custom field values for `bom` and `bomrevision` upon their creation. The important bit to take note of here is that the `itemInjectionScriptId` needs to be configured (see links at the bottom of article).

### Reading And Writing bom and bomrevision Metadata

You can map NetSuite fields related to `bom` and `bomrevision` item types just like you map fields for regular item types (`assemblyitem`, `inventoryitem`, etc...).

When loading a BOM, SharpSync will query the associated BOM and BOM Revision for any related `assemblyitem` that exists in NetSuite and fetch the `bom` and `bomrevision` metadata for the configured mappings.

Similarly, when submitting a BOM, SharpSync will query the associated BOM and BOM Revision for any related `assemblyitem` that exists in NetSuite and update the `bom` and `bomrevision` metadata for the configured mappings. If an `assemblyitem` is to be created in NetSuite, SharpSync will also automatically create an associated BOM and BOM Revision, and update the fields accordingly.

Some notes on the `bom` and `bomrevision` metadata:

* Some bom fields such as `bom.id` , `bom.name` , `bom.createdDate` , etc... are determined by a NetSuite or SharpSync logic, and their mappings should not be set to update NetSuite, and preferably be set to readonly.
* Some bomrevision fields such as `bomrevision.id` , `bomrevision.name` , `bomrevision.effectiveStartDate` , `bomrevision.effectiveEndDate` , etc... are determined by a NetSuite or SharpSync logic, and their mappings should not be set to update NetSuite, and preferably be set to readonly.
* The bom field `bom.useComponentYield` is a boolean field, but once it is set to `true` it can not be set back to `false` . This is a NetSuite limitation. In a related issue.
* For boms where `bom.useComponentYield` is set to `true` , a related `bomrevision`'s child components can have the field `bomrevisioncomponent.componentYield` set to values greater than 0 and less or equal than 100 (0 < x <= 100).
* For boms where `bom.useComponentYield` is set to `false` , a related `bomrevision`'s child components will have the field `bomrevisioncomponent.componentYield` values always ignored by NetSuite and kept at 100.

### Major steps

* [Configure the server side script](configure-server-side-script/)
* [Configure SharpSync to use server side scripting](configure-sharpsync-to-use-server-side-script.md)
* [Configure Routings](configure-routings.md) \[optional]

