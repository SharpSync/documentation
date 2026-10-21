---
description: >-
  What each message means when SharpSync refuses a BOM sent from a CAD add-in
  or uploaded as a CSV, and how to fix it.
---

# BOM not saved

When a CAD add-in sends a BOM, or you upload a CSV on the BOMs page, SharpSync checks the BOM's structure before it saves anything. If it finds a problem, it saves nothing and lists every problem it found:

```
The BOM was not saved. 3 problems:
• TOP-100/BOLT-5: quantity is empty
• TOP-100/SUB-200/: identifier is empty
• 'BOLT-5' (TOP-100/BOLT-5) and 'bolt-5' (TOP-100/SUB-200/bolt-5) differ only in capitalization
```

Fix the problems in your CAD files or your CSV, then send or upload the BOM again. Nothing was saved, so there is nothing to clean up first.

## Reading the messages

* **From a CAD add-in**, a row is named by its path: the identifiers from the top-level assembly down to the row, joined with `/`. `TOP-100/SUB-200/BOLT-5` is the `BOLT-5` inside `SUB-200` inside `TOP-100`. The identifier is the value of the property you chose as the data source's component identifier (see [Primary Identifiers](../data-sources.md#core-concept-primary-and-alternative-component-identifiers)).
* **From a CSV upload**, a row is named by its row number from your file: `Row 1.2.1`. A problem with the row number itself names the line instead, counting the header as line 1.
* An identifier is shown in quotes when its spaces or capital letters matter: `'BOLT-5 '` ends with a space.
* The list stops at 25 problems, followed by "…and N more". Fix those and send the BOM again to see the rest.

### Where the message appears

* **CSV upload:** a red alert on the BOMs page with one problem per line. Hover over the alert to keep it open. The refused file isn't kept, so upload the corrected file.
* **SOLIDWORKS PDM:** an error dialog after **SharpSync > Send BOM...**. The dialog also shows technical detail. Look for the line starting `The BOM was not saved`: the problems follow it, separated by `\n`.
* **Autodesk Fusion:** an error message listing the problems.

## The structure of the BOM

### No top-level row

```
No top-level row
No top-level row: one row number must have no '.'
```

A BOM has exactly one top-level row: the assembly itself. In a CSV, it's the row whose number has no row number delimiter, such as `1`.

**Fix:** add the top-level row to the CSV, numbered `1`, and number every other row under it (`1.1`, `1.2`, `1.2.1`).

### More than one top-level row

```
2 top-level rows (1, 2); a BOM has exactly one
```

**Fix:** number every row under one top-level row, or split the file into one CSV per assembly.

### No parent row

```
TOP-100/SUB-200/BOLT-5: no parent row TOP-100/SUB-200
Row 1.2.1: no parent row 1.2
```

The row sits under a row that isn't in the BOM: row `1.2.1` needs a row `1.2`.

**Fix:** add the missing row, or renumber the row so it sits under the right parent.

### Row number is empty (CSV)

```
Line 5: row number is empty
```

**Fix:** fill in the row number, or delete the line. Spreadsheet exports sometimes end with lines of commas only (`,,,`): delete those. The upload skips empty lines, so after an empty line the line number is lower than your editor shows.

### Row number used twice (CSV)

```
Row 1.1: used 2 times (lines 3 and 5)
```

Every row needs its own row number.

**Fix:** renumber one of the lines. If both lines are the same component, combine them into one line and add up the quantities.

### The same component twice under one parent

```
TOP-100/SUB-200/BOLT-5: sent 2 times
Rows 1.1 and 1.3 are the same component (BOLT-5); combine them into one row
```

Two rows under the same parent have the same identifier, so SharpSync can't tell them apart.

**Fix:** if they are the same part, combine them into one row and add up the quantities. If they are different parts, give each one its own identifier. [Duplicate component paths](duplicate-component-paths.md) walks through the usual cause: a copied part whose part number wasn't changed.

### Appears inside itself

```
CM-Demo appears inside itself (CM-Demo → Assem2 → CM-Demo)
Row 1.1.1: SUB appears inside itself (rows 1.1 → 1.1.1)
```

