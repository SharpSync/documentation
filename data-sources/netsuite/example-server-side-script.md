# Example Server Side Script

A custom script example follows below. You can certainly modify this to your heart's content, but the overarching details are captured here

Before uploading to Netsuite, save the below contents to a `*.js` file.

```javascript
/*
  Created for SharpSync.net
  Description: 
*/

/**
 * @NApiVersion 2.x
 * @NScriptType Restlet
 */
/**
 * @param {string} assemblyId
 */
define(['N/record', 'N/search', 'N/log', 'N/encode', 'N/error'], function (record, search, log, encode, error) {

  const itemTypes_e = {
    INVENTORY_ITEM: 'inventoryItem',
    NON_INVENTORY_ITEM: 'nonInventoryItem',
    NON_INVENTORY_RESALE_ITEM: 'nonInventoryResaleItem',
    NON_INVENTORY_PURCHASE_ITEM: 'nonInventoryPurchaseItem',
    ASSEMBLY_ITEM: 'assemblyItem'
  };

  /* 
      SharpSync always uses the following appState to confirm validity
      X-APPLICATION => header naming the application
      X-USERID => header naming the user (uuid)
      X-ORG_ID => header naming the organization
  */
  function get(context) {

    validateAppState(context.appState, encode, error)

    log.debug({
      title: 'gettingRecord',
      details: context
    });

    if (context.recordId) {
      var requestedRecord = record.load({
        type: "nonInventoryResaleItem",
        id: context.recordId,
        isDynamic: true
      });

      return {
        expAcc: requestedRecord.getValue({ fieldId: 'expenseaccount' }),
        incAcc: requestedRecord.getValue({ fieldId: 'incomeaccount' }),
        itemDetails: requestedRecord
      };
    }


    // Check any /GET query parameters here with context.{parameterName}
    if (!context.name) {
      throw error.create({
        name: 'MISSING_NAME',
        message: 'Missing required argument: item name',
        notifyOff: true
      });
    }

    //getValidTaxSchedules()
    return { name: context.name, itemDetails: itemDetails };
  }

  /* SharpSync always uses the following headers to confirm:
     X-APPLICATION => header naming the application
     X-USERID => header naming the user (uuid)
     X-ORG_ID => header naming the organization
  */
  function post(context) {

    /* TODO: Remove log.debug({ title: 'POST body', details: context }); */
    validatePostRequestPrereqs(context);
    validatePostRequestItemType(context, error, log);

    var itemRecord = CreateItemFromContext(context, record, log);

    log.debug({
      title: 'Item record',
      details: itemRecord
    });
    setCustomerSpecificDefaultValues(itemRecord, context.itemType);

    var itemId = itemRecord.save();
    var serializedItemRecord = JSON.parse(JSON.stringify(itemRecord));
    // must parse. For some reason when accessing it directly you get null
    return {
      applicationName: context.applicationName,
      sharpsyncId: context.sharpsyncId,
      organizationId: context.organizationId,
      itemId: itemId,
      itemName: context.name,
      destinationTypeRequested: context.itemType,
      baseRecordTypeName: serializedItemRecord.fields.baserecordtype || "unknown",
      url: '/services/rest/record/v1/' + serializedItemRecord.fields.baserecordtype + '/' + itemId + '/?expandSubResources=true'
    };
  }

  return {
    get: get,
    post: post
  };
});

function CreateItemFromContext(context, record, log) {

  var itemRecord;
  var isDynamic = true;

  switch (context.itemType) {
    case "inventoryitem":
      itemRecord = record.create({ type: record.Type.INVENTORY_ITEM, isDynamic: isDynamic });
      break;
    case "assemblyitem":

      log.debug({
        title: 'Context.itemMethod',
        details: context.itemMethod
      });

      if (context.itemMethod == "createBomLink") {
        linkBomToAssemblyItem(context, record);
      }
      else {
        itemRecord = record.create({ type: record.Type.ASSEMBLY_ITEM, isDynamic: isDynamic });
      }

      break;
    case "noninventoryresaleitem":
      itemRecord = record.create({ type: record.Type.NON_INVENTORY_ITEM, isDynamic: isDynamic });
      itemRecord.setValue({ fieldId: 'subtype', value: 'resale' });
      break;
    case "noninventorypurchaseitem":
      itemRecord = record.create({ type: record.Type.NON_INVENTORY_ITEM, isDynamic: isDynamic });
      itemRecord.setValue({ fieldId: 'subtype', value: 'purchase' });
      break;
    case "bom":
      itemRecord = createBomFromContext(context, record);
      break;
    default:
      throw error.create({
        name: 'INVALID_ITEM_TYPE',
        message: 'Invalid item type (' + context.itemType + '), must be one of {inventoryitem|assemblyitem|noninventoryresaleitem|noninventorypurchaseitem|bom}',
        notifyOff: true
      });
  }

  itemRecord.setValue({
    fieldId: 'itemid',
    value: context.name, // Change this to the actual item name
  });

  return itemRecord;
}

function linkBomToAssemblyItem(context, record) {
  var bomRecord = record.load({
    type: record.Type.BOM,
    id: context.bomId,
    isDynamic: true
  });

  var assemblyItemRecord = record.load({
    type: record.Type.ASSEMBLY_ITEM,
    id: context.assemblyItemId,
    isDynamic: true
  });

  assemblyItemRecord.setValue({
    fieldId: 'billofmaterials',
    value: context.bomId
  });

  assemblyItemRecord.setValue({
    fieldId: 'billofmaterialsrevision',
    value: context.bomRevisionId
  });

  assemblyItemRecord.save();
}

// These values are customized per customer implementation
// You would set your default values here for values that are not exposed through the API
// At the time of writing, it was not possible to set the taxSchedules for type inventoryItem
function setCustomerSpecificDefaultValues(itemRecord, destinationType) {

  itemRecord.setValue({
    fieldId: 'taxschedule',
    value: 2,
  });

  itemRecord.setValue({
    fieldId: 'costcategory',
    value: 4,
  });

  // value below can be any of {Purchase|Resale|Sale}
  if (destinationType === "noninventoryresaleitem") {
    itemRecord.setValue({
      fieldId: 'subtype',
      value: 'resale'
    });

    itemRecord.setValue({
      fieldId: 'incomeAccount',
      value: 54
    });

    itemRecord.setValue({
      fieldId: 'expenseAccount',
      value: 616
    });

  }
}

function getValidTaxSchedules() {

  var types = Object.keys(search.Type);
  var taxTypes = types.filter(function (type) {
    return type.toLowerCase().indexOf('tax') !== -1;
  });

  log.debug({
    title: 'Searchable tax types',
    details: taxTypes
  });

  var taxScheduleSearch = search.create({
    type: search.Type.TAX_TYPE,
    columns: ['internalid']
  });

  var taxSchedules = taxScheduleSearch.run().getRange({
    start: 0,
    end: 10
  });

  return taxSchedules;
}

function decodeBase64(base64String, encode) {
  // Decode the Base64 string using N/encode module
  var decodedString = encode.convert({
    string: base64String,
    inputEncoding: encode.Encoding.BASE_64,
    outputEncoding: encode.Encoding.UTF_8
  });
  return decodedString;
}

function validateApplicationName(applicationHostName, error) {
  if (applicationHostName !== 'dev.sharpsync.net' && applicationHostName !== 'localhost') {
    throw error.create({
      name: 'INVALID_APPLICATION',
      message: 'Invalid application',
      notifyOff: true
    });
  }
}

function validateSharpSyncId(sharpsyncId, error) {
  if (!sharpsyncId) {
    throw error.create({
      name: 'MISSING_SHARPSYNC_ID',
      message: 'Missing SharpSync Id',
      notifyOff: true
    });
  }
}

function validateOrganizationId(organizationId, error) {
  if (!organizationId) {
    throw error.create({
      name: 'MISSING_ORG_ID',
      message: 'Missing organization id',
      notifyOff: true
    });
  }
}

function validateAppState(base64String, encode, error) {

  const jsonString = decodeBase64(base64String, encode);
  const jsonObject = JSON.parse(jsonString);

  validateApplicationName(jsonObject['applicationName'], error);
  validateSharpSyncId(jsonObject['sharpsyncId'], error);
  validateOrganizationId(jsonObject['organizationId'], error);
}

function validatePostRequestPrereqs(requestBody, error) {
  validateApplicationName(requestBody['applicationName'], error);
  validateSharpSyncId(requestBody['sharpsyncId'], error);
  validateOrganizationId(requestBody['organizationId'], error);
}

function validatePostRequestItemType(context, error, log) {

  if (context.itemType !== "inventoryitem" && context.itemType !== "assemblyitem" && context.itemType !== "noninventoryresaleitem" && context.itemType !== "bom" && context.itemType !== "bom") {
    log.error({
      title: 'Invalid item type',
      details: 'Invalid item type (' + context.itemType + '), must be one of {inventoryitem|assemblyitem|noninventoryresaleitem|noninventorypurchaseitem|bom}',
      notifyoff: true
    });

    throw error.create({
      name: 'INVALID_ITEM_TYPE',
      message: 'Invalid item type (' + context.itemType + '), must be one of {inventoryitem|assemblyitem|noninventoryresaleitem|noninventorypurchaseitem|bom}',
      notifyOff: true
    });
  }
}

function createBomFromContext(context, record) {
  var itemRecord = record.create({
    type: record.Type.BOM,
    isDynamic: true,
    name: context.name,
    includechildren: true,
    usecomponentyield: true,
  });

  // set the subsidiary 
  itemRecord.setValue({
    fieldId: 'subsidiary',
    value: 1,
  });

  for (var key in context) {
    if (itemRecord.hasOwnProperty(key)) {
      itemRecord.setValue({
        fieldId: key,
        value: context[key]
      });
    }
  }

  return itemRecord;
}
```
