---
icon: list-check
---

# Checklist for automation

When automating data transfers, there are some fundamentals that must be in place.

The golden rule of data automation is:&#x20;

:heavy\_check\_mark: Standardization is paramount. Everything should be of a standard format

### Basic Checklist

Below is a checklist to assist you to successfully automate your data transfers:

* [Unique Component Identifiers (UCI).](unique-component-identifiers.md) We'll interchangeably refer to this as the Part Numbers
* [Distinct components for each part with a part number](distinct-components.md)
* A _sandbox_ environment for the ERP where you can play and break things.
* Access for SharpSync staff to the Sandbox environment. (i.e. a credential to test synchronization)
* At least a general idea of the data that needs to be transferred to the ERP such as:
  * Which field should be UCI be written to?
  * [How are raw materials consumed](./#material-consumption-management)?
  * What other fields should be sent to the ERP?&#x20;
* Optional: A sandbox environment for your CAD data (if doing bi-directional synchronization).

### More preparation: Data management

In addition to the overall checklist above, here are some basic tips and strategies for data management that prepares your for successful automation:

* Have a central fasteners library. Don't reuse (copy + paste) fasteners everywhere
* Use templates for your parts or assemblies with pre-populated descriptions (such as 'Description Goes Here'). This may seem redundant, but it is actually a surefire way to detect when a document is not ready for approval
* Use an automated number generation system
  * Yes you can use a spreadsheet, but make sure everyone using the system is using the same spreadsheet
  * No - don't think smart component numbers solves this problem
* Do not use smart component numbers
  * Use sequential numbers like PRT-12345, PRT-12346
  * It is easy to read and locate
  * It's reusable
  * Don't use PRT-SOMEPROJECTLEVEL-SOMEMATERIAL-SOMEREVISION. It's not wrong, but it's a matter of time before you can't use that scheme for everything, why? Because
    * SOMEPROJECTLEVEL defeats the purpose of reusability at different levels
    * SOMEMATERIAL is ok, but unnecessary - put it in the meta data
    * SOMEREVISION should not be part of the part number. If it is, it means with every revision the part number changes. If you truly desire a new part number change for every revision, rather use a new number and put the revision in the meta data.
    * Using a serial number is better in every case
    * You'll have tons of technical debt because of the reasons above which can't be easily addressed. It only becomes apparent after about 10 months to a year
* Approve your documents in your CAD, PDM or PLM system before sending it to the ERP.
* Have meta data in your CAD documents. More meta data is better than less.&#x20;
  * Part has an operation? Add an operation field.&#x20;
  * Part has a material? Add a material field
  * Part is Purchased? Mark it as `purchased` or `manuf`
  * We can take lots of data and reduce it to what the ERP needs, but we can't take nothing an make it fit the ERP through dynamic generation. There is an upper limit to what our rules engines can do



### What you _don't_ need for automation

We are experts at what we do. You're an expert at what you do. Very few people bridge the gap between CAD, PDM, PLM (on the left) and ERP (on the right). Let's refer to this as our fence with to parties on either side.

We don't need you to be an expert on _both sides of this fence._ That's where our expertise comes in.

Here's what you don't need:

* Programming knowledge
* Intricate knowledge of the CAD, PDM or PLM system (though more knowledge is better)
* Intricate knowledge of the ERP (though more knowledge is better)
* Knowledge about databases
* How the cloud works

Here's how we can help:

* We are software developers and we're good at what we do
* We have intricate knowledge of CAD, PDM, PLM and databases
* We know how ERP and cloud works

If you're struggling to get the information or meta data ready on either side of this fence, we have consultants that can assist with that.

If you're struggling, please reach out to us on our homepage.
