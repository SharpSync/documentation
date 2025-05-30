---
icon: js
---

# RESTlet Script Setup

## SuiteAPI Overview

SuiteAPI is an open source, alternative Web API for the NetSuite platform. It makes it easy for developers to integrate with NetSuite, regardless of the type of application that they're developing or the technology they're developing it with.

SharpSync has developed its own script based on the SuiteAPI open source code. You can follow the below steps to setup your NetSuite instance to use the SharpSync RESTlet script:&#x20;

#### Step: Save The SharpSync Script Locally

* See [SharpSync RESTlet Script](sharpsync-restlet-script.md)

#### Step: Upload Script File To NetSuite

* In NetSuite Navigate to `Documents` > `Files` > `File Cabinet`, select the system folder called `SuiteScripts`
* Click on the `New Folder` button again
  * Folder name: `SharpSync`
  * Subfolder of: `SuiteScripts`
  * Type `Documents and Files`
  * Click `Save`
* With the `SharpSync` folder selected in NetSuite, click `Add File`
* Select the `sharpsync-restlet-script.js` file saved from the previous step

#### Step: Create Script Deployment Record

* Navigate to `Setup` > `Customization` > `Scripts` > `New`&#x20;

or

* Navigate to `Customization` > `Scripting` > `Scripts` > `New`&#x20;
* In the Script file entry, enter `sharpsync-restlet-script.js`
* Click `Create Script Record`
* On the new form enter the values:
  * Name: `SharpSyncRESTletScript`
  * ID: `sharpsync-restlet-script`
  * Description: `Restlet for uploading files and thumbnails for SharpSync and creating items for advanced BOMS`
* Click on the `Deployments` tab at the bottom and in the first row set
  * Title: `SharpSyncRESTletScriptDeployment`
  * ID: `harpsync-restlet-script-deployment`
  * Deployed: `Yes`
  * Status: Change from `Testing` to `Released`
* Click `Save`
* Click the Deployments tab
* Click on the title of the deployment (`SharpSyncRESTletScriptDeployment`)
* Click `Edit` to edit the deployment
* Make sure you have checked the following audiences:
  * `Audience` --> `Roles` --> `All Roles`
  * `Audience` --> `Roles` --> `All Employees`
* Click `Save`
* Take note of the External URL. It will look something like this:
* https://{companyId}.restlets.api.netsuite.com/app/site/hosting/restlet.nl?script=925\&deploy=1
* Make sure to save the `scriptId` and `deploymentId` in the above url for later use

You are now done configuring the script in NetSuite.

## Original Source

SharpSync's script was developed based on the original work by [Tim Dietrich](https://timdietrich.me/).

See Also [SuiteAPI](https://suiteapi.com/)

If for some reason the website below is unavailable, a copy of the Suite API zip file can be found here: SuiteAPI-v2022.1.zip &#x20;

{% file src="../../../.gitbook/assets/SuiteAPI-v2022.1.zip" %}
Suite API zip
{% endfile %}

