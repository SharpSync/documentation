---
description: >-
  Manipulates (and returns the result of) the cell value given the JavaScript
  expression
---

# Text manipulation

Type: Export/Import

Description: Runs the specified JavaScript function on the cell value and returns a new value, replacing the original

This is an advanced rule which requires programming knowledge. It is more difficult to use, but much more powerful.

See the [Advanced Scripting ](../advanced-scripting.md)section for more detail.

In the image below, a graphical representation is shown of how changing a value with a preset rule results in values on-screen. The same principal applies for custom rules.

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

### Example: Trimming or replacing text

When running rules, each rule runs in the order specified (import / display / export)

The result of each rule being run is stored in the rule parameter `s`.

Given the text `Revision-A`, the following Text Manipulation rule will return the text 'Rev-A'

```javascript
return rowData.cells.revision.replace('ision', '');
```

At this point the value of `s` is the same as the value of `rowData.cells.revision`, because it is the first rule to run. So this could also have been written as&#x20;

```javascript
return s.replace('ision', '');
```

For every iteration of a rule that runs, the result is `s`. After running the above rule, the new value is stored in `s`, not in `rowData.cells.revision`. If you want to further manipulate the text and return only the letter 'A', you can use the following

```javascript
return s.replace('Rev-', '');
```

At this point, because this is the 2nd rule that runs, the value of `s` is _not_ the same as `rowData.cells.revision` because it was previously modified. The correct parameter to use is `s` .
