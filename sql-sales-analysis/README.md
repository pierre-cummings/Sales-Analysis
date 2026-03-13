# SQL Sales Analysis

## Project Overview

This project analyzes retail sales data using SQL to identify key business insights related to revenue performance, product performance, regional sales trends, and customer value.

The dataset represents order-level sales transactions containing customer information, product categories, geographic regions, and revenue values.

---

## Executive Summary

This analysis explores retail sales performance across customers, products, regions, and time.

Key findings include:

- Revenue is concentrated among a relatively small number of products and customers.
- Sales performance varies significantly across geographic regions.
- Monthly sales trends fluctuate, suggesting possible seasonal demand patterns.
- Identifying top-performing products and customers helps businesses prioritize marketing, inventory planning, and customer relationship management.

---

## Tools Used

- SQL
- Microsoft SQL Server
- SQL Server Management Studio

---

## Dataset Structure

The dataset contains order-level transaction data with the following key columns:

| Column | Description |
|------|------|
| Row_ID | Unique row identifier |
| Order_ID | Unique identifier for each order |
| Order_Date | Date the order was placed |
| Ship_Date | Date the order was shipped |
| Customer_ID | Unique customer identifier |
| Customer_Name | Name of the customer |
| Segment | Market segment |
| Country | Country of the order |
| City | City of the order |
| State | State of the order |
| Postal_Code | Postal code |
| Region | Geographic sales region |
| Product_ID | Unique product identifier |
| Category | Product category |
| Sub_Category | Product subcategory |
| Product_Name | Product name |
| Sales | Revenue generated from the order |

---

## Data Preparation

Before performing the analysis, the dataset was imported into a SQL Server database and structured into a table named **Superstore**.

Preparation steps included:

- Importing the CSV dataset into SQL Server
- Verifying column names and data types
- Using `DISTINCT Order_ID` when counting orders to avoid double-counting orders containing multiple products

---

## Business Questions

This SQL analysis answers the following business questions:

- What are the overall sales metrics?
- Which regions generate the most revenue?
- How do sales change over time?
- Which products generate the most revenue?
- Which customers generate the most revenue within each region?
- Which products perform best within each product category?

---

# 1. Overall Business Metrics

## Business Question

What are the total number of orders, total revenue, and average sale value?

## SQL Query

```sql
SELECT
COUNT(DISTINCT Order_ID) AS TotalOrders,
ROUND(SUM(Sales), 2) AS TotalSales,
ROUND(AVG(Sales), 2) AS AvgSalePerRow
FROM Superstore;
```

## Explanation

This query calculates key performance indicators for the dataset.

- `COUNT(DISTINCT Order_ID)` counts unique orders
- `SUM(Sales)` calculates total revenue
- `AVG(Sales)` calculates the average sales value per row

## Why This Matters to the Business

These metrics provide a high-level snapshot of business performance and establish baseline indicators used to evaluate overall sales health.

## Result

<img width="1905" height="829" alt="KPI Overview" src="https://github.com/user-attachments/assets/62fa102d-3f67-438d-a9f0-e71bf8af7047" />


---

# 2. Sales Performance by Region

## Business Question

Which geographic regions generate the most revenue?

## SQL Query

```sql
SELECT
Region,
ROUND(SUM(Sales), 2) AS TotalSales,
COUNT(DISTINCT Order_ID) AS TotalOrders
FROM Superstore
GROUP BY Region
ORDER BY TotalSales DESC;
```

## Explanation

This query aggregates revenue and order counts by region to compare geographic market performance.

## Why This Matters to the Business

Regional analysis helps businesses identify high-performing markets and determine where to focus inventory distribution, logistics support, and regional marketing.

## Result

![Sales By Region](screenshots/Sales By Region.png)

---

# 3. Monthly Sales Trend

## Business Question

How does revenue change over time?

## SQL Query

```sql
SELECT
FORMAT(Order_Date,'yyyy-MM') AS Month,
ROUND(SUM(Sales), 2) AS TotalSales,
COUNT(DISTINCT Order_ID) AS TotalOrders
FROM Superstore
GROUP BY FORMAT(Order_Date,'yyyy-MM')
ORDER BY Month;
```

