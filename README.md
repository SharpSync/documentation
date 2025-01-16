---
icon: hand-wave
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Welcome

Welcome to the SharpSync! Here you'll get an overview of all the amazing features it offers to help you setup your new integration.

You'll see some of the best parts of SharpSync in action — and find help on how you can manage and setup your integrations with your CAD and ERP systems.



### Jump right in

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Getting Started</strong></td><td>Register your organization</td><td><a href=".gitbook/assets/sharpsync-logo-64.svg">sharpsync-logo-64.svg</a></td><td></td><td><a href="https://app.sharpsync.net/organization/register">https://app.sharpsync.net/organization/register</a></td></tr><tr><td><strong>Basics: Data Sources</strong></td><td>Setting up Data sources</td><td><a href=".gitbook/assets/folders.webp">folders.webp</a></td><td></td><td><a href="fundamentals/data-sources.md">data-sources.md</a></td></tr><tr><td><strong>Basics: Property Mappings</strong></td><td>Setting up Property Mappings between sources</td><td></td><td></td><td><a href="fundamentals/property-mappings.md">property-mappings.md</a></td></tr><tr><td><strong>Basics: Import / Export Rules</strong></td><td>Using the standard rule templates</td><td></td><td></td><td></td></tr><tr><td><strong>Advanced: Import / Export Rules</strong></td><td>Using advanced rule templates</td><td></td><td></td><td></td></tr></tbody></table>

<figure><img src="https://sharpsync.net/wp-content/uploads/2024/01/SharpSync_Home_Banner-1200x313.png" alt=""><figcaption></figcaption></figure>

## SharpSync

### Resource links

Select an individual Data Source to view the configuration setup for that source.

<table data-full-width="false"><thead><tr><th width="317">Name</th><th width="108">Type</th><th width="119">Source</th><th>Sync</th><th>Status</th><th data-hidden>Documentation</th></tr></thead><tbody><tr><td><a href="data-sources/autodesk-inventor.md">AutoDesk Inventor</a></td><td>CAD</td><td>Primary</td><td>➡️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td><a href="data-sources/editor.md">CSV</a></td><td>Offline</td><td>Primary</td><td>➡️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td><a href="data-sources/ms-dynamics-365-business-central.md">MS Dynamics 365 Business Central</a></td><td>ERP</td><td>Secondary</td><td>↔️</td><td>[WIP]</td><td>Documentation</td></tr><tr><td><a href="data-sources/netsuite/">NetSuite</a></td><td>ERP</td><td>Secondary</td><td>↔️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td><a href="data-sources/odoo/">Odoo</a></td><td>ERP</td><td>Secondary</td><td>↔️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td><a href="data-sources/onshape.md">Onshape</a></td><td>CAD</td><td>Primary</td><td>↔️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td><a href="data-sources/propel-plm.md">Propel PLM</a></td><td>ERP/PLM</td><td>Secondary</td><td>↔️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td><a href="data-sources/solidworks.md">SOLIDWORKS</a></td><td>CAD</td><td>Primary</td><td>➡️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr><tr><td><a href="data-sources/solidworks-pdm.md">SOLIDWORKS PDM</a></td><td>PDM</td><td>Primary</td><td>↔️</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td>Documentation</td></tr></tbody></table>

See also Troubleshooting

Important concepts:

* What is SharpSync
* Getting started
* Data Sources
* Property mappings: Setup
* Property mappings: Rules
* Derivatives
* Subscriptions

### What is SharpSync?

SharpSync is a cloud based data synchronization application in the CAD <> ERP space. CAD Systems (such as SolidWorks, SolidEdge, Inventor, Onshape) has lots of data that needs to move to ERP systems (such as Netsuite, Dynamics365, Odoo). This data includes things like BOM data, inventory detail, thumbnails, and revision information. This synchronization space, although it's been around for a many years, leaves much to be desired as the main competitors are desktop based, and any changes to their apps requires manual updates on user machines by the developers, or copying and pasting settings between machine. This means everytime there is a change, you're billed for something you should be able to do yourself.

To that end, the main goals of the application are:

* Easy to use.
* Intuitive.
* Web based - no manual updates!
* To let you move data:
  * From CAD => ERP => CAD or
  * From PDM => ERP => PDM or
  * From PLM => ERP => PLM
* It does this by identifying differences in meta data (or custom properties) and the BOM structure between the 2 systems
* Displays this data in an easy to digest format onscreen.
* Reporting on BOMs submitted. Users must be able to troubleshoot problems with their own data
* It then let's the user submit and write back the changes to the CAD, ERP, PDM or PLM system.

[Marketing video](https://sharpsync.net/wp-content/uploads/2024/06/SharpSync-Promo-1.mp4) here

### What value does it provide?

Typically companies would employ a full time data entry person to manage the data between the 2 systems. This is an extremely time consuming and error prone process. Sharpsync automates this process and allows the user to focus on more value added tasks. We believe our product is well priced and provides great value for the price compared to traditional desktop solutions.

SharpSync was designed from the ground up to be used by users of the application, not to be silo'ed to the halls of developers. It has an very powerful rules engine that let's you, as the user, specify rules to massage your data when the data is imported, exported or displayed on screen. This reduces the amount of time spent agonizing over individuals rows in BOMs.

These rules are setup for data sources for the entire organization and all your users will draw the benefit of having a unified rule set for your data. That means:

* No need to copy + paste data from CAD => ERP.
* No need to employ someone to do said copying + pasting.
* No need to export + import settings across machines.
* Intuitive: You should be able to map your own mappings. Not pay a developer to do it.
