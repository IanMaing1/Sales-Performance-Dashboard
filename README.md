# Sales Analysis

## Table of Contents


### Project Overview

This data analysis project aims to provide comprehensive insights into the sales performance of a retailer specializing in bicycles, accessories, and apparel over the past four years. By analyzing various aspects of the sales data, we aim to understand the company's performance. 

### Data Sources

The following files were utilized for the data analysis project:

1. Dim_Customer.csv: Extracted client information, including city and name.
2. Dim_Date.csv: Combined 'DateKey' with 'OrderDateKey' from FACT_Internet_Sales.csv, and utilized Month number and Month name columns.
3. Dim_Sales_Territory.csv: Extracted sales information per country.
4. FACT_Internet_Sales.csv: Main file used to extract sales amounts for each year.
5. Dim_Product.csv: Extracted product names and product categories.
6. FACT_Budget.csv: contains the budget for various month each year
   
### Tools

1. SQL Server: Data cleaning and analysis. [Download here](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
2. Power BI: Creating reports and analysis. [Download here](https://www.microsoft.com/en-us/download/details.aspx?id=58494)

### Data Cleaning/preparation

1. Data Loading and Inspection: Importing the data files and inspecting them for any inconsistencies.
2. Handling Missing Values: Addressing and managing any missing data to ensure data integrity
3. Key Column Analysis and Joining: Identifying key columns in each file/table and joining the necessary information using SQL.
4. Data Formatting: Renaming column headers for easier identification and consistency.

### Exploratory Data Analysis (EDA)

EDA assists in laying the foundation for more detailed analysis by thoroughly understanding the specific details and patterns within the sales data.

1. Which products contribute the most to total sales?
2. How is sales distributed across different clients to understand customer buying behavior?
3. How does actual sales performance compare against budgeted targets across the four-year period?
4. Does sales performance vary significantly across different product categories or client segments?
5. What are the monthly or quarterly sales trends over the four-year period?

### Data Analysis

Dim Customer file
```sql
-- Cleaned DIM_Customers Table --
SELECT 
    c.[CustomerKey],
    c.[GeographyKey],
    c.[CustomerAlternateKey],
    CONCAT(c.[FirstName], ' ', c.[LastName]) AS FullName,
--    c.[NameStyle],
--    c.[MaritalStatus],
    CASE c.gender WHEN 'M' THEN 'Male' WHEN 'F' THEN 'Female' END AS Gender,
--    c.[YearlyIncome],
    g.[City] AS [City],
--    g.[StateProvinceCode],
--    g.[StateProvinceName],
 --   g.[CountryRegionCode],
 --   g.[EnglishCountryRegionName],
--    g.[SpanishCountryRegionName],
--    g.[FrenchCountryRegionName],
--    g.[PostalCode],
    g.[SalesTerritoryKey]
--    g.[IpAddressLocator]
FROM 
    [AdventureWorksDW2019].[dbo].[DimCustomer] c
INNER JOIN 
    [AdventureWorksDW2019].[dbo].[DimGeography] g ON c.[GeographyKey] = g.[GeographyKey];
```


Dim Date file
```sql
-- Cleaned DIM_Date Table --
SELECT [DateKey]
      ,[FullDateAlternateKey] AS Date
      --[DayNumberOfWeek]
      ,[EnglishDayNameOfWeek] AS Day
      --[SpanishDayNameOfWeek]
      --[FrenchDayNameOfWeek]
      --[DayNumberOfMonth]
      --[DayNumberOfYear]
      --[WeekNumberOfYear]
      ,CASE 
        WHEN [EnglishMonthName] = 'January' THEN 'Jan'
        WHEN [EnglishMonthName] = 'February' THEN 'Feb'
        WHEN [EnglishMonthName] = 'March' THEN 'Mar'
        WHEN [EnglishMonthName] = 'April' THEN 'Apr'
        WHEN [EnglishMonthName] = 'May' THEN 'May'
        WHEN [EnglishMonthName] = 'June' THEN 'Jun'
        WHEN [EnglishMonthName] = 'July' THEN 'Jul'
        WHEN [EnglishMonthName] = 'August' THEN 'Aug'
        WHEN [EnglishMonthName] = 'September' THEN 'Sep'
        WHEN [EnglishMonthName] = 'October' THEN 'Oct'
        WHEN [EnglishMonthName] = 'November' THEN 'Nov'
        WHEN [EnglishMonthName] = 'December' THEN 'Dec'
        ELSE [EnglishMonthName]  -- shows original text in case of unexpected values
    END AS Month
      --[SpanishMonthName]
      --[FrenchMonthName]
      ,[MonthNumberOfYear] AS MonthNo
      ,[CalendarQuarter] AS Quarter
      ,[CalendarYear] AS Year
      --[CalendarSemester]
      --[FiscalQuarter]
      --[FiscalYear]
      --[FiscalSemester]
  FROM [AdventureWorksDW2019].[dbo].[DimDate]
  WHERE CalendarYear >= 2020
```


Dim Sales territory
```sql
-- Cleaned Dim Sales Territory table --
SELECT [SalesTerritoryKey]
      ,[SalesTerritoryAlternateKey]
      ,[SalesTerritoryRegion]
      ,[SalesTerritoryCountry]
      ,[SalesTerritoryGroup]
--      ,[SalesTerritoryImage]
  FROM [AdventureWorksDW2019].[dbo].[DimSalesTerritory]
```


FACT_Internet_Sales
```sql
-- Cleaned FACT_InternetSales Table --
SELECT 
    s.[ProductKey],
    s.[OrderDateKey],
    s.[DueDateKey],
    s.[CustomerKey],
	--,[PromotionKey]
    s.[CurrencyKey],
    s.[SalesTerritoryKey],
    s.[SalesOrderNumber],
	 --,[SalesOrderLineNumber]
      --,[RevisionNumber]
      --,[OrderQuantity]
      --,[UnitPrice]
      --,[ExtendedAmount]
      --,[UnitPriceDiscountPct]
     -- ,[DiscountAmount]
    s.[ProductStandardCost],
    s.[TotalProductCost],
    s.[SalesAmount],
	--,[TaxAmt]
      --,[Freight]
     -- ,[CarrierTrackingNumber]
      --,[CustomerPONumber]
     -- ,[OrderDate]
      --,[DueDate]
     -- ,[ShipDate]
    LEFT(CAST(s.[OrderDateKey] AS VARCHAR(8)), 4) AS Year,
    FORMAT(DATEFROMPARTS(
            CAST(LEFT(CAST(s.[OrderDateKey] AS VARCHAR(8)), 4) AS INT), 
            CAST(SUBSTRING(CAST(s.[OrderDateKey] AS VARCHAR(8)), 5, 2) AS INT), 
            1), 'MMM') AS MonthName,
    e.[Title] AS EmployeeTitle
FROM 
    [AdventureWorksDW2019].[dbo].[FactInternetSales] s
JOIN 
    [AdventureWorksDW2019].[dbo].[DimEmployee] e ON s.[SalesTerritoryKey] = e.[SalesTerritoryKey];
```


Dim Products
```sql
-- Cleaned DIM_Products Table --
SELECT 
    p.[ProductKey],
    p.[ProductAlternateKey],
    p.[ProductSubcategoryKey],
    p.[EnglishProductName] AS [Product Name],
    p.[StandardCost],
--    p.[Color],
--    p.[ListPrice],
--    p.[Size],
--    p.[Weight],
--    p.[ProductLine],
--    p.[Class],
 --   p.[Style],
 --   p.[ModelName],
    pc.[EnglishProductCategoryName] AS [Product Category] 	-- Joined from Category Table
FROM 
    [AdventureWorksDW2019].[dbo].[DimProduct] p
INNER JOIN 
    [AdventureWorksDW2019].[dbo].[DimProductSubcategory] ps ON p.[ProductSubcategoryKey] = ps.[ProductSubcategoryKey]
INNER JOIN 
    [AdventureWorksDW2019].[dbo].[DimProductCategory] pc ON ps.[ProductCategoryKey] = pc.[ProductCategoryKey];
```


### Results/Findings

Summary of Sales Analysis:

2021 Performance:

1. Sales were below budget by $1,767,560.34.
2. Significant differences were observed in November and December.

2022 Performance:

1. Sales fell short of the budget by $4,601,550.99.
2. Shortfall persisted from March through December.

2023 Performance:

1. Sales exceeded the budget by $12,176,572.
2. Strong performance noted across most regions, particularly in the United States with sales increasing by $9.51M.


Product Sales Breakdown:

1. Bikes accounted for 96.05% of total sales, making it the highest-selling category.
2. Clothing accounted for 1.32% of total sales, the lowest-selling category.


Additional Insights:

1. Overall sales amounted to $44,354,406, surpassing the budgeted amount of $39,310,000.
2. Significant sales growth was observed in 2023.
3. Top-selling products included Mountain-200, Road-150, and Road-250.

Note: December 2020 and January 2024 are partial months and not included in the complete year analysis.
