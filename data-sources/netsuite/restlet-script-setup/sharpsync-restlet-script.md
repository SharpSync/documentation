---
icon: js
---

# SharpSync RESTlet Script

You'll find in this page the SharpSync NetSuite Restlet script that is required to allow SharpSync to perform certain operations that are not typically available from the standard NetSuite API.

Both NetSuite users of simple and advanced BOMs should upload this script.

This script is needed for the creation of files in NetSuite (such as PNG files for thumbnails), that can be used in both cases of simple and advanced BOMs.

The script is needed for the creation of items (`assemblyitem` , `inventoryitem`, etc...) in the case of advanced BOMs. Item creation for advanced BOMS requires providing a value for `taxschedule` that is not readily queryable from the standard API.&#x20;

<mark style="color:orange;">**TODO: Update the**</mark><mark style="color:orange;">**&#x20;**</mark><mark style="color:orange;">**`organizationSpecificTaxScheduleId`**</mark><mark style="color:orange;">**&#x20;**</mark><mark style="color:orange;">**variable value in the code below on line 21 to match your organization specific default**</mark><mark style="color:orange;">**&#x20;**</mark><mark style="color:orange;">**`taxschedule`**</mark><mark style="color:orange;">**&#x20;**</mark><mark style="color:orange;">**id for newly created items.**</mark>

Before uploading to Netsuite, save the below contents to a `*.js` file (example `sharpsync-restlet-script.js`).

