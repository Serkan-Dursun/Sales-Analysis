# SQL Database Creation and ETL Process

This document details the complete workflow for building the Sales Performance Analysis database, including schema creation, data import from Excel, data cleaning, enforcing data integrity, and preparing the data for analysis.

---

## 1. Database Schema Creation

The database was designed as a star schema, with a central fact table (`sales`) and several dimension and analytics tables. All relationships are enforced with primary and foreign keys.

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
