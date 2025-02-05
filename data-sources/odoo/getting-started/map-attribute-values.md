# Map attribute values

{% hint style="danger" %}
This page is a work in progress.&#x20;

Reading attribute values are supported, but writing is in progress. Check back here  frequently for updates
{% endhint %}



The status of implementation:

| Odoo Version | Read attributes                | Write attributes             |
| ------------ | ------------------------------ | ---------------------------- |
| 16           | Basic read \[work in progress] | \[scheduled for development] |
| 17           | :white\_check\_mark:           | \[scheduled for development] |
| 18           | \[scheduled for development]   | \[scheduled for development] |



Attributes in Odoo are analogous to configuration values in a CAD system. When adding an attribute to a product template or a product variant, you are creating a new variant (new configuration) of that product.

To setup attribute lists in SharpSync there are major steps:

* Read the values
* Setup a Render Type
* Write the values back to Odoo (CAD Systems do not supporting writing configuration information)

### Read the values

To view the values of an attribute for Odoo, use the list `product.attribute` (see also [List Names](../list-names.md))

The list `product.attribute` is special in that you can expand upon the query by adding attribute name at the end in square brackets.&#x20;

* Start by adding a [Property Mapping ](../../../fundamentals/property-mappings.md)for&#x20;

> product.template.attribute\_line\_ids

<table><thead><tr><th width="284">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Primary Accessor</td><td>(Unmapped)</td></tr><tr><td>Secondary Accessor</td><td><code>product.template.attribute_line_ids</code></td></tr><tr><td>List Name</td><td><code>product.attribute["Finish"]</code></td></tr><tr><td>List Value Selector</td><td>{id}:{name}</td></tr><tr><td>Prefer Odoo Value</td><td>checked</td></tr></tbody></table>



* Open the Property Mapping > Settings and enter a `List Name` of&#x20;

> product.attribute

Click the `Save` button. The list will update.

Pick the name of the attribute, and append it to the back of the `product.attribute` list included as an array parameter. E.g. if I had a list of `Finish` I would use

> product.attribute\["Finish"]

Click the `Save` button. Click the 'Refresh' button. The list will update.



This will query all the values of the Finish attribute. However this may return complex values (objects with many properties) which you could refine with a `List Value Selector` such as&#x20;

> {id} : {name}&#x20;

{% hint style="info" %}
You can select more complicated values, but this is a good starting point
{% endhint %}



The result returned from this may look something like this&#x20;

> 1:Powder Blue|9 : Satin Black

The next step is to format this data to display it as a selection List in SharpSync.



### Setup a Render Type

Let's say  the values returned from the above list is as follows:

> 11 : Red|12 : Satin Black|3 : White|4 : Black

This means that each attribute has an internal id, and a display name or 'name' value associated with the id value. Our next step is convert this list to a list of JSON objects in the form

> \[&#x20;
>
> &#x20; { "id" : "id1Value" , "name" : "displayName1" },
>
> &#x20; { "id" : "id2Value" , "name" : "displayName2" }
>
> ]

You can create this by hand or use the following prompt in ChatGPT with your text string&#x20;

> Convert the following string into a JSON array with "id" and "name" key value pair objects. The keys must be strings

* Click the copy button next to the generated list of values
* Change the Property Mapping [Rendering Type](../../../property-mappings/settings.md) to <mark style="color:blue;">`Advanced Multi Select List`</mark>
* For `List Display Selector`, enter <mark style="color:blue;">`name`</mark>
* For List Value Selector, enter <mark style="color:blue;">`id`</mark>
* For List Items, transform the values into an array of values as follows:

```json
[ 
  { "id" : "3", "name" : "White" },
  { "id" : "4", "name" : "Black" },
  { "id" : "11", "name" : "Red" },
  { "id" : "12", "name" : "Satin Black" }  
]
```

{% hint style="info" %}
Important! Make sure the <mark style="color:blue;">`id`</mark> parameter is a string (wrapped in quotes "")
{% endhint %}

* Click the save button
* Repeat this process for each attribute

You are now able to Select attributes from the BOM Comparison page.&#x20;

{% hint style="info" %}
While this functionality doesn't let you write values to Odoo yet, it will let you display existing values onscreen
{% endhint %}

The next step will be to parse the values from Odoo so that it automatically selects the correct value onscreen when the BOM is loaded from Odoo.



Add a new Rule Mapping:

<table><thead><tr><th width="162">Setting</th><th>Value</th></tr></thead><tbody><tr><td>Rule Type</td><td>Import</td></tr><tr><td>Rule Name</td><td>Text Manipulation</td></tr><tr><td>Value</td><td><p></p><pre class="language-javascript"><code class="lang-javascript">let sArr = JSON.parse(s); 
let attributeName = "Finish";  

if (Array.isArray(sArr)) 
{   
  let attributeList = sArr.find(item => item.attribute_id.display_name === attributeName);   
  if (!attributeList) 
     return []; 

  let retVal = attributeList.value_ids.map(vi => vi.id.toString()) || [];   
  
  return retVal; 
}  

return [];  
</code></pre></td></tr><tr><td>Enabled for</td><td>Odoo only</td></tr></tbody></table>



This will return a string of 'ids' to the screen.&#x20;





### Write the values back to Odoo

{% hint style="info" %}
Writing values back to Odoo is  a work in progress. Please check back here regularly
{% endhint %}

