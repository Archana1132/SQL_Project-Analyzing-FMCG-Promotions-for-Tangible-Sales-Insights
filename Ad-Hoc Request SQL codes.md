### Retail Analytics Report

## Overview

This README provides a summary of SQL queries used to generate various reports for retail analytics purposes. The SQL queries are designed to extract essential insights from the revenue data.

## Task 1: High-Value Discounted Products

```sql
-- Provide a list of products with a base price greater than 500 and that are featured in promo type of 'BOGOF' (Buy One Get One Free). 
-- This information will help us identify high-value products that are currently being heavily discounted, which can be useful for evaluating our pricing and promotion strategies.

SELECT DISTINCT 
    product_name,
    base_price,
    promo_type
FROM 
    Revenue 
WHERE 
    base_price > 500 
    AND promo_type = "BOGOF";
```

## Task 2 : Store Presence Overview

```sql
-- Generate a report that provides an overview of the number of stores in each city.
-- The results will be sorted in descending order of store counts, allowing us to identify the cities with the highest store presence.
-- The report includes two essential fields: city and store count, which will assist in optimizing our retail operations.

SELECT 
    city,
    COUNT(DISTINCT store_id) AS Store_count
FROM 
    Revenue
GROUP BY 
    city
ORDER BY 
    Store_count DESC;
```

## Task 3: Campaign Revenue Analysis

```sql

-- Generate a report that displays each campaign along with the total revenue generated before and after the campaign.
-- The report includes three key fields: campaign name, total_revenue(before_promotion), total revenue(after_promotion).
-- This report should help in evaluating the financial impact of our promotional campaigns. (Display the values in millions)

SELECT 
    campaign_name,
    CONCAT(ROUND(SUM(Revenue_Before_Promo)/1000000, 2), 'M') AS Revenue_Before_Promotion,
    CONCAT(ROUND(SUM(Revenue_After_Promo)/1000000, 2), 'M') AS Revenue_After_Promotion
FROM 
    Revenue 
GROUP BY 
    campaign_name
ORDER BY 
    campaign_name ASC;
```

## Task 4: Diwali Campaign ISU Analysis

```sql

-- Produce a report that calculates the Incremental Sold Quantity (ISU%) for each category during the Diwali campaign.
-- Additionally, provide rankings for the categories based on their ISU%.
-- The report will include three key fields: category, isu%, and rank order.
-- This information will assist in assessing the category-wise success and impact of the Diwali campaign on incremental sales.

WITH cte1 AS
(
    SELECT 
        category,
        SUM(ISU) AS ISU,
        ROUND(SUM(ISU)/SUM(`quantity_sold(before_promo)`)*100, 2) AS `ISU_%`
    FROM 
        revenue 
    WHERE 
        campaign_name='Diwali'
    GROUP BY 
        category
)

SELECT 
    *,
    RANK() OVER(ORDER BY `ISU_%` DESC) AS ISU_rank
FROM 
    cte1
ORDER BY 
    `ISU_%` DESC;

```

## Task 5: Top Products by Incremental Revenue Percentage (IR%)

```sql

-- Create a report featuring the Top 5 products, ranked by Incremental Revenue Percentage (IR%), across all campaigns.
-- The report will provide essential information including product name, category, and ir%.
-- This analysis helps identify the most successful products in terms of incremental revenue across our campaigns, assisting in product optimization.

SELECT 
    product_name,
    category,
    ROUND(SUM(IR)/SUM(Revenue_Before_Promo)*100, 2) AS `IR%`,
    RANK() OVER(ORDER BY SUM(IR)/SUM(Revenue_Before_Promo)*100 DESC) AS ranking
FROM 
    Revenue 
GROUP BY 
    product_name,
    category
ORDER BY 
    `IR%` DESC
LIMIT 5;

```
