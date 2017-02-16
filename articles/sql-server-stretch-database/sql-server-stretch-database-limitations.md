<properties
    pageTitle="Einschränkungen für Dehnen Datenbank | Microsoft Azure"
    description="Informationen Sie zu Einschränkungen für gedehnt Datenbank."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="anvang"/>

# <a name="limitations-for-stretch-database"></a>Einschränkungen für Dehnen Datenbank

Erfahren Sie mehr über die Einschränkungen für gedehnt\-aktiviert, Tabellen und Informationen zu Einschränkungen, die aktuell verhindern gedehnt für eine Tabelle aktivieren.

##  <a name="a-namecaveatsa-limitations-for-stretch-enabled-tables"></a><a name="Caveats"></a>Einschränkungen für gedehnt\-Tabellen aktiviert

Dehnen\-Tabellen aktivierte haben, die folgenden Einschränkungen.

### <a name="constraints"></a>Einschränkungen

-   Eindeutigkeit wird nicht erzwungen für eindeutige und PRIMÄRSCHLÜSSEL Einschränkungen in der Azure-Tabelle, die die migrierten Daten enthält.

### <a name="dml-operations"></a>DML-Vorgänge

-   Kann nicht aktualisiert oder Zeilen löschen, die wurden migriert oder Zeilen, die für die Migration in einem gedehnt berechtigt sind\-Tabelle aktiviert oder in einer Ansicht, die enthält gestreckt\-Tabellen aktiviert.

-   Einfügen von Zeilen kann nicht in einem gedehnt\-Tabelle auf einen verknüpften Server aktiviert.

### <a name="indexes"></a>Indizes

-   Sie können keinen Index für eine Ansicht, die gedehnt enthält erstellen\-Tabellen aktiviert.

-   Filter in SQL Server-Indizes werden nicht in der entfernten Tabelle übernommen.

##  <a name="a-namelimitationsa-limitations-that-currently-prevent-you-from-enabling-stretch-for-a-table"></a><a name="Limitations"></a>Einschränkungen, die aktuell verhindern gedehnt für eine Tabelle aktivieren

Die folgenden Elemente verhindern derzeit gedehnt für eine Tabelle aktivieren.

### <a name="table-properties"></a>Tabelleneigenschaften

-   Tabellen mit mehr als 1.023 Spalten oder mehr als 998 Indizes

-   FileTables oder den Tabellen, die FILESTREAM-Daten enthalten.

-   Tabellen repliziert werden, oder verwenden, die aktiv Änderungsprotokoll muss oder Änderung Erfassen von Daten

-   Arbeitsspeicher\-Tabellen optimiert

### <a name="data-types"></a>Datentypen

-   Text, Ntext und image

-   Zeitstempel

-   SQL\_Variant-Wert

-   XML

-   CLR-Datentypen einschließlich Geometrie, "geography", "hierarchyid" und CLR-Benutzer\-Typen definiert

### <a name="column-types"></a>Spaltentypen

-   Spalte\_festlegen

-   Berechnete Spalten

### <a name="constraints"></a>Einschränkungen

-   Standard- und Kontrollkästchen Einschränkungen

-   Fremdschlüssel wichtigsten Einschränkungen, die auf die Tabelle verweisen. In einem übergeordneten\-untergeordnete Beziehung \(beispielsweise Reihenfolge und Reihenfolge\_Details\), können Sie gedehnt aktivieren, für die untergeordnete Tabelle \(Reihenfolge\_Details\) , aber nicht für die übergeordnete Tabelle \(Reihenfolge\).

### <a name="indexes"></a>Indizes

-   Volltextindizes

-   XML-Indizes

-   Indizes

-   Indizierte Sichten, die auf die Tabelle verweisen.

## <a name="see-also"></a>Siehe auch

[Identifizieren von Datenbanken und Tabellen für gedehnt Datenbank durch Ausführen von gedehnt Datenbank Advisor](sql-server-stretch-database-identify-databases.md)

[Aktivieren Sie gedehnt Datenbank für eine Datenbank](sql-server-stretch-database-enable-database.md)

[Aktivieren Sie gedehnt Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md)
