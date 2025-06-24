## Campaign ROI and Efficiency Analysis
<strong> Business Story: </strong>
Analyze which marketing campaigns (Google Ads, Social Media, Direct Mail, Referrals) deliver the highest sales revenue, return on investment (ROI), and web conversion rates to optimize marketing spend allocation.

<strong> Reason: </strong>
Understanding campaign performance across multiple metrics enables data-driven budget allocation, identifies the most cost-effective channels, and supports strategic decisions on scaling successful campaigns while reducing spend on underperforming ones

``` sql
SELECT
    mc.campaign_name AS Campaign_Name,
    mc.channel AS Marketing_Channel,
    FORMAT(mc.spend, 'C0', 'en-US') AS Campaign_Spend,
    FORMAT(SUM(s.total), 'C0', 'en-US') AS Total_Sales_Generated,
    CASE 
        WHEN mc.spend > 0 THEN FORMAT(SUM(s.total) * 1.0 / mc.spend, 'N2') 
        ELSE 'N/A' 
    END AS ROI_Ratio,
    FORMAT(SUM(wa.visits), 'N0') AS Total_Web_Visits,
    FORMAT(SUM(wa.conversions), 'N0') AS Total_Web_Conversions,
    CASE 
        WHEN SUM(wa.visits) > 0 THEN FORMAT(SUM(wa.conversions) * 1.0 / SUM(wa.visits) * 100, 'N2') + '%'
        ELSE 'N/A' 
    END AS Web_Conversion_Rate,
    CASE 
        WHEN SUM(wa.conversions) > 0 THEN FORMAT(mc.spend * 1.0 / SUM(wa.conversions), 'C2', 'en-US')
        ELSE 'N/A' 
    END AS Cost_Per_Conversion
FROM
    marketing_campaigns mc
    LEFT JOIN sales s ON s.campaign_id = mc.campaign_id
    LEFT JOIN web_analytics wa ON wa.campaign_id = mc.campaign_id
GROUP BY
    mc.campaign_name, mc.channel, mc.spend
ORDER BY
    SUM(s.total) DESC;
```
---

![image](https://github.com/user-attachments/assets/9f47aef3-10c3-415b-a5b2-0e8e2ee811ec)


### Key Insights

- **Exceptional ROI:**  
  Several campaigns, such as "Back-to-School Tech," "Cloud Security Awareness," and "Homeowner Tech Support," achieved ROI ratios above 20,000, indicating extremely efficient use of marketing spend.

- **Consistently High Conversion Rates:**  
  Most campaigns maintain web conversion rates around 5%, with "Summer Security Special" and "VoIP Migration Drive" slightly higher, suggesting effective targeting and messaging.

- **Low Cost Per Conversion:**  
  All campaigns demonstrate impressively low cost per conversion (as low as $0.01–$0.08), maximizing the impact of each marketing dollar.

- **Top Revenue Drivers:**  
  "Emergency Response Campaign" and "Small Business IT Focus" are the highest in total sales generated, making them prime candidates for continued or increased investment.

- **Channel Effectiveness:**  
  Events, Social Media, and Referrals are among the most effective channels, each supporting multiple high-performing campaigns.

---

### Business Recommendations

- **Scale High-ROI Campaigns:**  
  Increase budget allocation to campaigns with the highest ROI and lowest cost per conversion, such as "Back-to-School Tech" and "Cloud Security Awareness."

- **Optimize Underperformers:**  
  While all campaigns are performing well, those with relatively lower ROI or higher cost per conversion (e.g., "New Year Network Upgrade," "VoIP Migration Drive") should be reviewed for potential improvements in targeting or creative.

- **Leverage Successful Channels:**  
  Use strategies from top-performing channels (Events, Social Media, Referrals) as templates for future campaigns.

- **Continuous Monitoring:**  
  Establish regular performance reviews to ensure ongoing optimization and to quickly identify any shifts in campaign effectiveness.

---

### Executive Summary

> The marketing team’s campaigns are delivering outstanding results, with high ROI, strong conversion rates, and minimal cost per conversion across all channels. Strategic scaling of the most efficient campaigns and ongoing optimization will further enhance marketing impact and revenue growth.
