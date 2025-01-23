# Setup

CSV (Comma Separated Values) files are utilized to import Bills of Materials (BOMs) from desktop-based CAD software such as SolidWorks, Inventor, SolidEdge or similar into SharpSync. Follow the steps below to begin importing data into SharpSync using CSV files.

### Prerequisites

* Ensure your CSV output is complete.
* CSVs should contain BOM data only.
* Columns can be empty, but row data must not exceed the number of columns.
* BOMs should be exported in an indented or structured format to encapsulate an assembly's hierarchy.
* Flattened BOMs are not recommended.
* For best results, ensure that the CSV is in UTF-8 format.

### Instructions

<figure><img src="../../.gitbook/assets/section_header_datasources.png" alt=""><figcaption></figcaption></figure>



1. On the Data Sources admin page, select "Csv" and click on the Add Data Source button.&#x20;

<figure><img src="../../.gitbook/assets/datasources_select_csv.png" alt=""><figcaption></figcaption></figure>



2. Click the Configure button, then click on the BOM Configuration tab.
3. Enter the names of each column header. Make sure that the Column Headers you enter match your BOM output.&#x20;

<figure><img src="../../.gitbook/assets/csv_specify_columns.png" alt=""><figcaption></figcaption></figure>



4. Click on Save to save your changes and close the window. (No Authentication is needed for CSV files)
5. Enter the Primary and Alternative Identifiers. Your Primary Identifier should correspond to the column that will uniquely identify components in the hierarchy. This will populate the Component Hierarchy in the BOM view and be the name used to search in the secondary [Data Source.](../../fundamentals/data-sources.md)

<figure><img src="../../.gitbook/assets/csv_primary_alternative_identifiers.png" alt=""><figcaption></figcaption></figure>



6. When you have finished, click on Update at the bottom to save.
