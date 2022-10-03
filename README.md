# SQL

Here are examples of SQL-queries that I can compose.

The document is built on the principle of task - answer.

Source of the database - **[w3schools.com](https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all)**

**1. Select all suppliers that are located in a city with a name starting with "S" but not in a country starting with "S".**
```sh
SELECT * 
FROM Suppliers
WHERE (City LIKE "S%") AND (Country NOT LIKE "S%");
```

**2. Output alphabetically order all cities from the database that are not in Germany, not in France, and not in England.**
```sh
SELECT DISTINCT City 
FROM Customers
WHERE Country NOT IN ("Germany", "France", "UK")
ORDER BY City ASC;
```

**3. Output information about employees in alphabetical order by Last Name and First Name who have a Bachelor of Arts degree.**
```sh
SELECT LastName, FirstName
FROM Employees
WHERE Notes LIKE "%BA%"
ORDER BY LastName ASC, FirstName ASC;
```

**4. Insert a new customer record in two ways:**

**a. Specify a unique identifier in the query**
```sh
INSERT INTO Customers
VALUES (92, "Very secrete company", "John Dou", "My adress is not your business", "Prague", "100 00", "Czech Republic");
```
**b. Do not specify a unique identifier in the query, the identifier should be generated automatically.**
```sh
INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
VALUES ("Very secrete company", "John Dou", "My adress is not your business", "Prague", "100 00", "Czech Republic");
```

**5. Update any record in the database.**
```sh
UPDATE Customers
SET CustomerName = "Some company"
WHERE CustomerID = 93;
```

**6. Delete any record in the database.**
```sh
DELETE FROM Customers
WHERE ContactName = "John Dou";
```

**7. Select the names of all people who buy seafood.**
```sh
SELECT DISTINCT ContactName
FROM Customers
LEFT JOIN Orders ON (Customers.CustomerID = Orders.CustomerID)
LEFT JOIN OrderDetails ON (Orders.OrderID = OrderDetails.OrderID)
LEFT JOIN Products ON (Products.ProductID = OrderDetails.ProductID)
LEFT JOIN Categories ON (Products.CategoryID = Categories.CategoryID)
WHERE CategoryName = "Seafood"
ORDER BY ContactName ASC;
```

**8. Output the name of the customer and the number of purchases made by him.**
```sh
SELECT DISTINCT CustomerName, COUNT(OrderID) as "Number of orders"
FROM Customers
LEFT JOIN Orders ON (Customers.CustomerID = Orders.CustomerID)
GROUP BY CustomerName
ORDER BY CustomerName ASC;
```

**9. Output the names and IDs of the five most purchased products. Also how many were sold.**
```sh
SELECT Products.ProductID, ProductName, COUNT(Quantity) as Total
FROM Orders
LEFT JOIN OrderDetails ON (Orders.OrderID = OrderDetails.OrderID)
LEFT JOIN Products ON (Products.ProductID = OrderDetails.ProductID)
GROUP BY Products.ProductID, ProductName
ORDER BY Total DESC
LIMIT 5;
```

**10. Show how many orders were placed by each shipper. Display the names of the shippers and the number of orders they have completed.**
```sh
SELECT ShipperName, COUNT(OrderID) as "Number of completed orders"
FROM Orders
LEFT JOIN Shippers ON (Shippers.ShipperID = Orders.ShipperID)
GROUP BY ShipperName;
```

**11. Get the names of customers and the average check of orders, which amount is from 1000 dollars (inclusive). The average order amount must be rounded up to two hundredths.**
```sh
SELECT CustomerName, ROUND(SUM(OrderPrice)/COUNT(OrderID), 2)  as "Average check of the order"
FROM
(SELECT CustomerName, Orders.OrderID, SUM(Price*Quantity) as OrderPrice
FROM Customers
INNER JOIN Orders ON (Customers.CustomerID = Orders.CustomerID)
LEFT JOIN OrderDetails ON (Orders.OrderID = OrderDetails.OrderID)
LEFT JOIN Products ON (Products.ProductID = OrderDetails.ProductID)
GROUP BY CustomerName, Orders.OrderID)
GROUP BY CustomerName
HAVING "Average check of the order" >= 1000;
```