A row has the same identifier as an assembly it sits in, and an assembly can't contain itself. The chain shows where: here, `Assem2` inside `CM-Demo` contains another `CM-Demo`. This usually means two different files share an identifier, such as a sub-assembly saved as a copy of its parent without a new part number.

**Fix:** give the inner one its own identifier.

## Identifiers

### Identifier is empty

```
Top-level row: identifier is empty
TOP-100/SUB-200/: identifier is empty
Row 1.1: identifier is empty
```

The row has no value in its identifier, so it has no name in the BOM. From an add-in, the path names the row's parent followed by `/`: here, the empty row is directly inside `SUB-200`.

**Fix:** fill in the property in the file. In a CSV, fill in the cell in the Component Name Column.

### Space at the start or end

```
TOP-100/BOLT-5 : 'BOLT-5 ' has a trailing space
Row 1.1: ' BOLT-5' has a leading space
```

SharpSync never trims identifiers: `BOLT-5 ` with a space would not match the item `BOLT-5` in your ERP. The message can also say `has leading and trailing spaces`.

**Fix:** remove the space from the property in the file, or from the CSV cell.

### Differ only in capitalization

```
'Assem2' (CM-Demo/Assem2) and 'assem2' (CM-Demo/Assem2/assem2) differ only in capitalization
'BOLT' (rows 1.1.1 and 1.2.1) and 'bolt' (row 1.3.1) differ only in capitalization
```

Two identifiers in the BOM are spelled the same except for capital letters. Many ERPs, Business Central for one, treat `assem2` and `ASSEM2` as the same item, so both rows would land on one item. This applies anywhere in the BOM, not only to rows side by side.

**Fix:** if they are the same part, use one spelling everywhere. If they are different parts, give them identifiers that differ by more than capitalization. From an add-in, the message names only the first row of each spelling, because fixing the file fixes every row that uses it. From a CSV, it names every row.

## Quantities

### Quantity is empty

```
TOP-100/BOLT-5: quantity is empty
Row 1.1: quantity is empty
```

Every row needs a quantity.

**Fix:** in a CSV, fill in the quantity cell. From an add-in, the add-in couldn't read the quantity: send the BOM again, and contact us if it happens again.

### Quantities SharpSync accepts

Only an empty quantity is refused. Any other value is saved as it was sent: `1,5`, `2 pcs`, `0` and `-1` all pass. Which quantities are valid is up to your [rules](../rules/), where you see the values in the BOM:

* **Decimal commas:** add the import rule [Replace all instances](../rules/import-export/replace-all-instances.md) to replace `,` with `.`, so `1,5` becomes `1.5`. Don't replace it with nothing: `1,5` would become `15`.
* **Stop a BOM with a wrong quantity from being submitted:** add the display rule [Text is a number](../rules/display/text-is-a-number.md) or [Number between](../rules/display/number-between.md), set to `block`.

## CSV columns

### No column with that name

```
No column named 'rowNo' (Row Number Column)
No column named 'PartNo' (Component Name Column)
No column named 'Qty' for the quantity
```

The header row has no column with that name. Names must match exactly, capital letters and spaces included. The Row Number Column and the Component Name Column are the names you entered in the upload dialog. The quantity column must be named after the **Primary Data Source Property** of your quantity mapping.

**Fix:** correct the name in the upload dialog or in the file's header row. Without the row number column nothing else can be checked, so fix that one first.

### Two columns with the same name

```
2 columns named 'Qty' for the quantity
```

**Fix:** rename or remove one of the columns.

## Before the BOM is checked

```
You must have exactly one quantity mapping set up and enabled.
```

SharpSync needs to know which property holds the quantity before it can check the BOM.

**Fix:** in your property mappings, make sure exactly one enabled mapping has **Is Quantity Property** checked in its [settings](../property-mappings/property-mapping-settings.md).

## Still stuck?

If a message isn't listed here, or you can't see why a row is refused, contact us on our [support desk](https://sharpsync.atlassian.net/servicedesk/customer/portals). Tell us when you sent the BOM and its top-level name, and we can look up the full list of problems.
