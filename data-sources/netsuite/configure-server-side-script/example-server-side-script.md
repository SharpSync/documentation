---
icon: js
---

# Example Server Side Script

A custom script example follows below.&#x20;

TODO: Update the `organizationSpecificTaxScheduleId` variable value in the code below on line 21 to match your organization specific default `taxschedule` id for newly created items.

Before uploading to Netsuite, save the below contents to a `*.js` file.

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
define(["N/record", "N/search", "N/log", "N/encode", "N/error"], function (
  record,
  search,
  log,
  encode,
  error
) {
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
  };

  const procedureTypes_e = {
    CREATE_FILE: "createFile",
    CREATE_ITEM: "createItem",
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

  function fileCreate(request) {
    if (
      typeof request.name == "undefined" ||
      request.name === null ||
      request.name === ""
    ) {
      throwParameterError("MISSING_PARAMETER", "No name was specified.");
    }

    if (
      typeof request.fileType == "undefined" ||
      request.fileType === null ||
      request.fileType === ""
    ) {
      throwParameterError("MISSING_PARAMETER", "No fileType was specified.");
    }

    if (typeof request.contents == "undefined" || request.contents === null) {
      throwParameterError("MISSING_PARAMETER", "No contents was specified.");
    }

    if (
      typeof request.description == "undefined" ||
      request.description === null
    ) {
      throwParameterError("MISSING_PARAMETER", "No description was specified.");
    }

    if (typeof request.encoding == "undefined" || request.encoding === null) {
      throwParameterError("MISSING_PARAMETER", "No encoding was specified.");
    }

    if (
      typeof request.folderID == "undefined" ||
      request.folderID === null ||
      request.folderID === ""
    ) {
      throwParameterError("MISSING_PARAMETER", "No folderID was specified.");
    }

    if (
      typeof request.isOnline == "undefined" ||
      request.isOnline === null ||
      request.isOnline === ""
    ) {
      request.isOnline = false;
    }

    if (
      typeof request.isInactive == "undefined" ||
      request.isInactive === null ||
      request.isInactive === ""
    ) {
      request.isInactive = false;
    }

    var fileObj = file.create({
      name: request.name,
      fileType: request.fileType,
      contents: request.contents,
      description: request.description,
      encoding: request.encoding,
      folder: request.folderID,
      isOnline: request.isOnline,
      isInactive: request.isInactive,
    });

    var fileID = fileObj.save();

    fileObj = file.load({ id: fileID });

    var response = {};
    response.file = {};
    response.file.info = fileObj;

    if (
      typeof request.returnContent != "undefined" &&
      request.returnContent === true
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

  /*
  SharpSync always uses the following appState to confirm validity for GET and POST requests:
  X-APPLICATION => header naming the application
  X-USERID => header naming the user (uuid)
  X-ORG_ID => header naming the organization
  */

  function get(context) {
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
      return fileCreate(context);
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
