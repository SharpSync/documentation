---
icon: chart-tree-map
description: '[docs in progress]'
---

# Odoo

_This document is a work in progress_

### Limitations at the time of writing



SharpSync supports different versions of Odoo:

* Odoo version 17.
* Odoo version 16. Should you require access to a different version please make contact with us at [SharpSync](https://sharpsync.net/about/)

Odoo is an open source ERP available in a selfhosted or cloud hosted option. Please also see:

* [Permissions required](https://github.com/SharpSync/docs/blob/main/datasources/odoo/markdown/permissions_required.md)
* [https://www.odoo.com/](https://www.odoo.com/),
* [Hosting options](https://github.com/SharpSync/docs/blob/main/datasources/odoo/markdown/hosting-options.md)

To configure an Odoo intance you need 4 things

* Odoo version number
* Database name (case sensitive)
* Username
* Password

### Setup steps



* Login on the application
* Navigate to data sources
* On the right > Select Odoo > Add
* Click the configure button
* On the first tab, enter the Database name, Username and password
* \[!Tip] To find the database name navigate to [https://your-odoo-instance/web/database/selector](https://your-odoo-instance/web/database/selector)
* On the second tab (BOM Configuration) enter the version number (Only version 17 supported at the time of writing, but contact us if you require access to older version)
* Click the `Save` button
* Click the `Authenticate` button

[![Configure Odoo](https://github.com/SharpSync/docs/raw/main/datasources/odoo/odoo-config-values.png)](https://github.com/SharpSync/docs/blob/main/datasources/odoo/odoo-config-values.png)

### Property mappings



After successfully authenticating with Odoo, navigate to the [property mappings](https://github.com/SharpSync/docs/blob/main/datasources/odoo/propertymapping/markdown/readme.md) The update should automatically trigger. If it does not, click the _Update_ button

The custom fields from Odoo has now succesfully been pulled into SharpSync.

Take note!: If you ever add new properties to Odoo, make sure to come back to the property mapping page and click the _Update_ button again.

SharpSync requires the MRP module (mrp) at a minimum in order to support an Odoo integration. This is because BOM information, more specifically the Quantities of individual items.

Routings are not currently supported but we are working towards it

### Extra information



For additional information please see:

* [List names](https://github.com/SharpSync/docs/blob/main/datasources/odoo/markdown/list_names.md)
* [Hosting options](https://github.com/SharpSync/docs/blob/main/datasources/odoo/markdown/hosting-options.md)
* [Permissions required](https://github.com/SharpSync/docs/blob/main/datasources/odoo/markdown/permissions_required.md)
