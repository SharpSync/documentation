# Configure accounts mappings

You want to setup a list of accounts to pick from for Income and Expense accounts. This will let you specify a default Income and expense account when creating new items (for each item type).

To setup a list of accounts to pick from in NetSuite, create a new property mapping for income account and a new mapping for expense account (make sure to create additional mapping pairs for each item type you need). This will work for any NetSuite field of type `account`.

### Account Property Mapping Settings

| Setting                   | Value                                                                                                                                                                                        |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Primary accessor          | <p>(unmapped) </p><p>or mapped to a Primary Source accessor if you have one</p>                                                                                                              |
| Secondary accessor        | <p>One of: </p><ul><li>assemblyitem.incomeAccount</li><li>inventoryitem.incomeAccount</li><li>noninventoryresaleitem.incomeAccount</li><li>assemblyitem.expenseAccount</li><li>etc</li></ul> |
| Update Primary on Submit  | unchecked                                                                                                                                                                                    |
| Update NetSuite on Submit | checked                                                                                                                                                                                      |
| Object Value Selector     | `refName`                                                                                                                                                                                    |
| List Name                 | `account`                                                                                                                                                                                    |
| List Value Selector \*    | `"id": "{id}", "displayName": "{acctNumber} - {acctName}"`                                                                                                                                   |
| Rendering Type            | `Advanced List`                                                                                                                                                                              |
| List Display Selector     | `displayName`                                                                                                                                                                                |
| List Value Selector       | `id`                                                                                                                                                                                         |
| List Items                |  see below \*\*                                                                                                                                                                              |
| Prefer NetSuite value     | checked                                                                                                                                                                                      |

\* A list value selector is a string that selects token values (text items wrapped in curly braces) from the returned JSON object. In our example above, when specifying the list name of `account`, the object returned for each list may look something like this:

```json
{
  "links": [
    {
      "rel": "self",
      "href": "https://[customerId].suitetalk.api.netsuite.com/services/rest/record/v1/account/677"
    }
  ],
  "accountContextSearch": {
    "links": [
      {
        "rel": "self",
        "href": "https://[customerId].suitetalk.api.netsuite.com/services/rest/record/v1/account/677/accountContextSearch"
      }
    ]
  },
  "acctName": "Beginning Equity AP",
  "acctNumber": "5305",
  "acctType": {
    "id": "Equity",
    "refName": "Equity"
  },
  "balance": 0.0,
  "eliminate": false,
  "id": "677",
  "inventory": false,
  "isInactive": false,
  "isSummary": false,
  "lastModifiedDate": "2024-05-06T14:55:00Z",
  "localizations": {
    "links": [
      {
        "rel": "self",
        "href": "https://[customerId].suitetalk.api.netsuite.com/services/rest/record/v1/account/677/localizations"
      }
    ]
  },
  "parent": {
    "links": [
      {
        "rel": "self",
        "href": "https://[customerId].suitetalk.api.netsuite.com/services/rest/record/v1/account/188"
      }
    ],
    "id": "188",
    "refName": "5800 Amanda L. Perry"
  },
  "revalue": false,
  "subsidiary": {
    "links": [
      {
        "rel": "self",
        "href": "https://[customerId].suitetalk.api.netsuite.com/services/rest/record/v1/account/677/subsidiary"
      }
    ]
  }
}
```

\*\* The list items will depend on the values returned in the `List Values`section after saving the property mapping the first time or clicking the refresh button and will look like this:

{% code overflow="wrap" %}
```
"id": "100000", "displayName": "100000 - Cash & Equivalents"|"id": "100100", "displayName": "100100 - Checking"|"id": "100200", "displayName": "100200 - Savings"|"id": "100300", "displayName": "100300 - Investments"|"id": "100400", "displayName": "100400 - Petty Cash"|"id": "100900", "displayName": "100900 - Other"
```
{% endcode %}

The list values returned needs to be formatted. You can use Chat GPT with this prompt:

```
Convert the following list of items into a Json array of objects with an "id' and "displayName" keyValue pair
```

Once done, it will produce a Json array (see below sample) which can be pasted in the `List Items`field in the Property Mapping settings.

```
[
  {"id": "100000", "displayName": "100000 - Cash & Equivalents"},
  {"id": "100100", "displayName": "100100 - Checking"},
  {"id": "100200", "displayName": "100200 - Savings"},
  {"id": "100300", "displayName": "100300 - Investments"},
  {"id": "100400", "displayName": "100400 - Petty Cash"},
  {"id": "100900", "displayName": "100900 - Other"}
]
```

{% hint style="info" %}
Take note: Ids should be unique in the list and there should not be a trailing comma after the last object `{ }`in the list above.
{% endhint %}

{% hint style="warning" %}
It is desirable to prefer the values for accounts from NetSuite over that of your CAD system since CAD file may not even include this information.&#x20;

Make sure to set the checkbox `Prefer NetSuite value` in the property Mapping settings
{% endhint %}

### Account Property Mapping Rules

Given this input, create the following rules:

* A `Set Empty Cells` import rule for your Primary/CAD source with the default id of the account, for example:

```
100200
```

* A `Text not empty` rule. This will prevent errors when submitting the BOM

