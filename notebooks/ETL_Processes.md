# Importing Dummy Data from Excel to SQL Server (ETL Process)

This document explains, step by step, how I performed an ETL (Extract, Transform, Load) process to move my mock sales and marketing data from Excel into SQL Server, clean it, 
and enforce data integrity for my Sales Performance Analysis project.

---

## 1. Extract: Preparing the Data

- I generated realistic mock data with Python and loaded into Excel, with each sheet representing a table (e.g., customers, products, sales, etc.).
- The data generation process is fully documented in [`notebooks/Python_Code_to_create_Excel_Dummy_Data.md`](Python_Code_to_create_Excel_Dummy_Data.md).
- Each sheet had clear column headers and no empty rows or columns.
- click here to see the excel file data/sales_performance_data.xlsx, or click here to see the image of the Excel tables.

---

## 2. Load: Importing Data Using SQL Server Import Wizard

1. I opened SQL Server Management Studio (SSMS).
2. Right-clicked my target database and selected **Tasks > Import Data...** to launch the SQL Server Import and Export Wizard.
3. Chose **Microsoft Excel** as the data source and selected my `.xlsx` file.
4. Set my SQL Server database as the destination.
5. Mapped each Excel sheet to a new table (e.g., `customers$`, `sales$`, etc.).
6. Completed the wizard, which created new tables in my database with a `$` suffix.

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

## 4. Enforcing Constraints
After migrating the data, I added primary key and foreign key constraints to the final tables to ensure data integrity and proper relationships.

## 5. Cleaning Up Temporary Tables
Once all data was migrated and validated, I dropped the temporary $ tables to keep the database clean:
DROP TABLE customers$;
DROP TABLE sales$;
-- Repeated for other imported tables

## 6. Summary of the ETL Process
Extract: Prepared and structured mock data in Excel (see notebooks/Python_Code_to_create_Excel_Dummy_Data.md for the code).
Load: Imported Excel sheets to SQL Server using the Import Wizard (created $ tables).
Transform: Cleaned and migrated data to final tables, removed duplicates, and enforced constraints.
Dropped the temporary $ tables.

