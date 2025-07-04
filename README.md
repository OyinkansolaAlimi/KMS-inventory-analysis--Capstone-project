# KMS-inventory-analysis--Capstone-project
A project required for a certification in Data Analysis after three months of learning with the Incubator Hub
### Project Topic: Kultra Mega Store (KMS) Sales and shipping Performance Analysis

## Project Overview: This is a comprehensive analysis of the inventory data set from the Abuja division of KMS leveraging SQL queries to extract valuable insights. The goal is to provide a data-driven overview of performance across various dimensions, including product categories, regions, customer segments, and shipping logistics, culminating in actionable recommendations for management and also provide a clear understanding of KMS's operational and customer landscape



``` SQL
create database KMS_db

select * from [KMS Sales Data]
select * from [dbo].[Order_Status]

alter table [KMS Sales Data]
alter column Product_Base_Margin decimal (10,3) not null

1. -------Which product category had the highest sales?
SELECT TOP 1 Product_Category, SUM(Sales) AS TotalSales
FROM [KMS Sales Data]
GROUP BY Product_Category 
ORDER BY TotalSales desc

2.-------What are the Top 3 and Bottom 3 regions in terms of sales? 
--------TOP 3 REGIONS
SELECT TOP 3 Region, SUM(Sales) AS TotalSales
FROM [KMS Sales Data]
GROUP BY Region
ORDER BY TotalSales DESC 
 -------BOTTOM 3 REGIONS
 SELECT TOP 3 Region, SUM(Sales) AS TotalSales
FROM [KMS Sales Data]
GROUP BY Region
ORDER BY TotalSales ASC

3.------What were the total sales of appliances in Ontario?
SELECT SUM(Sales) AS Total_Appliances_Sales_In_Ontario
     FROM [KMS Sales Data]
 WHERE Product_Sub_Category = 'Appliances'
 AND Province = 'Ontario'

  4---Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers 
  --FIND BOTTOM 10 CUSTOMERS
  Select top 10 Customer_Name,
  sum(sales) as TotalSales
  from [KMS Sales Data]
  group by Customer_Name
  order by Totalsales asc

  select * from [KMS Sales Data]

  5.----KMS incurred the most shipping cost using which shipping method? 
  SELECT TOP 1 "Ship_Mode", SUM("Shipping_Cost") AS TotalShippingCost
FROM [KMS Sales Data]
GROUP BY Ship_Mode
ORDER BY TotalShippingCost DESC

6.---Who are the most valuable customers, and what products or services do they typically purchase? 
-------CHECK FOR TOP 2 VALUABLE CUSTOMERS
Select top 2 Customer_Name,
  sum(sales) as TotalSales
   from [KMS Sales Data]
   GROUP BY Customer_Name
ORDER BY TotalSales DESC

   SELECT TOP 3 Product_Category, count (distinct Product_Name) as Products_Purchased
FROM [KMS Sales Data]
WHERE Customer_Name = 'Emily Phan'
GROUP BY Product_Category
ORDER BY Products_Purchased DESC

SELECT TOP 3 Product_Category, count (distinct Product_Name) as Products_Purchased
FROM [KMS Sales Data]
WHERE Customer_Name = 'Roy Skaria'
GROUP BY Product_Category
ORDER BY Products_Purchased DESC

7.---Which small business customer had the highest sales? 
      SELECT TOP 1 Customer_Name, SUM(Sales) AS TotalSales
      FROM [KMS Sales Data]
      WHERE Customer_Segment = 'Small Business'
     GROUP BY Customer_Name
    ORDER BY TotalSales DESC

8.--Which Corporate Customer placed the most number of orders in 2009 – 2012? 
    SELECT  top 1 Customer_Name, COUNT(DISTINCT Order_id) AS Orders_placed
FROM [KMS Sales Data]
WHERE Customer_Segment = 'Corporate'
and year (Order_date) 
 between 2009 and 2012
GROUP BY Customer_Name
ORDER BY Orders_placed DESC

9.----Which consumer customer was the most profitable one? 
SELECT TOP 1 Customer_Name, SUM(Profit) AS Total_Profit
FROM [KMS Sales Data]
WHERE Customer_Segment = 'Consumer'
GROUP BY Customer_Name
ORDER BY Total_Profit DESC
select * from [dbo].[KMS Sales Data]
select * from [dbo].[Order_Status]
10.----Which customer returned items, and what segment do they belong to? 
SELECT DISTINCT 
         s.Customer_Name, s.Customer_Segment, o.Status
FROM [KMS Sales Data] s
JOIN [dbo].[Order_Status] o
ON s.Order_ID = o.Order_ID
WHERE o.Status = 'Returned'

11---- did the company spend shipping cost appropriately based on order priority

select Order_Priority, Ship_mode,
count (Order_ID) as ordercount,
Round(sum(Sales - profit),2) AS Estimated_shipping_cost,
avg(datediff (day,[Order_Date],[Ship_date])) as AvgShipDays
from [KMS Sales Data]
group by Order_Priority, Ship_Mode
order by Order_Priority, Ship_Mode desc

select Order_Priority, Ship_mode,
count (*) as ordercount,
avg (Shipping_cost) as avgshippingcost
from [KMS Sales Data]
group by Order_Priority, Ship_Mode
order by Order_Priority, avgshippingcost desc

```
