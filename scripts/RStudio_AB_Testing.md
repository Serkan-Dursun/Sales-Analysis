# 📈 A/B Test Segment Analysis Report – Email Campaign

- This report provides a breakdown of customer segments based on A/B test results for Email Campaign ID 14.
- Group B’s email variant outperformed Group A in most segments, particularly among Enterprise and Startup customers.
- Based on this analysis, I recommend using Email B for these segments and refining the strategy for Nonprofit and Individual Homeowner groups.

## 🧪 Code to Generate Segment Summary

```r
install.packages("dplyr")
library(dplyr)

# Group by segment and AB test group
segment_summary <- ab_test %>%
  group_by(segment, AB_Test_Group) %>%
  summarise(
    Total_Customers = sum(Unique_Customers),
    Total_Transactions = sum(Total_Sales_Transactions),
    Total_Sales = sum(Total_Sales),
    Avg_Order_Value = round(mean(Avg_Order_Value), 2),
    .groups = 'drop'
  )
print(segment_summary)

```
---

### Code Results on R
![image](https://github.com/user-attachments/assets/8edda217-0fac-4232-a67b-870e68b81296)



## 📊 Segment-Level Results

| Segment               | Group A                              | Group B                              |
|-----------------------|--------------------------------------|--------------------------------------|
| Enterprise            | 1 customer, $200                     | 9 customers, $232,846                |
| Individual Homeowner  | 6 customers, $153,754 (avg $23,357)  | 7 customers, $135,464 (avg $13,350)  |
| Nonprofit             | 2 customers, $27,051                 | 2 customers, $7,114                  |
| Small Business        | 10 customers, $142,614 (avg $12,911) | 13 customers, $224,068 (avg $14,874) |
| Startup               | 5 customers, $57,228 (avg $10,330)   | 7 customers, $170,203 (avg $17,452)  |

---

## Fair A/B Test Comparison: Segment-Level Insights Visualization
To ensure a fair comparison between A and B groups, we focused on average order value and average sales per customer (not just totals), since group sizes differ across segments.
```r
library(ggplot2)

# Average Order Value by Segment and Group
ggplot(segment_summary, aes(x = segment, y = Avg_Order_Value, fill = AB_Test_Group)) +
  geom_bar(stat = "identity", position = "dodge") +
  labs(title = "Average Order Value by Segment and A/B Group",
       x = "Segment", y = "Average Order Value") +
  theme_minimal()

# Average Sales Per Customer by Segment and Group
ggplot(segment_summary, aes(x = segment, y = Avg_Sales_Per_Customer, fill = AB_Test_Group)) +
  geom_bar(stat = "identity", position = "dodge") +
  labs(title = "Average Sales Per Customer by Segment and A/B Group",
       x = "Segment", y = "Average Sales Per Customer") +
  theme_minimal()

```

---
### Results
![image](https://github.com/user-attachments/assets/4e15323b-24ac-49e2-a136-5c7228df927f)

--- 
## 🧠 Key Insights
For each segment and group, we report both average sales per customer (Total Sales ÷ Total Customers) and, where available, average order value (Total Sales ÷ Total Transactions). In cases where each customer made only one purchase, these values are identical.
- **Enterprise & Startup:**  
  Group B outperformed Group A by a wide margin in average sales per customer (*$25,872.89 vs$200.00**), reflecting both a larger customer base and higher spend per customer.

- **Individual Homeowner & Nonprofit:**  
  Group A had a higher average sales per customer (*$25,625.67 vs$19,352.00**), suggesting this segment responded better to Group A’s email variant.
  
- **Nonprofit:**  
  Both groups showed low engagement and revenue, but Group A led in average sales per customer (*$13,525.50 vs$3,557.00**).
  
- **Small Business:**  
 Group B slightly outperformed Group A in average sales per customer (*$17,082.15 vs$14,261.40**), but the difference is modest.

- **Startup :**  
  Group B also led in average sales per customer (*$24,314.71 vs$11,445.60**), indicating stronger engagement and higher value per customer.

  
---
These results highlight the importance of using average-based metrics for fair A/B test comparisons, especially when group sizes differ. Group B’s email variant is most effective for Enterprise, Startup, and Small Business segments, while Group A’s approach resonates more with Individual Homeowners and Nonprofits.
---

## ✅ Recommendations

- **Use average-based metrics** (average order value and average sales per customer) for A/B test decisions, especially when group sizes are unbalanced.
- **Visualize** both average order value and average sales per customer for each segment to guide strategy.

- **Deploy Email B for:**
  - Enterprise
  - Startups
  - Small Business

- **Use Email A for:**
  - Individual Homeowners

- **Rework the Nonprofit strategy:**
  - Consider custom messaging or a donation-oriented approach to improve engagement and results.

---
## Links
- To see the ETL process from SQL to R Studio, you can visit [this link](scripts/SQL_and_RStudio_A-B_Testing_Email_Marketing_.md)
---
