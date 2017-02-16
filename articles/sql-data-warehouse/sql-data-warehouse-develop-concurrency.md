<properties
   pageTitle="Parallelität und Arbeitsbelastung Management in SQL Data Warehouse | Microsoft Azure"
   description="Grundlegendes zu Parallelität und Arbeitsbelastung Management in Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>Parallelität und Arbeitsbelastung Management in SQL Data Warehouse

Zum Vorführen vorhersehbar Leistung bei, hilft Microsoft Azure SQL-Data Warehouse steuern Gleichzeitigkeitsgrade und Ressourcen Zuweisungen wie Arbeitsspeicher und CPU-Prioritäten. In diesem Artikel bietet eine Einführung in die Konzepte der Parallelität und Arbeitsbelastung Verwaltung erläutert, wie die beiden Features implementiert wurden und wie Sie diese in Ihr Datawarehouse steuern können. SQL Data Warehouse Arbeitsbelastung Management soll Sie Unterstützung für mehrere Benutzer Umgebungen helfen. Es ist nicht für mehrere Mandanten Auslastung vorgesehen.

## <a name="concurrency-limits"></a>Parallelitätsgrenzen

SQL Data Warehouse können bis zu 1.024 aktiven Verbindungen. Alle 1.024 Verbindungen können Abfragen gleichzeitig einreichen. Jedoch möglicherweise um Durchsatz zu optimieren, SQL Data Warehouse Warteschlange einige Abfragen, um sicherzustellen, dass jede Abfrage eine minimale Arbeitsspeicher erteilen erhält. Queuing tritt auf, während der Ausführung der Abfrage. Durch Warteschlange Abfragen Wenn Parallelitätsgrenzen erreicht sind, können SQL Data Warehouse erhöhen Gesamtdurchsatz, um sicherzustellen, dass die aktive Abfragen Zugriff auf kritisch erforderliche Speicherressourcen zu erhalten.  

Parallelitätsgrenzen unterliegen zwei Konzepte: *gleichzeitige Abfragen* und *Parallelität Steckplätze*. Für eine Abfrage ausführen müssen sie in die Abfrage Parallelität Begrenzung sowohl die Zuweisung der Parallelität ausführen.

