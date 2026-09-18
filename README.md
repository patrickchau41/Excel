# The Overview
This project is a data cleaning and dashboard analysis of a coffee sales dataset (sourced from Kaggle), covering 1,000+ transactions from 2019 through mid-2022. The raw data was split across three separate sheets — orders, customers, and products — with no single sheet containing everything needed for analysis. I combined and cleaned this data using XLOOKUP and pivot tables, then built an interactive dashboard to explore sales by coffee type, country, roast type, bag size, and loyalty card status.

# The Questions
Below are the questions I want to answer in my project:
1. How do coffee sales trend over time, broken down by coffee type?
2. Which countries drive the most sales?
3. Who are the top customers by total spend?
4. Does loyalty card membership actually increase average order value?
5. Has average order value changed over the life of the dataset?

# Tools I Used
- **Excel**: For data cleaning, combining sheets with **XLOOKUP**, and summarizing data with **Pivot Tables**.
- **Excel Charts & Slicers**: To build the interactive dashboard, including a date-range timeline and filters for roast type, size, and loyalty card status.

# Workbook Structure
The file is organized into the following sheets:
- **orders**: The main transaction-level table (1,000+ rows) — Order ID, Order Date, Customer ID, Product ID, Quantity, Sales, and lookup fields (Customer Name, Country, Coffee Type, Roast Type, Size, Loyalty Card) pulled in via XLOOKUP.
- **customers**: Raw customer reference data (name, email, country, loyalty card status).
- **products**: Raw product reference data (coffee type, roast type, size, unit price).
- **Total Sales**: A pivot table breaking down sales by year, month, and coffee type.
- **Country BarChart**: A pivot table summarizing total sales by country.
- **Country BarChart (2)**: A pivot table summarizing total sales by top customers.
- **Dashboard**: The final interactive dashboard, pulling together all of the above into one view.

View the file here: [Coffee_Dashboard.xlsx](Coffee_Dashboard.xlsx)

# Data Cleaning
The raw dataset needed to be consolidated and cleaned before analysis:
- Removed duplicate records
- Standardized product names and corrected date formatting across the dataset
- Used **XLOOKUP** to pull customer details (name, country, loyalty card status) and product details (coffee type, roast type, size, unit price) into the main orders sheet, based on Customer ID and Product ID
- Used **Pivot Tables** to summarize sales by year/month/coffee type, by country, and by customer

# The Dashboard

The final dashboard is fully interactive, with the following controls:
- **Order Date** timeline slicer (by month, 2019–2022)
- **Roast Type** filter (Dark, Light, Medium)
- **Size** filter (0.2kg, 0.5kg, 1.0kg, 2.5kg)
- **Loyalty Card** filter (Yes/No)

![Coffee Sales Overview Dashboard](assets/dashboard_overview.png)

# The Analysis

## 1. How do coffee sales trend over time, broken down by coffee type?

The dashboard's line chart tracks monthly sales for all four coffee types (Arabica, Excelsa, Liberica, Robusta) from January 2019 through August 2022, built from the **Total Sales** pivot table.

### Results

![Total Sales Over Time](assets/q1_sales_over_time.png)

### Insights

- Monthly sales are highly volatile across all four coffee types, with no single type consistently dominating — different types spike in different months, suggesting demand shifts rather than one steady bestseller.
- No clear seasonal pattern emerges across the four-year span; peaks and dips appear scattered rather than tied to a specific time of year.

## 2. Which countries drive the most sales?

Built from the **Country BarChart** pivot table, this breaks down total sales by the three countries in the dataset.

### Results

![Sales Per Country](assets/q2_sales_per_country.png)

### Insights

- The **United States** drives the vast majority of sales, totaling **$35,638.89** — more than 5x Ireland ($6,696.87) and nearly 13x the United Kingdom ($2,798.51).
- Sales are heavily concentrated in a single market, which could represent either a genuine market-size difference or a gap in international distribution/marketing worth investigating further.

## 3. Who are the top customers by total spend?

Built from the **Country BarChart (2)** pivot table, this ranks the top 5 customers by total sales across the full dataset.

### Results

![Top 5 Customers](assets/q3_top_customers.png)

### Insights

- The top 5 customers are tightly clustered in spend, ranging from $278.01 (Don Flintiff) to $317.07 (Allis Wilmore) — no single customer dominates total revenue.
- This suggests a broad, distributed customer base rather than a business that's reliant on a small number of high-value accounts.

## 4. Does loyalty card membership actually increase average order value?

Using the **Loyalty Card** slicer on the dashboard alongside the underlying order-level data, I compared average order value between loyalty members and non-members.

### Results

![Average Order Value: Loyalty vs Non-Loyalty](assets/q4_loyalty_comparison.png)

### Insights

- **Non-loyalty customers outspent loyalty program members by $2.81 per order** on average ($46.48 vs. $43.67) — a counter-intuitive finding that suggests the loyalty program isn't translating into higher order values for the customers enrolled in it.
- This is worth flagging to whoever manages the loyalty program: the incentive may need to be redesigned if the goal is to drive larger orders.

## 5. Has average order value changed over the life of the dataset?

Using the **Order Date** timeline slicer to isolate each year, I compared average order value year over year.

### Results

![Average Order Value by Year](assets/q5_yearly_trend.png)

### Insights

- Average order value declined every year of the dataset: from **$47.05** (2019) to **$46.07** (2020), **$44.12** (2021), and **$42.55** (2022 through August).
- Combined with the loyalty card finding above, this reinforces that the loyalty program failed to sustain customer spend over time — both loyalty members specifically, and the customer base as a whole, are spending less per order as time goes on.
- Note: 2022 reflects a partial year (data through August only), so it isn't a full 12-month comparison against the other years.
