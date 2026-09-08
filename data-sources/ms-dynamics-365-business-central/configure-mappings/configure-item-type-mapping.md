---
icon: diagram-subtask
---

# Configure Item Type Mapping

Every BOM line in Business Central has a **type**. SharpSync needs to know that type for each row, both to create new records and to write the line, so exactly one property mapping _must_ be marked as the item type mapping. You can leave both the primary and secondary accessors as _(Unmapped)_, but the mapping must exist.

The values depend on the _BOM type to sync_ you selected in the data source configuration:

| BOM type to sync | Allowed item type values   |
| ---------------- | -------------------------- |
| Assembly BOMs    | `Item` or `Resource`       |
| Production BOMs  | `Item` or `Production BOM` |

{% hint style="danger" %}
**The values are Business Central's own names, and they are matched exactly.** SharpSync writes the item type value to Business Central verbatim, so spelling and capitalisation matter: `Item`, not `item`, and `Production BOM` with the space.

If you configured this mapping with the lowercase `item|resource` values that earlier versions of this page prescribed, update the List Items **and any rule that returns the value** before syncing with extension version 2.0.0.0 or later. The lowercase values no longer work.
{% endhint %}

What the values mean:

- `Item`: a component that is an item in Business Central. Assemblies and sub-assemblies are items too.
- `Resource`: a resource line, such as labour, on an assembly BOM. Resource rows are not supported on production BOMs yet, and fail with a row-level error in production mode.
- `Production BOM`: a **phantom**, which is a line that points at another production BOM instead of an item. See [Production BOMs](../production-boms.md) for how phantoms are shown, created and shared.

### Item Type Property Mapping Settings

| Setting                      | Value                                                                                                            |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Accessor                     | `itemType`                                                                                                       |
| Primary accessor             | <p>(Unmapped) </p><p>or mapped to a Primary Source accessor if you have one</p>                                  |
| Secondary accessor           | (Unmapped)                                                                                                       |
| Update Primary on Submit     | unchecked                                                                                                        |
| Update Dynamics365 on Submit | checked (IMPORTANT)                                                                                              |
| Rendering Type               | Select List                                                                                                      |
| List Items                   | <p><code>Item\|Resource</code> for Assembly BOMs</p><p><code>Item\|Production BOM</code> for Production BOMs</p> |
| Is Item Type Property        | checked                                                                                                          |

The List Items give the user a list of allowed values to pick from in the comparison grid. Only include the values for the BOM type you sync.

### Item Type Property Mapping Rules

CAD systems generally have no item type property, so the value has to come from a rule. Add the following:

- A `Text Manipulation` import rule for the Primary Source (CAD source) that returns the type (can be based on metadata coming from your Primary Source). In most configurations every row is an item:

```javascript
return "Item";
```

- For production BOMs, return `Production BOM` only for the rows that really are phantoms. A phantom needs an explicit signal, such as a CAD property, a naming convention, or a known sub-assembly name:

```javascript
if (rowData.cells.partNumber.startsWith("PH-")) return "Production BOM";

return "Item";
```

{% hint style="warning" %}
Do **not** return `Production BOM` for every assembly row, for example with `rowData.isAssemblyRow`. That would turn every sub-assembly into a phantom, and a phantom is not an item in Business Central.
{% endhint %}

- A [`Text is not empty`](../../../fundamentals/rules/display/text-is-not-empty.md) display rule set to **block**. This prevents errors when submitting a BOM with no item type on a row.

### Catching a type mismatch before you submit

SharpSync never changes the type of a line that already exists in Business Central. If a row's item type differs from the line Business Central holds, for example `Production BOM` where Business Central has an `Item` line, the row fails at sync time and nothing is written for it. To fix it, change the value in Business Central, or unlink the row and add it back with the type you want.

To catch this before submitting, add a [`Text Evaluation`](../../../fundamentals/rules/display/text-evaluation.md) display rule on the item type mapping, set to **block**:

```javascript
/* Business Central's value: the differences object holds it whenever the two sides
   differed at load, otherwise the loaded cell already is Business Central's value */
const bcValue =
  rowData.differences && rowData.differences.itemType !== undefined
    ? rowData.differences.itemType
    : rowData.cells.itemType;

/* the value that will be submitted: a change made on screen lives in modifications,
    otherwise it is the loaded cell */
const chosenValue =
  rowData.modifications && rowData.modifications.itemType !== undefined
    ? rowData.modifications.itemType
    : rowData.cells.itemType;

if (bcValue !== undefined && chosenValue !== bcValue)
  return {
    message: `The item type differs from Business Central (${bcValue}). Unlink the row and add it back with the new type.`,
  };
```

The key in `rowData.differences`, `rowData.modifications` and `rowData.cells` is the accessor name of your mapping, `itemType` in the settings above. A row that does not exist in Business Central yet has no value to differ from, so the rule passes for it.
