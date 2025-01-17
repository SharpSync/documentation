# Integration tips

### Enable internal IDs

By enabling internal ids, you can hover over a field to see the internal ID it is linked to

> https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/section\_N3423996.html#To-display-internal-ID-values%3A

This id is the accessor you will select in SharpSync

### Viewing the source of SuiteScript

You can view the source of the SuiteScript API by navigating to

> Setup > Customization > Scripts > {your script} > View > More (top right) > SuiteScript API

This lists all the functions available to you when developing your scripts



### Discovering property (accessor) names

NetSuite item types have different backing properties depending on the area of NetSuite you're working in as well as for the item type. For example, `InventoryItem` types will not have the same properties or values as `AssemblyItem` types.

When creating new `InventoryItem` items you'll have to set certain item properties that are required such as (but not limited to)

* Country of manufacture
* Tax schedule
* Unit Type

However the name of these values will not correspond to what you're seeing onscreen. In the backend they may be called

* countryOfManufacture
* taxSchedule
* baseUnit

In order to discover what they are named, you can query a NetSuite `InventoryItem` resource using the following request

> \{{netsuite-api\}}/services/rest/record/v1/inventoryItem/{itemId}?expandSubResources=true

For example

> https://123456-sb1.suitetalk.api.netsuite.com/rest/record/v1/inventoryItem/20567?expandSubResources=true

This will yield a result such as this

```json
{
    "links": [
        {
            "rel": "self",
            "href": "https://4293129-sb1.suitetalk.api.netsuite.com/services/rest/record/v1/inventoryItem/20567?expandSubResources=true"
        }
    ],
    "assetAccount": {
        "links": [
            {
                "rel": "self",
                "href": "https://4293129-sb1.suitetalk.api.netsuite.com/services/rest/record/v1/account/133"
            }
        ],
        "id": "133",
        "refName": "1420 Inventory : Inventory Parts"
    },
    "atpMethod": {
        "id": "CUMULATIVE_ATP_WITH_LOOK_AHEAD",
        "refName": "Cumulative ATP with Look Ahead"
    },
    "autoLeadTime": true,
    "autoPreferredStockLevel": true,
    "autoReorderPoint": true,
    "availableToPartners": false,
    "averageCost": 1,
    "baseUnit": "1",

    // ... more 
}
```

From the example response above we can get accessor names to be shown in SharpSync. The names in the above sample start with

* links
* assetAccount,
* atpMethod
* autoLeadTime
* autoPreferredStockLevel
* autoReorderPoint
* availableToPartners
* averageCost
* baseUnit
* ... more

In SharpSync you're then able to select accessors from this list. Reminder that an accessor is just a property on a type. So `assetAccount` is an accessor (the data we're accessing) or property on `InventoryItem`
