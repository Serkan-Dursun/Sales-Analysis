# Sales Data Filtering and Reporting Macros

## Overview

This project consists of a set of Excel VBA macros designed to automate the generation of detailed sales reports from a multi-sheet sales dataset. The macros enable dynamic filtering of sales data based on multiple criteria — specifically **Region**, **Product Name**, and **Sales Representative** — and produce a well-formatted, easy-to-read report within Excel.

---

## What We Created

### 1. Multi-Filter Sales Report Macro

The core macro, `GenerateMultiValueFilterSalesReport_Formatted`, allows users to filter sales data by any combination of the following:

- **Region(s)**: One or more geographic regions where customers are located.
- **Product Name(s)**: One or more products sold.
- **Sales Representative(s)**: One or more sales reps are responsible for the sales.

Users can input multiple values for each filter, separated by commas. The macro performs **partial matching**, meaning it will include any sales records where the data contains the filter text anywhere in the relevant field.

### 2. Helper Functions

- `TrimArray`: Cleans up user input by trimming extra spaces from each filter value.
- `PartialMatchInArray`: Checks if a data value contains any of the filter values, enabling flexible partial matching.

---

## How We Created the Macro

### Data Preparation

The macro works with the following Excel sheets:

- **sales**: Contains transactional sales data with columns such as sale ID, date, customer ID, product ID, sales rep ID, quantity, unit price, and total.
- **customers**: Maps customer IDs to regions.
- **products**: Maps product IDs to product names.
- **sales_reps**: Maps sales rep IDs to sales rep names.

### Macro Logic

1. **User Input**: The macro prompts the user to enter filter values for Region, Product, and Sales Rep. Inputs can be multiple values separated by commas or left blank to ignore that filter.

2. **Data Lookup**: Using dictionaries, the macro quickly maps IDs in the sales data to their descriptive names or regions.

3. **Filtering**: For each sales record, the macro checks if the customer’s region, product name, and sales rep name match any of the user’s filter values using partial matching.

4. **Report Generation**: Matching records are copied to a new or cleared worksheet named **Filtered Sales Report**.

5. **Summary Calculation**: The macro calculates total sales, total quantity sold, and average unit price for the filtered data and displays these below the report.

6. **Formatting**: The report is formatted with styled headers, auto-fitted columns, borders, number formatting, frozen header row, autofilter dropdowns, and conditional formatting to highlight high sales totals.

---

## Benefits of This Macro

- **Efficiency**: Automates the tedious task of manually filtering and summarizing sales data.
- **Flexibility**: Supports filtering by multiple criteria simultaneously, with multiple values per criterion.
- **Accuracy**: Reduces human error by using exact and partial matching based on user input.
- **Readability**: Produces a polished, formatted report that is easy to interpret.
- **Scalability**: Can handle large datasets and complex filtering without manual intervention.
- **Reusability**: The macro can be run repeatedly with different filter criteria to generate various reports.

---

## Filtering Conditions and Logic

- **Partial Matching**: The macro includes any record where the filter text appears anywhere in the data field (case-insensitive). For example, filtering by "Tustin" will match "Tustin, California".
- **Multiple Values per Filter**: Users can input multiple comma-separated values for each filter. The macro includes records matching *any* of these values within that filter.
- **AND Logic Across Filters**: Records must satisfy *all* non-empty filters to be included. For example, if Region and Product filters are both specified, only records matching both criteria appear.
- **Blank Filters**: Leaving a filter blank means that the filter is ignored, and all values for that field are included.

---

## How to Use

1. Open the Excel workbook containing the sales data sheets.
2. Open the VBA editor (`Alt + F11`) and paste the macro code into a module.
3. Run the macro `GenerateMultiValueFilterSalesReport_Formatted`.
4. When prompted, enter filter values for Region, Product, and Sales Rep as desired. Use commas to separate multiple values.
5. Review the generated **Filtered Sales Report** sheet, which will contain the filtered data, summary, and formatted layout.
6. Use the autofilter dropdowns in the header row to further explore or sort the data.

---

## Potential Enhancements

- Add a user form with dropdowns for filter selection to improve usability.
- Export the filtered report to PDF or a new Excel file automatically.
- Add charts or visual dashboards summarizing the filtered data.
- Integrate with email automation to send reports to stakeholders.
- Implement error handling for invalid inputs or missing data.

---

## Conclusion

This macro project significantly streamlines sales data analysis by providing a powerful, flexible, and user-friendly tool for filtering and reporting. It empowers users to quickly generate customized sales reports tailored to specific regions, products, and sales representatives, enhancing decision-making and operational efficiency.

---

## Code Reference

The full VBA macro code is included in this repository for easy reuse and customization.

---

