# Excel Portfolio Projects

A collection of Excel projects showcasing end-to-end data work, from cleaning and combining messy real-world datasets to building interactive dashboards that surface actionable business insights.

## Projects

### [Coffee Sales Dashboard](coffee_sales_dashboard)

Cleans and combines a coffee sales dataset (1,000+ transactions, 2019–2026) spread across three separate sheets, then builds an interactive dashboard to explore sales by coffee type, country, roast type, bag size, and loyalty card status. Uncovers a counter-intuitive finding: non-loyalty customers outspend loyalty program members by $2.81 per order, and average order value has declined every year of the dataset. **Techniques:** XLOOKUP, Pivot Tables, Slicers, interactive dashboarding.

### [E-Commerce Marketing & Customer Analytics](ecommerce_analytics)

Analyzes a 138,000+ order e-commerce dataset (2021–2025, 24,900+ customers) across seven business questions, from marketing channel performance to RFM-based customer segmentation and product category profitability, then builds an interactive dashboard surfacing the results. Finds that top customers generate 71% more value than their size alone would predict, while an 18.62 percentage point profit margin gap between product categories reveals a real opportunity to reallocate marketing spend. Two misleading results, a regional margin gap and an inflated customer value ratio, were traced back to artifacts in the data and reported honestly rather than presented as findings. **Techniques:** RFM segmentation, XLOOKUP, Pivot Tables, PivotCharts, conditional formatting.

## Tools Used

- **Excel:** cleaning and transforming raw, multi-sheet datasets, from a thousand rows to over a hundred thousand, into analysis-ready tables
- **XLOOKUP:** combining data spread across separate sheets and files by shared IDs, without relying on rigid column ordering
- **Pivot Tables:** summarizing thousands of transaction-level rows into clear breakdowns by time, category, and customer
- **RFM Segmentation:** scoring and grouping customers by Recency, Frequency, and Monetary value using percentile-based thresholds to identify high-value and at-risk segments
- **Slicers & Interactive Dashboards:** connecting multiple charts to shared filters so a single click updates the full view
- **Conditional Formatting:** highlighting performance gaps and outliers directly within tables and charts
- **Git & GitHub:** version control and project sharing
