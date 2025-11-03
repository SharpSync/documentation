---
icon: square-exclamation
---

# Troubleshooting

Welcome to SharpSync troubleshooting section.

{% hint style="success" %}
First and foremost:&#x20;

* Exercise patience when troubleshooting. While the software is easy to use from the front end (a user's perspective) it has many moving parts with complicated under-the-hood logic. Patience is your friend.
* Take an interative approach, starting with smaller BOMs and working your way up to bigger BOMs.
* Take an interative approach, starting with few property mappings and working your way up to more.
* Make contact with us if you don't understand a message or situation.
* Make contact with us if you would like clarity or support.
* Log a support ticket for any bugs or feature requests. We look at and evaluate all requests.
{% endhint %}

Sometimes the errors might occur onscreen when attempting to load Bill of Material (BOM) information.

Below are some of the common issues that you might encounter and tips for avoiding or solving the problems:

* Items / components / parts must have a unique identifier in each of your sources. This could be the part number, the part name, or the part id but it must be unique within each source and cannot be empty.
* Make sure you have set up the correct primary component identifier for each of your SharpSync Data Sources that correspond to your source's unique identifier field. See here for more info: [DataSource \ Primary Identifiers](broken-reference)
* Make sure you have set up one quantity mapping to have SharpSync use when building an internal BOM hierarchy model. See here for more info: [Property Mapping Settings](../property-mappings/property-mapping-settings.md)
* Avoid [Duplicate Component Paths](duplicate-component-paths.md)
* [OAuth 2.0](oauth-2.0.md)

If you cannot find the information above, reach out to us on our [support desk](https://sharpsync.atlassian.net/servicedesk/customer/portals).
