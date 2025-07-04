# 🛒 KMS-inventory-analysis--Capstone-project

A project required for a certification in Data Analysis after three months of learning with the Incubator Hub

## 📌 Project Title
**Kultra Mega Store (KMS) Sales and shipping Performance Analysis**

## 📂 Project Overview
This is a comprehensive analysis of the inventory data set from the Abuja division of KMS leveraging **SQL Server** to extract valuable insights. The goal is to provide a data-driven overview of performance across various dimensions, including product categories, regions, customer segments, and shipping logistics, culminating in actionable recommendations for management and also provide a clear understanding of KMS's operational and customer landscape.

### Tool Used
 - SQL server
### Data Cleaning and Preparation

I loaded the data into the import wizard, reviewed the column mappings and ensure the detected data types were correct for the data and chose row ID as the primary key because it uniquely identified each individual item


``` SQL
create database KMS_db

select * from [KMS Sales Data]
select * from [dbo].[Order_Status]

alter table [KMS Sales Data]
alter column Product_Base_Margin decimal (10,3) not null

1. -------Which product category had the highest sales? Technology: 984248.409
SELECT TOP 1 Product_Category, SUM(Sales) AS TotalSales
FROM [KMS Sales Data]
GROUP BY Product_Category 
ORDER BY TotalSales desc

2.-------What are the Top 3 and Bottom 3 regions in terms of sales?
--------TOP 3 REGIONS =  WEST; 3547958.331, ONTARIO; 3019144.917 ,PRIARIE; 2773571.870
SELECT TOP 3 Region, SUM(Sales) AS TotalSales
FROM [KMS Sales Data]
GROUP BY Region
ORDER BY TotalSales DESC 
 -------BOTTOM 3 REGIONS = Nunavut; 106293.386 , Northwest Territories; 766752.169 , Yukon; 971448.733
 SELECT TOP 3 Region, SUM(Sales) AS TotalSales
FROM [KMS Sales Data]
GROUP BY Region
ORDER BY TotalSales ASC

3.------What were the total sales of appliances in Ontario? = 202346.840
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

  5.----KMS incurred the most shipping cost using which shipping method?= DELIVERY TRUCK; 51144.540
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

7.---Which small business customer had the highest sales? = Dennis kane 75967.591
      SELECT TOP 1 Customer_Name, SUM(Sales) AS TotalSales
      FROM [KMS Sales Data]
      WHERE Customer_Segment = 'Small Business'
     GROUP BY Customer_Name
    ORDER BY TotalSales DESC

8.--Which Corporate Customer placed the most number of orders in 2009 – 2012? = Adam Hart, 19 orders
    SELECT  top 1 Customer_Name, COUNT(DISTINCT Order_Quantity) AS Orders_placed
FROM [KMS Sales Data]
WHERE Customer_Segment = 'Corporate'
and year (Order_date) 
 between 2009 and 2012
GROUP BY Customer_Name
ORDER BY Orders_placed DESC

9.----Which consumer customer was the most profitable one? = Emily phan 34005.440
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

## 🔍 Findings and Interpretation
 - Product Performance; Identifying Revenue Drivers
   
Technology is the dominant product category by sales, followed by office supplies. This analysis highlights Technology as the primary revenue generator for KMS, contributing the largest share of total sales. Office Supplies also play a significant role in the company's revenue stream. 
 - Regional Sales Performance; Strategic Focus Areas

The **West**, **Ontario** and **Prairie** regions lead in sales, while **Nunavut**, **Northwest Territories** and **Yukon** are the lowest performing regions. Geographic sales performance insights are crucial for resource allocation and the strong performance in the top three regions suggests these are areas with high demand, warranting continued investment. Conversely, the significantly lower sales in the bottom three regions identifies them as potential areas for strategic intervention like targeted marketing campaigns or even evaluating the efficiency of existing sales and distribution channels.
 - Customer Segmentation and Value

Identifying individual high-value customers (like **Emily Phan and Roy Skaria**) and understanding their purchasing habits and preferences (e.g. **Emily Phan's** strong inclination towards Office Supplies and Technology) is critical for personalized retention strategies. 
 - Shipping and Logistics Efficiency

Delivery Truck is the most expensive shipping method on average per line item, contrary to the typical assumption of it being "economical." Regular Air and Express Air have similar, lower average costs, but Express Air is considerably faster. The allocation of shipping methods based on order priority shows potential misalignment.
For High and Critical priority orders, a significant volume still uses Regular Air despite Express Air being the fastest. This might imply a trade-off where KMS prioritizes cost savings over maximum speed for some urgent orders, or that Express Air is not consistently chosen when speed is paramount. Management should investigate the underlying reasons for the high average cost of Delivery Truck shipments and re-evaluate the allocation of shipping methods for high-priority orders to ensure optimal balance between delivery speed, customer satisfaction, and cost efficiency

## ✅ Recommendations for KMS Management
Based on this analysis, KMS management should consider the following actions:

 1. **Optimize Product Portfolio** :Deep dive into the profitability of Technology and Office Supplies and For lower-performing categories, evaluate if they should be de-emphasized, re-marketed, or if their pricing/cost structure needs adjustment.
 2. **Targeted Regional Growth**: Develop specific strategies for the **Nunavut, Yukon and Northwest Territories regions**.This could involve market research, localized marketing campaigns, or re-evaluating distribution channels to improve accessibility and demand.
 3. **Enhance Customer Loyalty**: Implement personalized retention strategies for the top-value customers. This could include exclusive offers, loyalty programs, or dedicated account management based on their identified product preferences.
 4. **Re-evaluate Shipping Strategy**: Revisit the allocation rules for Express Air and Regular Air for high-priority orders to ensure alignment between service level and cost, based on true business needs and customer expectations.
 5. **Address Return Root Causes**: Conduct a deeper analysis into returned items to identify common products, reasons for return, and associated costs. This could uncover issues related to product quality, descriptions, or packaging that need to be addressed to reduce return rates and improve customer satisfaction.

## 💡 Key Takeaways
 - Technology Drives Revenue: Technology is the clear leader in sales, making it a critical focus area for continued investment and strategic growth.
 - Regional Disparities: While **'West',Ontario and Priarie** are strong performers, **Nunavut, Northwest Territories and Yukon** represent significant opportunities for targeted market development and resource allocation.
 - High-Value Customer Focus: Identifying and nurturing high-value customers through personalized strategies is essential for maximizing customer lifetime value and fostering loyalty.
 - Shipping Cost Optimization Needed: The high average cost of Delivery Truck shipments requires immediate investigation. Additionally, the current allocation of shipping methods for high-priority orders may not align with desired speed and efficiency.

