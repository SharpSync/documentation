---
description: >-
  This pages serves as a general guideline for managing data and configurations
  in PDM, and why you may face challenges.
icon: cubes-stacked
---

# Data Management

## Why a Simple Part Numbering System Beats “Intelligent” Codes for CAD Data

In CAD and ERP integrations, one of the most common points of resistance is from teams who already have a _clever_, _meaningful_, or _hierarchical_ part numbering scheme. It might encode everything from product family to revision level to finish in a single string like `ENG-MECH-PLT-001-A`.\
At first glance, this seems smart. In practice, it can cause more pain than productivity.

Here’s why a **simple, sequential numeric or alphanumeric** part number system is often the better long-term choice.

***

### The Problem with “Intelligent” Part Numbers

**1. They bake meaning into the ID.**\
When part numbers carry meaning (e.g., `VAL-SS-1IN-304` = Stainless Steel Valve, 1 Inch, 304 Grade), any change in the part’s specification can make the code inaccurate. Renumbering ripples through drawings, BOMs, ERP records, inventory labels, and customer documentation — a costly, error-prone process.

**2. They lock you into today’s classification logic.**\
Product lines evolve, materials change, categories merge. A code that made perfect sense five years ago might make no sense now — but you’re stuck with it if it’s embedded in thousands of records.

**3. They encourage “shoehorning.”**\
When you run out of space in a category or have a part that doesn’t fit the existing code pattern, people invent workarounds — awkward suffixes, inconsistent abbreviations — leading to confusion and data quality issues.

**4. They slow onboarding and searching.**\
New staff need to learn the code structure before they can even find a part. Searching by attributes becomes difficult if users don’t know the correct segment order.

***

### The Benefits of a Simple Numbering System

A **simple** system — e.g., `00012345` or `AB000123` — treats the part number as just an **ID**, not a description.

**1. Stability**\
The part number never changes, even if the part’s material, supplier, or category changes. Documentation stays correct.

**2. Flexibility**\
You can reclassify, reorganize, or add attributes without touching the part number. Changes happen in metadata, not in the identifier.

**3. Easier Integration**\
Simple IDs work well with CAD, PDM, ERP, PLM, and APIs. They avoid parsing logic and reduce mapping errors.

**4. Less Training Required**\
New team members don’t have to learn a complex coding system. They can search by description, attributes, or category — which is what modern systems are built for.

**5. Supports Automation**\
With unique, dumb IDs, automated numbering sequences are easy to maintain and prevent accidental duplication.

### How can we do this in PDM?

Simply setup a serial number for PDM to generate new Part and Assembly numbers. Drawings will automagically follow their model numbers.

## Would you be able to explain the difference between using the @ and default configurations when loading items in SharpSync?

The @ configuration in PDM lists all items connected to an assembly. It completely ignores configurations.

So if assembly A1 has a reference to A2, and A2 has reference to P2 and P3, but P3 is suppressed like this:

```html
A1
├-P1
└-A2
  ├-P2
  └-P3 (suppressed)
```

* Selecting the '@' configuration for A1 in the BOM view _will_ show P3 and select its _custom properties_, not its configuration properties.
* Selecting the '@' configuration for A1 shows all the custom properties for all items below that assembly. It does not display the configuration specific properties, and does not honor suppression states.
* Selecting the 'default' (or whichever you pick) configuration for A1 in the BOM view will _not_ show P3. <mark style="color:orange;">In 99% of the cases, this is the desired outcome.</mark>

When you make a configuration selection in SharpSync, the configuration you choose ultimately determines the structure (based on suppression states) and the custom properties (or configuration specific properties in the case of a selected configuration).

So if you can help it: Pick a 'default' (or whichever) configuration, not the '@'. This will give you the correct result. This is a completely separate issue to using configurations for assemblies and parts which presents other challenges.



## How should we manage weldments and sheet metal lengths or quantities?

Weldments and sheet metal tend to be problematic for companies because of the way their lengths, units of measure and general material consumption works.

Let's continue with our assembly example:&#x20;

\
A1 has weldment P1 with Qty 1. It also has weldment P2, so

```
A1 : 1
├-P1 : 1
└-A2 : 1
  ├-P2 : 2
  └-P3 (suppressed)
```

If we look at the raw material for weldment part P1, its a C-Beam 10.5 x 5 (where the 5 is the qty in _inches_) and for P2 its C-Beam 11.5 x 10. So actually what you should be seeing is:

```
A1 : 1
├-P1 : 5
└-A2 : 1
  ├-P2 : 10
  └-P3 (suppressed)
```

