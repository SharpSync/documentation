# Troubleshooting

[New BOMs are created instead of reusing existing BOMs](troubleshooting.md#new-boms-are-created-instead-of-reusing-existing-boms)



### New BOMs are created instead of reusing existing BOMs

**Problem:**

Whenever you submit a BOM, a new BOM revision is created  for the Odoo part / assembly. You're expecting to see a single BOM, but a new one is created every time

**Fix:**

STEP 1: Under the following setting (BOM Scheme Name)

> Datasources > Odoo > Configure > Configuration > Use this scheme to create new BOM revision&#x20;

Make sure that the scheme is correct

STEP 2: In the Property Mappings

Make sure that the value for `mrp.bom.code` is:

* Either NOT mapped OR
* If mapped, then  set the `Prefer Odoo Value`  to true
* If the Scheme Name uses an additional property (e.g. Revision), make sure this property has a column mapped and is populated
