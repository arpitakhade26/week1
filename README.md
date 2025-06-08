1.	List of all customers
            SELECT * FROM Customers;
2.	Customers where company name ends in 'N'
           SELECT * FROM Customers WHERE CompanyName LIKE '%N';
3.	Customers in Berlin or London
             SELECT * FROM Customers WHERE City IN ('Berlin', 'London');
4.	Customers in UK or USA
             SELECT * FROM Customers WHERE Country IN ('UK', 'USA');
5.	All products sorted by product name
              SELECT * FROM Products ORDER BY ProductName;
6.	Products starting with 'A'
              SELECT * FROM Products WHERE ProductName LIKE 'A%';
7.	Customers who placed an order
                SELECT DISTINCT Customers* FROM Customers
                JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
8.	Customers in London who bought Chai
                SELECT DISTINCT Customers*FROM Customers
                JOIN Orders ON Customers.CustomerID = Orders.CustomerID
                JOIN [Order Details] ON Orders.OrderID = [Order Details].OrderID
               JOIN Products ON [Order Details].ProductID = Products.ProductID
               WHERE Customers.City = 'London' AND Products.ProductName = 'Chai';
9.	Customers who never placed an order
               SELECT * FROM Customers
               WHERE CustomerID NOT IN (SELECT DISTINCT CustomerID FROM Orders);
10.	Customers who ordered Tofu
               SELECT DISTINCT Customers.*
               FROM Customers
              JOIN Orders ON Customers.CustomerID = Orders.CustomerID
             JOIN [Order Details] ON Orders.OrderID = [Order Details].OrderID
             JOIN Products ON [Order Details].ProductID = Products.ProductID
             WHERE Products.ProductName = 'Tofu';
11.	Details of the first order
               SELECT * FROM Orders
               ORDER BY OrderDate ASC
                 LIMIT 1;
12.	Details of most expensive order
                SELECT Orders.*, SUM(UnitPrice * Quantity * (1 - Discount)) AS Total
                FROM Orders
               JOIN [Order Details] ON Orders.OrderID = [Order Details].OrderID
               GROUP BY Orders.OrderID
               ORDER BY Total DESC
                LIMIT 1;
13.	OrderID and average quantity per order
                SELECT OrderID, AVG(Quantity) AS AvgQty
               FROM [Order Details]
               GROUP BY OrderID;
14.	OrderID, min and max quantity
             SELECT OrderID, MIN(Quantity) AS MinQty, MAX(Quantity) AS MaxQty
            FROM [Order Details]
            GROUP BY OrderID;
15.	Managers and number of employees reporting
               SELECT e2.ReportsTo AS ManagerID, COUNT(*) AS NumReports
                FROM Employees e1
               JOIN Employees e2 ON e1.ReportsTo = e2.EmployeeID
              GROUP BY e2.ReportsTo;
16.	Orders with total quantity > 300
                 SELECT OrderID, SUM(Quantity) AS TotalQty
                 FROM [Order Details]
                 GROUP BY OrderID
                 HAVING SUM(Quantity) > 300;


17.	Orders on or after 1996-12-31
           SELECT * FROM Orders
           WHERE OrderDate >= '1996-12-31';
18.	Orders shipped to Canada
             SELECT * FROM Orders
             WHERE ShipCountry = 'Canada';
19.	Orders with total > 200
               SELECT Orders.OrderID, SUM(UnitPrice * Quantity * (1 - Discount)) AS Total
               FROM Orders
               JOIN [Order Details] ON Orders.OrderID = [Order Details].OrderID
              GROUP BY Orders.OrderID
               HAVING Total > 200;
20.	Countries and sales per country
             SELECT ShipCountry, SUM(UnitPrice * Quantity * (1 - Discount)) AS TotalSales
             FROM Orders
            JOIN [Order Details] ON Orders.OrderID = [Order Details].OrderID
            GROUP BY ShipCountry;
21.	Customer name and number of orders
             SELECT ContactName, COUNT(Orders.OrderID) AS NumOrders
             FROM Customers
             LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID
             GROUP BY ContactName;
22.	Customers with more than 3 orders
             SELECT ContactName
             FROM Customers
            JOIN Orders ON Customers.CustomerID = Orders.CustomerID
           GROUP BY ContactName
           HAVING COUNT(Orders.OrderID) > 3;
23.	Discontinued products ordered between 1997-01-01 and 1998-01-01
                 SELECT DISTINCT Products.*
                   FROM Products
            JOIN [Order Details] ON Products.ProductID = [Order Details].ProductID
            JOIN Orders ON Orders.OrderID = [Order Details].OrderID
             WHERE Discontinued = 1 AND OrderDate BETWEEN '1997-01-01' AND '1998-01-01';
24.	Employee and supervisor names
            SELECT e1.FirstName AS EmployeeFirst, e1.LastName AS EmployeeLast,
              e2.FirstName AS SupervisorFirst, e2.LastName AS SupervisorLast
              FROM Employees e1
              LEFT JOIN Employees e2 ON e1.ReportsTo = e2.EmployeeID;
