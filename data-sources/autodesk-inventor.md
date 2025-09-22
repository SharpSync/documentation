---
description: Inventor is a CAD System for BREP solid body design
icon: chart-tree-map
---

# Autodesk Inventor

## Autodesk Inventor Setup

Follow the steps below to begin importing data into SharpSync using Autodesk Inventor assembly files (part files are not supported. If this is a requirement, please feel free to reach out to us).

### Prerequisites

* Download and install the Autodesk Inventor Add-in from the `Downloads` section
* Install the add-in
* An Inventor assembly file

### Setup the CSV Data source

* From the `Datasources` section, add the CSV data  source as the Primary Data source
* Change the Primary Identifier to `Part Number`, click the `Save` button
* Click the `Configure` button > `BOM Configuration`
* On a new line each, enter the Custom Properties to read, the current built-in properties in Autodesk Inventor 2025 in the `Design Tracking Properties` Property Set are:



```
Appearance
Authority
Catalog Web Link
Categories
Checked By
Cost
Cost Center
Creation Time
Date Checked
Defer Updates
Density
Description
Design Status
Designer
Document SubType
Document SubType Name
Engineer
Engr Approved By
Engr Date Approved
External Property Revision Id
Flat Pattern Area
Flat Pattern Defer Update
Flat Pattern Length
Flat Pattern Width
Language
Last Updated With
Manufacturer
Mass
Material
Material Identifier
Mfg Approved By
Mfg Date Approved
Parameterized Template
Part Icon
Part Number
Part Property Revision Id
Project
Proxy Refresh Date
Quantity
Sheet Metal Area
Sheet Metal Length
Sheet Metal Rule
Sheet Metal Width
Size Designation
Standard
Standard Revision
Standards Organization
Stock Number
SurfaceArea
Template Row
User Status
Valid MassProps
Vendor
Volume
Weld Material
```

* Each value entered here will be available as an Accessor in the [`Property Mappings`](../fundamentals/property-mappings/) tab. See also the [Property Mappings](../fundamentals/property-mappings/) section.
* Click the `Save` button
* On the main data source tab, make sure that the `Primary Identifier` matches with the `Part Number` Inventor property or any other Inventor property that you use as unique component identifier. The primary component identifier is the identifier that is unique across data sources.

### Setup the Property Mappings

You can follow the instructions in the following link to setup your property mappings

* Property mappings: Setup
* Make sure you add the property `Quantity` as a property mapping and check its `Quantity Property` checkbox in the Property Mappings Settings panel.

### Configure the Add-in

After installing the Autodesk Inventor add-in, you'll need to configure how it will generate BOMs.

* Click on the `SharpSync` ribbon tab
* Click on the `Login` button
* Enter your SharpSync username & password then click on the `Login` button
* If successful, the window will close
* Click on the `Settings` button
* If the login succeed you should see the `Primary Component Identifier` & `Secondary Component Identifier` listed
* Once your Inventor assembly is ready, click on the `Export Bom` button which will automatically export the Bom to SharpSync by opening a new web browser page
* The add-in supports exporting any Inventor Assembly `Model State`, make sure that the appropriate `Model State is active before clicking on the Export Bom` button
