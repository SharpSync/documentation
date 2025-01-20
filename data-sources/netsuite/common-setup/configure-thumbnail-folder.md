# Thumbnail Folder

## NetSuite Setup of Thumbnail folder

To successfully upload thumbnails to NetSuite, you need the following 3 things:

* OAuth `clientId` and `clientSecret` (aka consumer key and consumer secret)
* A `scriptId`
* A `folderId`
* A variable to assign the fileId to

Steps involved in setting up NetSuite:

* [x] [Setup OAuth ](../)- gives you permissions to upload files + `clientId` and `clientSecret`
* [x] Setup Restlet for uploading thumbnails (SuiteApi) - gives you the `scriptId`
* [x] Setup an upload folder - gives you the `folderId`
* [x] Putting it all together - configuring the datasource in SharpSync

***

### Setup OAuth

**Goal**: To get the the necessary permissions to upload files

Steps required:

* Setup OAuth (this step)
* Install SuiteApi
* Create a new thumbnails folder in NetSuite
* Setup the datasource to point to the new restlet

See NetSuite OAuth Setup

***

### Setup restlet for uploading thumbnails (SuiteApi)

Steps required:

* (done) Setup OAuth
* Install SuiteApi (this step)
* Create a new thumbnails folder in NetSuite
* Setup the datasource to point to the new restlet

See Setup restlet for uploading thumbnails (SuiteApi) Original source Suite API Setup

***

### Setup an upload folder

**Goal**: To get a folder id

Steps required:

* (done) Setup OAuth
* (done) Install SuiteApi
* Create a new thumbnails folder in NetSuite (this step)
* Setup the datasource to point to the new restlet

See Setup an upload folder See Create an upload folder

### Putting it all together - Setup the datasource

**Goal**: To configure the datasource in SharpSync

Steps required:

* (done) Setup OAuth
* (done) Install SuiteApi
* (done) Create a new thumbnails folder in NetSuite
* Setup the datasource to point to the new restlet (this step)

With the copied URL in the previous step, and the `folderId` and `scriptId` in hand, it's time to setup the datasource in SharpSync.

In SharpSync add a new data source > NetSuite.

#### Configuring NetSuite

There are 2 configuration sections for each datasource

* Authentication
* BOM Configuration

**Authentication**

* Navigate to the Datasources > Config UI
* Select the NetSuite datasource > Add
* Set the server url > Click 'Update'
* Click the Ping button to test the connection. This is optional and will not work if ICMP is disabled on the server
* Click the 'Configure' button
*   Enter the following values:

    | Name                | Description                                                                                                                                    | Recommended value                                                                 |
    | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
    | Base API Path       | The location where data is pulled from                                                                                                         | https://{companyId}.suitetalk.api.netsuite.com                                    |
    | Authentication Type | Which authentication method you'd like to use to connect to NetSuite                                                                           | OAuth 2.0                                                                         |
    | OAuth Url           | The URL against which your OAuth authentication occurs                                                                                         | https://{companyId}.app.netsuite.com/app/login/oauth2/authorize.nl                |
    | OAuth Token Url     | The URL against which your OAuth token is refreshed                                                                                            | https://{companyId}.suitetalk.api.netsuite.com/services/rest/auth/oauth2/v1/token |
    | Client Id           | The client id generated by the integration record created                                                                                      | -                                                                                 |
    | Client Secret       | The client secrete generated by the integration record created                                                                                 | -                                                                                 |
    | OAuth Scopes        | The scopes required to get data. The `restlets` scope is optional but highly recommended. Without it you will not be able to upload thumbnails | rest\_webservices,restlets                                                        |

**BOM Configuration**

| Name                           | Description                                                                                                                               | Recommended value                                                                                                                                                                             |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Top level assembly column name | When querying the data you may use this value to identify assemblies (optional)                                                           | itemid                                                                                                                                                                                        |
| SuiteQL BOM Query              | The query used to select data using SuiteQl                                                                                               | `SELECT ItemID, id, displayName FROM Item WHERE ItemID = '{uniquePartNumber}'`. Note that the place holder `{uniquePartNumber}` must stay as this is used to find items related to assemblies |
| Servlet URL                    | The url of the servlet where thumbnails will be uploaded. Optional but recommented. If specified, include the `folderId` param at the end | https://{companyId}.restlets.api.netsuite.com/app/site/hosting/restlet.nl?script=2943\&deploy=1\&folderId={folderId}                                                                          |
| Thumbnail column name          | The column to update in NetSuite using the file ID of the uploaded thumbnail                                                              | custitem\_mycol\_image                                                                                                                                                                        |

* Enter the URL of the restlet, followed by `&folderId=` and the id of the folder in the first steps
* Enter the folder ID into the UI in the form

> `{{netsuite-api}}`/app/site/hosting/restlet.nl?script={yourScriptId}\&deploy=1\&folderId={folderId}

e.g.

> `{{netsuite-api}}`/app/site/hosting/restlet.nl?script=`2743`\&deploy=1\&folderId=`19578359`

See also

* OAuth