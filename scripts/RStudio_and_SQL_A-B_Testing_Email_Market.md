## A/B Test Impact on Sales: Email Marketing Campaign

### Business Story

I conducted an A/B test on our **email marketing campaign** ("Disaster Recovery Awareness") to determine which email variant drives better sales performance. 
Customers were randomly assigned to either **Group A** or **Group B**. 
The goal: measure the impact of each variant on sales, average order value, and customer engagement across different regions and segments,
and prepare the dataset for further statistical analysis and visualization in R Studio.

---

### Step-by-Step Plan

1. **Identify Groups:**  
   Each sale for the email campaign (`campaign_id = 14`) is assigned to either Group A or Group B.

2. **Define Metrics:**  
   - **Total Sales Transactions:** Number of sales per group.
   - **Unique Customers:** Number of unique customers per group.
   - **Total Sales:** Sum of all sales per group.
   - **Average Order Value (AOV):** Average sales per transaction.
   - **Customer Demographics:** Segment results by region and segment for deeper insights.

3. **Aggregate & Compare:**  
   Calculate each metric for both groups and compare performance by region and segment.

4. **Statistical Testing:**  
   Export the results for t-tests and chi-square tests in R Studio to assess the significance of observed differences.

5. **Export for R Studio:**  
   Prepare a detailed dataset for further modeling, prediction, and visualization.

---

### SQL Query for Data Extraction

```sql
SELECT
  s.group_name AS AB_Test_Group,
  c.region,
  c.segment,
  COUNT(DISTINCT s.customer_id) AS Unique_Customers,
  COUNT(*) AS Total_Sales_Transactions,
  SUM(s.total) AS Total_Sales,
  AVG(s.total) AS Avg_Order_Value
FROM sales s
JOIN customers c ON s.customer_id = c.customer_id
JOIN marketing_campaigns mc ON s.campaign_id = mc.campaign_id
WHERE s.campaign_id = 14
  AND mc.channel = 'Email'
GROUP BY s.group_name, c.region, c.segment
ORDER BY s.group_name, c.region, c.segment;
```
---
![image](https://github.com/user-attachments/assets/fe55759d-4e6d-45b9-b25f-a0e325b8e80f)
---


### Connected R Studio to SQL for data transfer 
```r
install.packages("odbc")
install.packages("DBI")

library(DBI)
library(odbc) ```
---
![RtoSQL](https://github.com/user-attachments/assets/1e9ddc84-6db4-48a3-90db-f0a4e4d82c13)
---

### Checked Data Accuracy
To see if the connection is accurate, i compared the results, and it was accurate.

''' R
ab_test <- dbGetQuery(con, "
  SELECT
    s.group_name AS AB_Test_Group,
    c.region,
    c.segment,
    COUNT(DISTINCT s.customer_id) AS Unique_Customers,
    COUNT(*) AS Total_Sales_Transactions,
    SUM(s.total) AS Total_Sales,
    AVG(s.total) AS Avg_Order_Value
  FROM sales s
  JOIN customers c ON s.customer_id = c.customer_id
  JOIN marketing_campaigns mc ON s.campaign_id = mc.campaign_id
  WHERE s.campaign_id = 14
    AND mc.channel = 'Email'
  GROUP BY s.group_name, c.region, c.segment
  ORDER BY s.group_name, c.region, c.segment
")

# View the first rows
head(ab_test)

View(ab_test)
```
--- 
### R Studio A/B Testing for Email Marketing
---



