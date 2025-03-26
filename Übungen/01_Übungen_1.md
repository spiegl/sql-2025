# Übungen zu SQL

Als Basis für die Übungen steht das Dataset [sample_data.sql](../sample_data.sql) zur Verfügung.

## Select

1. Zeige alle Lieferanten ("suppliers") mit Name und Land
2. Zeige alle Länder aller Lieferanten der Datenbank
3. Zeige nur den Namen und das Herkunftsland aller Kunden

## Where

1. Zeige alle Kunden die aus Deutschland und Berlin kommen
2. Zeige alle Kunden aus Berlin oder München
3. Zeige alle Kunden, die nicht aus Deutschland kommen
4. Zeige nur österreichische Kunden
5. Zeige nur Lieferanten aus den USA

## Order By

1. Zeige alle Produkte nach Preis absteigend sortiert
2. Zeige alle Produkte nach Name alphabetisch sortiert
3. Zeige alle Kunden aus Österreich sortiert nach Postleitzahl
4. Zeige alle Kunden, die nicht aus Mexico kommen sortiert nach Herkunftsland

## Join

1. Zeige Kunden und deren Bestellungen (nur Kunden mit mind. einer Bestellung)
2. Zeige alle Kunden und deren Bestellungen
3. Zeige alle Produkte und deren Lieferanten
4. Zeige alle Mitarbeiter und deren zugeordnete Bestellungen (Vorname, Nachname und Bestellnummer)
5. Zeige alle Produkte mit Produktnamen und Kategorienamen

## Aggregatfunktionen

1. Was ist der Durschnittspreis aller Produkte?
2. Was ist der höchste Preis eines Produktes?
3. Was ist der geringste Preis eines Produktes?
4. Wie viele Personen kommen aus Österreich?

## Group By

1. Zeige den durchschnittlichen Produktpreis per Produktkategorie
2. Zeige die Anzahl der Kunden pro Land
3. Zeige alle Kunden mit der Anzahl ihrer Bestellungen
4. Zeige alle Bestellungen mit Bestellwert absteigend sortiert
