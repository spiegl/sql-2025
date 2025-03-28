```sql
SELECT * 
FROM (
	SELECT Products.ProductName, Products.Price, (Products.Price * 1.08) as DollarPrice
	FROM Products
	LEFT JOIN Categories ON Products.CategoryID = Categories.CategoryID
	Order BY DollarPrice DESC
) as subquery
WHERE DollarPrice > 20
Order BY DollarPrice DESC;
```

```sql
SELECT ROUND(AVG(Products.Price), 2) as MyPrice
FROM Products;
```


```sql
SELECT ROUND(MyPrice,2) 
FROM (
	SELECT AVG(Products.Price) as MyPrice
	FROM Products
) as subquery;
```

```sql
SELECT Country, round((Number*100/Total),1) as percentage 
FROM (
	SELECT Customers.Country, COUNT(Customers.CustomerID) as Number, (
		SELECT COUNT(Customers.CustomerID)
    	FROM Customers
	) as Total
	FROM Customers
	GROUP BY Customers.Country
) as subquery
ORDER BY percentage DESC;
```

```sql
SELECT Shippers.ShipperName, COUNT(Orders.OrderID) as 'Orders'
FROM Shippers
LEFT JOIN Orders ON Shippers.ShipperID = Orders.ShipperID
GROUP BY Shippers.ShipperName;
```

```sql
SELECT Shippers.ShipperName, (
		SELECT count(Orders.OrderID)
    	FROM Orders
    	WHERE Orders.ShipperID = Shippers.ShipperID
	) as 'Orders'
FROM Shippers;
```

```sql
SELECT *
FROM Calbes
WHERE (
	Calbes.ProductNumber LIKE "%10KV%"
	OR Calbes.ProductNumber LIKE "%1KV%" 
)
AND (
	Calbes.ProductNumber LIKE "%NYM%"
    OR Calbes.ProductNumber LIKE "%NYA%"
)
```

```sql
SELECT Customers.CustomerID, Customers.CustomerName, COUNT(Orders.OrderID) as orders
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID
WHERE Customers.Country = 'USA'
GROUP BY Customers.CustomerID
HAVING COUNT(Orders.OrderID) >= 5
ORDER BY orders DESC
```

```sql
SELECT Customers.CustomerID, Customers.CustomerName, COUNT(Orders.OrderID) as orders
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID
WHERE Customers.Country = 'USA'
GROUP BY Customers.CustomerID
HAVING COUNT(Orders.OrderID) >= 5
ORDER BY orders DESC
LIMIT 10
```

```sql
CREATE PROCEDURE getEmployeesBirthdate
(
    @Birthdate date
)
AS
BEGIN

    SELECT FirstName, LastName, BirthDate
       FROM Employees
       WHERE Birthdate = @Birthdate;
END
GO
```

```sql
exec getEmployeesBirthdate1 "2063-08-30"
```