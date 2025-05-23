# Übungen SQL (3)

Als Basis für die Übungen steht das Dataset [sample_data.sql](../sample_data.sql) zur Verfügung.

## Sub query

- Zeige alle Produkte und deren Kategorie mit einer sub query.
- Zeige alle Orders samt Kundennamen mit einer sub query.

```sql
SELECT
    Products.ProductName, (
        SELECT Categories.CategoryName
        FROM Categories
        WHERE Categories.CategoryID = Products.CategoryID
    ) as Category
FROM Products
```

```sql
SELECT Orders.OrderID, (
    SELECT Customers.CustomerName
    FROM Customers
    WHERE Customers.CustomerID = Orders.CustomerID
) as Customer
FROM Orders
```

## LIKE

- Zeige die Namen aller Kunden, die mit 'J' beginnen.
- Zeige alle Kunden, die ein 'e' im Namen haben.
- Zeige alle Lieferanten aus Städten, die mit 'n' enden.
- Zeige alle Bestellungen von ID 10251 bis ID 10259 mittels Vergleichsoperatoren.
- Zeige alle Bestellungen von ID 10251 bis ID 10259 mittels BETWEEN Operator.
- Zeige alle Kunden mit den IDs 8, 15 und 24.

```sql
SELECT *
FROM Customers
WHERE Customers.CustomerName LIKE 'J%'
```

```sql
SELECT *
FROM Customers
WHERE Customers.CustomerName LIKE '%e%'
```

```sql
SELECT *
FROM Customers
WHERE Customers.City LIKE '%n'
```

```sql
SELECT *
FROM Orders
WHERE Orders.OrderID >= 10251
AND Orders.OrderID <= 10259
```

```sql
SELECT *
FROM Orders
WHERE Orders.OrderID BETWEEN 10251 AND 10259
```

```sql
SELECT *
FROM Customers
WHERE Customers.CustomerID IN (8, 15, 24)
```

## INSERT

- Füge einen Lieferanten hinzu (Heinz Müller vom Unternehmen A-Z Transporte, aus 1110 Wien Ailecgasse 15).
- Füge einen Mitarbeiter hinzu (Franz Xaver, geb. am 15.08.1976 mit der Notiz 'Programmierer').
- Für eine Bestellung mit mindestens 2 Produkten hinzu. Achte dabei darauf, dass mehrere Tabellen befüllt werden müssen.

```sql
INSERT INTO Suppliers (SupplierName, ContactName, Address, City, PostalCode, Country)
VALUES ('A-Z Transporte', 'Heinz Müller', 'Ailecgasse 15', 'Vienna', '1110', 'Austria');
```

oder

```sql
INSERT INTO Suppliers
VALUES (NULL, 'A-Z Transporte', 'Heinz Müller', 'Ailecgasse 15', 'Vienna', '1110', 'Austria', NULL);
```

```sql
INSERT INTO Employees (Employees.LastName, Employees.FirstName, Employees.BirthDate, Employees.Notes)
VALUES ('Xaver', 'Franz', '1976-08.15', 'Programmer')
```

```sql
# Bestellung
INSERT INTO Orders
VALUES (NULL, 1, 1, CURRENT_DATE(), 1);

# Produkte
INSERT INTO Order_details
VALUES (
    NULL,
    (SELECT Orders.OrderID FROM Orders ORDER BY Orders.OrderID DESC LIMIT 1),
    1,
    1
);

INSERT INTO Order_details
VALUES (
    NULL,
    (SELECT Orders.OrderID FROM Orders ORDER BY Orders.OrderID DESC LIMIT 1),
    2,
    1
);
```

## UPDATE

- Der Kunde mit der ID 14 ist umgezogen und wohnt nun in der Bahnhofstraße 8. Aktualisiere die Adresse.
- Unsere neuer Mitarbeiter hat sich fälschlicherweise als Franz vorgestellt. Sein Vorname ist jedoch Fritz.

```sql
UPDATE Customers
SET Customers.Address = 'Bahnhofstraße 8'
WHERE Customers.CustomerID = 14
```

```sql
UPDATE Employees
SET Employees.FirstName = 'Fritz'
WHERE Employees.FirstName = 'Franz'
```

## DELETE

- Lösche den Kunden mit der ID 24.
- Lösche die Bestellung mit der ID 2.

```sql
DELETE FROM Customers
WHERE Customers.CustomerID = 24
```

oder

```sql
DELETE FROM Order_details
WHERE Order_details.OrderID IN (
    SELECT Orders.OrderID
    FROM Orders
    WHERE Orders.CustomerID = 24
);

DELETE FROM Orders
WHERE Orders.CustomerID = 24;

DELETE FROM Customers
WHERE Customers.CustomerID = 24;
```

```sql
DELETE FROM Orders
WHERE Orders.OrderID = 2
```
