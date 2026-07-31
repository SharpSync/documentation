---
icon: list-tree
---

# Configure Mappings

{% hint style="danger" %}
This document is a work in progress.

Check back here  frequently for updates.
{% endhint %}

For the basics on how to setup mappings, check out [Property Mappings](../../../fundamentals/property-mappings/).

### Configure Dynamics 365 Business Central Property Mappings

* Navigate to `Property Mapping`
* After creating/adding your property mappings from your primary/CAD source, select the corresponding Dynamics 365 Business Central property from the drop down selection boxes.
*   Dynamics 365 Business Central properties of type `array` are fields whose values are fetched from a field enum list in Dynamics 365 Business Central (field type `OPTION`). SharpSync will return a list of objects with keys `name` and `ordinal` corresponding to the enum identifiers and values. When mapping an array type field, render it as a select list with the values being the concatenated string of `name` values separated by a pipe symbol `|` .

    * Example field values from Dynamics 365 Business Central:

    <pre data-overflow="wrap"><code>{
      "name": " ",
      "ordinal": 0
    }|{
      "name": "Item",
      "ordinal": 1
    }|{
      "name": "Resource",
      "ordinal": 2
    }
    </code></pre>

    * Example `List Items` string for a `Select List` rendering:

    <pre data-overflow="wrap"><code>Item|Resource
    </code></pre>
* Dynamics 365 Business Central properties of type `nestedObject` are fields whose values are fetched from a related table in Dynamics 365 Business Central. For example the property `Tax Group Code` takes possible `Code` values from a related table. In order to query the possible Dynamics 365 Business Central values for a given property, in SharpSync, when configuring the Property Mapping Settings of a `nestedObject` type property, insert a corresponding key in the `List Name` input then click on the SAVE button. Possible inputs could be:
  * `Code`
  * `No.`
  * `Dimension Code`
  * `Deferral Code`
  * `Location Code`
  * `SAT Classification`
  * `Name`

For more info check a comprehensive but not exhaustive list [here](list-names-for-nestedobject-mappings.md).



* You must set up an [Item Type Property Mapping](configure-item-type-mapping.md).
* You must set up a [Quantity Property Mapping](configure-quantity-mapping.md).
