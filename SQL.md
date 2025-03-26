# SQL cheat sheet <!-- omit in toc -->

## Inhaltsverzeichnis <!-- omit in toc -->

<!-- TOC -->

- [SELECT - Datensätze lesen](#select---datensätze-lesen)
	- [SELECT](#select)
		- [Alias](#alias)
		- [Distinct](#distinct)
	- [WHERE - Bedingungen](#where---bedingungen)
		- [Logische Operatoren](#logische-operatoren)
		- [Weitere Vergleichsoperatoren](#weitere-vergleichsoperatoren)
			- [LIKE](#like)
			- [IN](#in)
			- [BETWEEN](#between)
		- [NULL Values](#null-values)
	- [ORDER BY - Sortierung](#order-by---sortierung)
	- [JOIN - Datensätze/Tabellen verknüpfen](#join---datensätzetabellen-verknüpfen)
		- [Weitere JOINs](#weitere-joins)
	- [Aggregatfunktionen](#aggregatfunktionen)
		- [GROUP BY](#group-by)
		- [HAVING](#having)
	- [Sub query](#sub-query)
	- [LIMIT](#limit)
- [INSERT - Datensätze einfügen](#insert---datensätze-einfügen)
- [UPDATE - Datensätze aktualisieren / ändern](#update---datensätze-aktualisieren--ändern)
- [DELETE - Datensätze löschen](#delete---datensätze-löschen)
<!-- /TOC -->

## SELECT - Datensätze lesen

### SELECT

```sql
SELECT column1, column2, ...
FROM table_name;
```

Beispiele

```sql
SELECT * FROM Customers;
```

```sql
SELECT CustomerName, City FROM Customers;
```

```sql
SELECT Customers.* FROM Customers;
```

```sql
SELECT Customers.CustomerName, Customers.City FROM Customers;
```

#### Alias

```sql
SELECT column_name AS alias_name
FROM table_name;
```

Beispiele

```sql
SELECT CustomerName AS Name, City AS Stadt FROM Customers
```

```sql
SELECT CustomerName AS "Name", City AS "Stadt" FROM Customers;
```

```sql
SELECT c.CustomerName, c.City FROM Customers as c;
```

#### Distinct

```sql
SELECT DISTINCT column1, column2, ...
FROM table_name;
```

Beispiele

```sql
SELECT DISTINCT Country FROM Customers;
```

[&raquo; Übungen zu Select](./Übungen/01_Übungen_1.md#select)

### WHERE - Bedingungen

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

Beispiele

```sql
SELECT * FROM Customers
WHERE City = 'Mannheim';
```

```sql
SELECT * FROM Customers
WHERE CustomerID = 7;
```

[&raquo; SQL Operatoren](https://www.w3schools.com/sql/sql_operators.asp)

#### Logische Operatoren

AND Syntax

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition1 AND condition2 AND condition3 ...;
```

OR Syntax

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition1 OR condition2 OR condition3 ...;
```

NOT Syntax

```sql
SELECT column1, column2, ...
FROM table_name
WHERE NOT condition;
```

Beispiele

```sql
SELECT * FROM Customers
WHERE Country = 'Austria' AND City = "Graz"
```

```sql
SELECT * FROM Customers
WHERE City = "Graz" OR City = "Salzburg"
```

```sql
SELECT * FROM Customers
WHERE NOT Country='Germany';
```

```sql
SELECT * FROM Customers
WHERE Country='Austria' AND (City='Graz' OR City='Innsbruck');
```

[&raquo; Übungen zu Where](./Übungen/01_Übungen_1.md#where)

#### Weitere Vergleichsoperatoren

##### LIKE

Wildcards

- `%` Alle oder kein Zeichen
- `_` Genau ein Zeichen

Beispiele

```sql
SELECT *
FROM Customers
WHERE County LIKE "A%"
```

```sql
SELECT *
FROM Customers
WHERE City LIKE "%i%"
```

##### IN

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1, value2, ...);
```

oder

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name IN (SELECT STATEMENT);
```

Beispiele

```sql
SELECT * FROM Customers
WHERE Country IN ('Germany', 'Austria', 'Italy');
```

##### BETWEEN

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

Beispiele

```sql
SELECT * FROM Products
WHERE Price BETWEEN 10 AND 20;
```

[&raquo; Übungen zu den Vergleichsoperatoren](./Übungen/03_Übungen_3.md#like)

#### NULL Values

    NULL != 0 != "NULL" != ""

Der `NULL` Wert deklariert ein Feld ohne Daten.

```sql
SELECT *
FROM Customers
WHERE CustomerName is null
```

```sql
SELECT *
FROM Customers
WHERE CustomerName is not null
```

### ORDER BY - Sortierung

```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;
```

Beispiele

```sql
SELECT * FROM Customers
ORDER BY City;
```

```sql
SELECT * FROM Customers
ORDER BY CustomerName DESC;
```

```sql
SELECT DISTINCT Country FROM Customers
ORDER BY Country ASC;
```

```sql
SELECT * FROM Customers
ORDER BY Country ASC, City DESC;
```

[&raquo; Übungen zu Order By](./Übungen/01_Übungen_1.md#orderby)

### JOIN - Datensätze/Tabellen verknüpfen

```sql
SELECT column_name(s)
FROM table1
INNER JOIN table2 ON table1.column_name = table2.column_name;
```

Beispiele

```sql
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```

```sql
SELECT Orders.OrderID, Customers.CustomerName, Employees.*
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID
INNER JOIN Employees ON Employees.EmployeeID = Orders.EmployeeID
```

```sql
SELECT Orders.OrderID, Customers.CustomerName, Employees.LastName, Employees.FirstName
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID
INNER JOIN Employees ON Employees.EmployeeID = Orders.EmployeeID
```

#### Weitere JOINs

![SQL Joins](./SQL_Joins.png)

```sql
SELECT column_name(s)
FROM table1
LEFT JOIN table2 ON table1.column_name = table2.column_name;
```

```sql
SELECT column_name(s)
FROM table1
RIGHT JOIN table2 ON table1.column_name = table2.column_name;
```

Beispiele

```sql
SELECT *
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID
```

```sql
SELECT *
FROM Orders
RIGHT JOIN Customers ON Customers.CustomerID = Orders.CustomerID
```

[&raquo; Übungen zu Join](./Übungen/01_Übungen_1.md#join)

### Aggregatfunktionen

- MIN()
- MAX()
- COUNT()
- AVG()
- SUM()

```sql
SELECT AVG(column_name)
FROM table_name
WHERE condition;
```

Beispiele

```sql
SELECT MAX(Price) AS LargestPrice
FROM Products;
```

```sql
SELECT AVG(Price)
FROM Products;
```

[&raquo; Übungen zu den Aggregatfunktionen](./Übungen/01_Übungen_1.md#aggregatfunktionen)

#### GROUP BY

```sql
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
ORDER BY column_name(s);
```

Beispiele

```sql
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country;
```

```sql
SELECT AVG(Price) as "Preis im Schnitt", Categories.CategoryName
FROM Products
INNER JOIN Categories on Products.CategoryID = Categories.CategoryID
GROUP BY Categories.CategoryName
ORDER BY AVG(Price)
```

[&raquo; Übungen zu Group By](./Übungen/01_Übungen_1.md#groupby)

#### HAVING

```sql
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
HAVING condition
ORDER BY column_name(s);
```

Beispiele

```sql
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5;
```

```sql
SELECT Country, COUNT(SupplierID) as "Anzahl"
FROM Suppliers
WHERE Country != "USA"
GROUP BY Country
HAVING COUNT(SupplierID) > 2
ORDER BY Country DESC
```

### Sub query

```sql
SELECT *
FROM table_name1
WHERE column1 = (SELECT column1 FROM table_name2);
```

```sql
SELECT *
FROM (
    SELECT *
    FROM table_name
) as new;
```

Beispiele

```sql
FROM ProductName, (SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.ProductID)
FROM Products

SELECT ProductName, (SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID) as "Kategorei"
FROM Products

SELECT *
FROM Orders
WHERE Orders.CustomerID IN (
    SELECT Customers.CustomerID
    FROM Customers
    WHERE Country = "Austria"
)

```

```sql
SELECT AVG(summe)
FROM (
    SELECT Orders.*, sum(Products.Price) as summe
    FROM Orders
    INNER JOIN Order_details on Orders.OrderID = Order_details.OrderID
    INNER JOIN Products ON Products.ProductID = Order_details.ProductID
    group by Orders.OrderID
) as sub-query
```

[&raquo; Übungen zu Sub Queries](./Übungen/03_Übungen_3.md#subquery)

### LIMIT

```sql
SELECT column_name(s)
FROM table_name
WHERE condition
LIMIT number;
```

Beispiele

```sql
SELECT * FROM Customers
LIMIT 5;
```

```sql
SELECT * FROM Customers
LIMIT 3,6;
```

## INSERT - Datensätze einfügen

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

oder

```sql
INSERT INTO table_name
VALUES (value1, value2, value3, ...);
```

Beispiele

```sql
INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
VALUES ('Daniel', 'Daniel Mur-Spiegl', 'Kaiserjägerstraße 1', 'Innsbruck', '6020', 'Austria');
```

```sql
INSERT INTO Customers (CustomerName, City, Country)
VALUES ('Daniel', 'Innsbruck', 'Austria');
```

[&raquo; Übungen zu Insert](./Übungen/03_Übungen_3.md#insert)

## UPDATE - Datensätze aktualisieren / ändern

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

Beispiele

```sql
UPDATE Customers
SET ContactName = 'Hermann Bauer', City = 'Bregenz'
WHERE CustomerID = 6;
```

```sql
UPDATE Customers
SET ContactName = 'Yuki'
WHERE Country = 'Japan';
```

**ABER ACHTUNG!**

```sql
UPDATE Customers
SET ContactName = 'Josef';
```

[&raquo; Übungen zu Update](./Übungen/03_Übungen_3.md#update)

## DELETE - Datensätze löschen

```sql
DELETE FROM table_name
WHERE condition;
```

Beispiele

```sql
DELETE FROM Customers WHERE CustomerName='Alfreds Futterkiste';
```

**ABER ACHTUNG!**

```sql
DELETE FROM table_name;
```

[&raquo; Übungen zu Delete](./Übungen/03_Übungen_3.md#delete)
