---
description: '[docs in progress]'
icon: chart-tree-map
---

# SolidWorks

### Bill of Material (BOM) level features

<table><thead><tr><th width="335.0078125">Feature</th><th width="115.390625" align="center">Read</th><th width="144.890625" align="center">Create</th><th width="113.16796875" align="center">Update</th></tr></thead><tbody><tr><td>BOM hierarchy</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td>BOM meta data **</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center">N/A</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td></tr><tr><td>BOM quantities</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td>Component thumbnails</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td>BOM Configurations</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center">N/A</td><td align="center">N/A</td></tr><tr><td>File derivative transfers (e.g. STEP, DXF)</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center">N/A</td><td align="center">N/A</td></tr></tbody></table>

\*\* Updates to BOM metadata are available on request

## SolidWorks Add-in Setup

SolidWorks files (\*.sldasm, \*.sldprt) are utilized to import Bills of Materials (BOMs) from desktop-based CAD software into SharpSync. (Drawing files' BOM's are not currently supported, but if demand is high enough we will consider it) Follow the steps below to begin importing data into SharpSync using CSV files.

* Prerequisites
* Setup Instructions
* Push a Bill of Materials to SharpSync

### Prerequisites

* An installation of Solidworks 2023/4 or later (engage us for older versions)
* Download and install the SOLIDWORKS Addin from the `Downloads` section
* Installation of the addin
* An assembly or part file
* Drawings Bill of Materials are not supported yet, but talk to us about integration

### Setup Instructions

#### Setup the CSV datasource in SharpSync

* From the `Datasources` section, add the CSV datasource as the Primary datasource
* Click the `Configure` button > `BOM Configuration`
* On a new line each, enter the Custom Properties to read
* These properties should be the standard properties that exist in any given SolidWorks file. If it does not exist, a blank value will be used
* Properties are read from the `Configuration` tab first, then the `Custom` tab
* Each value entered here will be available as an `Accessor` (Property) in the `Property Mappings` tab
* Make sure to add the Quantity / Qty / qty. column (this will be used later in the property mappings). (The exact naming is not important, as long as it reflects the quantity of parts in an assembly)

> For example if you want to display custom properties `Number`, `Description`, `Material`, then enter these on a new line each

* Click the `Save` button
* On the main Data Source tab, make sure that the `Primary Component Identifier` matches with your `Number` custom property.

The primary component identifier is the identifier that is unique across data source domains. If this is `Number` or `No` or `PartNumber` then the assumption is this property exists in both SolidWorks and your ERP solution. (NOTE: It does not have to be called `Number`. It can be called anything as long as it exists as a SolidWorks custom property. If it does not exist, the file name will be used as the fallback value)

In the example below I've setup

* Number
* Description
* Revision
* Qty (Settings > Is Quantity property must be enabled)

#### Configure the Add-in

After installing the SOLIDWORKS add-in, you'll need to configure how it will generate BOMs. Make sure your SOLIDWORKS files have the required properties. (Flattened BOMs are not supported)

* Click Tools > SharpSync > Login
  * Enter your SharpSync username / password > Click Login
  * If successful, the window will close
* Click Tools > SharpSync > Settings
  * Click the radio button to use `Custom Property`
* If the login succeed you should see the `Primary Component Identifier` listed
* This is the preferred method of working with the BOM

### Push a Bill of Materials to SharpSync

Pushing data from SolidWorks to SharpSync is easy and straight forward. To push a Bill of Materials (BOM) to SharpSync do the following:

* Open a part or assembly file
* Make sure you've logged in to SharpSync (Click the login button at least once)
* Click the Push BOM button
* The active configuration of the assembly is used to display data in SharpSync

<figure><img src="../.gitbook/assets/swx_push_bom.png" alt=""><figcaption><p>Click the button to send the Bill of Materials to SharpSync</p></figcaption></figure>

* The configuration for the assembly is loaded in SharpSync.
* The version will always be shown as `current`.
* If you want to use information from different versions, the add-in for `SolidWorks PDM Professional` is recommended.

<figure><img src="../.gitbook/assets/swx_new_bom_visible.png" alt=""><figcaption></figcaption></figure>



In the example below, the hierarchy in SharpSync is displayed using the mapped `Number` property mapped in SharpSync.

Notice that the component names are taken from the `Number` primary component identifier

<figure><img src="../.gitbook/assets/swx_hierarchy_displayed.png" alt=""><figcaption></figcaption></figure>



You're now ready to submit this to your ERP.
