---
icon: chart-tree-map
description: '[docs in progress]'
---

# Odoo

_This document is a work in progress_

### Limitations at the time of writing



SharpSync supports different versions of Odoo:

* Odoo version 17.
* Odoo version 16. Should you require access to a different version, please contact us at [SharpSync](https://sharpsync.net/about/)

Odoo is an open-source ERP available in a self-hosted or cloud-hosted option. Please also see:

* [Permissions required](https://github.com/SharpSync/docs/blob/main/datasources/odoo/markdown/permissions_required.md)
* [https://www.odoo.com/](https://www.odoo.com/),
* [Hosting options](https://github.com/SharpSync/docs/blob/main/datasources/odoo/markdown/hosting-options.md)

To configure an Odoo intance you need 4 things

* Odoo version number
* Database name (case sensitive)
* Username
* Password

### Setup steps

* In the Navigation Bar, select Data Sources
* On the right (in Data Sources), select Odoo as the Data Source and click ADD DATASOURCE
*   Change the Server URL to:

    ```
    https://{myEnterprizeInstance}.dev.odoo.com
    ```
* Scroll to the bottom of the page and click UPDATE
* Click CONFIGURE
* On the first tab (Authentication), change the Base API Path to: `https://{myEnterprizeInstance}.dev.odoo.com`
* Leave the Authentication Types dropdown on Basic Authentication.
* Enter the Database name.&#x20;

{% hint style="info" %}
To find the database name navigate to [https://{myEnterprizeInstance}/web/database/selector](https://your-odoo-instance/web/database/selector)
{% endhint %}

{% hint style="warning" %}
The database name is case sensitive.
{% endhint %}

* Enter the username and password.
* The configuration should look something like the image below:

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

* On the second tab (BOM Configuration) enter the version number (Only version 17 is supported at the time of writing, but contact us if you require access to an older version)
* Click the `Save` button to save and close the form.
* Click AUTHENTICATE. If the configuration is successful, the Authentication Status will update and show <mark style="color:green;">Connected</mark>.&#x20;
* Configure the Primary and Secondary Identifiers as follows:
  * Primary Identifier: name
  * Secondary Identifier: product\_tmpl\_id

After successfully authenticating with Odoo, the update should automatically trigger. If it does not, click the _Update_ button.

The custom fields from Odoo have now successfully been pulled into SharpSync.

{% hint style="info" %}
&#x20;If you ever add new properties to Odoo, make sure to come back to the property mapping page and click the _Update_ button again.
{% endhint %}

{% hint style="info" %}
SharpSync requires the MRP module (mrp) at a minimum in order to support an Odoo integration. This is because of BOM information, more specifically the Quantities of individual items.
{% endhint %}

Routings are not currently supported but we are working towards it.

Please make sure to set up your Secondary Data Source. For more information, refer to the other Data Sources listed in the Navigation Bar.

Once completed with the setup for both Primary and Secondary Data Sources, proceed to setup [Property Mappings](../../fundamentals/property-mappings.md).

### Extra information

For additional information please see:

* [List names](https://github.com/SharpSync/docs/blob/main/datasources/odoo/markdown/list_names.md)
* [Hosting options](https://github.com/SharpSync/docs/blob/main/datasources/odoo/markdown/hosting-options.md)
* [Permissions required](https://github.com/SharpSync/docs/blob/main/datasources/odoo/markdown/permissions_required.md)
