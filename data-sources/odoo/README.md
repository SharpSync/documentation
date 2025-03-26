---
icon: chart-tree-map
---

# Odoo

Odoo is an open source ERP and e-commerce platform

## Odoo supported features

Out of the box, the Odoo integration supports the following features for all supported versions:

### Bill of Material (BOM) level features

<table><thead><tr><th width="341.11328125">Feature</th><th width="100.5390625" align="center">Read</th><th width="121.1328125" align="center">Create</th><th width="142.67578125" align="center">Update</th></tr></thead><tbody><tr><td>BOM hierarchy</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td></tr><tr><td>BOM meta data</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td></tr><tr><td>BOM quantities</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td></tr><tr><td>Component thumbnails</td><td align="center">N/A</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td></tr><tr><td>Advanced BOMs</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td></tr><tr><td>Attributes and variants</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span>***</td></tr><tr><td>Routings</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td></tr><tr><td>File derivative transfers (e.g. STEP, DXF)**</td><td align="center">N/A</td><td align="center">N/A</td><td align="center">[on roadmap]</td></tr></tbody></table>



\*\* It should be noted that there are many ways to transfer files. We're in the process of adding file transfers for NetSuite, but please note that consultation services are required to understand your use case + configuration options. Each customer's implementation of file transfers will be unique.

\*\*\* Single attribute, single value. Multiple attributes are scheduled for development.

SharpSync supports different versions of Odoo:

* Odoo 18 is slated for early-mid 2025
* Odoo version 17 \[full support].
* Odoo version 16 \[full support].&#x20;

| Item    | Status                    | Support |
| ------- | ------------------------- | ------- |
| Odoo 18 | Scheduled for development | N/A     |
| Odoo 17 | Available                 | Full    |
| Odoo 16 | Available                 | Full    |



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
