# Advanced Bill of Materials

Advanced BOMs are NetSuite's way of keeping track of Bill of Materials (BOM) snapshots. You can think of an advanced BOM as the revision of a Bill of Materials at a certain point in time.

Advanced BOMs use the following notable features:

* Individual revisions of components
* Activation dates
* Routings
* Manufacturing steps

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### Using Advanced BOMs

There are some limitations with the use of advanced BOMs via the NetSuite API. For this reason (at the time of writing), we make use of a custom server side script to create advanced BOMS.

Server side scripting uses a script in NetSuite to set default custom field values which are not accessible via the NetSuite API. The important bit to take note of here is that the `itemInjectionScriptId` needs to be configured (see below).

### Major steps

* [Configure the server side script](configure-server-side-script/)
* [Configure SharpSync to use server side scripting](configure-sharpsync-to-use-server-side-script.md)
* [Configure Routings](configure-routings.md) \[optional]

