# Export Weldment Cut Lists

SharpSync supports the ability to export cut lists.

{% hint style="info" %}
We're still building this feature out with more functionality, so please reach out to us via our support portal to request better feature integration
{% endhint %}

### Setup the custom columns

To work with cut weldment lists, do the following:

* Navigate to Data Sources > Onshape > Configure > Configuration
* Change the selection for cut list properties from `None` to `Specify`&#x20;
*   You'll note the default columns as:

    * Qty
    * Description
    * Length

    <figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>
* Once all the columns have been specified, click the `Save` button

{% hint style="success" %}
Tip! You may add any additional column header names here. The name must match the exported columns from the Onshape cut list feature to work
{% endhint %}

{% hint style="warning" %}
Whenever you click the `Save`  button for a Data Source, you _must_ click the `Authenticate` button again before proceeding to the <code class="expression">space.vars.entity_property_mappings</code>
{% endhint %}

### Update the <code class="expression">space.vars.entity_property_mappings</code>

To use the columns in a <code class="expression">space.vars.entity_property_mapping</code>, we have to first refresh the mappings

* Navigate to Property Mappings > Click the \`Update\` button for Onshape
* There will be new properties that are listed for the cut list items (they will match the columns specified in the first point)

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Minimum required Property Mappings**

Add the following _minimum_ properties for cut lists to work:&#x20;

* Cut List (Length)
* Cut List (Qty)

**A note on column naming in the Data Source Configuration Options:**

* The Length column may be named something else, but the accessor in SharpSync Property Mappings must be named _**cutListLength**_
* The Qty column may be named something else, but the accessor in SharpSync Property Mappings must be named _**cutListQty**_
{% endhint %}

### Displaying the Cut List

To get the cut list to show in SharpSync do the following:

* Navigate to a cut list feature in Onshape
* Right click the feature > Applications > Export Cut List
* The cut list will show up in the SharpSync UI

{% hint style="warning" %}
**At the time of writing**

* The top level name displayed will be the element name, this feature is still being fine tuned and your input would be appreciated, please reach out to use through the support portal by raising a new feature request or enhancement request
* The Length column is automatically summed by the multiplying the Qty \* Length
{% endhint %}
