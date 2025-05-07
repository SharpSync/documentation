---
icon: file-import
---

# Setting up Derivatives

Derivatives are files that are derived from other files or documents.&#x20;

{% hint style="info" %}
See also [derivatives.md](../../advanced/derivatives.md "mention").&#x20;
{% endhint %}

In Onshape, derivatives are primarily files used for exporting, such as PDF, DXF, ZIP, IGES, etc.

Onshape is different to a typical offline CAD file or PDM system in that it does not store 'Files'. It stores Documents. Each document may contain one ore more elements (documents within documents). Each element may be one of the following:

* A single part body document
* A multi-part body document
* An assembly
* A drawing
* A derivative (PDF, DXF, IGES)
* An import (e.g. STEP, IGES, ZIP, etc)

Derivatives supported by SharpSync are all types that can be derived from native Onshape Types. This means that:

* If you can export it from Onshape using an "Export" option then ...
* You can use it in SharpSync.



To setup derivatives in SharpSync, navigate to the Derivatives section. The procedure is explained in that section. What is not explained there, are how to setup derivatives of Drawings, which themselves are derivatives of CAD models. This will be explained below

{% hint style="info" %}
You must be an organization admin to be able to do this
{% endhint %}

In this section, we'll talk you through:



* Transferring a Drawing URL
* Transferring a Drawing PDF



### Transferring a Drawing URL

There is only a single option for Onshape drawings, and that is the transfer of the URL. You cannot transfer the drawing itself. To transfer a drawing, you must convert it to a PDF first or let SharpSync do the conversion for you. To convert a drawing to a PDF, the drawing must exist first.&#x20;

Navigate to the 'Derivatives' section.

Add a new derivative by Selecting 'Drawing'

Configure the settings as shown below

| Derivative Setting    | Description                                                                                                                             | Value                                                                                                                                                                                                                                                                                                                                                        |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Template              | The derivative header name as the user will see it. Modify to suit your needs.                                                          | DRAWING                                                                                                                                                                                                                                                                                                                                                      |
| Extension             | The default extension you wish to use when moving the file from Onshape to your ERP. Modify to suit your needs.                         | drawing                                                                                                                                                                                                                                                                                                                                                      |
| Search pattern        | The search pattern to use when searching for documents in Onshape. Only exact matching results are returned. Modify to suit your needs. | <p><code>DRW-{rowData.componentName}</code><br><br>The above is a suggestion only, but note that if you use different naming conventions or more than 1 naming convention, then multiples of DRAWING derivatives must be added. <br><br>Using the example above, if the document name is <code>A1</code> then the search will be for <code>DRW-A1</code></p> |
|  Naming Pattern       | The naming pattern to use when generating new documents. Modify to suit your needs.                                                     | <p><code>{rowData.componentName}-{rowData.cells.revision}</code><br><br>The above suggestion</p>                                                                                                                                                                                                                                                             |
| Generate for Drawings |                                                                                                                                         | checked                                                                                                                                                                                                                                                                                                                                                      |
| Property Mapping      | The property mapping to use when setting the value. You must have a property mapping value set if you want to transfer a url            | This value will be customer specific. Select a Property Mapping. Mine was called `drawingUrl`                                                                                                                                                                                                                                                                |
| Transfer URL          | Whether to transfer the URL by default. If unchecked, the user must manually check this option every time.                              | checked                                                                                                                                                                                                                                                                                                                                                      |
| Enabled               | Whether the template may be accessed by users when displaying BOM data                                                                  | checked                                                                                                                                                                                                                                                                                                                                                      |



Now that we have the derivative template setup, we are ready to transfer the drawing URL to a property in our destination ERP. To do this:

* Load a bill of material in SharpSync
* Look for drawing rows (Default color is a blue-ish color and it will have the same name as the part or assembly
* Click the button to the right (looks like a Folder +)

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption><p>A derivative button appears when valid derivative templates exist</p></figcaption></figure>

* Add a new derivate (Pick the drawing template we just setup)



<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption><p>The Store URL checkbox is automatically selected based on our template settings</p></figcaption></figure>

* The values are filled based on the settings and the  row's cells.
* Click the Submit button.&#x20;
* When the BOM has finished processing, the URL of the drawing will be written to the ERP.

### Transferring a Drawing PDF

A PDF is a derivative of a drawing. The structure at the moment is

> Assembly > Drawing > PDF

The assembly must exist for the drawing to exist. The drawing must exist for the PDF to exist.

The next step is to create the PDF derivative template and then use it in the BOM view.

* Navigate to the Derivatives section
* Add a new Derivative Template > Pick PDF

Configure the settings as shown below:

| Derivative Setting    | Description                                                                                                                             | Value                                                                                            |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| Template              | The derivative header name as the user will see it. Modify to suit your needs.                                                          | PDF                                                                                              |
| Extension             | The default extension you wish to use when moving the file from Onshape to your ERP. Modify to suit your needs.                         | pdf                                                                                              |
| Search pattern        | The search pattern to use when searching for documents in Onshape. Only exact matching results are returned. Modify to suit your needs. | At the time of writing, pre-generated PDFs are not supported, so leave this field empty.         |
|  Naming Pattern       | The naming pattern to use when generating new documents. Modify to suit your needs.                                                     | <p><code>{rowData.componentName}-{rowData.cells.revision}</code><br><br>The above suggestion</p> |
| Generate for Drawings |                                                                                                                                         | Checked                                                                                          |
| Property Mapping      | The property mapping to use when setting the value. You must have a property mapping value set if you want to transfer a url            | This value will be customer specific. Select a Property Mapping. Mine was called `drawingUrl`    |
| Transfer File         | Whether to transfer the File by default. If unchecked, the user must manually check this option every time.                             | checked                                                                                          |
| Enabled               | Whether the template may be accessed by users when displaying BOM data                                                                  | checked                                                                                          |

Now that we have the derivative template setup, we are ready to transfer the drawing PDF to our destination ERP. To do this:

* Load a bill of material in SharpSync
* Look for drawing rows (Default color is a blue-ish color and it will have the same name as the part or assembly
* Click the button to the right (looks like a Folder +)

&#x20;

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption><p>A derivative button appears when valid derivative templates exist</p></figcaption></figure>

* Add a new derivate (Pick the drawing template we just setup)

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption><p>The Store File checkbox is automatically selected because of our template settings</p></figcaption></figure>

* The values are filled based on the settings and the row's cells.
* Click the Submit button.&#x20;
* When the BOM has finished processing, the PDF of the drawing will be saved to the ERP.
