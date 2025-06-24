# 📈 A/B Test Segment Analysis Report – Email Campaign

This report provides a breakdown of customer segments based on A/B test results for Email Campaign ID 14.

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

## 🧠 Key Insights

- **Group B dominates** in Enterprise, Startup, and Small Business segments for total revenue, reach, and average order values.
- **Group A excels** among Individual Homeowners, with a higher average spend per customer.
- **Nonprofit** is a weak segment for both versions, with low engagement and revenue.

---

## ✅ Recommendations

- **Deploy Email B** for:
  - Enterprise
  - Startups
  - Small Business

- **Use Email A** for:
  - Individual Homeowners

- **Nonprofit strategy should be reworked:**
  - Consider custom messaging or a donation-oriented approach to improve engagement and results.

---
