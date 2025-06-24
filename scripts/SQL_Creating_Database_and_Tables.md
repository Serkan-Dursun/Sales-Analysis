# SQL Database Creation and ETL Process Documentation

This document details the end-to-end process for building the Sales Performance Analysis database, including schema creation, data import from Excel, data cleaning, and enforcing data integrity with primary and foreign keys.

---

## 1. Database Schema Creation

The final database schema was designed based on a star schema, with a central fact table (`sales`) and several dimension and analytics tables. All relationships are enforced with primary and foreign keys.



**SQL code to create all tables:**

```sql
-- Table: customers
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name NVARCHAR(100),
    region NVARCHAR(50),
    segment NVARCHAR(30),
    join_date DATE,
    satisfaction_score INT
);

-- Table: products
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name NVARCHAR(100),
    category NVARCHAR(30),
    unit_price INT
);

-- Table: sales_reps
CREATE TABLE sales_reps (
    sales_rep_id INT PRIMARY KEY,
    sales_rep_name NVARCHAR(100),
    region NVARCHAR(50)
);

-- Table: marketing_campaigns
CREATE TABLE marketing_campaigns (
    campaign_id INT PRIMARY KEY,
    campaign_name NVARCHAR(100),
    channel NVARCHAR(30),
    start_date DATE,
    end_date DATE,
    spend INT,
    leads INT,
    conversions INT,
    campaign_objective NVARCHAR(50),
    owner NVARCHAR(50),
    status NVARCHAR(20)
);

-- Table: sales (fact table)
CREATE TABLE sales (
    sale_id INT PRIMARY KEY,
    date DATE,
    customer_id INT,
    product_id INT,
    sales_rep_id INT,
    campaign_id INT,
    quantity INT,
    unit_price INT,
    total INT,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id),
    FOREIGN KEY (sales_rep_id) REFERENCES sales_reps(sales_rep_id),
    FOREIGN KEY (campaign_id) REFERENCES marketing_campaigns(campaign_id)
);

-- Table: web_analytics
CREATE TABLE web_analytics (
    date DATE,
    campaign_id INT,
    visits INT,
    conversions INT,
    bounce_rate FLOAT,
    PRIMARY KEY (date, campaign_id),
    FOREIGN KEY (campaign_id) REFERENCES marketing_campaigns(campaign_id)
);

-- Table: ab_test_results
CREATE TABLE ab_test_results (
    result_id INT PRIMARY KEY,
    test_name NVARCHAR(50),
    group_name NVARCHAR(20),
    date DATE,
    impressions INT,
    conversions INT,
    metric NVARCHAR(50),
    notes NVARCHAR(255),
    test_status NVARCHAR(20),
    campaign_id INT,
    FOREIGN KEY (campaign_id) REFERENCES marketing_campaigns(campaign_id)
);
```

## 2. Data Extraction and Import

- Data was generated in Excel using Python.
- The SQL Server Import Wizard was used to import data from the Excel sheets.
- During the first import, tables were created without the `$` suffix, but the data was incomplete (many `NULL` values).
- A second import created new tables with the `$` suffix (e.g., `customers$`, `sales$`), which contained the correct data.
---
 ## 3. Data Cleaning and Migration
Data was migrated from the $ staging tables to the final tables (without $), ensuring correct data types and relationships.
Duplicates were removed using Common Table Expressions (CTEs) and the ROW_NUMBER() function.
Example: Removing duplicates and inserting unique customers
``` SQL ;WITH UniqueCustomers AS (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY (SELECT 0)) AS rn
    FROM customers$
)
INSERT INTO customers (customer_id, customer_name, region, segment, join_date, satisfaction_score)
SELECT customer_id, customer_name, region, segment, join_date, satisfaction_score
FROM UniqueCustomers
WHERE rn = 1;
```
- This process was repeated for all tables (products$, sales$, etc.).
---

## 4. Assigning Primary and Foreign Keys
-The final tables were created with explicit primary and foreign key constraints (see schema above).

-This ensures referential integrity and enforces all relationships as designed in the ERD.

---
## 5. Dropping Staging Tables
After successful migration and validation, the temporary $ tables were dropped to keep the database clean.
``` sql
DROP TABLE customers$;
DROP TABLE products$;
DROP TABLE sales$;
DROP TABLE sales_reps$;
DROP TABLE marketing_campaigns$;
DROP TABLE web_analytics$;
DROP TABLE ab_test_results$;
```
---
## 6. Final Data Validation
- Verified that all data was correctly imported, deduplicated, and that all relationships were enforced.
- The database is now ready for analysis, reporting, and dashboarding.
---

## 7. Notes on the ETL Process

- During the initial import from Excel, the SQL Server Import Wizard created tables without the `$` suffix, but the data was incomplete (many `NULL` values). On a second import, the wizard created new tables with the `$` suffix (e.g., `customers$`), which contained the correct data. Data was then migrated and cleaned from these `$` tables into the final tables (without `$`), which were created with the correct schema, primary keys, and foreign keys. After validation, the `$` tables were dropped.

---
## 8. Useful Links
- For a detailed, step-by-step ETL workflow, see [here](https://github.com/Serkan-Dursun/SerkanDursun-Portfolio/blob/76f409f865216cc7e12a0de1246648a2cd38ba71/notebooks/ETL_Processes.md).
- For the Python code used to generate the Excel dummy data, see [here](https://github.com/Serkan-Dursun/SerkanDursun-Portfolio/blob/main/notebooks/Python_Code_to_create_Excel_Dummy_Data.md).
- To view the database schema diagram, click [here](https://github.com/Serkan-Dursun/SerkanDursun-Portfolio/blob/main/images/SQL_Sales_Analysis_db_Diagram.jpg).
-  _Tip: Right-click and choose “Open link in new tab” to keep your place in this document._