Here comes the problem, because the parts are inserted as parts (not as weldments or sheet metal sheets), the Qty is wrong. Instead of reflecting the correct quantities, you see the _instance count._

To correct this, override the BOM properties for the parts so that what you should be seeing is:

<figure><img src="../../.gitbook/assets/image (44).png" alt="BOM Quantity Overwritten in SolidWorks"><figcaption></figcaption></figure>

The alternative to this method is to create a subitem (not overriding the BOM qty) for each raw material item, where the structure would then resemble this:

```
A1 : 1
├-P1 : 1
|   └-P4 : 5   <-- new subitem **
└-A2 : 1
  ├-P2 : 1
  |  └-P5: 10  <-- new subitem **
  └-P3 (suppressed)
```

\*\* new subitem added for each item made from raw materials. This shows the total qty consumed

SharpSync has the ability to generate material rows automagically using the 'Is Material Property' setting in property mapping in conjunction with the setting "Generate Materials Sub items" in the Primary Data source (See [Broken link](broken-reference "mention"))

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

## Configuration Management for PDM

Configurations in PDM ... the elephant in the room... lets talk about configurations as a tool.&#x20;

First and foremost: Configurations are a _design_ tool. It was never meant for automating data transfer. It is meant to speed up _design_.

\
In SolidWorks you have the ability to create configurations, derived configurations, and derived parts with derived configurations. Derived parts with projected geometry from derived configurations of inserted parts into other parts. We can easily see how this can go wrong in so many ways.

Just because you have a hammer, that doesn't mean everything is a nail. It's not because the software can't do it. It's because, as humans, we have only so much processing power available to use when the pressures of work mounts. To reduce errors, simple = successful. Is it more work now? Yes. Is it less painful later? Yes!!  It's like going to the gym or starting a new diet - of course it's horrible for you now right!? But later on ... 100% satisfaction.

\
Advice given to customers over the years have been (If you want to speed up design to a maximum):

1. Have a master part with a configuration table. Configure the part => save as external and use that new part going forward (this is known as configuration ripping) OR
2. Have a master part with equations. Open the file, tweak the equations, save a copy out as a new file and use the new file in any assemblies.
3. Name this master part accordingly (e.g. Prefix it with MP or something so that if anyone sees it, they know this is a base for other parts).
4. Prevent the part from moving through a normal workflow with some condition. It will prevent it from being used in production assemblies.\


Here are the justifications for the above reasoning:

* Part numbers are hard to track in deep configuration trees. Yes we can use an excel sheet to configure part numbers. Yes we can do it. But the tools to get to the information becomes hard to use because those tools still mostly are created with the 1 document = 1 part number approach, not 1 document = 100s of part numbers.
* Revisioning a configuration in _PDM is not possible. You can only revision the entire document. If you make a change to a configuration, you also have to revision all other unchanged configurations (and by implication all parts generated by those configurations!_ This means you have to update your ERP's entire base of items should you revision something like a part that has 1000s of configurations because all those parts have just bumped up a rev.
* _There is no visibility on deep level configuration changes._ When making a change to a configuration, PDM only stores the fact that a person checked the file out + in. Let's be honest, no one reads the version comments. So if someone edits configuration 1280/6000, there is no way to know which configuration that person edited. You would have to do a before/after of the file and manually find the difference. Because of this - data management and revision control of configurations becomes and absolute nightmare. The best approach by far is 1 part = 1 part number = 1 item in the ERP. Despite our very sophisticated tools today, this is still the best approach.
* _Drawings become a problem_. If you are documenting a part with 10 configurations, then you also have to only have 1 drawing with 10 sheets. Creating 10 drawings is not the answer, because if you create 10 drawings, then your reference tree (where used) will look incorrect in PDM. But this means you have inconsistency between the number of drawings / number of parts. Just more unnecessary admin at this point for everyone.
* File exports suffer from the same problem (imagine export DXFs or a multiconfiguration part) - its hard to track.
* This list goes on...



Here are the benefits:

* 1 click import ⇒ your ERP of choice.
* ... no other reasons needed.\


Seriously though. The import (overarching) goal here is this: Your company wants to take their data to the next level. You want to go on holiday. That means working smarter not harder. That means automation, not manual input. Moving the data between different systems automatically. And the golden rule of CAD data automation is:&#x20;

> Things must be _consistent and predictable._

If you sometimes use configurations, and some times do not, then there is no cohesive decision in the company about how design is structured.&#x20;

The important decision process here: Start with your ERP. Find out how to manage that well. Work your way back through the maze to your CAD / PDM system. Standardize how things are done. Then let SharpSync do the heavy lifting for you. You got this!
