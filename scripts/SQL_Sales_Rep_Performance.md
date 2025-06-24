## Sales Representative Performance Analysis

### Business Story
Evaluate the overall sales effectiveness of each sales representative, highlighting top performers by total sales, product volume, customer reach, and customer revenue contribution.

### Reason
A comprehensive view of sales representative performance enables leadership to identify top performers, allocate resources effectively, design targeted incentive programs, and uncover opportunities for coaching and growth.

---

### SQL Query

```sql
WITH rep_customer_sales AS (
    SELECT
        s.sales_rep_id,
        s.customer_id,
        COUNT(*) AS num_sales
    FROM sales s
    GROUP BY s.sales_rep_id, s.customer_id
)
SELECT
    sr.sales_rep_name AS Sales_Rep,
    FORMAT(SUM(s.total), 'C0', 'en-US') AS Total_Sales,
    COUNT(s.product_id) AS Total_Products_Sold,
    COUNT(DISTINCT s.customer_id) AS Unique_Customers_Served,
    CAST(COUNT(s.product_id) AS FLOAT) / NULLIF(COUNT(DISTINCT s.customer_id), 0) AS Avg_Products_Per_Customer,
    FORMAT(SUM(s.total) * 1.0 / NULLIF(COUNT(DISTINCT s.customer_id), 0), 'C0', 'en-US') AS Customer_Revenue_Contribution,
    FORMAT(
        COUNT(DISTINCT CASE WHEN rcs.num_sales > 1 THEN rcs.customer_id END) * 100.0 /
        NULLIF(COUNT(DISTINCT rcs.customer_id), 0), 'N2'
    ) + '%' AS Repeat_Purchase_Rate
FROM
    sales s
    JOIN sales_reps sr ON s.sales_rep_id = sr.sales_rep_id
    LEFT JOIN rep_customer_sales rcs ON s.sales_rep_id = rcs.sales_rep_id
GROUP BY
    sr.sales_rep_name
ORDER BY
    SUM(s.total) DESC;
```
---

![image](https://github.com/user-attachments/assets/cd5bc75b-76f6-44d8-b573-481769b74820)


---
### Key Insights

- **Top Performers:**  
  Jennifer Lee and Lisa Wang consistently lead in total sales, product volume, and customer reach, setting a high standard for the team.

- **Cross-Selling Strength:**  
  High average products per customer among top reps highlights their ability to fulfill multiple customer needs within single relationships.

- **Customer Value Maximization:**  
  Jennifer Lee’s high customer revenue contribution suggests a focus on high-value accounts and effective upselling.

- **Customer Loyalty:**  
  The highest repeat purchase rates are seen among the top performers, reflecting strong relationship management and customer retention.

- **Opportunities for Growth:**  
  Some reps have strong product sales but lower repeat purchase rates, indicating potential to improve customer follow-up and retention strategies.

---

### Business Recommendations

- **Recognize and Reward Excellence:**  
  Incentivize top performers for achievements in sales, customer value, and repeat business to reinforce successful behaviors.

- **Promote Cross-Sell Best Practices:**  
  Facilitate knowledge sharing and targeted training on cross-selling techniques to help all reps increase average products per customer.

- **Enhance Customer Retention:**  
  Provide coaching and tools for reps with lower repeat purchase rates to improve follow-up, relationship management, and upselling.

- **Diversify Customer Portfolios:**  
  Encourage reps to broaden their customer base to reduce dependency on a few large accounts and mitigate risk.

- **Monitor and Address Gaps:**  
  Use regular KPI reviews to identify underperforming areas and tailor support or training accordingly.

---

### Executive Summary

> This analysis reveals clear leaders in sales performance, customer value, and customer loyalty, while also highlighting areas for team-wide improvement.
> By focusing on cross-sell training, customer retention strategies, portfolio diversification, and targeted support, the team can drive sustainable revenue growth and maintain a competitive edge.

---
