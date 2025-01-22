---
icon: chart-tree-map
---

# Odoo

Odoo is an open source ERP and e-commerce platform

## Odoo supported features

Out of the box, the Odoo integration supports the following features for all supported versions:

### Bill of Material (BOM) level features

| Feature                                        |      Differences     |     Modifications    |          Updates          |
| ---------------------------------------------- | :------------------: | :------------------: | :-----------------------: |
| BOM hierarchy                                  | :white\_check\_mark: | :white\_check\_mark: |    :white\_check\_mark:   |
| BOM meta data                                  | :white\_check\_mark: | :white\_check\_mark: |    :white\_check\_mark:   |
| BOM quantities                                 | :white\_check\_mark: | :white\_check\_mark: |    :white\_check\_mark:   |
| Component thumbnails                           |          N/A         |          N/A         |    :white\_check\_mark:   |
| Advanced BOMs                                  | :white\_check\_mark: | :white\_check\_mark: |    :white\_check\_mark:   |
| Attributes and variants                        |  \[work in progress] |  \[work in progress] |    \[work in progress]    |
| Routings                                       | :white\_check\_mark: | :white\_check\_mark: |    :white\_check\_mark:   |
| File derivative transfers (e.g. STEP, DXF)\*\* |          N/A         |          N/A         | \[marked for development] |

\*\* It should be noted that there are many ways to transfer files. We're in the process of adding file transfers for NetSuite, but please note that consultation services are required to understand your use case + configuration options. Each customer's implementation of file transfers will be unique.

SharpSync supports different versions of Odoo:

* Odoo 18 is slated for early 2025
* Odoo version 17 \[full support].
* Odoo version 16 \[full support].&#x20;

| Item    | Status            | Support |
| ------- | ----------------- | ------- |
| Odoo 18 | Slated early 2025 | N/A     |
| Odoo 17 | Available         | Full    |
| Odoo 16 | Available         | Full    |



Should you require access to a different version, please contact us at [SharpSync](https://sharpsync.net/about/)

Odoo is an open-source ERP available in a self-hosted or cloud-hosted option. Please also see:

* [Permissions required](https://github.com/SharpSync/docs/blob/main/datasources/odoo/markdown/permissions_required.md)
* [https://www.odoo.com/](https://www.odoo.com/),
* [Hosting options](https://github.com/SharpSync/docs/blob/main/datasources/odoo/markdown/hosting-options.md)

To configure an Odoo instance in SharpSync you need at least the following 4 things

* Odoo version number
* Database name (case sensitive)
* Username
* Password

### Configuration

* [Authentication](getting-started/authentication-+-configuration.md)

After successfully authenticating with Odoo, the update should automatically trigger. If it does not, click the _Update_ button.

The custom fields from Odoo have now successfully been pulled into SharpSync.

{% hint style="info" %}
&#x20;If you ever add new properties to Odoo, make sure to come back to the property mapping page and click the _Update_ button again.
{% endhint %}

{% hint style="info" %}
SharpSync requires the MRP module (mrp) at a minimum in order to support an Odoo integration. This is because of BOM information, more specifically the Quantities of individual items are managed using this module.
{% endhint %}

Routings are not currently supported but we are working towards it.

Please make sure to set up your Secondary Data Source. For more information, refer to the other Data Sources listed in the Navigation Bar.

Once completed with the setup for both Primary and Secondary Data Sources, proceed to setup [Property Mappings](../../fundamentals/property-mappings.md).

### Extra information

For additional information please see:

* [List names](https://github.com/SharpSync/docs/blob/main/datasources/odoo/markdown/list_names.md)
* [Hosting options](https://github.com/SharpSync/docs/blob/main/datasources/odoo/markdown/hosting-options.md)
* [Permissions required](https://github.com/SharpSync/docs/blob/main/datasources/odoo/markdown/permissions_required.md)
