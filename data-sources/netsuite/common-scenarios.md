# Common Scenarios

## Common scenarios

* Setup a list of accounts to pick from (for Income and Expense accounts)
* Setting up accounts from a list
* Setting up a default value for income and expense accounts
* Set all new assemblies as isPhantom with rulecheck + evaluation
* Discovering property (accessor) names
* Setup a 'where used' link

### Set all new assemblies as isPhantom with rulecheck + evaluation

This setup will cause all new assemblies in NetSuite to be marked as Phantom. Onscreen you will also see any new assemblies (not marked as Phantom) showing with an error.

![image](../../.gitbook/assets/netsuite_isphantom.png)

Prerequisites: You have a property mapping set where the accessorName is `phantomYN`

#### New Rule

Accessor name: `isPhantom` Property Mapping in Netsuite: `isPhantom` Rule: `Text manipulation` (Import)

Value:

```javascript
const isNewAssemblyRow = rowData.isAssemblyRow && rowData.isMissingInSecondaryDatasource == true && rowData.isFoundInSecondaryDatasource == false;

if (isNewAssemblyRow)
  { return true; }
else
  { return rowData.cells.isPhantom || rowData.differences.isPhantom || true; }
```

#### New Rule

Accessor name: `isPhantom` Property Mapping in Netsuite: `isPhantom` Rule: `Text evaluation` (Display Rule) Value:

```javascript
const isNewAssemblyRow = rowData.isAssemblyRow && rowData.isMissingInSecondaryDatasource == true && rowData.isFoundInSecondaryDatasource == false;

if (isNewAssemblyRow && (rowData.cells.isPhantom === "false" || (`isPhantom` in rowData.modifications === true && rowData.modifications.isPhantom === false)))
{
  return { status: 'failure', message: `New Assemblies must be set to isPhantom=true` }
}
```

### Discovering property (accessor) names

NetSuite item types have different backing properties depending on the area of NetSuite you're working in as well as for the item type. For example, `InventoryItem` types will not have the same values as `AssemblyItem` types.

When creating new `InventoryItem` items you'll have to set certain item properties that are required such as (but not limited to)

* Country of manufacture
* Tax schedule
* Unit Type

However the name of thse values will not correspond to what you're seeing onscreen. In the backend they may be called

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

### Setup a 'where used' link

In NetSuite it is often useful to know which assemblies or BOM revisions are using a specific component. We'll make use of the rowData object + secondarySourceComponentId (The id of the item in NetSuite) to generate the link.

Create a new import rule

#### New Rule

Rule: `Text manipulation` (Import Rule)

Value: `Javascript`

```javascript
return 'https://[companyId].app.netsuite.com/core/pages/itemchildrecords.nl?id=' + rowData.secondarySourceComponentId + '&t=InvtItem%05ProjectCostCategory&rectype=-10';
```

Property mapping render type: Url Enable: Prefer NetSuite value (checked)

_Result_ A new link is display which you can click in the BOM. This link will open a new tab showing all the inventory records making use of this component / assembly
