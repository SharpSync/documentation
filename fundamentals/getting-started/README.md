---
icon: arrow-right-to-bracket
---

# Getting Started

For pricing information and up to date information on the available modules, head over to [SharpSync.net](https://sharpsync.net) for material related to the application as well as contact information.

### To use the app

* Navigate to [https://app.sharpsync.net](https://app.sharpsync.net)
* [Register](registration.md) an organization, using your company email (we do not currently support social logins)
* You will receive an email notifying you that we've received the request
* We'll make contact with you through the email that you provided to confirm the organization details
* After confirmation, login to SharpSync and begin your setup by picking your [Data Sources](../data-sources.md).



### The main concepts

<table><thead><tr><th width="221">Concept</th><th>Explanation</th></tr></thead><tbody><tr><td><a href="broken-reference">Data Source</a></td><td>A source of data where Bill of Material (BOM) information may be pulled from or pushed to</td></tr><tr><td><a href="broken-reference">Property Mapping</a></td><td>A mapping of meta data from one data source to another</td></tr><tr><td><a href="../../property-mappings/property-mapping-settings/">Accessor</a></td><td><p>The key name of a meta data object that is retrieved from a source. In the object <br><br><code>{ "name" : "andries" }</code> </p><p><br>the accessor (key) is 'name'</p></td></tr><tr><td><a href="../rules.md">Rule Template</a></td><td><p>A rule let's you edit a cell value automagically.</p><p></p><p>It is typically used as a business rule that determines the final shape of the data.</p><p></p><p>For example, </p><p><em>"All quantities must be formatted as a 3 digit decimal"</em>, so we specify a rule to do it.</p></td></tr><tr><td><a href="../../advanced/derivatives.md">Derivative</a></td><td>A file created from another file. E.g. a PDF is a derivative of a drawing. A STEP file is a derivative of a CAD model</td></tr></tbody></table>
