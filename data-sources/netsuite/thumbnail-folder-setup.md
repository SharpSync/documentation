---
icon: folder-open
---

# Thumbnail Folder Setup

While not strictly necessary, it is recommended to setup a thumbnails folder in NetSuite. This lets you place created image files in a convenient location in NetSuite to be able to use as item thumbnails for example.

### Create A NetSuite Thumbnails Folder

* Navigate to NetSuite
* Click on the `Documents` menu item
* Click on the `Files` sub menu item > File Cabinet
* Click on the `New Folder` button
  * Folder name: `SharpSync`
  * Subfolder of: nothing
  * Type `Documents and Files`
  * Click `Save`
* Click on the `New Folder` button again
  * Folder name: `Thumbnails`
  * Subfolder of: `SharpSync`
  * Type `Documents and Files`
  * Click `Save`

With this last folder selected in the file cabinet, take note of the folder id in the URL. This will be used later. The URL at the top will look something like this:

> https://`{companyId}`.app.netsuite.com/app/common/media/mediaitemfolders.nl?`folder=20149768`\&whence=\&cmid=...

Copy the value of the `folderId` and keep it for later reference