- Gleichzeitige Abfragen werden die Abfragen zur gleichen Zeit ausführen. SQL Data Warehouse unterstützt bis zu 32 gleichzeitige Abfragen auf das größere DWU.
- Parallelität Steckplätze werden basierend auf DWU zugewiesen. Jede 100 DWU bietet 4 Parallelität Steckplätze. Angenommen, eine DW100 4 Parallelität Steckplätze und zuweist DW1000 40. Jede Abfrage verbraucht eine oder mehrere Parallelität Steckplätze, hängt von der [Ressourcenklasse](#resource-classes) der Abfrage an. Abfragen, die in der Klasse Smallrc Ressource ausgeführt nutzen eine Parallelität Slot. Abfragen in einer höheren Ressourcenklasse ausführen nutzen weiteren Parallelität Slots.

Die folgende Tabelle beschreibt die Grenzwerte für die gleichzeitige Abfragen Parallelität Zeitnischen auf die verschiedenen DWU Größen.

### <a name="concurrency-limits"></a>Parallelitätsgrenzen

|  DWU   | Max gleichzeitige Abfragen  | Parallelität Umsetzungsplätzen |
| :----  | :---------------------: | :-------------------------: |
| DW100  |            4            |                4            |
| DW200  |            8            |                8            |
| DW300  |           12            |               12            |
| DW400  |           16            |               16            |
| DW500  |           20            |               20            |
| DW600  |           24            |               24            |
| DW1000 |           32            |               40            |
| DW1200 |           32            |               48            |
| DW1500 |           32            |               60            |
| DW2000 |           32            |               80            |
| DW3000 |           32            |              120            |
| DW6000 |           32            |              240            |

Wenn einer der beiden Schwellenwerte erfüllt ist, werden neue Abfragen in der Warteschlange und auf einer ersten-in/First ausgeführt.  Während einer Abfragen endet und die Anzahl der Abfragen und Steckplätze unter die Grenzwerte fällt, werden in der Warteschlange Abfragen zur Verfügung. 

> [AZURE.NOTE]  *Wählen Sie* Abfragen, die ausgeführt werden ausschließlich auf dynamische Management Ansichten (DMVs) oder Katalogsansichten sind nicht keines der Parallelitätsgrenzen geregelt. Sie können das System unabhängig von der Anzahl der Abfragen ausgeführt werden, daran überwachen.

## <a name="resource-classes"></a>Ressourcenklassen

Ressource Klassen Hilfe, die Sie arbeitsspeicherzuteilung und CPU-Phasen, die zu einer Abfrage angegebenen steuern. Sie können vier Ressourcenklassen eines Benutzers in Form von *Datenbankrollen*zuweisen. Die vier Ressourcenklassen sind **Smallrc**, **Mediumrc**, **Largerc**und **Xlargerc**. Benutzer in Smallrc wurde eine kleinere Menge an Speicherplatz und können höhere gleichzeitige Verarbeitung nutzen. Im Gegensatz dazu Xlargerc zugewiesene Benutzer sind sehr viel Speicherplatz angegeben, und weniger der ihre Abfragen können daher gleichzeitig ausgeführt werden.

Standardmäßig ist jeder Benutzer ein Mitglied der Ressourcenklasse kleine, Smallrc. Das Verfahren `sp_addrolemember` wird verwendet, um die Klasse, vergrößern und `sp_droprolemember` wird verwendet, um die Klasse zu verkleinern. Dieser Befehl vergrößert z. B. die Klasse von Loaduser zu Largerc:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```

Empfiehlt sich das ist ihre Ressourcenklassen ändern, sondern eine Ressourcenklasse dauerhaft Benutzer zuweisen. Beispielsweise erstellen lädt mit gruppierten Columnstore Tabellen Indizes höherer Qualität, wenn mehr Arbeitsspeicher belegt. Um sicherzustellen, dass lädt in höhere Speicher verfügen, erstellen Sie einen Benutzer speziell für das Laden von Daten und dauerhaft weisen Sie diesen Benutzer zu einer höheren Ressourcenklasse zu.

Es gibt einige Arten von Abfragen, die nicht aus einer größeren arbeitsspeicherzuteilung sinnvoll ist. Das System ignoriert deren Klasse ressourcenzuteilung und führen Sie diese Abfragen immer in der Ressourcenklasse kleine aus stattdessen. Wenn in der Ressourcenklasse kleine immer diese Abfragen ausgeführt werden, können sie ausgeführt, wenn Parallelität Steckplätze unter Druck stehen und weiteren Slots als benötigt wird nicht benötigen. Weitere Informationen finden Sie unter [Ressource Klasse Ausnahmen](#query-exceptions-to-concurrency-limits) .

Einige Informationen zur Ressourcenklasse:

- *Alter Rolle* über die Berechtigung ist erforderlich, die Klasse eines Benutzers ändern.  
- Obwohl Sie einen Benutzer ein oder mehrere der höheren Ressourcenklassen hinzufügen können, dauert Benutzer auf den Attributen der höchsten Ressourcenklasse, die sie zugeordnet sind. D. h. ist, wenn Mediumrc und Largerc ein Benutzers zugewiesen ist, die höhere Klasse (Largerc) die Klasse, die berücksichtigt wird.  
- Die Klasse des Benutzers administrative System kann nicht geändert werden.

Ein detailliertes Beispiel finden Sie unter [ändern Benutzer Beispiel für Ressourcen](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>Arbeitsspeicherzuteilung

Es gibt vor- und Nachteile zunehmender eines Benutzers Ressource Class. Zunehmender einer Ressourcenklasse für einen Benutzer wird ihre Abfragen auf mehr Speicher Zugriff die bedeuten kann, dass Abfragen schneller ausgeführt werden.  Höhere Ressourcenklassen verringern jedoch auch die Anzahl der gleichzeitigen Abfragen, die ausgeführt werden können. Dies ist das Verhältnis zwischen Reservierung sehr viel Speicherplatz zu einer einzelnen Abfrage oder den anderen Abfragen, die benötigen Sie auch Arbeitsspeicher Zuweisungen, um gleichzeitig ausgeführt wird. Wenn ein Benutzer hohe Zuweisungen Arbeitsspeicher für eine Abfrage angegeben ist, werden andere Benutzer nicht auf die gleiche Speicher zum Ausführen einer Abfrage zugreifen.

In der folgenden Tabelle wird jede Verteilung durch DWU und Ressourcen Klasse belegten Speicher zugeordnet.

### <a name="memory-allocations-per-distribution-mb"></a>Arbeitsspeicher Zuweisungen pro Verteilung (MB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |   100   |    100   |    200  |    400   |
| DW200  |   100   |    200   |    400  |    800   |
| DW300  |   100   |    200   |    400  |    800   |
| DW400  |   100   |    400   |    800  |  1.600   |
| DW500  |   100   |    400   |    800  |  1.600   |
| DW600  |   100   |    400   |    800  |  1.600   |
| DW1000 |   100   |    800   |  1.600  |  3.200   |
| DW1200 |   100   |    800   |  1.600  |  3.200   |
| DW1500 |   100   |    800   |  1.600  |  3.200   |
| DW2000 |   100   |  1.600   |  3.200  |  6.400   |
| DW3000 |   100   |  1.600   |  3.200  |  6.400   |
| DW6000 |   100   |  3.200   |  6.400  |  12,800  |

Aus der obigen Tabelle können Sie sehen, dass eine Abfrage ausgeführt wird, klicken Sie auf eine DW2000 in der Klasse Xlargerc Ressource auf 6.400 MB Speicher innerhalb jeder der 60 verteilten Datenbanken zugreifen möchten.  In SQL Data Warehouse gibt es 60 Verteilung. Daher sollten zum Berechnen der Summe arbeitsspeicherzuteilung für eine Abfrage in einer angegebenen Ressourcenklasse die oben angegebenen Werte 60 multipliziert werden.

### <a name="memory-allocations-system-wide-gb"></a>Arbeitsspeicher Zuweisungen System organisationsweite (GB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |    6    |    6     |    12   |    23    |
| DW200  |    6    |    12    |    23   |    47    |
| DW300  |    6    |    12    |    23   |    47    |
| DW400  |    6    |    23    |    47   |    94    |
| DW500  |    6    |    23    |    47   |    94    |
| DW600  |    6    |    23    |    47   |    94    |
| DW1000 |    6    |    47    |    94   |   188    |
| DW1200 |    6    |    47    |    94   |   188    |
| DW1500 |    6    |    47    |    94   |   188    |
| DW2000 |    6    |    94    |   188   |   375    |
| DW3000 |    6    |    94    |   188   |   375    |
| DW6000 |    6    |   188    |   375   |   750    |

In dieser Tabelle der gesamte System Arbeitsspeicher Zuweisungen können Sie sehen, dass eine Abfrage ausgeführt wird, klicken Sie auf eine DW2000 in der Klasse Xlargerc Ressource, insgesamt 375 GB Arbeitsspeicher zugeordnet wird (6.400 MB * 60 Verteilung / 1.024 konvertieren in GB) über den gesamten Ihrer SQL Data Warehouse.

## <a name="concurrency-slot-consumption"></a>Parallelität Slot Verbrauch

SQL Data Warehouse gewährt mehr Speicherplatz zu Abfragen in höheren Ressourcenklassen ausführen. Speicher ist eine feste Ressource aus.  Deshalb können mehr Speicher pro Abfrage, die weniger gleichzeitigen Abfragen ausgeführt werden. In der folgenden Tabelle noch Mal alle vorherigen Konzepte in einer Einzelansicht, die die Anzahl der Parallelität DWU verfügbar und die von jeder Ressourcenklasse verbraucht anzeigt.

### <a name="allocation-and-consumption-of-concurrency-slots"></a>Zuteilung und die Verwendung von Parallelität Steckplätze

|  DWU   | Maximale Anzahl gleichzeitiger Abfragen  | Parallelität Umsetzungsplätzen | Steckplätze untersuchten smallrc |  Steckplätze untersuchten mediumrc |  Steckplätze untersuchten largerc |  Steckplätze untersuchten xlargerc |
| :----  | :---------------------: | :-------------------------: | :-----: | :------: | :-----: | :------: |
| DW100  |            4            |                4            |    1    |     1    |    2    |    4     |
| DW200  |            8            |                8            |    1    |     2    |    4    |    8     |
| DW300  |           12            |               12            |    1    |     2    |    4    |    8     |
| DW400  |           16            |               16            |    1    |     4    |    8    |   16     |
| DW500  |           20            |               20            |    1    |     4    |    8    |   16     |
| DW600  |           24            |               24            |    1    |     4    |    8    |   16     |
| DW1000 |           32            |               40            |    1    |     8    |   16    |   32     |
| DW1200 |           32            |               48            |    1    |     8    |   16    |   32     |
| DW1500 |           32            |               60            |    1    |     8    |   16    |   32     |
| DW2000 |           32            |               80            |    1    |    16    |   32    |   64     |
| DW3000 |           32            |              120            |    1    |    16    |   32    |   64     |
| DW6000 |           32            |              240            |    1    |    32    |   64    |  128     |


Aus dieser Tabelle können Sie die Ausführung von SQL Data Warehouse sehen, wie DW1000 maximal 32 gleichzeitige Abfragen und insgesamt 40 Parallelität Steckplätze zuweist. Wenn alle Benutzer in Smallrc ausgeführt werden, würde 32 gleichzeitige Abfragen gewährt werden, da jeder Abfrage 1 Parallelität Slot nutzen möchten. Wenn für alle Benutzer auf ein DW1000 in Mediumrc ausgeführt wurden, jede Abfrage 800 MB pro Verteilung für eine gesamte arbeitsspeicherzuteilung 47 GB pro Abfrage zugewiesen werden würde und Parallelität auf 5 Benutzer begrenzt wäre (40 Parallelität Steckplätze / 8 Steckplätze pro Mediumrc Benutzer).

## <a name="query-importance"></a>Abfrage Wichtigkeit

SQL Data Warehouse implementiert Ressourcenklassen mithilfe von Gruppen Arbeitsbelastung an. Es gibt insgesamt acht Arbeitsbelastung Gruppen, die das Verhalten der Ressourcenklassen über die verschiedenen DWU Größen steuern. Für alle DWU verwendet SQL Data Warehouse nur vier der acht Arbeitsbelastung Gruppen. Dies ist sinnvoll, da jede Gruppe Arbeitsbelastung auf eine der vier Ressourcenklassen zugeordnet ist: Smallrc, Mediumrc, Largerc, oder Xlargerc. Grundlegendes zu den Gruppen Arbeitsbelastung die Wichtigkeit ist, dass einige dieser Gruppen Arbeitsbelastung in höherer *Priorität*festgelegt sind. Wichtigkeit dient zum CPU planen. Abfragen mit hoher Wichtigkeit ausführen erhalten dreimal mehr CPU-Zyklen als die mit mittlerer Wichtigkeit. Daher ermitteln Parallelität Slot Zuordnungen auch CPU-Priorität. Wenn Sie eine Abfrage mindestens 16 Steckplätze belegt, wird er mit hoher Wichtigkeit ausgeführt.

Die folgende Tabelle zeigt die Wichtigkeit Zuordnungen für jede Gruppe Arbeitsbelastung an.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Arbeitsbelastung Gruppe Zuordnungen zu Parallelität Steckplätze und Wichtigkeit

| Arbeitsbelastung Gruppen | Parallelität Slot Zuordnung | MB / Verteilung | Wichtigkeit Zuordnung |
| :-------------- | :----------------------: | :---------------: | :----------------- |
| SloDWGroupC00   |            1             |         100       |       Mittel       |
| SloDWGroupC01   |            2             |         200       |       Mittel       |
| SloDWGroupC02   |            4             |         400       |       Mittel       |
| SloDWGroupC03   |            8             |         800       |       Mittel       |
| SloDWGroupC04   |           16             |       1.600       |       Hohe         |
| SloDWGroupC05   |           32             |       3.200       |       Hohe         |
| SloDWGroupC06   |           64             |       6.400       |       Hohe         |
| SloDWGroupC07   |          128             |      12,800       |       Hohe         |

Aus dem Diagramm **Zuteilung und die Verwendung von Parallelität Steckplätze** können Sie sehen, dass eine DW500 1, 4, 8 oder 16 Parallelität Steckplätze für Smallrc, Mediumrc, Largerc und Xlargerc, Hilfethemas verwendet. Sie können diese Werte im vorhergehenden Diagramm, um die Wichtigkeit für jede Ressourcenklasse suchen nachschlagen.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>DW500 Zuordnung der Ressourcenklassen, Wichtigkeit

| Ressourcenklasse | Arbeitsbelastung Gruppe | Parallelität Steckplätze verwendet | MB / Verteilung | Wichtigkeit: hoch |
| :------------- | :------------- | :--------------------: | :---------------: | :--------- |
| smallrc        | SloDWGroupC00  |           1            |         100       |   Mittel   |
| mediumrc       | SloDWGroupC02  |           4            |         400       |   Mittel   |
| largerc        | SloDWGroupC03  |           8            |         800       |   Mittel   |
| xlargerc       | SloDWGroupC04  |          16            |        1.600      |   Hohe     |


Die folgende Abfrage DMV können die Unterschiede Ressource arbeitsspeicherzuteilung ausführlich hinsichtlich der die Ressourcenkontrolle eigenständig oder aktiven und Archiv Ihrer Verwendung der Arbeitsbelastung Gruppen bei der Problembehandlung zu analysieren.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                      AS node_type
    ,pn.pdw_node_id                 AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024              AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                  AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                     AS group_max_dop
    ,wg.effective_max_dop               AS group_effective_max_dop
    ,wg.total_request_count             AS group_total_request_count
    ,wg.total_queued_request_count          AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON  wg.pdw_node_id  = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT  pool_name
,       pool_max_mem_MB
,       group_name
,       group_importance
,       (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,       node_name
,       node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,   group_request_max_memory_grant_pcnt
,   group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Abfragen, die Parallelitätsgrenzen berücksichtigen

Ressourcenklassen sind, die meisten Abfragen unterliegen. Diese Abfragen müssen in der sowohl die gleichzeitige Abfrage- und Parallelität Slot Schwellenwerte passen. Ein Benutzer kann nicht wählen Sie eine Abfrage aus der Slot Parallelitätsmodell ausschließen.

Zur Bestätigung berücksichtigt die folgenden Aussagen Ressourcenklassen:

- EINFÜGEN-AUSWÄHLEN
- AKTUALISIEREN
- LÖSCHEN
- Markieren Sie (wenn Benutzertabellen Abfragen)
- ALTER INDEX NEU ERSTELLEN
- ALTER INDEX NEU ORGANISIEREN
- ALTER TABELLE NEU ERSTELLEN
- INDEX ERSTELLEN
- GRUPPIERTE COLUMNSTORE INDEX ERSTELLEN
- ERSTELLEN SIE DIE TABELLE AS SELECT (CTAS)
- Das Laden von Daten
- Bewegung Datenoperationen durchgeführt, indem Sie die Daten Bewegung Service (DMS)

## <a name="query-exceptions-to-concurrency-limits"></a>Ausnahmen von der Abfrage Parallelitätsgrenzen

Einige Abfragen berücksichtigt nicht die Klasse, zu der der Benutzer zugeordnet ist. Diese Ausnahmen für die Parallelitätsgrenzen vorgenommen wurden, wenn die Arbeitsspeicherressourcen für einen bestimmten Befehl erforderlich niedrig, häufig sind, da der Befehl eines Vorgangs Metadaten ist. Das Ziel dieser Ausnahmen ist zur Vermeidung von größer Arbeitsspeicher Zuweisungen für Abfragen, die sie nie benötigen. In diesen Fällen wird die standardmäßige small Ressourcenklasse (Smallrc) immer unabhängig von der ist-Klasse dem Benutzer zugewiesenen verwendet. Beispielsweise `CREATE LOGIN` wird immer in Smallrc ausgeführt. Die Ressourcen erforderlich, um diesen Vorgang zu erfüllen eingesetzt werden sehr niedrig, damit es nicht, die Abfrage in der Slot Parallelitätsmodell aufnehmen sinnvoll ist.  Diese Abfragen werden auch keine Beschränkung der Beschränkung auf Parallelität 32-Benutzer, die eine unbegrenzte Anzahl von diese Abfragen nach oben bis zum Sitzungslimit 1.024 Sitzungen ausgeführt werden kann.

Die folgenden Aussagen Ressourcenklassen nicht beachtet werden:

- Erstellen oder Löschen der Tabelle
- ÄNDERN SIE DIE TABELLE... WECHSELN, PARTITION ZUSAMMENFÜHREN oder Teilen
- ALTER INDEX DEAKTIVIEREN
- INDEX LÖSCHEN
- Erstellen, aktualisieren oder DROP STATISTICS
- TABELLE VERKÜRZEN
- ALTER AUTORISIERUNG
- ERSTELLEN VON LOGIN
- Erstellen, ALTER oder DROP USER
- Erstellen, ALTER oder DROP PROCEDURE
- Erstellen oder DROP VIEW
- EINFÜGEN VON WERTEN
- Wählen Sie aus dem Systemansichten und DMVs
- ERLÄUTERN SIE
- DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="change-a-user-resource-class-example"></a>Ändern einer Ressource Beispiel für Benutzer

1. **Login erstellen:** Öffnen Sie eine Verbindung mit Ihrer **master** -Datenbank auf dem SQLServer hosten die Data Warehouse SQL-Datenbank, und führen Sie die folgenden Befehle.

    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```

    > [AZURE.NOTE] Es ist eine gute Idee, den einen Benutzer in der master-Datenbank für Benutzer Azure SQL-Data Warehouse zu erstellen. Erstellen eines Benutzers in das Master-Shape kann ein Benutzer mithilfe von Tools wie SSMS ohne Angabe eines Datenbanknamens anmelden.  Außerdem können sie Objekt-Explorer verwenden, um alle Datenbanken auf einem SQLServer anzuzeigen.  Weitere Details zum Erstellen und Verwalten von Benutzern finden Sie unter [Secure einer Datenbank in SQL Data Warehouse][].

2. **Erstellen SQL Data Warehouse Benutzer:** Öffnen Sie eine Verbindung mit **SQL Data Warehouse** -Datenbank, und führen Sie den folgenden Befehl aus.

    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```

3. **Erteilen von Berechtigungen:** Im folgenden Beispiel wird gewährt `CONTROL` auf **Die Data Warehouse SQL** -Datenbank. `CONTROL`Ebene ist der Datenbank vergleichbar mit Db_owner in SQL Server.

    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```

4. **Ressourcenklasse vergrößern:** Verwenden Sie die folgende Abfrage zum Hinzufügen eines Benutzers zu einer höheren Arbeitsbelastung Management Rolle aus.

    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```

5. **Verkleinern Ressourcenklasse:** Verwenden Sie die folgende Abfrage zum Entfernen eines Benutzers aus einer Arbeitsbelastung Management Rolle aus.

    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```

    > [AZURE.NOTE] Es ist nicht möglich, einen Benutzer aus Smallrc entfernen.

## <a name="queued-query-detection-and-other-dmvs"></a>In der Warteschlange Abfrage Erkennung und andere DMVs

Sie können die `sys.dm_pdw_exec_requests` DMV zum Identifizieren von Abfragen, die in einer Parallelität Warteschlange warten. Warten ein Slot Parallelität Abfragen haben Status **angehalten**.

```sql
SELECT   r.[request_id]              AS Request_ID
        ,r.[status]              AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]              AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Arbeitsbelastung Management Rollen können angezeigt werden, mit `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

Die folgende Abfrage zeigt, welche Rolle jedem Benutzer zugeordnet ist.

```sql
SELECT   r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id     = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id   = m.principal_id
WHERE   r.name IN ('mediumrc','largerc', 'xlargerc');
```

SQL Data Warehouse weist die folgenden Arten warten:

- **LocalQueriesConcurrencyResourceType**: Abfragen, die sich außerhalb der Slot Parallelitätsframework befinden. DMV Abfragen und System Funktionen wie `SELECT @@VERSION` sind Beispiele für lokale Abfragen.
- **UserConcurrencyResourceType**: Abfragen, die innerhalb der Slot Parallelitätsframework befinden. Abfragen für Endbenutzer Tabellen darstellen Beispiele, bei die dieser Ressourcentyp verwenden möchten.
- **DmsConcurrencyResourceType**: Bewegung Datenoperationen infolge wartet.
- **BackupConcurrencyResourceType**: Diese warten zeigt an, dass eine Datenbank gesichert wird. Der maximale Wert für diesen Ressourcentyp ist 1. Wenn Sie mehrere Sicherungskopien zur gleichen Zeit angefordert wurden, werden die anderen Warteschlange.

Die `sys.dm_pdw_waits` DMV kann verwendet werden, um die Ressourcen finden Sie unter eine Anforderung warten.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,   SESSION_ID()            AS Current_session
,   s.[status]          AS Session_status
,   s.[login_name]
,   s.[query_count]
,   s.[client_id]
,   s.[sql_spid]
,   r.[command]         AS Request_command
,   r.[label]
,   r.[status]          AS Request_status
,   r.[submit_time]
,   r.[start_time]
,   r.[end_compile_time]
,   r.[end_time]
,   DATEDIFF(ms,r.[submit_time],r.[start_time])     AS Request_queue_time_ms
,   DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,   DATEDIFF(ms,r.[end_compile_time],r.[end_time])      AS Request_execution_time_ms
,   r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE   w.[session_id] <> SESSION_ID();
```

Die `sys.dm_pdw_resource_waits` DMV zeigt nur die von einer bestimmten Abfrage verbraucht Ressourcen wartet. Ressource Wartezeit misst nur warten auf Ressourcen bereitgestellt werden im Gegensatz zu Signal Wartezeit, also die Zeit, die für die zugrunde liegenden SQL Server Planung die Abfrage auf die CPU benötigt wird.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE   [session_id] <> SESSION_ID();
```

Die `sys.dm_pdw_wait_stats` DMV für Archiv Ihrer Trendanalyse von Wartezeiten verwendet werden kann.

```sql
SELECT  w.[pdw_node_id]
,       w.[wait_name]
,       w.[max_wait_time]
,       w.[request_count]
,       w.[signal_time]
,       w.[completed_count]
,       w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwalten von Datenbankbenutzer und Sicherheit finden Sie unter [Secure einer Datenbank in SQL Data Warehouse][]. Weitere Informationen wie größere Ressourcen können Klassen gruppierte Columnstore Index Qualität verbessern, finden Sie unter [Rebuilding Indizes Segment Qualität zu verbessern].

<!--Image references-->

<!--Article references-->
[Sichern einer Datenbank in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Neuerstellen von Indizes zur Verbesserung der Qualität segment]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Sichern einer Datenbank in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
