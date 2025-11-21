---
icon: bug
---

# Troubleshooting

* [New BOMs are created instead of reusing existing BOMs](troubleshooting.md#new-boms-are-created-instead-of-reusing-existing-boms)
* [The unit of measure defined on the order line doesn't belong to the same category as the unit of measure Unit defined on the product.  ](troubleshooting.md#the-unit-of-measure-defined-on-the-order-line-doesnt-belong-to-the-same-category-as-the-unit-of-meas)

### New BOMs are created instead of reusing existing BOMs

**Problem:**

Whenever you submit a BOM, a new BOM revision is created  for the Odoo part / assembly. You're expecting to see a single BOM, but a new one is created every time

**Fix:**

STEP 1: Under the following setting (BOM Scheme Name)

> DataSources > Odoo > Configure > Configuration > Use this scheme to create new BOM revision&#x20;

Make sure that the scheme is correct

STEP 2: In the Property Mappings

Make sure that the value for `mrp.bom.code` is:

* Either NOT mapped OR
* If mapped, then  set the `Prefer Odoo Value`  to true
* If the Scheme Name uses an additional property (e.g. Revision), make sure this property has a column mapped and is populated



### The unit of measure defined on the order line doesn't belong to the same category as the unit of measure defined on the product. &#x20;

There are 3 locations where a unit of measure may be entered:

* The product template (General Information tab and Inventory Tab)
* The Bill of Materials for the product (Quantity unit for the BOM)
* The Bill of Materials product line item

This error message occurs when the BOM unit of measure, on the BOM of the line item, is different to the BOM unit of measure of the line item itself.



Let's take BOM with assembly A1, and Part P1.&#x20;

A1

A1 ⇒ P1

Each component has their own Bill of materials, e.g. A1\_BOM and P1\_BOM. It would look like this

A1 ⇒ A1\_BOM

A1 ⇒ P1 ⇒ P1\_BOM

Each of the components (and each of the BOMs) has a property Unit of Measure, so

A1 (Unit of measure) ⇒ A1\_BOM (Unit of measure)&#x20;

A1 (Unit of measure) ⇒ P1 (Unit of measure)  ⇒ P1\_BOM (Unit of measure)&#x20;

If the (Unit of measure) selected on any of these items does not fall in the same category, you'll see this error showing up.
