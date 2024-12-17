---
icon: chart-tree-map
---

# Data Sources

A datasource is exactly that - a source of data that you configure to read/write data from, or a source of data with an add-in to push data ➡️ SharpSync. Each source may be registered as either a _primary_ or a _secondary_ datasource.

It works as follows:

_primary datasource_ ↔️ SharpSync ↔️ _secondary datasource_

### Core concepts

* A _primary_ datasource is typically a CAD / PDM / PLM source. It is the origin of your CAD data.
* A _secondary_ datasource is typically an ERP / MRP source.
* SharpSync uses both sources to do bi-directional synchronization, meaning it pulls information from the _primary_ and writes to the _secondary_. It can also pull information from the _secondary_ and write to the _primary_.
* Currently the application is limited to a single _primary_ source and a single _secondary_ source

### Resource links

Select an individual datasource to view the configuration setup for that source.

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th>Sync</th><th>Status</th><th>Documentation</th></tr></thead><tbody><tr><td>CSV</td><td>Static / offline</td><td>single direction</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td><a href="../data-sources/editor.md">Documentation</a></td></tr><tr><td>Inventor</td><td>CAD</td><td>single-directional</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td>MS Dynamics</td><td>ERP</td><td>bi-directional</td><td>[In progress]</td><td>Documentation</td></tr><tr><td>NetSuite</td><td>ERP</td><td>bi-directional</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td><a href="../data-sources/netsuite/">Documentation</a></td></tr><tr><td>Odoo</td><td>ERP</td><td>bi-directional</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td>Onshape</td><td>CAD</td><td>bi-directional</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td>PropelPLM</td><td>PDM</td><td>bi-directional</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td>SOLIDWORKS</td><td>PDM</td><td>bi-directional</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td>SOLIDWORKS</td><td>CAD</td><td>single-directional</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr></tbody></table>

See also TroubleshootingGitBook also allows you to set up a bi-directional sync with an existing repository on GitHub or GitLab. Setting up Git Sync allows you and your team to write content in GitBook or in code, and never have to worry about your content becoming out of sync.
