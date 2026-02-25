---
icon: square-check
---

# Material Consumption Management

To properly synchronize data with an ERP, you'll need to understand how raw materials are consumed. Samples of raw materials may include:

* Extrusion lengths
* Steel plate of a certain thickness



There are 2 ways to deal with material consumption:

* As a direct value (it's a custom property)
* As a direct child (it's a child of the component)



Let's use the example of weldments or extrusions. You have an assembly with this structure:

```
A1 - Assembly 1
  - P1 - Galvanized Bracket - bought - qty 1 - unit of measure each
  - P2 - Aluminium Extrusion - manufactured - qty 3.42m - unit of measure length
```

* Assembly A1 has parts P1 and P2.
* P1 is made of galvanized steel and is bought from an external supplier
* P2 is made inhouse of Aluminium Extrusion, length 1.5 meters

In the ERP you will specify that P1 is a bought item, but P2 is a item you make based on raw materials you have on hand (Aluminium Extrusion)

The best way to indicate this in your ERP is to create an Aluminium Extrusion child for P2. Let's say the part number for the raw material AS-5005, then the Bill Of Materials should look like this in the ERP:

```
A1 - Assembly 1
  - P1 - Galvanized Bracket - bought - qty 1
  - P2 - Aluminium Extrusion - manufactured - qty
    - AS-5005 - Aluminium Extrusion - qty 3.42
```

_You don't have to use this method,_ but this is a common pattern seen in ERPs and existing customer implementations.&#x20;

What you should do, is _have an understanding of how raw materials are consumed in **your** ERP_ and how quantities are tracked on purchase orders.

See the [Row Component Rules](../../../advanced/row-component-rules.md) for automagically generating nested rows for materials

