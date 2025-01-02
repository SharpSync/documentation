---
icon: chart-tree-map
---

# Data Sources

A `Data Source` is exactly that - a source of data you configure to read/write data from, or a source of data with an add-in to push data to SharpSync.&#x20;

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

The way SharpSync works is that it searches the source for the _Primary_ identifier (in this case the primary identifier is `PartNumber`). So it  searches the source for an item with an identifier of `PartNumber` that has a value of `123.`&#x20;

TODO fix all iterations\*\*

If nothing is found using an exact match (i.e. no component with `PartNumber` of `123`), it will search the source using the _Alternative Component Identifier_. In this case, the alternative identifier is `'name'`, so the source is searched for a component property 'name' with a value of 123.

### Core concepts: Setup

To complete a Data Source's setup, you have to complete at least the following two items:

* Setup authentication
* Setup BOM configuration

### Resource links

Select an individual Data Source to view the configuration setup for that source.

<table data-full-width="false"><thead><tr><th>Data Source</th><th>Type</th><th>Sync</th><th>Status</th><th>Documentation</th></tr></thead><tbody><tr><td>CSV</td><td>Static / offline</td><td>single direction</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td><a href="../data-sources/editor.md">Documentation</a></td></tr><tr><td>Inventor</td><td>CAD</td><td>single-directional</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td>MS Dynamics</td><td>ERP</td><td>bi-directional</td><td>[In progress]</td><td>Documentation</td></tr><tr><td>NetSuite</td><td>ERP</td><td>bi-directional</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td><a href="../data-sources/netsuite/">Documentation</a></td></tr><tr><td>Odoo</td><td>ERP</td><td>bi-directional</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td>Onshape</td><td>CAD</td><td>bi-directional</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td>PropelPLM</td><td>PDM</td><td>bi-directional</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td>SOLIDWORKS</td><td>PDM</td><td>bi-directional</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td>SOLIDWORKS</td><td>CAD</td><td>single-directional</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr></tbody></table>

See also [Troubleshooting](https://github.com/SharpSync/docs/blob/main/datasources/troubleshooting_datasources.md)
