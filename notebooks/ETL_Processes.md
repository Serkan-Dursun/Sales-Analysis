# Importing Dummy Data from Excel to SQL Server (ETL Process)

This document explains, step by step, how I performed an ETL (Extract, Transform, Load) process to move my mock Sales Performance data from Excel into SQL Server, clean it, and enforce data integrity for my Sales Performance Analysis project.

---

## 1. Extract: Preparing the Data

- I generated realistic mock data with Python and loaded it into Excel, with each sheet representing a table: `customers`, `products`, `sales`, `sales_reps`, `marketing_campaigns`, `web_analytics`, and `ab_test_results`.
- The data generation process is fully documented in [`notebooks/Python_Code_to_create_Excel_Dummy_Data.md`](Python_Code_to_create_Excel_Dummy_Data.md).
- Each sheet had clear column headers and no empty rows or columns.
- Click [here](../data/sales_performance_data.xlsx) to see the Excel file (`data/sales_performance_data.xlsx`), or click [here](../images/EXCEL_Sales_Analysis_Data_Tables.jpg) to see the image of the Excel tables.

---

## 2. Load: Importing Data Using SQL Server Import Wizard

- Launched **SQL Server Management Studio (SSMS)** and connected to the target database instance.
- Navigated to the desired database, right-clicked, and selected **Tasks > Import Data...** to initiate the SQL Server Import and Export Wizard.
- Specified **Microsoft Excel** as the data source and provided the path to the `.xlsx` file containing the mock data.
- Selected the target SQL Server database as the destination for the import operation.
- Configured the mapping of each Excel worksheet to a corresponding staging table in SQL Server (e.g., `customers$`, `sales$`, etc.).
- Executed the import process, resulting in the creation of new staging tables (with a `$` suffix) in the database, each populated with the data from the respective Excel sheets.

---

## 3. Transform: Cleaning and Migrating Data

- The Import Wizard created tables like `customers$`, `products$`, etc.
- I created my final tables (without the `$`), defining correct data types, primary keys, and foreign keys.
- To ensure data quality, I checked for and removed duplicates before migrating data.

**Example: Removing duplicates and inserting unique customers**
```sql
;WITH UniqueCustomers AS (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY (SELECT 0)) AS rn
    FROM customers$
)
INSERT INTO customers (customer_id, customer_name, region, segment, join_date, satisfaction_score)
SELECT customer_id, customer_name, region, segment, join_date, satisfaction_score
FROM UniqueCustomers
WHERE rn = 1; 
```

## 4. Enforcing Constraints
After migrating the data, I added primary key and foreign key constraints to the final tables to ensure data integrity and proper relationships.

## 5. Cleaning Up Temporary Tables
Once all data was migrated and validated, I dropped the temporary $ tables to keep the database clean:
```sql
DROP TABLE customers$;
DROP TABLE sales$;
-- Repeated for other imported tables
```
--- 

## 6. Summary of the ETL Process
Extract: Prepared and structured mock data in Excel (see notebooks/Python_Code_to_create_Excel_Dummy_Data.md for the code).
Load: Imported Excel sheets to SQL Server using the Import Wizard (created $ tables).
Transform: Cleaned and migrated data to final tables, removed duplicates, and enforced constraints.
Dropped the temporary $ tables.

