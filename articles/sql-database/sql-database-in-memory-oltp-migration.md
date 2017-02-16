<properties
    pageTitle="In-Memory OLTP verbessert SQL Txn Perf | Microsoft Azure"
    description="Adopt In-Memory OLTP Transaktionen Leistung in einer vorhandenen SQL­Datenbank."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor="MightyPen"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="use-in-memory-oltp-preview-to-improve-your-application-performance-in-sql-database"></a>Verwenden von In-Memory OLTP (Preview) zum Verbessern der Leistung der Anwendung in SQL-Datenbank

Für optimale Leistung von OLTP Arbeitsbelastung in [Premium](sql-database-service-tiers.md) Azure SQL-Datenbanken ohne die Leistungsstufe erhöhen, kann [In-Memory OLTP](sql-database-in-memory.md) verwendet werden.

Wie folgt vor, um In-Memory OLTP in einer vorhandenen Datenbank zu übernehmen.

## <a name="step-1-ensure-your-premium-database-supports-in-memory-oltp"></a>Schritt 1: Stellen Sie sicher, dass Ihre Premium-Datenbank In-Memory OLTP unterstützt.

Premium-Datenbanken, die im November 2015 oder höher erstellte unterstützen das Feature In-Memory. Sie können prüfen, ob Ihre Premium Datenbank das Feature In-Memory unterstützt, indem Sie die folgende Transact-SQL-Anweisung ausführen. In-Memory wird unterstützt, ist das Ergebnis zurückgegebene 1 (nicht 0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* steht für die *Verarbeitung von extrem Transaktion*

Wenn Ihre vorhandene Datenbank in eine neue V12 Premium-Datenbank verschoben werden muss, können Sie die folgenden Verfahren exportieren und importieren Sie die Daten verwenden.

#### <a name="export-steps"></a>Exportschritte

Exportieren Sie Ihrer Datenbank Herstellung zu einer Bacpac indem Sie entweder:

- Die [Exportieren](sql-database-export.md) Funktionalität im [Portal](https://portal.azure.com/).

- Die Funktionalität der **Anwendung auf Datenebene exportieren** in eine [auf dem neuesten Stand SSMS.exe](http://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio).
 1. Erweitern Sie im **Objekt-Explorer**- **Datenbanken** .
 2. Mit der rechten Maustaste in Ihrer Datenbank-Knotens.
 3. Klicken Sie auf **Vorgänge** > **Datenebene Anwendung exportieren**.
 4. Arbeiten Sie das Fenster des Assistenten, das angezeigt wird.


#### <a name="import-steps"></a>Importschritte

Importieren Sie die Bacpac in eine neue Premium-Datenbank.

1. Im Azure- [portal](https://portal.azure.com/)
 - Navigieren Sie auf dem Server.
 - Wählen Sie die Option [Datenbank importieren](sql-database-import.md) .
 - Wählen Sie eine Ebene Preise Premium aus.

2. Verwenden Sie SSMS, um die Bacpac importieren:
 - Im **Objekt-Explorer**mit der rechten Maustaste des Knotens **Datenbanken** .
 - Klicken Sie auf **die Datenebene Anwendung importieren**.
 - Arbeiten Sie das Fenster des Assistenten, das angezeigt wird.


## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>Schritt 2: Identifizieren von Objekten zum Migrieren von In-Memory-OLTP

SSMS enthält einen **Transaktion Leistung Analyse Übersicht** Bericht, den Sie anhand einer Datenbank mit einer aktiven Arbeitsbelastung ausgeführt werden können. Der Bericht identifiziert Tabellen und gespeicherten Prozeduren, die für die Migration zu In-Memory OLTP geeignet sind.

In SSMS, um den Bericht zu erstellen:
- Klicken Sie in der **Objekt-Explorer**mit der rechten Maustaste Ihrer Datenbank-Knotens.
- Klicken Sie auf **Berichte** > **Standardberichte** > **Transaktion Leistung Analyse Übersicht**.

Weitere Informationen finden Sie unter [festlegen, wenn Sie einer Tabelle oder gespeicherte Prozedur sollte portiert werden zum In-Memory OLTP](http://msdn.microsoft.com/library/dn205133.aspx).


## <a name="step-3-create-a-comparable-test-database"></a>Schritt 3: Erstellen einer Testdatenbank vergleichbaren

Nehmen Sie an, dass der Bericht gibt an, dass die Datenbank eine Tabelle enthält, die aus einer Tabelle Speicher optimiert konvertierte profitieren würden. Es empfiehlt sich, dass Sie zunächst testen, um die Angabe von testen zu bestätigen.

Sie benötigen eine Testkopie der Herstellung Datenbank. Die Datenbank prüfen muss auf der gleichen Ebene der Dienst Ebene als Datenbank.

Zum Testen zu erleichtern, gibt Sie die Testdatenbank wie folgt:

1. Verbinden Sie mit der Testdatenbank mithilfe von SSMS ein.

2. Um zu vermeiden, benötigen die Option mit (SNAPSHOT) in Abfragen, legen Sie die Datenbankoption wie in der folgenden T-SQL-Anweisung dargestellt:
```
ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
```


## <a name="step-4-migrate-tables"></a>Schritt 4: Migrieren von Tabellen

Erstellen, und füllen Sie eine Speicher optimiert die Kopie der Tabelle, die Sie testen möchten. Sie können es erstellen, indem Sie entweder:

- Die praktischen Arbeitsspeicher Optimierung-Assistenten in SSMS.
- Manuelle T-SQL.


#### <a name="memory-optimization-wizard-in-ssms"></a>Optimierung-Assistenten in SSMS Arbeitsspeicher

So verwenden Sie diese Migrationsoption

1. Verbinden Sie mit der Testdatenbank mit SSMS.

2. Im **Objekt-Explorer**mit der rechten Maustaste auf die Tabelle, und klicken Sie dann auf **Arbeitsspeicher Optimierung Advisor**.
 - Die **Tabelle Arbeitsspeicher Optimizer Advisor** -Assistent wird angezeigt.

3. Klicken Sie im Assistenten auf **Migration Überprüfung** (oder die Schaltfläche **Weiter** ), um festzustellen, ob die Tabelle eine nicht unterstützte Features enthält, die in Tabellen Speicher optimiert nicht unterstützt werden. Weitere Informationen finden Sie unter:
 - Der *Arbeitsspeicher Optimierung Checkliste* unter [Arbeitsspeicher Optimierung Advisor](http://msdn.microsoft.com/library/dn284308.aspx).
 - [Transact-SQL-Konstrukte von In-Memory OLTP nicht unterstützt](http://msdn.microsoft.com/library/dn246937.aspx).
 - [Migrieren zu In-Memory OLTP](http://msdn.microsoft.com/library/dn247639.aspx).

4. Wenn die Tabelle keine nicht unterstützte Features enthält, kann der Advisor die eigentliche Schema und Migration von Daten für Sie durchführen.


#### <a name="manual-t-sql"></a>Manuelle T-SQL

So verwenden Sie diese Migrationsoption

1. Verbinden Sie mit Ihrer Testdatenbank über SSMS (oder einem ähnlichen Tool).

2. Das vollständige T-SQL-Skript für die Tabelle und ihre Indizes zu erhalten.
 - In SSMS mit der rechten Maustaste Ihrer Tabellenknotens.
 - Klicken Sie auf **Skript Tabelle als** > **erstellen, um** > **Neues Abfragefenster**.

3. Fügen Sie im Fenster Skript mit (MEMORY_OPTIMIZED = ON) die CREATE TABLE-Anweisung.

4. Wenn ein gruppierte Index vorhanden ist, ändern Sie ihn in NONCLUSTERED ein.

5. Benennen Sie die vorhandene Tabelle mithilfe von SP_RENAME aus.

6. Erstellen Sie die neue Speicher optimiert Kopie der Tabelle, indem der bearbeiteten CREATE TABLE-Skript ausführen.

7. Kopieren Sie die Daten Ihrer Tabelle Speicher optimiert mithilfe von einfügen... WÄHLEN SIE * IN:
    
```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>Schritt 5 (optional): Migrieren von gespeicherten Prozeduren

Das Feature In-Memory kann auch eine gespeicherte Prozedur zum Verbessern der Leistung ändern.


### <a name="considerations-with-natively-compiled-stored-procedures"></a>Aspekte mit systembedingt kompilierte gespeicherte Prozeduren

Eine systembedingt kompilierte gespeicherte Prozedur müssen auf ihrer T-SQL mit-Klausel die folgenden Optionen:

- NATIVE_COMPILATION

- SCHEMABINDING: Bedeutung Tabellen, dass die gespeicherte Prozedur ihrer Spaltendefinitionen in keiner Weise, die die gespeicherte Prozedur beeinträchtigen kann geändert werden, es sei denn, Sie legen Sie die gespeicherte Prozedur haben kann.


Ein systemeigener Modul muss eine große [ATOMAREN Bausteinen](http://msdn.microsoft.com/library/dn452281.aspx) zum Verwalten von Transaktionen verwenden. Es gibt keine Rolle für eine explizite beginnen Transaktion oder für Transaktion zurücksetzen. Wenn der Code einen Verstoß gegen eine Regel Business erkennt, können sie den atomaren Block mit einer Anweisung [AUSLÖSEN](http://msdn.microsoft.com/library/ee677615.aspx) beenden.


### <a name="typical-create-procedure-for-natively-compiled"></a>Übliche erstellen Verfahren zum kompiliert systembedingt

Normalerweise entspricht der T-SQL-Code zum Erstellen einer systembedingt kompilierten gespeicherten Prozedur ähnlich wie die folgende Vorlage:

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

- Für die TRANSACTION_ISOLATION_LEVEL ist MOMENTAUFNAHME der am häufigsten vorkommenden Wert für die systembedingt kompilierte gespeicherte Prozedur. Jedoch eine Teilmenge der anderen Werte werden ebenfalls unterstützt:
 - LESEN SIE WIEDERHOLT
 - SERIALISIERBAR


- Der Sprachwert muss in der Ansicht sys.languages vorhanden sein.


### <a name="how-to-migrate-a-stored-procedure"></a>Wie Sie eine gespeicherte Prozedur migrieren

Die Migrationsschritte sind:


1. Das Skript CREATE PROCEDURE an die reguläre interpretiert gespeicherte Prozedur zu erhalten.

2. Schreiben Sie ihren Header entsprechend die vorherige Vorlage ein.

3. Prüfen Sie, ob die gespeicherte Prozedur T-SQL-Code alle Funktionen verwendet, die für systembedingt kompilierte gespeicherte Prozeduren nicht unterstützt werden. Implementieren Sie problemumgehung, falls dies erforderlich.
 - Ausführliche Informationen finden Sie unter [Migrationsprobleme für gespeicherte Prozeduren systembedingt kompiliert](http://msdn.microsoft.com/library/dn296678.aspx).

4. Benennen Sie die alte gespeicherte Prozedur mithilfe von SP_RENAME. Oder legen Sie es einfach ab.

5. Führen Sie Ihre bearbeiteten erstellen Verfahren T-SQL-Skript.


## <a name="step-6-run-your-workload-in-test"></a>Schritt 6: Ihre Arbeitsbelastung in Test ausgeführt

Führen Sie eine Arbeitsbelastung in der Testdatenbank, die die Arbeitsbelastung ähnelt, die in der Herstellung Datenbank ausgeführt wird. Dadurch sollte die Leistung Gain erreicht durch die Verwendung der In-Memory-Feature für Tabellen und gespeicherten Prozeduren angezeigt.

Die Arbeitsbelastung wichtigsten Attribute sind:

- Anzahl der aktiven Verbindungen.

- / Lese-Verhältnis.


Zum Anpassen können, und führen Sie die Arbeitsbelastung testen, erwägen Sie das praktischen ostress.exe Tool, das in [hier](sql-database-in-memory.md)dargestellt.


Führen Sie zum Netzwerkwartezeit minimieren, Ihre Test in der gleichen geografische Region Azure, in dem die Datenbank vorhanden ist.


## <a name="step-7-post-implementation-monitoring"></a>Schritt 7: Nach der Implementierung Überwachung

Beachten Sie die Effekte Leistung der Herstellung der In-Memory-Implementierungen Überwachung aus:

- [Monitor In-Memory-Speicher](sql-database-in-memory-oltp-monitoring.md).

- [Verwenden von dynamischen Management Ansichten Azure SQL-Datenbank für die Überwachung](sql-database-monitoring-with-dmvs.md)


## <a name="related-links"></a>Links zu verwandten Themen

- [In-Memory-OLTP (In-Memory Optimization)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Einführung in systembedingt kompilierte gespeicherte Prozeduren](http://msdn.microsoft.com/library/dn133184.aspx)

- [Arbeitsspeicher Optimierung Advisor](http://msdn.microsoft.com/library/dn284308.aspx)

