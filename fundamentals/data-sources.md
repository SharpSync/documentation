---
icon: uncharted
---

# Data Sources

<figure><img src="../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

`Data Source` is exactly that - a source of data you configure to read/write data from, or a source of data with an add-in to push data to SharpSync.&#x20;

A primary source will be a Bill of Materials (BOM) structure and metadata (e.g., part number, description, revision). A secondary source will be Inventory records and BOM data.

Each source may be registered as either a _Primary_ or a _Secondary_ Data Source.&#x20;

It works as follows:

* _Primary Data Source_ ↔️ SharpSync ↔️ &#x53;_&#x65;condary Data Source_

### Core concept: Sources

* A _Primary_ Data Source is typically a CAD / PDM / PLM source. It is the origin of your CAD data.
* A &#x53;_&#x65;condary_ Data Source is typically an ERP / MRP source.
* SharpSync uses both sources to do bi-directional synchronization, meaning it pulls information from the _Primary_ and writes to the _Secondary_. It can also pull information from the _Secondary_ and write to the _Primary_.
* Currently the application is limited to a single _Primary_ source and a single _Secondary_ source

### Core concept: Primary and Alternative Component Identifiers

SharpSync is a synchronization tool. It synchronizes data between two Data Sources. To do this, it uses the following two settings:

* Primary Component Identifier: The preferred property or meta data name to search
* Alternative Component Identifier: The alternative property or meta data name to search if the primary returns an empty result.

A Primary (or alternative) component identifier is the meta data, part number or custom property that uniquely identifies the component item (part, assembly, item or drawing) in a Data Source.

Typically this property (in a CAD environment) would be something like `Number`, `PartNumber`, `No`, `PartNo.`  or similar.

Typically in an ERP system, this would be something like `Number`, `PartNo`, `ItemId`, `Id` or similar.



**Example**

Let's say we're working with a part called '123'. Our primary identifier is `'PartNumber'` and our alternative identifier is `'name'.`

The way SharpSync works is that it searches the source for the _Primary_ identifier (in this case the primary identifier is `PartNumber`). So it  searches the source for a component identifier  `PartNumber` with a value of `123.`&#x20;

If nothing is found using an exact match (i.e. no component identifier `PartNumber` with a value of `123`), it will search the source using the _Alternative Component Identifier_. In this case, the alternative identifier is`name`, so the source is searched for a component identifier `name` with a value of `123`.

As an example of a Primary Identifier, we have included a screenshot from OnShape. This will of course vary from other platforms:

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1).png" alt="" width="563"><figcaption><p>OnShape Property Map</p></figcaption></figure>

### Core concept: Bom Revision Names

In the Configuration section of a Secondary Source (Typically an ERP), you will find a BOM Revision Scheme field where a naming convention must be specified for BOM revisions.

Some examples may be:

<table><thead><tr><th width="504">Scheme name</th><th>Sample result</th></tr></thead><tbody><tr><td>{rowData.componentName}</td><td>A1</td></tr><tr><td>{rowData.componentName}_BOM</td><td>A1_BOM</td></tr><tr><td>{rowData.componentName}_{rowData.cells.revision}</td><td>A1_A or A1_B</td></tr></tbody></table>



<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption><p>Set the schema name. Use curly braces {} to capture dynamic fields from the row data</p></figcaption></figure>

Some ERPs allows the management of different Bills of Materials for the same assembly at different stages of the design process, typically when a new revision occurs. To facilitate this, a scheme name has to be specified.

In the example above, the name of the component may be used. In addition to this, you can use any [Accessor](property-mappings/) Name from a [Property Mapping](property-mappings/) to get dynamic values.



What's important here is that, when specifying this Schema name, you should _not_ map the value in a property mapping. In other words, do not map the BOM Name or Code in a Property Mapping (you _can_, but you _shouldn't_). This is because, when not finding a BOM with a matching name, a _new_ BOM will be created, which is not what you'd want.

### Core concepts: Setup

To complete a Data Source's setup, you have to complete at least the following two items:

* Setup authentication
* Setup BOM configuration

### Resource links

Select an individual Data Source to view the configuration setup for that source.

<table data-full-width="false"><thead><tr><th width="317">Name</th><th width="108">Type</th><th width="119">Source</th><th>Sync</th><th>Status</th><th data-hidden>Documentation</th></tr></thead><tbody><tr><td><a href="../data-sources/autodesk-inventor.md">AutoDesk Inventor</a></td><td>CAD</td><td>Primary</td><td>➡️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td><a href="../data-sources/autodesk-fusion-in-progress.md">AutoDesk Fusion</a></td><td>CAD</td><td>Primary</td><td>➡️</td><td>[WIP]</td><td></td></tr><tr><td><a href="../data-sources/editor/">CSV</a></td><td>Offline</td><td>Primary</td><td>➡️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td><a href="../data-sources/ms-dynamics-365-business-central/">MS Dynamics 365 Business Central</a></td><td>ERP</td><td>Secondary</td><td>⬅️ ➡️️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td><a href="../data-sources/netsuite/">NetSuite</a></td><td>ERP</td><td>Secondary</td><td>⬅️ ➡️️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td><a href="../data-sources/odoo/">Odoo</a></td><td>ERP</td><td>Secondary</td><td>⬅️ ➡️️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td><a href="../data-sources/onshape/">Onshape</a></td><td>CAD</td><td>Primary</td><td>⬅️ ➡️️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td><a href="../data-sources/propel-plm/">Propel PLM</a></td><td>ERP/PLM</td><td>Secondary</td><td>⬅️ ➡️️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td><a href="../data-sources/solidworks.md">SOLIDWORKS</a></td><td>CAD</td><td>Primary</td><td>➡️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td><a href="../data-sources/solidworks-pdm.md">SOLIDWORKS PDM</a></td><td>PDM</td><td>Primary</td><td>⬅️ ➡️️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr></tbody></table>

{% hint style="info" %}
If you want a primary offline CAD source to sync both ways (SolidWorks, Inventor, CSV), please contact us for a custom implementation
{% endhint %}



See also [Troubleshooting](https://github.com/SharpSync/docs/blob/main/datasources/troubleshooting_datasources.md)
