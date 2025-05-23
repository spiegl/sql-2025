# Übungen zu SQL

Als Basis für die Übungen steht das Dataset [sample_data.sql](sample_data.sql) zur Verfügung.

## Select

1. Zeige alle Lieferanten ('suppliers') mit Name und Land
2. Zeige alle Länder aller Lieferanten der Datenbank
3. Zeige nur den Namen und das Herkunftsland aller Kunden

```sql
SELECT SupplierName, Country
FROM Suppliers
```

```sql
SELECT DISTINCT Country
FROM Suppliers
```

```sql
SELECT CustomerName, Country
FROM Customers
```

## Where

1. Zeige alle Kunden die aus Deutschland und Berlin kommen
2. Zeige alle Kunden aus Berlin oder München
3. Zeige alle Kunden, die nicht aus Deutschland kommen
4. Zeige nur österreichische Kunden
5. Zeige nur Lieferanten aus den USA

```sql
SELECT *
FROM Customers
WHERE Country = 'Germany' AND City = 'Berlin'
```

```sql
SELECT *
FROM Customers
WHERE City = 'München' OR City = 'Berlin'
```

```sql
SELECT *
FROM Customers
WHERE NOT Country = 'Germany'
```

```sql
SELECT *
FROM Customers
WHERE Country = 'Austria'
```

```sql
SELECT *
FROM Suppliers
WHERE Country = 'USA'
```

## Order By

1. Zeige alle Produkte nach Preis absteigend sortiert
1. Zeige alle Produkte nach Name alphabetisch sortiert
1. Zeige alle Kunden aus Österreich sortiert nach Postleitzahl
1. Zeige alle Kunden, die nicht aus Mexico kommen sortiert nach Herkunftsland

```sql
SELECT *
FROM Products
ORDER BY Price DESC
```

```sql
SELECT *
FROM Products
ORDER BY ProductName ASC
```

```sql
SELECT *
FROM Customers
ORDER BY PostalCode
```

```sql
SELECT *
FROM Customers
WHERE NOT Country = 'Mexico'
ORDER BY Country
```

## Join

1. Zeige Kunden und deren Bestellungen (nur Kunden mit mind. einer Bestellung)
1. Zeige alle Kunden und deren Bestellungen
1. Zeige alle Produkte und deren Lieferanten
1. Zeige alle Mitarbeiter und deren zugeordnete Bestellungen (Vorname, Nachname und Bestellnummer)
1. Zeige alle Produkte mit Produktnamen und Kategorienamen

```sql
SELECT *
FROM Customers
INNER JOIN Orders ON Customers.CustomerID = Orders.CustomerID
```

```sql
SELECT *
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID
```

```sql
SELECT *
FROM Products
INNER JOIN Suppliers ON Products.SupplierID = Suppliers.SupplierID
```

```sql
SELECT Employees.FirstName, Employees.LastName, Orders.OrderID
FROM Employees
INNER JOIN Orders ON Employees.EmployeeID = Orders.EmployeeID
```

```sql
SELECT Products.ProductName, Categories.CategoryName
FROM Products
INNER JOIN Categories on Products.CategoryID = Categories.CategoryID
```

## Aggregatfunktionen

1. Was ist der Durschnittspreis aller Produkte?
2. Was ist der höchste Preis eines Produktes?
3. Was ist der geringste Preis eines Produktes?
4. Wie viele Personen kommen aus Österreich?

```sql
SELECT AVG(Price)
FROM Products
```

```sql
SELECT MAX(Price)
FROM Products
```

```sql
SELECT MIN(Price)
FROM Products
```

```sql
SELECT COUNT(CustomerID)
FROM Customers
WHERE Country = 'Austria'
```

## Group By

1. Zeige den durchschnittlichen Produktpreis per Produktkategorie
2. Zeige die Anzahl der Kunden pro Land
3. Zeige alle Kunden mit der Anzahl ihrer Bestellungen
4. Zeige alle Bestellungen mit Bestellwert absteigend sortiert

```sql
SELECT CategoryName, AVG(Price)
FROM Products
INNER JOIN Categories ON Products.CategoryID = Categories.CategoryID
GROUP BY CategoryName
```

```sql
SELECT Country, COUNT(CustomerID)
FROM Customers
GROUP BY Country
```

```sql
SELECT Customers.*, COUNT(Orders.OrderID)
FROM Customers
left JOIN Orders ON Customers.CustomerID = Orders.CustomerID
GROUP BY Customers.CustomerID
```

```sql
SELECT Orders.OrderID, sum(Products.Price)
FROM Orders
INNER JOIN Order_details ON Orders.OrderID = Order_details.OrderID
INNER JOIN Products ON Order_details.ProductID = Products.ProductID
GROUP BY Orders.OrderID
ORDER BY sum(Products.Price) DESC
```
