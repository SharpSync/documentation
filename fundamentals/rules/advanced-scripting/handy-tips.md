---
description: This page
icon: js
cover: >-
  https://images.unsplash.com/photo-1518773553398-650c184e0bb3?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwyfHxjb2RlfGVufDB8fHx8MTczNTgxODA2MHww&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Handy Tips

### Checking for new rows

Include the following code at the start of your rule to check for new rows. Only works for:

* Export Manipulation
* Text Manipulation (Export)
* Comparison Rules&#x20;
* Display rules

Does not work for&#x20;

* Text Manipulation (Import) because you only have access to a single source

&#x20;

```javascript
const isNewRow = rowData.isMissingInSecondaryDatasource == true && rowData.isFoundInSecondaryDataso
```

### Converting text to a JSON  list

A common thing we see is to convert a list of id + value pairs into a list of JSON.

You'll often see data like this in SharpSync which may be a list of values from one or more sources

```
1:Value1|2:Value80|3:value15
```

You might want to convert this into a JSON list like this&#x20;

```
[
    {id} : {display_name}
]

```

So in our example the output will be&#x20;

```json5
[
    { "id" : 1, "displayName" : "Value1" },
    { "id" : 2, "displayName" : "Value80" },
    { "id" : 3, "displayName" : "value15" },
]
```

The easiest way to do this is to use an LLM like ChatGpt, Gemini, Claude, Copilot or any other publicly available LLM (or your own hosted one).

To do so enter the prompt below, then copy and paste the list:

```
Convert the following string into a JSON array with "id" and "displayName" key value pair objects. The keys must be strings
```

### Conditional logging in the browser dev tools

Logging in the dev tools can quickly overwhelm anyone. Create a  function `logOutput` at the top of your advanced script rule

```javascript

let shouldLogOutput = false;

/* optional, leave empty to skip all */
const monitoredComponents = ['C-34014-100', 'C-34014-200']; 

if (monitoredComponents.includes(rowData.componentName))
  shouldLogOutput = true;
 
const logOutput = (message, data) =>
{
  if (shouldLogOutput)
    console.debug("(" + pm.accessor + ") " + rowData.componentName + ": " + message, data);
};
```

Then further on in the script, log some data

```javascript
logOutput("Skipping this item because value is " + rowData.cells.myValue);
```

You can then easily control the logging for the entire accessor by simply toggling the value for&#x20;

> shouldLogOutput

to `true` or `false`

The output will look like this&#x20;

> (accessorName) C-34014-100: Skipping this item because value is: MyValue
