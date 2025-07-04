# DSA-DATA-ANALYSIS-PROJECT-KMS
This project forms part of my Data Analytics Bootcamp capstone and focuses on uncovering actionable insights from Kultra Mega Stores (KMS) sales data. Using SQL Server, I explored various case scenarios to provide strategic decision support. 

## 📚 Table of Contents

- [Project Title](#dsa-data-analysis-project)
- [Step-by-Step Documentation](#step-by-step-documentation-employed-during-the-project)
  - [Project Overview](#project-overview)
    - [Objectives](#objectives)
    - [Case Scenarios Covered](#case-scenarios-covered)
  - [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
  - [Step 1: Creation of Database and Data Importation](#step-1-step-1-creation-of-database-and-data-importation)
- [Case Scenario Queries](#case-scenario-queries)
  - [Top-Performing Product Category](#1-which-product-category-had-the-highest-sales)
  - [Top and Bottom 3 Regions by Sales](#2-what-are-the-top-3-and-bottom-3-regions-in-terms-of-sales)
  - [Appliance Sales in Ontario](#3-what-were-the-total-sales-of-appliances-in-ontario)
  - [Low-Spending Customers Analysis](#4-advise-the-management-of-kms-on-what-to-do-to-increase-the-revenue-from-the-bottom-10-customers)
  - [Most Expensive Shipping Method](#5-kms-incurred-the-most-shipping-cost-using-which-shipping-method)
  - [High-Value Customers and Their Orders](#6a-who-are-the-most-valuable-customers)
  - [Small Business Customer with Highest Sales](#7-which-small-business-customer-had-the-highest-sales)
  - [Corporate Customer Order Frequency](#8-which-corporate-customer-placed-the-most-number-of-orders-in-2009--2012)
  - [Most Profitable Consumer Customer](#9-which-consumer-customer-was-the-most-profitable-one)
  - [Return Behavior Analysis](#10-which-customer-returned-items-and-what-segment-do-they-belong-to)
  - [Shipping Cost vs Order Priority](#11-if-the-delivery-truck-is-the-most-economical-but-the-slowest-shipping-method)
- [Key Insights](#key-insights)
- [Trends & Patterns](#trends--patterns)
- [Strategic Recommendations](#strategic-recommendations)
- [Skills Demonstrated](#skills-demonstrated)
- [Final Thoughts](#final-thoughts)



## STEP BY STEP DOCUMENTATION EMPLOYED DURING THE PROJECT

### PROJECT OVERVIEW

Objectives:

- Clean and analyze sales and customer data.
 
- Answer business questions around profitability, customer retention, and logistics.
 
- Generate strategic recommendations based on real performance trends.

- Support customer-centric and cost-efficient decision-making.


Case Scenarios Covered:

1. Identify top-performing product categories.
2. Analyze regional performance and recommend targeted strategies.
3. Evaluate product sales in specific regions (e.g., appliances in Ontario).
4. Recommend strategies to engage low-spending customers.
5. Analyze efficiency of shipping methods based on order priority.
6. Profile high-spending or loyal customers.
7. Recommend retention strategies for top clients.
8. Leverage frequent buyers for loyalty programs or subscriptions.
9. Model a profitable customer persona based on behavior.
10. Understand return behavior and its impact on cost.
11. Recommend logistics restructuring for optimal delivery performance.

### EXPLORATORY DATA ANALYSIS (EDA)

EDA (Exploratory Data Analysis) is the process of exploring and understanding a dataset before doing any modeling. It helps you:

- Understand the data's structure and patterns

- Find missing values or outliers

- Visualize trends using charts and graphs

- Prepare data for further analysis

- It often involves statistics, data cleaning, and visualizations like histograms, box plots, and scatter plots.

### Step 1: Creation of Database and Data Importation
Approach:

create database DSA_PROJECT_DB

Used SQL Server Management Studio (SSMS) Import Wizard to load the KMS Sql Case Study.csv file into a table named DBO.[KMS Sql Case Study].

Actions:

Right-clicked on the target database → Tasks → Import Flat File.

Set data types, adjusted column names, and validated data rows.

create Database DSAproject_db

select top 10 * from dbo.[KMS Sql Case Study]

-- Checking for Null----
SELECT 
  COUNT(*) AS TotalRows,
  COUNT(Order_ID) AS Order_ID_NotNull,
  COUNT(Customer_Name) AS Customer_Name_NotNull
FROM dbo.[KMS Sql Case Study];

-- Data types (optional: use this if you are not sure)-----
SELECT 
  COLUMN_NAME, 
  DATA_TYPE 
FROM INFORMATION_SCHEMA.COLUMNS 
WHERE TABLE_NAME = 'DBO.[KMS Sql Case Study]';

-------Case Scenario I---
-----1. Which product category had the highest sales?----

SELECT 
    [Product_Category],
    SUM(Sales) AS Total_Sales
FROM 
    DBO.[KMS Sql Case Study]
GROUP BY 
    [Product_Category]
ORDER BY 
    Total_Sales DESC;


	---2. What are the Top 3 and Bottom 3 regions in terms of sales?

	 
SELECT 
    Region,
    SUM(Sales) AS Total_Sales
FROM 
    dbo.[KMS Sql Case Study]
GROUP BY 
    Region
ORDER BY 
    Total_Sales DESC;


	SELECT TOP 3
    Region,
    SUM(Sales) AS Total_Sales
FROM 
    dbo.[KMS Sql Case Study]
GROUP BY 
    Region
ORDER BY 
    Total_Sales DESC;


SELECT TOP 3
    Region,
    SUM(Sales) AS Total_Sales
FROM 
    dbo.[KMS Sql Case Study]
GROUP BY 
    Region
ORDER BY 
    Total_Sales ASC;


---Bottom 3---(1) Nunavut with 116,376.47 (2)Northwest Territories with 800,847.35 (3) Yukon with 975,867.39


----3. What were the total sales of appliances in Ontario?----

SELECT 
    Province,
    [Product_Sub_Category],
    SUM(Sales) AS Total_Appliance_Sales
FROM 
    dbo.[KMS Sql Case Study]
WHERE 
    Province = 'Ontario'
    AND [Product_Sub_Category] = 'Appliances'
GROUP BY 
    Province, [Product_Sub_Category];


	-----4. Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers

	SELECT TOP 10
    [Customer_Name],
    SUM(Sales) AS Total_Sales
FROM 
    dbo.[KMS Sql Case Study]
GROUP BY 
    [Customer_Name]
ORDER BY 
    Total_Sales ASC;

	----products bought by the bottom 10 customers

	SELECT 
    [Customer_Name],
    [Product_Category],
    COUNT(*) AS Num_Orders,
    SUM(Sales) AS Total_Spent
FROM 
    dbo.[KMS Sql Case Study]
WHERE 
    [Customer_Name] IN (
        SELECT TOP 10 [Customer_Name]
        FROM dbo.[KMS Sql Case Study]
        GROUP BY [Customer_Name]
        ORDER BY SUM(Sales) ASC
		)
GROUP BY 
    [Customer_Name], [Product_Category]
ORDER BY 
    [Customer_Name], Total_Spent DESC;


	----5. KMS incurred the most shipping cost using which shipping method?

	SELECT 
    [Ship_Mode],
    SUM([Shipping_Cost]) AS Total_Shipping_Cost
FROM 
    dbo.[KMS Sql Case Study]
GROUP BY 
    [Ship_Mode]
ORDER BY 
    Total_Shipping_Cost DESC;

	-----Case Scenario II
	----6a. Who are the most valuable customers?

	SELECT 
    [Customer_Name],
    [Customer_Segment],
    SUM(Sales) AS Total_Sales
FROM 
    dbo.[KMS Sql Case Study]
GROUP BY 
    [Customer_Name], [Customer_Segment]
ORDER BY 
    Total_Sales DESC;

	---6b. And what products or services do they typically purchase?
	SELECT 
    [Customer_Name],
    [Product_Category],
    COUNT(*) AS Num_Orders,
    SUM(Sales) AS Category_Sales
FROM 
    dbo.[KMS Sql Case Study]
WHERE 
    [Customer_Name] IN (
        SELECT TOP 10 [Customer_Name]
        FROM dbo.[KMS Sql Case Study]
        GROUP BY [Customer_Name]
        ORDER BY SUM(Sales) DESC
    )
GROUP BY 
    [Customer_Name], [Product_Category]
ORDER BY 
    [Customer_Name], Category_Sales DESC;


	----7. Which small business customer had the highest sales?

	SELECT 
    [Customer_Name],
    SUM(Sales) AS Total_Sales
FROM 
    dbo.[KMS Sql Case Study]
WHERE 
    [Customer_Segment] = 'Small Business'
GROUP BY 
    [Customer_Name]
ORDER BY 
    Total_Sales DESC;

	----8. Which Corporate Customer placed the most number of orders in 2009 – 2012?

	----Filter by Segment and Date Range
	SELECT 
    [Customer_Name],
    COUNT(DISTINCT [Order_ID]) AS Number_Of_Orders
FROM 
    dbo.[KMS Sql Case Study]
WHERE 
    [Customer_Segment] = 'Corporate'
    AND YEAR([Order_Date]) BETWEEN 2009 AND 2012
GROUP BY 
    [Customer_Name]
ORDER BY 
    Number_Of_Orders DESC;

	-----9. Which consumer customer was the most profitable one?

	SELECT 
    [Customer_Name],
    SUM(Profit) AS Total_Profit
FROM 
    dbo.[KMS Sql Case Study]
WHERE 
    [Customer_Segment] = 'Consumer'
GROUP BY 
    [Customer_Name]
ORDER BY 
    Total_Profit DESC;

	----10. Which customer returned items, and what segment do they belong to?

	SELECT 
    [Customer_Name],
    [Customer_Segment],
    COUNT(*) AS Suspected_Returns
FROM 
    dbo.[KMS Sql Case Study]
WHERE 
    Profit < 0
GROUP BY 
    [Customer_Name], [Customer_Segment]
ORDER BY 
    Suspected_Returns DESC;

	---11. If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company appropriately spent shipping costs based on the Order Priority? Explain your answer.

	SELECT 
    [Order_Priority],
    [Ship_Mode],
    COUNT(*) AS NumOrders,
    SUM([Shipping_Cost]) AS Total_Shipping_Cost
FROM 
    dbo.[KMS Sql Case Study]
GROUP BY 
    [Order_Priority], [Ship_Mode]
ORDER BY 
    [Order_Priority], Total_Shipping_Cost DESC;


### KEY INSIGHTS:
- Technology emerged as the highest revenue-generating product category.
- Top 3 Regions outperform others and should be prioritized for new launches.
- Low-spending customers can be activated through bundling, loyalty offers, and personalized recommendations.
- Express Air is being misused for low-priority orders, leading to cost inefficiencies.
- High-frequency buyers are ideal candidates for subscription-based services.
- Return behaviors suggest improvement is needed in product clarity and customer communication.

### TRENDS & PATTERNS
- Appliance sales vary by region, indicating the need for location-specific marketing.
- Shipping method mismatch (e.g., high-cost methods used for non-urgent orders) highlights optimization opportunities.
- Customer segmentation shows that behavior-based strategies can boost retention and order value.

### STRATEGIC RECOMMENDATIONS
- Marketing: Promote technology bundles and upsell to top buyers.
- Operations: Create shipping rules tied to order urgency to reduce logistics costs.
- Customer Retention: Launch loyalty programs for top-tier customers and reactivation strategies for dormant ones.
- Logistics: Shift low-priority deliveries to cost-efficient shipping methods.

- Product Strategy: Use purchase and return trends to optimize product mix and descriptions.


### SKILLS DEMONSTRATED

- Exploratory Data Analysis
  
- Data Cleaning
  
- Pivot Table Mastery
  
- SQL Query Thinking

- Insight Derivation

- Dashboard Creation

- Business Storytelling

### FINAL THOUGHTS
  
This capstone project reflects not just technical skill, but real-world problem-solving and business thinking. 
I’m proud of how far I’ve come in this journey and excited about what lies ahead in the data analytics field.

 Let’s connect! I’m open to feedback, collaboration, or opportunities in data analysis.

###### EMAIL: abkcoaching@gmail.com
###### PHONE: +2349064562159
###### LinkedIn: https://www.linkedin.com/in/maryoguntoyinbo


















