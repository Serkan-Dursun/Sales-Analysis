# 📊 Sales Performance Analysis and Heatmap Visualization by Region in R Studio

This document outlines the process of creating a sales performance heatmap using R and SQL Server, focusing on **average sales per transaction** to ensure fair comparison across regions with different numbers of sales representatives.

---

## 🎯 Objective

Create a heatmap visualization showing **average sales performance by region and product** to identify:
- Which products perform best in each region
- Regional performance patterns that aren't skewed by rep distribution
- Data-driven insights for sales strategy optimization

---

## 🔧 Technical Setup

### Required R Packages
```r
library(DBI)
library(odbc)
library(ggplot2)
library(dplyr)

---
📈 Analysis Approach
Why Average Instead of Total Sales?
Problem: Different regions have varying numbers of sales representatives:

Irvine: 70% of customers (heavily weighted)
Other regions: 10% each (Santa Ana, Costa Mesa, Tustin)
Solution: Use average sales per transaction instead of total sales to ensure fair comparison across regions.

---

## 💻 Data Extraction
SQL Query for Average Sales by Region and Product
```sql
sales_region_product_avg <- dbGetQuery(con, "
  SELECT
    c.region,
    p.product_name,
    AVG(s.total) AS avg_sales
  FROM sales s
  JOIN customers c ON s.customer_id = c.customer_id
  JOIN products p ON s.product_id = p.product_id
  GROUP BY c.region, p.product_name
  ORDER BY c.region, p.product_name
")
```

---
## Data Validation on R
Checked the structure of extracted data
```r
head(sales_region_product_avg)
str(sales_region_product_avg)

# Verify data completeness
summary(sales_region_product_avg)

```

---


### 🎨 Visualization
Heatmap Creation
```r
library(ggplot2)

# Create heatmap
ggplot(sales_region_product_avg, aes(x = product_name, y = region, fill = avg_sales)) +
  geom_tile(color = "white") +
  scale_fill_gradient(low = "white", high = "darkgreen") +
  labs(
    title = "Average Sales per Transaction by Region and Product",
    subtitle = "Fair comparison across regions with different rep distributions",
    x = "Product",
    y = "Region",
    fill = "Avg Sales ($)"
  ) +
  theme_minimal() +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1),
    plot.title = element_text(size = 14, face = "bold"),
    plot.subtitle = element_text(size = 12, color = "gray60")
  )


```

---

## Results
![image](https://github.com/user-attachments/assets/90c894ff-1490-4a29-805d-4829a66341ef)
- Product Performance: Which products have higher average transaction values
- Regional Patterns: Performance differences that aren't due to rep count
- Strategic Opportunities: Underperforming product-region combinations
---

## 📊 Heatmap Analysis: Average Sales per Transaction by Region and Product

---

### **Key Observations**

- **Product-Region Standouts:**  
  - *Santa Ana, California* shows the highest average sales for **CCTY Systems** and **Telecom Solutions**, indicating these products are particularly successful in this region.
  - *Tustin, California* has strong performance for **Managed IT** and **Emergency Support**, suggesting a local preference or higher-value deals for these services.
  - *Costa Mesa, California* sees relatively high averages for **Disaster Recovery** and **Technical Consultancy**.

- **Irvine, California:**  
  Despite having the largest customer base, Irvine’s average sales per transaction are more evenly distributed across products, with no single product dominating. This suggests a balanced market or more standardized deal sizes.

- **Product Consistency:**  
  Products like **Data Storage**, **Helpdesk**, and **Firewall Setup** tend to have lower average sales across all regions, possibly indicating these are lower-ticket or more commoditized offerings.

- **Regional Specialization:**  
  Some products (e.g., **CCTY Systems**, **Managed IT**) have high average sales in only one or two regions, hinting at regional specialization, local demand, or effective sales strategies in those areas.

- **Potential Gaps:**  
  There are a few missing or very light cells (e.g., **Mobile Device Management** in Costa Mesa), which could indicate either no sales or very low average sales for those product-region combinations. This may represent untapped market opportunities.

---

### **Business Insights & Recommendations**

- **Leverage Regional Strengths:**  
  Focus marketing and sales efforts for **CCTY Systems** and **Telecom Solutions** in Santa Ana, and for **Managed IT** and **Emergency Support** in Tustin, where these products already perform well.

- **Investigate High Performers:**  
  Analyze what drives high average sales for certain products in specific regions (e.g., sales tactics, customer needs, local partnerships) and replicate these strategies in other regions.

- **Address Underperformers:**  
  For products with consistently low average sales (e.g., **Helpdesk**, **Firewall Setup**), consider revisiting pricing, bundling, or value proposition, or focus on upselling/cross-selling in those categories.

- **Explore Untapped Markets:**  
  Where there are missing or very low values, assess whether there is a lack of demand, awareness, or sales effort. Pilot targeted campaigns to test market potential.

- **Balanced Approach in Irvine:**  
  Since Irvine’s sales are more evenly spread, maintain a broad product offering but look for opportunities to create standout products through differentiation or targeted promotions.

---
*This heatmap provides actionable insights into where your products are excelling or underperforming on a per-transaction basis, allowing for smarter, data-driven regional and product strategies.*




