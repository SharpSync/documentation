---
description: Inventor is a CAD System for BREP solid body design
icon: uncharted
---

# Autodesk Inventor

## Autodesk Inventor Add-in Setup

The SharpSync add-in for Autodesk Inventor sends the Bill of Materials (BOM) of an open Inventor assembly to SharpSync. Part files and drawings aren't supported: if you need them, please reach out to us.

* [Prerequisites](autodesk-inventor.md#prerequisites)
* [Allow the add-in in Inventor](autodesk-inventor.md#allow-the-add-in-in-inventor)
* [Setup Instructions](autodesk-inventor.md#setup-instructions)
* [Send a Bill of Materials to SharpSync](autodesk-inventor.md#send-a-bill-of-materials-to-sharpsync)
* [What the BOM includes](autodesk-inventor.md#what-the-bom-includes)
* [When the BOM is not sent](autodesk-inventor.md#when-the-bom-is-not-sent)

### Prerequisites

* An installation of Autodesk Inventor 2025 or later
* If an earlier add-in called **SharpSync Inventor Addin 2026** is installed, remove it first: in Windows, open `Settings` > `Apps` > `Installed apps`, find it and click `Uninstall`. The new installer doesn't replace it, and Inventor would load both
* Download the **SharpSync Inventor Client** from the `Downloads` section in `SharpSync`. Its name ends with the add-in's version (for example `3.3.1`). Close Inventor and run the installer. It installs the add-in for every user of the PC, in `C:\ProgramData\Autodesk\Inventor Addins\SharpSync Inventor Add-in`, and replaces any earlier version of this add-in
* An Inventor assembly file

### Allow the add-in in Inventor

The first time Inventor starts with the add-in, it can block it, showing `Add-in Manager Security Alert`: "The following add-ins could not be authenticated and have been blocked by Autodesk Inventor", with **SharpSync BOM Sync** in the list. To unblock it:

* Click `Launch Add-in Manager`. (Later, you can open it from `Tools` > `Add-Ins`.)
* On the `Applications` tab, select **SharpSync BOM Sync**. Its `Load Behavior` shows `Blocked`
* Under `Load Behavior`, clear `Block`, then check `Load Automatically` and `Loaded/Unloaded`
* Click `OK`

Inventor remembers your choice, so it doesn't ask again. If Inventor asks whether to **Allow** or **Block** the add-in instead, choose **Allow**.

Open an assembly: the **SharpSync** tab is on the ribbon.

### Setup Instructions

#### Setup the CSV datasource in SharpSync

* From the [`Data sources`](../fundamentals/data-sources.md#core-concept-sources) section, add the CSV Data Source as the Primary Data Source
* Set its `Primary Component Identifier` to the Inventor field that identifies a component both in Inventor and in your ERP, usually `Part Number`. It can be any built-in field or a custom iProperty
* Set its `Alternative Component Identifier` too: the add-in doesn't send a BOM without one
* Click the `Configure` button > `BOM Configuration`
* In `Column header names (each on new line)`, enter the Inventor fields you want to read, one per line. Each becomes an `Accessor` you can map in the [`Property Mappings`](../fundamentals/property-mappings/) tab
  * Add the names to the ones already in the box rather than replacing them: the box is shared with your other CAD add-ins and CSV uploads
  * You can paste the [sample list below](autodesk-inventor.md#inventor-fields) before you install the add-in, then use the add-in's `Field List` button on your own assembly to add its custom properties
* Click the `Save` button

{% hint style="success" %}
A file with no value in the Primary Component Identifier stops the push: see [When the BOM is not sent](autodesk-inventor.md#when-the-bom-is-not-sent)
{% endhint %}

#### Inventor fields

The add-in reads the fields you map from every iProperty group: the built-in fields on the `Summary`, `Project`, `Status` and `Physical` tabs, and your custom iProperties.

* A field a file doesn't have is sent as an empty cell
* A custom iProperty with the same name as a built-in field isn't read: the built-in field is used, even when it's empty. Give the custom iProperty another name
* A parameter is read only when its `Export Parameter` box is checked in Inventor's `Parameters` dialog, which makes it a custom iProperty

The add-in's `Field List` button lists the fields on the open assembly and the components in its BOM, in two boxes, each with a `Copy` button:

* `Inventor fields`: `QTY` and `Item QTY` (see [Quantity](autodesk-inventor.md#quantity)), then the built-in fields, A to Z
* `Custom properties`: the custom iProperties, A to Z

It leaves out fields no one maps to an ERP, such as the thumbnail, GUIDs and Inventor's own update switches. You can still type any field's name into the box yourself.

Here is the `Inventor fields` box for a sample assembly. A file can carry fields this list doesn't, and newer Inventor versions add fields, so the `Field List` button is the complete list for your own assembly.

```
QTY
Item QTY
Appearance
Author
Authority
Catalog Web Link
Category
Checked By
Comments
Company
Cost
Cost Center
Creation Time
Date Checked
Density
Description
Design Status
Designer
Document SubType Name
Engineer
Engr Approved By
Engr Date Approved
Flat Pattern Area
Flat Pattern Length
Flat Pattern Width
Keywords
Language
Last Saved By
Last Updated With
Manager
Manufacturer
Mass
Material
Material Identifier
Mfg Approved By
Mfg Date Approved
Part Number
Project
Revision Number
Revit Category
Sheet Metal Area
Sheet Metal Length
Sheet Metal Rule
Sheet Metal Width
Size Designation
Standard
Standard Revision
Standards Organization
Stock Number
Subject
SurfaceArea
Title
User Status
Vendor
Volume
Weld Material
```

#### Setup the Property Mappings

Follow [Property Mappings](../fundamentals/property-mappings/) to map the fields you want to see in SharpSync.

#### Quantity

Add one mapping for the quantity, with the accessor `QTY` or `Item QTY`, and check its `Is Quantity Property` setting. The two send different numbers:

| Accessor   | Sends                                                                        | A rivet modelled once, with QTY typed as 22 | A part with a base quantity of 2.500 in, used twice |
| ---------- | ---------------------------------------------------------------------------- | ------------------------------------------- | --------------------------------------------------- |
| `QTY`      | What the `QTY` column of Inventor's `Structured` BOM shows, units included    | `22`                                        | `5.000 in`                                          |
| `Item QTY` | The count of each item                                                       | `1`                                         | `2`                                                 |

* A quantity mapping with any other accessor, such as `Quantity`, stops the push: see [When the BOM is not sent](autodesk-inventor.md#when-the-bom-is-not-sent)
* You can also map the other one, without `Is Quantity Property`, to see both: it's filled too
* An iProperty named `QTY` or `Item QTY` isn't read: the BOM's own quantity is always sent
* Units stay in the text. If your ERP needs a plain number, use a property mapping rule to remove them

#### Configure the Add-in

After installing the add-in, open an assembly and use the **SharpSync** tab:

* `Login`: the SharpSync login page opens in a window. Log in the same way as on the SharpSync website. If successful, the window will close
* `Logout`: logs you out of the add-in
* `Settings`: shows the `Primary Component Identifier` and `Secondary Component Identifier` of your CSV Data Source. You can't change them here: change them in the Data Source
* `Field List`: lists the fields to paste into `Column header names` (see [Inventor fields](autodesk-inventor.md#inventor-fields))
* `Send BOM`: sends the open assembly's BOM (see below)
* `About`: shows the add-in's version

The tab's icons follow Inventor's light or dark theme. If you change the theme, restart Inventor to update them.

### Send a Bill of Materials to SharpSync

* Open the assembly, and make its `Model State` the one you want: the active model state is sent
* Make sure you've logged in to SharpSync (click the `Login` button at least once)
* Click the `Send BOM` button. It's greyed out, and Inventor's status bar says `Sending the BOM to SharpSync...`, until the BOM is sent or a problem is shown
* SharpSync's BOMs list opens in your browser, with the BOM you sent expanded

Sending the BOM leaves your assembly as it was: Inventor doesn't ask you to save it afterwards, and your BOM Editor settings are unchanged.

### What the BOM includes

The BOM follows Inventor's `Structured` BOM of the open assembly, at all levels, so each component's `BOM Structure` decides how it appears:

| In Inventor                     | In SharpSync                                                                                                                         |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `Normal`                        | A row. A sub-assembly has its children under it                                                                                      |
| `Phantom`                       | No row. Its children move up one level and their quantities multiply: a phantom used 3 times that holds 2 bolts gives 6 bolts       |
| `Reference`                     | Left out                                                                                                                             |
| `Purchased` sub-assembly        | One row, without its children, even if the `Structured` tab lists them. To send its children, set it to `Normal`                     |
| `Inseparable` sub-assembly      | A row, with its children under it. To send it as one row, set it to `Purchased`                                                      |
| Suppressed component            | Left out, with everything under it, whatever `Hide Suppressed Components in BOM` says                                                |
| Virtual component               | A row, with its own iProperties                                                                                                      |

So SharpSync can show fewer rows than the `Structured` tab: a `Purchased` sub-assembly's children, and suppressed components when `Hide Suppressed Components in BOM` is off.

Each row gets an image of its component. A virtual component has no file, so its row has no image. If an image can't be made, the row is sent without one.

### When the BOM is not sent

If there's a problem, the add-in shows one `BOM not sent` dialog that explains it, and nothing is saved. Fix the problem, then click the `Send BOM` button again.

**You're not logged in, or your login has expired:**

```
You're not logged in to SharpSync, or your login has expired, so the BOM was not sent. Click Login on the SharpSync tab, then send the BOM again.
```

**The quantity mapping's accessor isn't `QTY` or `Item QTY`** (see [Quantity](autodesk-inventor.md#quantity)):

```
The quantity mapping's accessor "Quantity" isn't one the Inventor add-in can send, so the BOM was not sent. In the web app, change it to "QTY" (the QTY column of Inventor's Structured BOM) or "Item QTY" (the count of each item), then send the BOM again.
```

The add-in also stops when the CSV Data Source isn't set up, has no `Primary Component Identifier` or `Alternative Component Identifier`, or has no enabled property mappings or quantity mapping, and when the open document isn't an assembly. Each message says what to change.

**Inventor can't read the BOM**, usually because a file can't be found:

```
Inventor could not read this assembly's BOM, so it was not sent.

This could be caused by a file that could not be found. Resolve any missing files, then try to send the BOM again.
```

The add-in then checks your files. A list stops at 25 lines, followed by "…and N more". Fix those and send the BOM again to see the rest.

**A file has no identifier.** Each file is named with its model state:

```
These files have no 'Part Number' value, so the BOM was not sent:
bracket.ipt ([Primary])
bolt.ipt (LONG)
```

Give each file a value. A file's model states can each hold their own value, so check the model state named.

**Two files or model states share an identifier under one parent:**

```
BOLT-5 is used by 2 different files under TOP-100: bolt.ipt, bolt-copy.ipt
BOLT-5 is used by 2 model states of bolt.ipt under TOP-100: SHORT, LONG
```

An identifier names one file in one model state, so give each one its own identifier, or use one file. The same file in the same model state used several times is fine: it's one row with its count.

**Inventor merged files into one row.** Inventor's part number row merge joins files that share a Part Number into one row of the BOM, which SharpSync can't split:

```
Inventor merged 2 different files into one row under TOP-100, as they share the Part Number DUP-1: dup-a.ipt, dup-b.ipt
```

Give each file its own Part Number. If your Primary Component Identifier isn't `Part Number`, you can instead turn off part number merge in the BOM Editor, so the files are sent as separate rows.

Once these pass, SharpSync checks the BOM's structure. [BOM not saved](../fundamentals/troubleshooting/bom-not-saved.md) explains each of its messages and how to fix it.
