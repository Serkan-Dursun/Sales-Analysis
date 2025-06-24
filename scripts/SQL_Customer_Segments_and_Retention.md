## High-Value Customer Segments & Retention Opportunities
<strong>Business Story:</strong>
Identify which customer segments (e.g., Small Business, Enterprise, Nonprofit) generate the most revenue, and flag high-value customers who haven’t purchased in the last 6 months for retention campaigns.

<strong>Reason:</strong>
Understanding revenue contribution and recent activity by segment enables you to design more effective retention strategies. By distinguishing between loyal and at-risk customers, you can launch targeted campaigns—rewarding loyalty to encourage advocacy, and re-engaging inactive customers with personalized offers—ultimately maximizing revenue stability and long-term growth.
```sql
WITH customer_lifetime AS (
    SELECT
        c.customer_id,
        c.customer_name,
        c.segment,
        SUM(s.total) AS lifetime_value,
        MAX(s.date) AS last_purchase_date
    FROM
        customers c
        LEFT JOIN sales s ON c.customer_id = s.customer_id
    GROUP BY
        c.customer_id, c.customer_name, c.segment
)
SELECT
    segment,
    COUNT(customer_id) AS num_customers,
    SUM(lifetime_value) AS total_segment_sales,
    AVG(lifetime_value) AS avg_customer_value,
    COUNT(CASE WHEN last_purchase_date < DATEADD(MONTH, -6, GETDATE()) OR last_purchase_date IS NULL THEN 1 END) AS at_risk_customers
FROM
    customer_lifetime
GROUP BY
    segment
ORDER BY
    total_segment_sales DESC;
```
---
## Results 
![image](https://github.com/user-attachments/assets/e30990b3-a204-4b03-bc54-ef305a98d072)

---

### Key Insights

- **Small Business is the Largest and Most Lucrative Segment:**  
  With 37 customers and nearly $7M in total sales, Small Business is your top revenue driver. The average customer value is also the highest, just edging out Enterprise and Startup.

- **Individual Homeowners Are a Significant Revenue Source:**  
  This segment, often overlooked in B2B analysis, brings in almost $4M with a strong average value per customer. This indicates a healthy demand for your services among individuals, not just organizations.

- **Enterprise and Startup Segments Are High Value, But Smaller in Size:**  
  Both segments have high average values per customer, similar to Small Business, but fewer customers. These are likely strategic accounts—losing even one could have a noticeable impact.

- **Nonprofit Segment, While Small, Still Contributes Substantial Revenue:**  
  With only 9 customers, Nonprofits still generate over $1.5M, and their average value is not far behind other segments.

- **All Customers Are At-Risk:**  
  The “at-risk customers” count matches the total number of customers in each segment, suggesting that all customers have not purchased in the last 6 months. This is a critical retention warning: every customer is currently at risk of churning.

---

### Business Recommendations

- **Urgent Retention Campaign Needed:**  
  Since all customers are flagged as at-risk, immediate action is required. Consider personalized outreach, loyalty incentives, or re-engagement campaigns for every segment.

- **Segmented Retention Strategies:**  
  - For Small Business and Enterprise: Offer tailored solutions, account management, or exclusive deals.
  - For Individual Homeowners: Use targeted email or SMS campaigns, perhaps with seasonal offers.
  - For Startups and Nonprofits: Highlight cost-saving bundles or partnership opportunities.

- **Investigate Customer Activity:**  
  Double-check if the “at-risk” logic is correct (e.g., is the sales data up to date?). If so, this may indicate a seasonal business or a recent drop in engagement.

- **Monitor and Report:**  
  Set up regular reporting on at-risk customers and segment performance to catch churn risk early in the future.

---

### Executive Summary

> All customer segments are currently at risk of attrition, despite strong average values and significant total sales across segments. Immediate, segment-specific retention efforts are recommended to protect revenue and maintain customer relationships.

