---
description: BOM Sets represent a collection of all configurations related to a given BOM
icon: chart-tree-map
---

# BOM Sets

<figure><img src="../.gitbook/assets/Screenshot 2026-02-20 at 12.57.06 PM.png" alt=""><figcaption></figcaption></figure>



{% hint style="warning" %}
The BOM Sets feature is a new feature that is still under development and is only available by request. Please contact our team for more info.
{% endhint %}

After exporting a BOM Set from a supported Primary DataSource, a BOM Set card will appear in the BOM Sets page.

<figure><img src="../.gitbook/assets/Screenshot 2026-02-20 at 1.00.52 PM.png" alt=""><figcaption></figcaption></figure>

The related configurations of a BOM Set will start loading automatically in the background. And can be viewed by navigating to the individual BOM Set page by clinking on the BOM Set name or the navigation arrow.

<figure><img src="../.gitbook/assets/Screenshot 2026-02-20 at 1.17.48 PM.png" alt=""><figcaption></figcaption></figure>

Once all configurations have been loaded with Primary Load Status of  `loadCompleted` , a zip file containing a CSV file per configuration can be downloaded by clicking on the "Download CSVs" button.

<figure><img src="../.gitbook/assets/Screenshot 2026-02-20 at 1.17.48 PM (1).png" alt=""><figcaption></figcaption></figure>

Column headers of BOM configuration CSV files represent the Display Names of the primary Data Source fields / properties as setup in the Property Mappings and their order is determined by the existing sort order of the mappings.&#x20;

#### **Reverting BOMs in a BOM Set**

Should errors occur in the loading or syncing process of a BOM in a BOM Set, the option to revert a BOM will be displayed.

<figure><img src="../.gitbook/assets/Screenshot 2026-03-05 at 11.34.24 AM.png" alt=""><figcaption></figcaption></figure>

Reverting a BOM will reset its loading or sync status (if with errors or warnings).

The option to mass revert all BOMs with issues in a BOM Set will also be enabled by right clicking on any cell under the `REVERT` column --> `Revert` --> `Revert All Boms With Issues`

<figure><img src="../.gitbook/assets/Screen Shot 2026-03-05 at 11.34.33 AM [2].png" alt=""><figcaption></figcaption></figure>
