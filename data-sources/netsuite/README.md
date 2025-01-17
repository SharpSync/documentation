---
icon: chart-tree-map
---

# Netsuite



NetSuite is an ERP and e-commerce platform by Oracle.&#x20;

SharpSync currently only supports OAuth2 authentication for NetSuite.

* [Authentication](authentication-+-configuration.md)
* [Common Setup](common-setup/)
* [Common Scenarios](common-scenarios.md)

## NetSuite supported features

Out of the box, the NetSuite integration supports the following features:

### Bom level features

| Feature                                        |      Differences     |     Modifications    |          Updates          |
| ---------------------------------------------- | :------------------: | :------------------: | :-----------------------: |
| Bom hierarchy                                  | :white\_check\_mark: | :white\_check\_mark: |    :white\_check\_mark:   |
| Bom meta data                                  | :white\_check\_mark: | :white\_check\_mark: |    :white\_check\_mark:   |
| Bom quantities                                 | :white\_check\_mark: | :white\_check\_mark: |    :white\_check\_mark:   |
| Component thumbnails                           |          N/A         |          N/A         |    :white\_check\_mark:   |
| Advanced BOMs                                  | :white\_check\_mark: | :white\_check\_mark: |    :white\_check\_mark:   |
| Routings                                       |    \[In progress]    |    \[In progress]    |    :white\_check\_mark:   |
| File derivative transfers (e.g. STEP, DXF)\*\* |          N/A         |          N/A         | \[marked for development] |

\*\* It should be noted that there are many ways to transfer files. We're in the process of adding file transfers for NetSuite, but please note that consultation services are required to understand your use case + configuration options. More information to follow in the configuration of file transfers from the CAD system.

### Item level features

The list of items that are supported are shown below. These have been officially tested and are supported. Please note that we're open for discussion on other item types, and we treat them as part of our consulation services. Most item types would not require any implementation as they follow the same standard in the NetSuite API.

| Item type                |      Differences     |     Modifications    |        Updates       |
| ------------------------ | :------------------: | :------------------: | :------------------: |
| inventoryitem            | :white\_check\_mark: | :white\_check\_mark: | :white\_check\_mark: |
| assemblyitem             | :white\_check\_mark: | :white\_check\_mark: | :white\_check\_mark: |
| noninventoryresaleitem   | :white\_check\_mark: | :white\_check\_mark: | :white\_check\_mark: |
| noninventorypurchaseitem | :white\_check\_mark: | :white\_check\_mark: | :white\_check\_mark: |
| bom                      | :white\_check\_mark: | :white\_check\_mark: | :white\_check\_mark: |
| bomrevision              | :white\_check\_mark: | :white\_check\_mark: | :white\_check\_mark: |
| routing operation        |                      |                      | :white\_check\_mark: |
| routing step             |                      |                      | :white\_check\_mark: |
| serializedinventoryitem  |     \[on roadmap]    |     \[on roadmap]    |     \[on roadmap]    |
| serializedassemblyitem   |                      |     \[on roadmap]    |     \[on roadmap]    |
