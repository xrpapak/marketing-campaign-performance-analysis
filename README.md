
# Marketing Campaign Performance Analysis

This project analyzes marketing campaigns' effectiveness using SQL and Python, focusing on KPIs such as ROAS, CPA, Conversion Rate, and Customer Satisfaction. The goal was to extract meaningful insights from campaign-level data and visualize trends supporting data-driven marketing strategy decisions.

---

## 📁 Dataset Overview

The dataset comprises several marketing KPIs and campaign attributes:
- Budget, Clicks, Conversions
- Revenue Generated, ROI, CPA, ROAS
- Discount Levels, Units Sold, Bundle Price
- Customer Satisfaction (Post Refund)
- Campaign Text Metadata (Common Keywords, Subscription Tier)

---

## 🧮 SQL Analysis

### Insight 1: Average KPIs per Subscription Tier

```sql
SELECT
  Subscription_Tier,
  ROUND(AVG(Conversion_Rate), 2) AS Avg_Conversion_Rate,
  ROUND(AVG(CPA), 2) AS Avg_CPA,
  ROUND(AVG(ROAS), 2) AS Avg_ROAS,
  ROUND(AVG(Customer_Satisfaction_Post_Refund), 2) AS Avg_Satisfaction
FROM
  marketing_kpis
GROUP BY
  Subscription_Tier;
```

The Basic tier had the lowest CPA, while the Standard tier exhibited the highest ROAS. Customer Satisfaction scores were relatively consistent across tiers.

---

### Insight 2: Correlation Between All KPIs

```sql
SELECT
  * EXCEPT(Campaign_ID)
FROM
  marketing_kpis;
```

A correlation matrix was computed via Python to identify strong and weak linear relationships among numerical KPIs. For example, CPA negatively correlates with ROAS, as expected.

_Image: `correlation_matrix_marketing_product_kpis.png`_

---

### Insight 3: Discount Level vs Conversion Rate

```sql
SELECT
  Discount_Level,
  ROUND(AVG(Conversion_Rate), 2) AS Avg_Conversion_Rate
FROM
  marketing_kpis
GROUP BY
  Discount_Level
ORDER BY
  Discount_Level;
```

While we expected higher discounts to yield better conversions, the graph showed inconsistency beyond 30–40% discount, indicating diminishing returns.

_Image: `discount_level_vs_conversion_rate.png`_

---

### Insight 4: Campaigns with High ROAS, Low CPA, and High Satisfaction

```sql
SELECT
  Campaign_ID,
  CPA,
  ROAS,
  Customer_Satisfaction_Post_Refund,
  Revenue_Generated,
  Common_Keywords
FROM
  marketing_kpis
WHERE
  CPA < 10
  AND ROAS > 20
  AND Customer_Satisfaction_Post_Refund >= 3;
```

Bubble plot visualization identified top campaigns across keywords like *Affordable* and *Innovative*.

_Image: `highlight_perfect_campaigns.png`_

---

### Insight 5: Revenue Distribution

```sql
SELECT
  Revenue_Generated
FROM
  marketing_kpis;
```

Revealed that the revenue distribution was relatively uniform with some outliers.

_Image: `revenue_generated_distribution.png`_

---

### Insight 6: ROAS Distribution by Customer Satisfaction

```sql
SELECT
  Customer_Satisfaction_Post_Refund,
  ROAS
FROM
  marketing_kpis;
```

Visualized ROAS variance across satisfaction levels; the higher the satisfaction, the more consistent the ROAS.

_Image: `roas_distribution_by_customer_satisfaction.png`_

---

### Insight 7: ROAS vs CPA across Subscription Tiers

```sql
SELECT
  CPA,
  ROAS,
  Subscription_Tier,
  Revenue_Generated
FROM
  marketing_kpis;
```

Bubble plot revealed Premium campaigns often had lower CPA and higher ROAS simultaneously.

_Image: `roas_vs_cpa.png`_

---

## 🐍 Python Analysis

### Data Cleaning

```python
# Remove duplicates
df = df.drop_duplicates()

# Handle missing values
df['Revenue_Generated'] = df['Revenue_Generated'].fillna(df['Revenue_Generated'].median())
df['Customer_Satisfaction_Post_Refund'] = df['Customer_Satisfaction_Post_Refund'].fillna(df['Customer_Satisfaction_Post_Refund'].mode()[0])
```

Outliers were also identified using z-score filtering in some KPIs (e.g., Revenue, CPA).

---

### Visualizations

- Barplot: Average KPIs per Subscription Tier  
  _Image: `images/average_kpis_per_subscription_tier.png`_

- Heatmap: Correlation matrix of marketing & product KPIs  
  _Image: `images/correlation_matrix_marketing_product_kpis.png`_

- Lineplot: Discount Level vs Conversion Rate  
  _Image: `images/discount_level_vs_conversion_rate.png`_

- Bubble Plot: Perfect Campaigns (Low CPA, High ROAS, High Satisfaction)  
  _Image: `images/highlight_perfect_campaigns.png`_

- Histogram + KDE: Revenue Distribution  
  _Image: `images/revenue_generated_distribution.png`_

- Boxplot: ROAS by Customer Satisfaction  
  _Image: `images/roas_distribution_by_customer_satisfaction.png`_

- Bubble Plot: ROAS vs CPA by Subscription Tier  
  _Image: `images/roas_vs_cpa.png`_

---

## 📌 Summary of Findings

- Higher discounts do not always yield better conversion.
- The "Affordable" keyword consistently aligns with better campaign performance.
- Low CPA & high ROAS combinations are rare but highly valuable.
- Premium tier campaigns appear more optimized than Standard or Basic.
- There is a weak correlation between click-based metrics and final ROAS, signaling creative/textual importance.

---

## 👤 Author

Christos Papakostas
