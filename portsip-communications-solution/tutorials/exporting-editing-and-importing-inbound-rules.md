# Exporting, Editing, and Importing  Inbound Rules

> **Note:** This guide also applies to other PortSIP PBX export and import operations that use CSV files, especially when the exported data contains large numeric values that must be preserved as text.

Follow the steps below to export, modify, and re-import PortSIP PBX inbound rules using a CSV file.

> **Important:** Do not open the exported CSV file directly by double-clicking it. Excel may automatically convert large numeric values, such as the **Provider ID**, into scientific notation or an incorrect number format. This can corrupt the data and cause the import to fail or produce incorrect results.

### Steps

1. Export the inbound rules from PortSIP PBX as a CSV file.
2. Do **not** open the CSV file directly. Instead, open Microsoft Excel first and create a new blank workbook.
3.  In Excel, import the CSV file by selecting:

    **Data > From Text/CSV**
4. Select the exported CSV file, then choose **Transform Data** instead of loading it directly.
5.  In the Power Query Editor, locate the **Provider ID** column and change its data type to **Text**.

    This step is required to prevent Excel from converting the Provider ID into scientific notation or another incorrect numeric format.
6. After confirming that the **Provider ID** column is set to **Text**, load the data into Excel.
7.  You can now duplicate or modify the inbound rules as needed.

    When editing the rules, make sure to:

    * Update the inbound rule name as required.
    * Ensure that the value in the **DID Numbers** column belongs to the DID pool range of the corresponding trunk.
    * Avoid changing the **Provider ID** value unless necessary.
    * Do not allow Excel to reformat the **Provider ID** column.
8.  After completing the changes, save the file as a **CSV UTF-8** file.

    In Excel, choose:

    **File > Save As > CSV UTF-8 (Comma delimited) (\*.csv)**
9. Import the saved CSV file back into PortSIP PBX.
10. After saving the CSV file, do **not** open it again in Excel before importing it into PBX.

    If you need to verify the CSV content, use a plain text editor such as Notepad++, VS Code, or another text editor. Reopening the file in Excel may cause the **Provider ID** column to be automatically converted again, which may corrupt the CSV data.

### Recommended Workflow

```
Export CSV from PortSIP PBX
→ Open Excel first
→ Data > From Text/CSV
→ Transform Data
→ Set Provider ID as Text
→ Edit or duplicate inbound rules
→ Save as CSV UTF-8
→ Import the CSV back into PortSIP PBX
```

Do not use the following workflow:

```
Export CSV
→ Double-click to open in Excel
→ Edit
→ Save
→ Reopen in Excel
→ Import
```

This workflow may cause Excel to convert the **Provider ID** into an incorrect format.

