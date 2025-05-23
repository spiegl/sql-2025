# Übungen zu SQL (2)

Als Basis für die Übungen steht das Dataset [sample_data.sql](../sample_data.sql) zur Verfügung.

## Zusammenfassende Übungen

1. Zeige alle Lieferanten, die aus Japan kommen mit den Attributen SupplierName, ContactName, Address, City und Country.
2. Zeige alle Städte der Tabelle Kunden, aufsteigend sortiert.
3. Zeige alle Produkte mit einen höherem Preis als 100, sortiert nach Produktname mit den Attributen Name, Einheit und Preis.
4. Zeige alle Kategorien und zugehörige Produkte an, geordnet nach Kategoriename und dann nach Produktname (nur Name der Kategorie und Name des- Produktes)
5. Zeige alle Lieferdienste ('Shippers') mit der Anzahl der versendeten Bestellungen
6. Wie viele Lieferanten gibt es?
7. Zeige die Anzahl der Lieferanten, gruppiert nach Land, absteigend sortiert nach Ländername.

```sql
SELECT SupplierName, ContactName, Address, City, Country
FROM Suppliers
WHERE Country = 'Japan'
```

```sql
SELECT DISTINCT City
FROM Customers
ORDER BY City
```

```sql
SELECT ProductName, Unit, Price
FROM Products
WHERE Price > 100
ORDER BY ProductName
```

```sql
SELECT Categories.CategoryName, Products.ProductName
from Products
inner JOIN Categories
ORDER  BY Categories.CategoryName, Products.ProductName
```

oder

```sql
SELECT CategoryName, ProductName
FROM Categories, Products
WHERE Products.CategoryID = Categories.CategoryID
ORDER BY ProductName, CategoryName
```

```sql
SELECT Shippers.ShipperName, COUNT(Orders.OrderID)
FROM Orders
INNER JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID
GROUP By Shippers.ShipperID
```

```sql
SELECT count(*)
FROM Suppliers
```

```sql
SELECT Country, COUNT(*)
FROM Suppliers
GROUP BY Country
ORDER BY Country DESC
```

## Erweiternde Übungen

1. Zeige alle Kunden ohne Bestellungen.
2. Zeige den durchschnittlichen Bestellwert aller Bestellungen.

```sql
SELECT *
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID
WHERE Orders.CustomerID is null
```

```sql
SELECT AVG(summe)
FROM (
    SELECT Orders.*, sum(Products.Price * Order_details.Quantity) as summe
    FROM Orders
    INNER JOIN Order_details on Orders.OrderID = Order_details.OrderID
    INNER JOIN Products ON Products.ProductID = Order_details.ProductID
    group by Orders.OrderID
) as subquery
```