{% code lineNumbers="true" %}
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
define([
  "N/record",
  "N/search",
  "N/log",
  "N/encode",
  "N/error",
  "N/file",
], function (record, search, log, encode, error, file) {
  const SHARPSYNC_RESTLET_SCRIPT_VERSION = "2.0.0";
  // TODO - UPDATE/CHANGE THIS TO YOUR ORGANIZATION'S DEFAULT TAX SCHEDULE ID
  const organizationSpecificTaxScheduleId = 1;

  const sharpSyncConstants_e = {
    APPLICATION_NAME: "applicationName",
    SHARPSYNC_ID: "sharpsyncId",
    ORGANIZATION_ID: "organizationId",
    PROCEDURE: "procedure",
    ITEM_TYPE: "itemType",
    NAME: "name",
    CUSTOM_PROPERTIES: "customProperties",
    RELATED_ITEM_ID: "relatedItemId",
    RECORD_TYPE: "recordType",
    RECORD_PROPERTIES: "recordProperties",
  };

  const procedureTypes_e = {
    CREATE_FILE: "createFile",
    CREATE_ITEM: "createItem",
    CREATE_RECORD: "createRecord",
  };

  const itemTypes_e = {
    ASSEMBLY_ITEM: "assemblyItem",
    INVENTORY_ITEM: "inventoryItem",
    NON_INVENTORY_RESALE_ITEM: "nonInventoryResaleItem",
    NON_INVENTORY_PURCHASE_ITEM: "nonInventoryPurchaseItem",
  };

  const getQueryTypeObjects_e = {
    TAX_SCHEDULE: "taxSchedule",
  };

  const getQueryResponseType_e = {
    LIST: "list",
    SINGLE: "single",
  };

  function throwParameterError(name, message) {
    throw error.create({
      name: name,
      message: message,
      notifyOff: true,
    });
  }

  function throwArrayParameterError(errorParamName, arrayValue, array) {
    throw error.create({
      name: errorParamName,
      message:
        "Invalid value (" +
        arrayValue +
        ") must be one of {" +
        array.join("|") +
        "}",
      notifyOff: true,
    });
  }

  function decodeBase64(encode, base64String) {
    // Decode the Base64 string using N/encode module
    var decodedString = encode.convert({
      string: base64String,
      inputEncoding: encode.Encoding.BASE_64,
      outputEncoding: encode.Encoding.UTF_8,
    });

    return decodedString;
  }

  function isValidUUID(uuid) {
    if (!uuid) {
      return false;
    }

    // Regular expression for validating UUID format
    var uuidPattern =
      /^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/i;
    // Check if the UUID matches the pattern and is not all zeros
    return (
      uuidPattern.test(uuid) && uuid !== "00000000-0000-0000-0000-000000000000"
    );
  }

  function validateApplicationName(applicationHostName) {
    if (applicationHostName !== "app.sharpsync.net") {
      throwParameterError(
        "INVALID_APPLICATION",
        "Invalid application: (" + applicationHostName + ")"
      );
    }
  }

  function validateSharpSyncId(sharpsyncId) {
    if (!sharpsyncId) {
      throwParameterError("MISSING_SHARPSYNC_ID", "Missing SharpSync user id");
    }
    if (!isValidUUID(sharpsyncId)) {
      throwParameterError("INVALID_SHARPSYNC_ID", "Invalid SharpSync user id");
    }
  }

  function validateOrganizationId(organizationId) {
    if (!organizationId) {
      throwParameterError(
        "MISSING_ORG_ID",
        "Missing SharpSync organization id"
      );
    }
    if (!isValidUUID(organizationId)) {
      throwParameterError(
        "INVALID_ORG_ID",
        "Invalid SharpSync organization id"
      );
    }
  }

  function arrayIncludesCaseInsensitive(array, value) {
    var lowerCaseArray = [];
    for (var i = 0; i < array.length; i++) {
      lowerCaseArray.push(array[i].toLowerCase());
    }
    return lowerCaseArray.indexOf(value.toLowerCase()) > -1;
  }

  function getArrayFromEnum(enumObj) {
    var enumArray = [];
    for (var key in enumObj) {
      if (enumObj.hasOwnProperty(key)) {
        enumArray.push(enumObj[key]);
      }
    }
    return enumArray;
  }

  function validateGetRequestAppState(requestUrlObj, encode, log) {
    var jsonString = decodeBase64(encode, requestUrlObj.appState);
    var jsonObject = JSON.parse(jsonString);

    validateApplicationName(jsonObject[sharpSyncConstants_e.APPLICATION_NAME]);
    validateSharpSyncId(jsonObject[sharpSyncConstants_e.SHARPSYNC_ID]);
    validateOrganizationId(jsonObject[sharpSyncConstants_e.ORGANIZATION_ID]);

    var queryArray = getArrayFromEnum(getQueryResponseType_e);

    if (
      arrayIncludesCaseInsensitive(queryArray, requestUrlObj.queryType) ===
      false
    ) {
      throwArrayParameterError(
        "INVALID_QUERY_TYPE",
        jsonObject["queryType"],
        queryArray
      );
    }
  }

  function validatePostRequestBody(requestBody) {
    validateApplicationName(requestBody[sharpSyncConstants_e.APPLICATION_NAME]);
    validateSharpSyncId(requestBody[sharpSyncConstants_e.SHARPSYNC_ID]);
    validateOrganizationId(requestBody[sharpSyncConstants_e.ORGANIZATION_ID]);
  }

  function listTaxSchedules(search, log) {
    var taxScheduleSearch = search.create({
      type: "taxschedule",
      columns: ["internalid", "name"],
    });

    var taxSchedules = [];
    taxScheduleSearch.run().each(function (result) {
      taxSchedules.push({
        id: result.getValue("internalid"),
        name: result.getValue("name"),
      });
      return true;
    });

    log.debug({
      title: "Retrieved Tax Schedules",
      details: JSON.stringify(taxSchedules),
    });

    return taxSchedules;
  }

  function createItemFromContext(context, record, log) {
    var itemRecord;
    var isDynamic = true;

    var itemTypesArray = getArrayFromEnum(itemTypes_e);

    if (
      arrayIncludesCaseInsensitive(
        itemTypesArray,
        context[sharpSyncConstants_e.ITEM_TYPE]
      ) === false
    ) {
      throwParameterError(
        "INVALID_ITEM_TYPE",
        "Invalid item type (" +
          context[sharpSyncConstants_e.ITEM_TYPE] +
          "), must be one of " +
          itemTypesArray.join("|")
      );
    }

    switch (context[sharpSyncConstants_e.ITEM_TYPE]) {
      case "assemblyitem":
        itemRecord = record.create({
          type: record.Type.ASSEMBLY_ITEM,
          isDynamic: isDynamic,
        });
        break;
      case "inventoryitem":
        itemRecord = record.create({
          type: record.Type.INVENTORY_ITEM,
          isDynamic: isDynamic,
        });
        break;
      case "noninventorypurchaseitem":
        itemRecord = record.create({
          type: record.Type.NON_INVENTORY_ITEM,
          isDynamic: isDynamic,
        });
        itemRecord.setValue({ fieldId: "subtype", value: "purchase" });
        break;
      case "noninventoryresaleitem":
        itemRecord = record.create({
          type: record.Type.NON_INVENTORY_ITEM,
          isDynamic: isDynamic,
        });
        itemRecord.setValue({ fieldId: "subtype", value: "resale" });
        break;
      default:
        throwParameterError(
          "INVALID_ITEM_TYPE",
          "Invalid item types (" +
            context[sharpSyncConstants_e.ITEM_TYPE] +
            ") sent, must be one of " +
            itemTypesArray.join("|")
        );
    }

    // Set the item name
    itemRecord.setValue({
      fieldId: "itemid",
      value: context[sharpSyncConstants_e.NAME],
    });

    // Set the organization specific taxschedule as this is a required field for items
    // And this field is not queryable from the NetSuite API
    itemRecord.setValue({
      fieldId: "taxschedule",
      value: organizationSpecificTaxScheduleId,
    });

    for (var customPropertyKey in context[
      sharpSyncConstants_e.CUSTOM_PROPERTIES
    ]) {
      if (
        context[sharpSyncConstants_e.CUSTOM_PROPERTIES].hasOwnProperty(
          customPropertyKey
        )
      ) {
        var customPropertyValue =
          context[sharpSyncConstants_e.CUSTOM_PROPERTIES][customPropertyKey];
        var setValue;

        // Check if the value is an object and has an 'id' property (for single select fields)
        if (
          typeof customPropertyValue === "object" &&
          customPropertyValue !== null &&
          customPropertyValue.hasOwnProperty("id")
        ) {
          setValue = customPropertyValue.id;
        }
        // Check if the value is an object and has an 'items' array property (for multi-select fields)
        else if (
          typeof customPropertyValue === "object" &&
          customPropertyValue !== null &&
          customPropertyValue.hasOwnProperty("items") &&
          Array.isArray(customPropertyValue.items) // Checks if it's an array
        ) {
          // For 'items', NetSuite expects an array of internal IDs.
          // We map the array of objects [{id: '1'}, {id: '2'}] to ['1', '2']
          setValue = customPropertyValue.items.map(function (item) {
            // This map correctly transforms it
            return item.id;
          });
        }
        // Otherwise, it's a simple primitive value (string, number, boolean)
        else {
          setValue = customPropertyValue;
        }

        // Skip setting itemid again if it's in custom properties (already set above)
        // The taxschedule will never be in custom properties, unless future changes are made to the NetSuite API
        if (customPropertyKey === "itemid") {
          continue;
        }

        try {
          itemRecord.setValue({
            // Convert the custom property key to lower case to match NetSuite's field ID convention
            fieldId: customPropertyKey.toLowerCase(),
            value: setValue,
          });
        } catch (e) {
          log.error({
            title: "Error setting custom property: " + customPropertyKey,
            details:
              "Value: " +
              JSON.stringify(customPropertyValue) +
              ", Error: " +
              e.message,
          });
        }
      }
    }

    return itemRecord;
  }

  function createFileFromContext(context) {
    if (
      typeof context.name == "undefined" ||
      context.name === null ||
      context.name === ""
    ) {
      throwParameterError("MISSING_PARAMETER", "No name was specified.");
    }

    if (
      typeof context.fileType == "undefined" ||
      context.fileType === null ||
      context.fileType === ""
    ) {
      throwParameterError("MISSING_PARAMETER", "No fileType was specified.");
    }

    if (typeof context.contents == "undefined" || context.contents === null) {
      throwParameterError("MISSING_PARAMETER", "No contents was specified.");
    }

    if (
      typeof context.description == "undefined" ||
      context.description === null
    ) {
      throwParameterError("MISSING_PARAMETER", "No description was specified.");
    }

    if (typeof context.encoding == "undefined" || context.encoding === null) {
      throwParameterError("MISSING_PARAMETER", "No encoding was specified.");
    }

    if (
      typeof context.folderID == "undefined" ||
      context.folderID === null ||
      context.folderID === ""
    ) {
      throwParameterError("MISSING_PARAMETER", "No folderID was specified.");
    }

    if (
      typeof context.isOnline == "undefined" ||
      context.isOnline === null ||
      context.isOnline === ""
    ) {
      context.isOnline = false;
    }

    if (
      typeof context.isInactive == "undefined" ||
      context.isInactive === null ||
      context.isInactive === ""
    ) {
      context.isInactive = false;
    }

    var fileObj = file.create({
      name: context.name,
      fileType: context.fileType,
      contents: context.contents,
      description: context.description,
      encoding: context.encoding,
      folder: context.folderID,
      isOnline: context.isOnline,
      isInactive: context.isInactive,
    });

    var fileID = fileObj.save();

    fileObj = file.load({ id: fileID });

    var response = {};
    response.file = {};
    response.file.info = fileObj;

    if (
      typeof context.returnContent != "undefined" &&
      context.returnContent === true
    ) {
      var contents = fileObj.getContents();

      contents = encode.convert({
        string: contents,
        inputEncoding: encode.Encoding.UTF_8,
        outputEncoding: encode.Encoding.BASE_64,
      });

      response.file.content = contents;
    }

    return response;
  }

  createRecordFromContext = function (context) {
    if (
      typeof context[sharpSyncConstants_e.RECORD_TYPE] == "undefined" ||
      context[sharpSyncConstants_e.RECORD_TYPE] === null ||
      context[sharpSyncConstants_e.RECORD_TYPE] === ""
    ) {
      throwParameterError("MISSING_PARAMETER", "No record type was specified.");
    }

    if (
      typeof context[sharpSyncConstants_e.RELATED_ITEM_ID] == "undefined" ||
      context[sharpSyncConstants_e.RELATED_ITEM_ID] === null ||
      context[sharpSyncConstants_e.RELATED_ITEM_ID] === ""
    ) {
      throwParameterError(
        "MISSING_PARAMETER",
        "No related item id was specified."
      );
    }

    var recordToCreate = record.create({
      type: context[sharpSyncConstants_e.RECORD_TYPE],
      defaultValues: { item: context[sharpSyncConstants_e.RELATED_ITEM_ID] },
      isDynamic: true,
    });

    for (var key in context[sharpSyncConstants_e.RECORD_PROPERTIES]) {
      if (context[sharpSyncConstants_e.RECORD_PROPERTIES].hasOwnProperty(key)) {
        var value = context[sharpSyncConstants_e.RECORD_PROPERTIES][key];
        recordToCreate.setValue({ fieldId: key, value: value });
      }
    }

    var recordId = recordToCreate.save();

    log.debug({
      title: "Record created successfully via SharpSync RESTlet",
      details:
        "Record Type: " +
        context[sharpSyncConstants_e.RECORD_TYPE] +
        " - " +
        "Record ID: " +
        recordId,
    });

    return recordId;
  };

  /*
  SharpSync always uses the following appState to confirm validity for GET and POST requests:
  X-APPLICATION => header naming the application
  X-USERID => header naming the user (uuid)
  X-ORG_ID => header naming the organization
  */

  function get(context) {
    log.debug({
      title: "GET request received for SharpSync RESTlet",
      details: "Version: " + SHARPSYNC_RESTLET_SCRIPT_VERSION,
    });

    validateGetRequestAppState(context, encode, log);
    if (context.queryType === getQueryResponseType_e.LIST) {
      if (context.queryTypeObject === getQueryTypeObjects_e.TAX_SCHEDULE) {
        return listTaxSchedules(search, log);
      }
    }

    if (!context[sharpSyncConstants_e.NAME]) {
      throwParameterError(
        "MISSING_ITEM_NAME",
        "Missing required argument: name"
      );
    }

    return { name: context[sharpSyncConstants_e.NAME] };
  }

  function post(context) {
    log.debug({
      title: "POST request received for SharpSync RESTlet",
      details: "Version: " + SHARPSYNC_RESTLET_SCRIPT_VERSION,
    });

    validatePostRequestBody(context);

    if (
      context[sharpSyncConstants_e.PROCEDURE] == procedureTypes_e.CREATE_ITEM
    ) {
      var itemRecord = createItemFromContext(context, record, log);
      // Save the new item and get its id
      var itemId = itemRecord.save();

      // must parse. For some reason when accessing it directly you get null
      var serializedItemRecord = JSON.parse(JSON.stringify(itemRecord));

      return {
        applicationName: context[sharpSyncConstants_e.APPLICATION_NAME],
        sharpsyncId: context[sharpSyncConstants_e.SHARPSYNC_ID],
        organizationId: context[sharpSyncConstants_e.ORGANIZATION_ID],
        itemId: itemId,
        itemName: context[sharpSyncConstants_e.NAME],
        destinationTypeRequested: context[sharpSyncConstants_e.ITEM_TYPE],
        baseRecordTypeName:
          serializedItemRecord.fields.baserecordtype || "unknown",
        url:
          "/services/rest/record/v1/" +
          serializedItemRecord.fields.baserecordtype +
          "/" +
          itemId +
          "/?expandSubResources=true",
      };
    } else if (
      context[sharpSyncConstants_e.PROCEDURE] == procedureTypes_e.CREATE_FILE
    ) {
      return createFileFromContext(context);
    } else if (
      context[sharpSyncConstants_e.PROCEDURE] == procedureTypes_e.CREATE_RECORD
    ) {
      return createRecordFromContext(context);
    }

    throwParameterError(
      "PROCEDURE_NOT_SUPPORTED",
      "The context procedure is not valid or not yet supported"
    );
  }

  return {
    get: get,
    post: post,
  };
});

```
{% endcode %}
