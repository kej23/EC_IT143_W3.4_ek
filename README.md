# EC_IT143_W3.4_ek
Adding deliverable W3.4.
/*****************************************************************************************************************
NAME:    EC_IT143_W3.4_ek
PURPOSE: Create Answers wk3.4...

MODIFICATION LOG:
Ver      Date        Author        Description
-----   ----------   -----------   -------------------------------------------------------------------------------
1.0     03/30/2025   ELVIS KIRUNDA       1. Built this script for EC IT143


RUNTIME: 
Xm Xs

NOTES: 
This is where I talk about what this script is, why I built it, and other stuff...
 
******************************************************************************************************************/

-- Q1: What is the total Sales Amount? (By Edwin Chepsiror)
-- ANSWER1: Question goes on the previous line, intoduction to the answer goes on this line...
SELECT SUM(TotalDue) AS TotalSalesAmount  --Calculates sum for total due and returns it as total sales amount.
FROM Sales.SalesOrderHeader;              --Specifies source table where the values will be picked from.

-- Q2: What are the total number of products? (By Edwin Chepsiror)
-- ANSWER2;Question goes on the previous line, intoduction to the answer goes on this line...
SELECT COUNT(*) AS TotalProducts            --Counts the total number of individual products listed in the Production.Product table and returns them as TotalProducts.
FROM Production.Product;

--Q3: What is the total revenue generated from sales in the Sales.SalesOrderHeader and Sales.SalesOrderDetail tables?  (By Christopher Okojie)
-- ANSWER3: Question goes on the previous line, intoduction to the answer goes on this line...
SELECT SUM(UnitPrice * OrderQty) AS TotalRevenue  --The sum() function aggregates these revenue values across all rows to compute the total revenue for the entire dataset and returns them as TotalRevenue.
FROM Sales.SalesOrderDetail;                      --Source of the data.

--Q4: Which customer has placed the highest number of orders, based on the Sales.Customer and Sales.SalesOrderHeader tables?  ? (By Christopher Okojie)
-- ANSWER4: Question goes on the previous line, intoduction to the answer goes on this line...
SELECT TOP 1 c.CustomerID, COUNT(soh.SalesOrderID) AS NumberOfOrders    --Retrieves the customerid and the total count of sales orders () for each customer then uses top 1 clause to return the one with the highest order numbers
FROM Sales.Customer c
JOIN Sales.SalesOrderHeader soh ON c.CustomerID = soh.CustomerID        --Performs an inner join between the sales.customer table and the sales.salesorderheader table by matching rows that have the same customerid in both tables.
GROUP BY c.CustomerID                                                --Groups the result set by each unique customerid.
ORDER BY NumberOfOrders DESC;                                           --Orders the grouped results by numberoforders in descending order.

--Q5: Which sales representatives had the highest sales performance in 2021 by revenue? ? (By Me)
-- ANSWER5:Question goes on the previous line, intoduction to the answer goes on this line...
SELECT p.FirstName + ' ' + p.LastName AS SalesRep, SUM(soh.TotalDue) AS TotalRevenue   --Combines the first and last names from the person.person table then assigns alias salesrep, calculates total revenue for each salesrep in 2021 and assigns alias total revenue for the summed value.
FROM Sales.SalesOrderHeader soh
JOIN Sales.SalesPerson sp ON soh.SalesPersonID = sp.BusinessEntityID               --Joins the Sales.SalesOrderHeader table to the Sales.SalesPerson table, matching rows where the SalesPersonID in SalesOrderHeader corresponds to the Businessentityid in salesperson.
JOIN HumanResources.Employee e ON e.BusinessEntityID = sp.BusinessEntityID         --Joins the Sales.SalesPerson table to the HumanResources.Employee table. This connects salesperson data with employee details via the shared Businessentityid.
JOIN Person.Person p ON e.BusinessEntityID = p.BusinessEntityID                    --Joins the HumanResources.Employee table to the person.person table to retrieve personal information (such as the first and last names) of the salesperson.
WHERE YEAR(OrderDate) = 2021                                                       --Filters the orders to include only those placed in the year 2021.
GROUP BY p.FirstName, p.LastName                                                   --Groups the results by the first and last name of the salesperson. This ensures the revenue is calculated individually for each salesperson.
ORDER BY TotalRevenue DESC;                                                        --Sorts the results in descending order of total revenue, so the salesperson with the highest revenue appears at the top.

--Q6: Analyse how many orders were placed for each product category during the holiday season (November–December) for the years 2020–2022. (By Me)
-- ANSWER6: Question goes on the previous line, intoduction to the answer goes on this line...
SELECT pc.Name AS ProductCategory, YEAR(soh.OrderDate) AS Year, COUNT(DISTINCT soh.SalesOrderID) AS Orders  --Retrieves the product category name from the product category(pc) table and aliases it as product category.
FROM Sales.SalesOrderHeader soh
JOIN Sales.SalesOrderDetail sod ON soh.SalesOrderID = sod.SalesOrderID   --Performs an inner join between the SalesOrderHeader table and the SalesOrderDetail table to link sales orders with their detailed line items.
JOIN Production.Product p ON p.ProductID = sod.ProductID                 --Links the SalesOrderDetail table with the Product table to associate each line item with its respective product.
JOIN Production.ProductSubcategory ps ON ps.ProductSubcategoryID = p.ProductSubcategoryID    --Joins the Product table with the ProductSubcategory table to categorize each product into its specific subcategory.
JOIN Production.ProductCategory pc ON pc.ProductCategoryID = ps.ProductCategoryID            --Links the ProductSubcategory table with the ProductCategory table to group subcategories into broader product categories.
WHERE MONTH(soh.OrderDate) IN (11, 12)                                    --Filters the data to include only orders placed in November or December.
  AND YEAR(soh.OrderDate) BETWEEN 2020 AND 2022                      
GROUP BY pc.Name, YEAR(soh.OrderDate)                                      --Groups the results by ProductCategory and Year to calculate the count of distinct orders () for each category in each year.
ORDER BY Year, ProductCategory;                                            --Sorts the final output by year (ascending) and product category (alphabetical order).

--Q7: Can you create a list of tables that contain either the CustomerID column or the ContactTypeID?  (By Robert Young)
-- ANSWER7: Question goes on the previous line, intoduction to the answer goes on this line...
SELECT TABLE_NAME, COLUMN_NAME                                 --This retrieves the table names and column names where the specified column names exist ensuring that the results are limited to the tables containing these columns.
FROM INFORMATION_SCHEMA.COLUMNS                                --This indicates that the query is being executed against the INFORMATION_SCHEMA.COLUMNS system view.These views provide metadata about the database schema, such as tables, columns, data types, and constraints.
WHERE COLUMN_NAME IN ('CustomerID', 'ContactTypeID');          --Filters the results to include only columns whose names match CustomerID or ContactTypeID.

--Q8:   Can you create a list of tables that contain either the ProductCategoryID column or the SalesOrderID?  (By Robert Young)
-- A1: Question goes on the previous line, intoduction to the answer goes on this line...
SELECT TABLE_NAME, COLUMN_NAME                            --This retrieves the table names and column names where the specified column names exist ensuring that the results are limited to the tables containing these columns.
FROM INFORMATION_SCHEMA.COLUMNS                           --This indicates that the query is being executed against the INFORMATION_SCHEMA.COLUMNS system view.These views provide metadata about the database schema, such as tables, columns, data types, and constraints.
WHERE COLUMN_NAME IN ('ProductCategoryID', 'SalesOrderID');   --Filters the results to include only columns whose names match ProductCategoryID or SalesOrderID.

--SELECT GETDATE() AS my_date;
