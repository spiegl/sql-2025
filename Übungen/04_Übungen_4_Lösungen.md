# Übungen zu SQL (4)

Als Basis für die Übungen steht das Dataset [sample_data.sql](../sample_data.sql) zur Verfügung.

## Zusammenfassende Übungen

1. Zeige alle Produkte samt Namen des Lieferanten mittels sub query.
2. Zeige die Namen aller Kunden, die mit "K" beginnen.
3. Zeige alle Kunden aus Wien, London und Berlin.
4. Zeige alle Produkte mit einem Preis zwischen 45 und 54 Euro.
5. Füge einen neuen Kunde hinzu (Moritz Moser aus 6020 Innsbruck, Maximilianstraße 15).
6. Der Ansprechpartner von Kunde Nummer 6 hat sich zu "Johanna Meister" geändert.
7. Kunde Nummer 50 möchte aufgrund seiner Rechte der DSGVO aus unserer Datenbank gelöscht werden.

```sql
SELECT Products.ProductName,
    (
        SELECT Suppliers.SupplierName
        FROM Suppliers
        WHERE Suppliers.SupplierID = Products.SupplierID
    ) as Lieferanten
FROM Products;

# JOIN als Vergleich
SELECT Products.ProductName, Suppliers.SupplierName
FROM Products
INNER JOIN Suppliers on Products.SupplierID = Products.SupplierID;
```

```sql
SELECT *
FROM Customers
WHERE Customers.CustomerName LIKE "K%"
```

```sql
# mit IN
SELECT *
FROM Customers
WHERE Customers.City IN ("Vienna", "London", "Berlin");

# mit OR
SELECT *
FROM Customers
WHERE Customers.City = "London" OR Customers.City = "Berlin" OR Customers.City = "Vienna";
```

```sql
# mit BETWEEN
SELECT *
FROM Products
WHERE Price BETWEEN 45 AND 54;

# mit AND
SELECT *
FROM Products
WHERE Price > 45 AND Price < 54;
```

```sql
INSERT INTO Customers (CustomerName, Address, City, PostalCode)
VALUES ("Moritz Moser", "Maximilianstraße 15", "Innsbruck", "6020");

# oder mit SET
INSERT INTO Customers
SET CustomerName = "Janine Moser",
    Address = "Maximilianstraße 15",
    City = "Innsbruck",
    PostalCode = "6020";
```

```sql
UPDATE Customers
SET Customers.ContactName = "Johanna Meister"
WHERE Customers.CustomerID = 6
```

```sql
DELETE FROM Customers
WHERE Customers.CustomerID = 50
```

## Erweiternde Übungen

1. Füge ein neues Produkt samt einer einem Lieferanten hinzu. Achte dabei darauf, dass mehrere Tabellen befüllt werden müssen. (Leonidas Chocolate Special, 200 Gramm für 15 Euro (Kategorie 3 - Confections) vom Lieferanten Leonidas aus 1000 Brüssel, Rue Joseph II 128, Belgien, keine Telefonnummer bekannt).
2. Wir verkaufen unser Nordamerikageschäft an einen Mitbewerber und müssen daher alle Kunden, samt Bestellungen aus den USA, Mexiko und Kanada aus unsere Datenbank löschen.

```sql
INSERT INTO Suppliers (SupplierID, SupplierName, ContactName, Address, City, PostalCode, Country, Phone) VALUES (NULL, 'Leonidas', NULL, 'Rue Joseph II 128', 'Brüssel', '1000', 'Belgien', NULL);
INSERT INTO Products (ProductID, ProductName, SupplierID, CategoryID, Unit, Price) VALUES (NULL, 'Leonidas Chocolate Special', (SELECT SupplierID FROM Suppliers WHERE SupplierName = 'Leonidas' LIMIT 1), '3', '200 Gramm', '15');
```

```sql
DELETE FROM Order_details
WHERE Order_details.OrderID IN (
    SELECT Orders.OrderID
    FROM  Orders
    WHERE Orders.CustomerID IN (
        SELECT Customers.CustomerID
        FROM Customers
        WHERE Customers.Country IN ("USA", "Canada", "Mexico")
    )
);


DELETE FROM Orders
WHERE Orders.CustomerID IN (
    SELECT Customers.CustomerID
    FROM Customers
    WHERE Customers.Country IN ("USA", "Canada", "Mexico")
);

DELETE FROM Customers
WHERE Customers.Country IN ("USA", "Canada", "Mexico");
```
