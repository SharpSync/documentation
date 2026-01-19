---
icon: js
---

# Common operations on data

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

### Using a JSON List as an advanced selection list