## Explanation

This query groups sales by month to analyze revenue trends and detect possible seasonal demand patterns.

## Why This Matters to the Business

Understanding time-based sales patterns helps organizations forecast demand, plan marketing campaigns, and prepare inventory for seasonal fluctuations.

## Result

![Sales Trend](screenshots/Sales Trend By Month.png)

---

# 4. Top Products by Sales

## Business Question

Which products generate the highest revenue?

## SQL Query

```sql
SELECT TOP 10
Product_Name,
ROUND(SUM(Sales), 2) AS TotalSales
FROM Superstore
GROUP BY Product_Name
ORDER BY TotalSales DESC;
```

## Explanation

This query identifies the products contributing the highest revenue.

## Why This Matters to the Business

High-performing products often drive a large share of total revenue. Identifying these products helps guide marketing priorities, inventory planning, and promotional strategies.

## Result

![Top Product Sales](screenshots/Top Product Sales.png)

---

# 5. Top Customers by Region

## Business Question

Which customers generate the most revenue within each region?

## SQL Query

```sql
WITH CustomerSales AS (
    SELECT
        Customer_Name,
        Region,
        COUNT(DISTINCT Order_ID) AS Number_Of_Orders,
        SUM(Sales) AS TotalSales
    FROM Superstore
    GROUP BY Region, Customer_Name
),

RankedCustomers AS (
    SELECT
        Customer_Name,
        Region,
        Number_Of_Orders,
        TotalSales,
        ROW_NUMBER() OVER (
            PARTITION BY Region
            ORDER BY TotalSales DESC
        ) AS Customer_Rank
    FROM CustomerSales
)

SELECT
Customer_Name,
Region,
Number_Of_Orders,
ROUND(TotalSales, 2) AS Total_Sales,
Customer_Rank
FROM RankedCustomers
WHERE Customer_Rank <= 3
ORDER BY Region, Customer_Rank;
```

## Explanation

This query identifies the top customers within each region based on revenue.

- A **Common Table Expression (CTE)** calculates customer-level sales metrics
- A **window function (`ROW_NUMBER`)** ranks customers within each region by revenue

## Why This Matters to the Business

Identifying high-value customers helps organizations focus retention strategies, account management, and targeted promotions.

## Result

![Top Customers](screenshots/top_customers_per_region.png)

---

# 6. Top Products Within Each Category

## Business Question

Which products perform best within each product category?

## SQL Query

```sql
WITH ProductSales AS (
    SELECT
        Category,
        Product_Name,
        SUM(Sales) AS TotalSales
    FROM Superstore
    GROUP BY Category, Product_Name
),

RankedProducts AS (
    SELECT
        Category,
        Product_Name,
        TotalSales,
        ROW_NUMBER() OVER (
            PARTITION BY Category
            ORDER BY TotalSales DESC
        ) AS Product_Rank
    FROM ProductSales
)

SELECT
Category,
Product_Name,
ROUND(TotalSales, 2) AS Total_Sales,
Product_Rank
FROM RankedProducts
WHERE Product_Rank <= 3
ORDER BY Category, Product_Rank;
```

## Explanation

This query ranks products within each category based on revenue using a window function.

## Why This Matters to the Business

Understanding top-performing products within each category helps managers optimize product assortment, pricing strategies, and promotional focus.

## Result

![Top Products By Category](screenshots/top_products_by_category_rank.png)

---

# Key Business Insights

- Sales performance varies significantly across geographic regions.
- A relatively small number of products generate a large portion of total revenue.
- Monthly sales fluctuate over time, indicating possible seasonality in customer demand.
- Certain customers generate significantly higher revenue than others within their regions.

---

# Repository Structure

```
sql-sales-analysis
│
├── README.md
└── outputs
    ├── KPI Overview.png
    ├── Sales By Region.png
    ├── Sales Trend By Month.png
    ├── Top Product Sales.png
    ├── top_customers_per_region.png
    └── top_products_by_category_rank.png
```

---

# Skills Demonstrated

- SQL Querying
- Data Aggregation
- Window Functions (`ROW_NUMBER`)
- Business Performance Analysis
- Data Exploration
- Translating data into business insights
