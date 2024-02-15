# Store Performance Analysis

## 1) Top 10 stores in terms of Incremental Revenue (IR) from Promotions

```sql
Select 
    store_id,
    city,
    concat(round(SUM(IR)/1000000,2),'M') as Incremental_Revenue
From 
    Revenue
GROUP BY 
    store_id,
    city
ORDER BY 
    Incremental_Revenue desc
LIMIT 10;
```
## 2) Bottom 10 stores by Incremental Sold Units (ISU) during promotional offers
```sql

Select 
    store_id,
    city,
    sum(ISU) as Incremental_Sold_Unit
From 
    Revenue
GROUP BY 
    store_id,
    city
ORDER BY 
    Incremental_Sold_Unit asc
LIMIT 10;
```

# Promotion Analysis

## 1) Top 2 promotional types based on highest Incremental Revenue (IR)
``` sql

Select 
    promo_type,
    concat(round(SUM(IR)/1000000,2),'M') as Incremental_Revenue
From 
    Revenue
GROUP BY 
    promo_type
ORDER BY 
    Incremental_Revenue desc
LIMIT 2;

```
## 2) Bottom 2 Promotional types based on Incremental Sold Units (ISU)
``` sql

Select 
    promo_type,
    SUM(ISU) as Incremental_Sold_Unit
From 
    Revenue
GROUP BY 
    promo_type 
ORDER BY 
    Incremental_Sold_Unit asc
LIMIT 2;

```
# Promotion Balance Analysis
## Which promotion strikes the balance between Incremental Revenue (IR) and Incremental Sold Units (ISU)
```sql

Select 
    promo_type,
    sum(ISU) as ISU,
    round(sum(ISU)/sum(`quantity_sold(before_promo)`)*100,2) as `ISU_%`,
    CONCAT(ROUND(sum(IR)/1000000,2),'M') AS IR,
    round(SUM(IR)/SUM(Revenue_Before_Promo)*100,2) as `IR%`
From 
    revenue 
GROUP BY 
    promo_type
ORDER BY 
    IR desc,
    ISU desc;

```

# Product and Category Analysis
## Products with significant lift in sales from promotions
```sql

select 
    category,
    product_name,
    sum(`quantity_sold(before_promo)`) as Sales_Before_Promotion,
    sum(Sales_After_Promotion) as Sales_After_Promotion,
    sum(ISU) as ISU
From 
    revenue
GROUP BY 
    category,
    product_name
ORDER BY 
    ISU desc;

```

