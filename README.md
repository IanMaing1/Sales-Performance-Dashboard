# Sales Analysis

### Project Overview

This data analysis project aims to provide comprehensive insights into the sales performance of a retailer specializing in bicycles, accessories, and apparel over the past four years. By analyzing various aspects of the sales data, we aim to understand the company's performance. 

### Data Sources

The following files were utilized for the data analysis project:

1. Dim_Customer.csv: Extracted client information, including city and name.
2. Dim_Date.csv: Combined 'DateKey' with 'OrderDateKey' from FACT_Internet_Sales.csv, and utilized Month number and Month name columns.
3. Dim_Sales_Territory.csv: Extracted sales information per country.
4. FACT_Internet_Sales.csv: Main file used to extract sales amounts for each year.
5. Dim_Product.csv: Extracted product names and product categories.
   
### Tools

1. SQL Server: Data cleaning and analysis. [Download here](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
2. Power BI: Creating reports and analysis. [Download here](https://www.microsoft.com/en-us/download/details.aspx?id=58494)

### Data Cleaning/preparation

1. Data Loading and Inspection: Importing the data files and inspecting them for any inconsistencies.
2. Handling Missing Values: Addressing and managing any missing data to ensure data integrity
3. Key Column Analysis and Joining: Identifying key columns in each file/table and joining the necessary information using SQL.
4. Data Formatting: Renaming column headers for easier identification and consistency.
