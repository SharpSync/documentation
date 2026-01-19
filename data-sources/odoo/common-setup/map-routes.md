---
icon: route
---

# Map routes

Routes in Odoo are warehouse locations where a product may be 'routed' to. A location can be a manufacturing location (some operation is performed) or a physical location (a warehouse itself).

### Add a Property Mapping

To parse the values from the routes, setup the following property mapping.

To update this from SharpSync, start by adding a [Property Mapping ](../../../fundamentals/property-mappings/)for&#x20;

> product.template.route\_ids

<table><thead><tr><th width="284">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Primary Accessor</td><td>(Unmapped) [or property of your choice]</td></tr><tr><td>Secondary Accessor</td><td><code>product.template.route_ids</code></td></tr><tr><td>List name</td><td><code>stock.route</code></td></tr><tr><td>List value selector</td><td>{id} : {display_name}</td></tr><tr><td>Prefer Odoo Value</td><td>checked</td></tr><tr><td>Update Odoo on submit</td><td>checked</td></tr><tr><td>Rendering Type</td><td>Advanced Selection List <sup>**see setup below</sup></td></tr></tbody></table>

After adding the List value selector, click the Save button at the bottom.

The list values will update.  Use this to [generate a list of JSON objects](../../../fundamentals/rules/advanced-scripting/common-operations-on-data.md)&#x20;

Change rendering type

### Change the rendering type to Advanced Selection List

### Add an import rule

Add an import Rule for Odoo. This rule will convert multiple numeric values list e.g.&#x20;

```
[ 1, 5 ]
```

into a user friendly list like this on screen

```
[ Buy, Manufacture ]
```

Add a new import rule and make sure it is checked only for Odoo. Set the text to this:

```js
/* routes Odoo import set default as manuf (6) if no input is given */
const buy = 5; /* adjust as necessary - this will be in the list when you first query odoo */
const manuf = 6;

if (!s) {
  return [manuf];
}

/* try to extract an array */
if (Array.isArray(s)) 
{
 /* default value returned if there is nothing */
    if (s.length === 0) {
    return [ manuf ];
  }
  return s.sort((a, b) => a - b);
}

/* not a native array - so parse plain text , repeating the logic */
try {
  const parsed = JSON.parse(s);
  if (Array.isArray(parsed)) 
  {
    if (parsed.length === 0) 
      return [ manuf ];
    else 
      return parsed.sort((a, b) => a - b);
  } 
  else 
    return [ parsed ];
} catch {
  /* open the dev tools to check for this error if you suspect one */
  console.error("routes: Failed to parse Odoo route input as JSON:", s);
  return [ s ];
}

```

