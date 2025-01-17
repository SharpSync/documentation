---
icon: chart-tree-map
---

# NetSuite

NetSuite is an ERP and e-commerce platform by Oracle.&#x20;

SharpSync currently only supports OAuth2 authentication for NetSuite.

* [Authentication](authentication-+-configuration.md)
* [Common Setup](common-setup/)
* [Common Scenarios](common-setup/configure-new-assemblies-as-isphantom.md)

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
| Routings                                       | :white\_check\_mark: | :white\_check\_mark: |    :white\_check\_mark:   |
| File derivative transfers (e.g. STEP, DXF)\*\* |          N/A         |          N/A         | \[marked for development] |

\*\* It should be noted that there are many ways to transfer files. We're in the process of adding file transfers for NetSuite, but please note that consultation services are required to understand your use case + configuration options. Each customer's implementation of file transfers will be unique.

### Item level features

The list of items that are supported are shown below. These have been officially tested and are supported. Please note that we're open for discussion on other item types, and we treat them as part of our consultation services. Most item types would not require any implementation as they follow the same standard in the NetSuite API.

<table><thead><tr><th width="100">Item type</th><th align="center">Differences</th><th align="center">Modifications</th><th align="center">Updates</th></tr></thead><tbody><tr><td><code>inventoryitem</code></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td></tr><tr><td><code>assemblyitem</code></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td></tr><tr><td><code>noninventoryresaleitem</code></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td></tr><tr><td><code>noninventorypurchaseitem</code></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td></tr><tr><td><code>bom</code></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td></tr><tr><td><code>bomrevision</code></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td></tr><tr><td><code>routingoperation</code></td><td align="center">[on roadmap</td><td align="center">[on roadmap</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td></tr><tr><td><code>routingstep</code></td><td align="center">[on roadmap</td><td align="center">[on roadmap</td><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="2705">✅</span></td></tr><tr><td><code>serializedinventoryitem</code></td><td align="center">[on roadmap]</td><td align="center">[on roadmap]</td><td align="center">[on roadmap]</td></tr><tr><td><code>serializedassemblyitem</code></td><td align="center">[on roadmap]</td><td align="center">[on roadmap]</td><td align="center">[on roadmap]</td></tr></tbody></table>
