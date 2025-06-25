# Sales Performance Analysis

## Overview
This project showcases my ability to analyze sales data, identify business trends, and provide actionable insights to inform sales and marketing strategies. The analysis uses SQL for data extraction and cleaning, Tableau for interactive dashboards, and Excel VBA for automation and reporting.

---

## Table of Contents
- [Objectives](#objectives)
- [Tools & Technologies](#tools--technologies)
- [Business Impact](#business-impact)
- [SQL Database Schema](#database-schema)
- [Data Generation & Import](#data-generation--import)
- [Project Structure & Key Links](#project-structure--key-links)

---

## Objectives
- Identify key sales trends and performance drivers
- Optimize campaign targeting and client acquisition strategies
- Automate recurring sales reports to improve efficiency
- Visualize sales pipeline and forecasting for business stakeholders

---

## Tools & Technologies
- SQL (data extraction, cleaning, and transformation)
- Tableau (dashboard creation and data visualization)
- Excel VBA (report automation and advanced analytics)
- Power Query (data preparation in Excel)

---

## Business Impact
- Improved sales forecasting accuracy and pipeline visibility
- Enabled data-driven decision-making for sales and marketing teams
- Increased efficiency through automation and streamlined reporting

---

## Database Schema

The database is designed as a star schema, with a central fact table (`sales`) and several dimension and analytics tables. All relationships are enforced with primary and foreign keys.

**Entity-Relationship Diagram (ERD):**

![SQL Entity Relational Database Diagram](images/SQL_Sales_Analysis_db_Diagram.jpg)

**Main Tables:**
- `customers`: Customer information, including business and individual segments.
- `products`: IT services and products offered.
- `sales_reps`: Sales representatives and their regions.
- `marketing_campaigns`: Details of marketing campaigns, channels, and performance.
- `sales`: Fact table recording each sales transaction, linked to all relevant dimensions.
- `web_analytics`: Daily web metrics for each campaign.
- `ab_test_results`: Results of A/B tests, linked to campaigns for integrated analysis.

---

## Data Generation & Import

- **Mock Data**: Generated using Python, with realistic company and individual names, product lists, and campaign details.  
  See [`notebooks/Python_Code_to_create_Excel_Dummy_Data.md`](notebooks/Python_Code_to_create_Excel_Dummy_Data.md).
- **Excel Data File**: [`data/sales_performance_data.xlsx`](data/sales_performance_data.xlsx)
- **Import Process**:  
  - Data imported into SQL Server using the Import Wizard.  
  - Temporary `$` tables created by the wizard were cleaned, deduplicated, and migrated into final tables.  
  - All primary and foreign key constraints are enforced for data integrity.  
  - Orphaned and duplicate records were identified and removed.
- **Schema Evolution**:  
  - Surrogate keys (e.g., `result_id`) added for uniqueness.  
  - Foreign keys added to connect A/B test results to marketing campaigns.
- **ETL Documentation**:  
  See [`notebooks/Excel_to_SQL_Import_Guide.md`](notebooks/ETL_Processes.md) for a detailed step-by-step ETL process.

---

## Project Structure & Key Links