25.	Employee ID and total sale
              SELECT Employees.EmployeeID, SUM(UnitPrice * Quantity * (1 - Discount)) AS TotalSales
              FROM Employees
              JOIN Orders ON Employees.EmployeeID = Orders.EmployeeID
              JOIN [Order Details] ON Orders.OrderID = [Order Details].OrderID
             GROUP BY Employees.EmployeeID;
26.	Employees with 'a' in first name
              SELECT * FROM Employees
               WHERE FirstName LIKE '%a%';
27.	Managers with >4 reports
              SELECT ReportsTo, COUNT(*) AS NumReports
              FROM Employees
             WHERE ReportsTo IS NOT NULL
              GROUP BY ReportsTo
               HAVING COUNT(*) > 4;
28.	Orders and product names
              SELECT Orders.OrderID, Products.ProductName
               FROM Orders
               JOIN [Order Details] ON Orders.OrderID = [Order Details].OrderID
              JOIN Products ON [Order Details].ProductID = Products.ProductID;
29.	Orders by the best customer
                SELECT Orders.*
                 FROM Orders
           WHERE CustomerID = (
           SELECT TOP 1 CustomerID
           FROM Orders
           JOIN [Order Details] ON Orders.OrderID = [Order Details].OrderID
          GROUP BY CustomerID
          ORDER BY SUM(UnitPrice * Quantity * (1 - Discount)) DESC
            );
30.	Orders by customers without a fax
             SELECT Orders.*
             FROM Orders
            JOIN Customers ON Orders.CustomerID = Customers.CustomerID
            WHERE Customers.Fax IS NULL;
31.	Postal codes where Tofu was shipped
             SELECT DISTINCT ShipPostalCode
             FROM Orders
             JOIN [Order Details] ON Orders.OrderID = [Order Details].OrderID
             JOIN Products ON [Order Details].ProductID = Products.ProductID
             WHERE Products.ProductName = 'Tofu';
32.	Products shipped to France
            SELECT DISTINCT Products.ProductName
            FROM Orders
            JOIN [Order Details] ON Orders.OrderID = [Order Details].OrderID
           JOIN Products ON [Order Details].ProductID = Products.ProductID
           WHERE ShipCountry = 'France';
33.	Product names and categories for 'Specialty Biscuits, Ltd.'
             SELECT Products.ProductName, Categories.CategoryName
             FROM Products
            JOIN Suppliers ON Products.SupplierID = Suppliers.SupplierID
            JOIN Categories ON Products.CategoryID = Categories.CategoryID
            WHERE Suppliers.CompanyName = 'Specialty Biscuits, Ltd.';
34.	Products never ordered
         SELECT * FROM Products
          WHERE ProductID NOT IN (
         SELECT DISTINCT ProductID FROM [Order Details]
            );
35.	Products with low stock and no orders
            SELECT * FROM Products
           WHERE UnitsInStock < 10 AND UnitsOnOrder = 0;
36.	Top 10 countries by sales
              SELECT ShipCountry, SUM(UnitPrice * Quantity * (1 - Discount)) AS Sales
              FROM Orders
              JOIN [Order Details] ON Orders.OrderID = [Order Details].OrderID
              GROUP BY ShipCountry
             ORDER BY Sales DESC
             LIMIT 10;
37.	Orders per employee for customer IDs A–AO
            SELECT EmployeeID, COUNT(OrderID) AS NumOrders
             FROM Orders
            WHERE CustomerID BETWEEN 'A' AND 'AO'
            GROUP BY EmployeeID;
38.	Date of most expensive order
              SELECT OrderDate
               FROM Orders
               JOIN [Order Details] ON Orders.OrderID = [Order Details].OrderID
               GROUP BY Orders.OrderID, OrderDate
               ORDER BY SUM(UnitPrice * Quantity * (1 - Discount)) DESC
                LIMIT 1;
39.	Product revenue
              SELECT ProductName, SUM(UnitPrice * Quantity * (1 - Discount)) AS Revenue
              FROM Products
             JOIN [Order Details] ON Products.ProductID = [Order Details].ProductID
             GROUP BY ProductName;
40.	Supplier ID and number of products
              SELECT SupplierID, COUNT(*) AS ProductCount
              FROM Products
              GROUP BY SupplierID;
41.	Top 10 customers by business
               SELECT Customers.CustomerID, Customers.ContactName,
               SUM(UnitPrice * Quantity * (1 - Discount)) AS TotalSales
               FROM Customers
               JOIN Orders ON Customers.CustomerID = Orders.CustomerID
               JOIN [Order Details] ON Orders.OrderID = [Order Details].OrderID
               GROUP BY Customers.CustomerID, Customers.ContactName
               ORDER BY TotalSales DESC
               LIMIT 10;
42.	Total revenue of the company
                SELECT SUM(UnitPrice * Quantity * (1 - Discount)) AS TotalRevenue
                FROM [Order Details];

