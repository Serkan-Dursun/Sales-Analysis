![image](https://github.com/user-attachments/assets/85e5bad0-6b4b-4964-8180-82223ded9323)

---
R Studio Code
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
