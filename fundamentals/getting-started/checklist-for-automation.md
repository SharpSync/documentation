---
icon: list-check
---

# Checklist for automation

When automating data transfers, there are some fundamentals that must be in place.

{% hint style="success" %}
The golden rule of data automation is:&#x20;

**Standardization is paramount. Everything should be of a standard format**
{% endhint %}

Below is a checklist to assist you to successfully automate your data transfer, with optional items marked accordingly.

* Components with at least some form of unique identifier. We'll refer to this as the Part Number.
* A sandbox environment for the ERP.
* Access to SharpSync staff for the Sandbox environment. (e.g. a credential to test synchronization)
* An idea of the data that needs to be transferred to the ERP.
* Optional: A sandbox environment for your CAD data (if doing bi-directional synchronization).

**Definition: Component**

A component is a part or assembly at any hierarchy level in the assembly. A component can also be something that is not modelled in CAD, such as weld rods, glue, etc.

<table><thead><tr><th width="187">Requirement Status</th><th width="213">Item</th><th>Description</th></tr></thead><tbody><tr><td>Required</td><td>Part Number</td><td>Each component has a unique part number that is in the CAD, PDM, PLM or ERP</td></tr><tr><td>Optional</td><td>Part Description</td><td>Each component has a non-unique description that is in the CAD, PDM, PLM or ERP</td></tr><tr><td>Optional</td><td>Matching item code</td><td>Each component has a unique matching  number that spans the CAD, PDM, PLM and also in the ERP</td></tr></tbody></table>

That is it. If you have unique part numbers, you're off to a good start. This is almost the only thing you need. Of course there's more to automation than just synchronizing Part Numbers.&#x20;

Part Numbers are the bridge. They are the unique codes that span multiple systems. In our use case, it will be the same number across the CAD, PDM, PLM or ERP.

Here is a typical list of things that you'll want to synchronize in addition to creating.

* Part Number
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
* Whether an item is assembled, kitted, bundled, you name it

### What you don't need for automation

We are experts at what we do. You're an expert at what you do. Very few people bridge the gap between CAD, PDM, PLM (on the left) and ERP (on the right). Let's refer to this as our fence with to parties on either side.

We don't need you to be an expert on _both sides of this fence._ That's where our expertise comes in.

If you're struggling to get the information or meta data ready on either side of this fence, we have consultants that can assist with that.

If you're struggling, please reach out to us on our homepage.
