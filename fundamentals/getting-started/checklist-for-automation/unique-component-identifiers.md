---
description: A discussion on the bridge between CAD, PDM, PLM and ERP
icon: square-check
---

# Unique Component Identifiers

Components with at least some form of unique identifier. We'll refer to this as the Part Numbers or Unique Component Identifiers (UCI). While most of our documentation refers to Part Numbers, internally at SharpSync we refer to these as Unique Component Identifiers.



### **What is a Component?**

In Manufacturing, a component is a standalone part or assembly or a welded assembly at any hierarchy level in the assembly.&#x20;

A component can also be something that is not modelled in CAD, such as weld rods, glue, raw materials, etc.

### What is an identifier?

An identifier is some text that can be used to locate a part or component. A unique identifier does just that - it removes ambiguity between similar looking parts or components.

### What part of the identifier do you need?

For a UCI, we need the whole text or string value. We can't use a portion of it. Ideally we also should not be using more than the whole string (e.g. Attempting to append the revision of a component to the end of the part number is not a good idea. It's better to just use the part number, and store all the data in the CAD system, and simply link to the specific revision or version in the ERP)

<table><thead><tr><th width="187">Requirement Status</th><th width="213">Item</th><th>Description</th></tr></thead><tbody><tr><td>Required</td><td>Part Number</td><td>Each component has a unique part number that is in the CAD, PDM, PLM or ERP</td></tr><tr><td>Optional</td><td>Part Description</td><td>Each component has a non-unique description that is in the CAD, PDM, PLM or ERP</td></tr><tr><td>Optional</td><td>Matching item code</td><td>Each component has a unique matching  number that spans the CAD, PDM, PLM and also in the ERP</td></tr></tbody></table>

That is it. If you have unique part numbers, you're off to a great start. This is almost the only thing you need. Of course there's more to automation than just synchronizing Part Numbers.&#x20;

Part Numbers are the bridge. They are the unique codes that span multiple systems. In our use case, it will be the same number across the CAD, PDM, PLM or ERP.

### Why do you need this?

When sending data from your CAD, PDM, PLM source to your ERP, you need to be able to track:&#x20;

* Which order it was sold on
* What the revision of that part was&#x20;
* What the manufacturing or purchase information was
* Which parts supercede this part (if any)



Here is a typical list of other properties or meta data that you'll want to synchronize in addition to the part number



* Description
* Revision
* Quantity



And then here's some extended items that you might want to send to your ERP:

* Weight
* Unit of Measure
* BOM Operations or Routes
* Material
* Thumbnail
* Whether an item is phantom&#x20;
* Whether an item is assembled, kitted, bundled,&#x20;
* You name it... there's always more
