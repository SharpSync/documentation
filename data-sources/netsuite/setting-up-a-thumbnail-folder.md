---
icon: folder-open
---

# Setting up a thumbnail folder

While not strictly necessary, it is recommended to setup a thumbnails folder in NetSuite. This lets you generate an image preview for a file in NetSuite.

## NetSuite Setup of Thumbnail folder

To successfully upload thumbnails to NetSuite, you need the following:

* OAuth `clientId` and `clientSecret` (aka consumer key and consumer secret)
* A `scriptId`
* A `folderId`
* A variable to assign the `fileId` to

Steps involved in setting up NetSuite:

* [Setup OAuth](oauth-setup/) - gives you permissions to upload files + `clientId` and `clientSecret`
* Setup restlet for uploading thumbnails ([SuiteApi](suite-api-setup.md)) - gives you the `scriptId`
* Setup an [upload folder](create-an-uploads-folder.md) - gives you the `folderId`

### Putting it all together - Setup the datasource

With the copied URL in the previous step, and the `folderId` and `scriptId` in hand, it's time to setup the datasource in SharpSync.

In SharpSync add a new data source > NetSuite.

#### Configuring NetSuite

There are 2 configuration sections for each Data Source

* Authentication
* Configuration

Check out [Authentication + Configuration](authentication-+-configuration.md) for instructions.

