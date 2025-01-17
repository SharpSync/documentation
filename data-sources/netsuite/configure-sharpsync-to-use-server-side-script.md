# Configure SharpSync to use Server Side Script

Server side scripting allows SharpSync to configure fields in NetSuite that are not available via the REST api.

* Add Netsuite as a data source
* Click the `Configure` button
* Navigate to the `Configuration` section
*   Add a servlet url

    > https://\[companyId].restlets.api.netsuite.com/app/site/hosting/restlet.nl?script=3187\&itemInjectionScriptId=3187\&deploy=1\&folderId=3561
* The `itemInjectionScriptId` param above is the same as the one in the previous step
* Select `Use server side scripting`
* Select `Use advanced BOMs`
* Configure your `Bom Naming scheme`
* Configure your `Bom Revision Naming Scheme`
* Click the Save button

A custom script example follows below. You can certainly modify this to your heart's content, but the overarching details are captured here

Before uploading to Netsuite, save the below contents to a `*.js` file. See the example called Example script
