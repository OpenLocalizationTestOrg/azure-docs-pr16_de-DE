<properties
   pageTitle="Grenzwerte für SQL Data Warehouse Kapazität | Microsoft Azure"
   description="Höchstwerte für Verbindungen, Datenbanken, Tabellen und Abfragen zur SQL Data Warehouse."
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
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="sql-data-warehouse-capacity-limits"></a>Grenzwerte für SQL Data Warehouse Kapazität

In den folgenden Tabellen enthalten die zulässige Höchstwerte für verschiedene Bestandteile des Azure SQL-Data Warehouse.


## <a name="workload-management"></a>Arbeitsbelastung management

| Kategorie            | Beschreibung                                       | Maximum            |
| :------------------ | :------------------------------------------------ | :----------------- |
| [Datawarehouse Einheiten (DWU)][]| Max DWU für ein einzelnes SQL Data Warehouse | 6000               |
| [Datawarehouse Einheiten (DWU)][]| Max DWU für einen einzelnen SQLServer         | Standardmäßig 6000<br/><br/> Standardmäßig weist jede SQLServer (z. B. myserver.database.windows.net) ein DTU Kontingent von 45.000 dem können bis zu 6000 DWU. Dieses Kontingent ist einfach Sicherheit beschränkt. Sie können Ihr Kontingent durch [Erstellen einer Support-Ticket][] und *Kontingent* als den Anforderungstyp auswählen erhöhen.  Zum Berechnen der DTU müssen, die Summe der 7.5 multiplizieren, die DWU erforderlich. Sie können Ihre DTU Stromverbrauch aus dem SQL Server-Blade im Portal anzeigen. Angehaltene und dauerhaften angehaltene Datenbanken zählen gegen das Kontingent DTU. |
| Datenbankverbindung | Gleichzeitige öffnen Sitzungen                          | 1024<br/><br/>Wir unterstützen maximal 1024 aktive Verbindungen, von die jede Anfragen in einer Datenbank SQL Data Warehouse gleichzeitig senden kann. Beachten Sie, dass bestehen Einschränkungen hinsichtlich der Anzahl der Abfragen, die tatsächlich gleichzeitig ausgeführt werden können. Wenn die Parallelität überschritten wird, geht die Anfrage in eine interne Warteschlange wartet er, verarbeitet werden sollen.|
| Datenbankverbindung | Maximaler Arbeitsspeicher für vorbereiteter Anweisungen            | 20 MB              |
| [Arbeitsbelastung management][] | Maximale Anzahl gleichzeitiger Abfragen                    | 32<br/><br/> Standardmäßig können SQL Data Warehouse maximal 32 gleichzeitige Abfragen und Warteschlangen verbleibende Abfragen ausführen.<br/><br/>Wenn Benutzer zugeordnet sind, auf eine höhere Ressourcenklasse oder SQL Data Warehouse mit niedriger DWU konfiguriert ist, kann die Parallelitätsebene verringern. Einige Abfragen, wie DMV Abfragen darf immer ausgeführt werden.|
| [Tempdb][]          | Maximale Größe von Tempdb                                | 399 GB pro DW100. Daher wird bei DWU1000 Tempdb 3,99 TB Größe entsprechend angepasst |


## <a name="database-objects"></a>Datenbankobjekte

| Kategorie          | Beschreibung                                  | Maximum            |
| :---------------- | :------------------------------------------- | :----------------- |
| Datenbank          | Max Größe                                     | 240 TB komprimiert auf dem Datenträger<br/><br/>Dieser Speicherplatz von Tempdb oder Log Raum unabhängig ist, und daher wird dieser Speicherplatz dauerhaften Tabellen ausschließlich.  Gruppierte Columnstore Komprimierung wird auf 5 X geschätzt.  Diese Komprimierung ermöglicht die Datenbank vergrößert ungefähr 1 PB, wenn alle Tabellen gruppierte Columnstore (der Standard-Table-Datentyp) werden kann.|
| Tabelle             | Max Größe                                     | 60 TB komprimiert auf dem Datenträger   |
| Tabelle             | Tabellen pro Datenbank                          | 2 Milliarden          |
| Tabelle             | Spalten pro Tabelle                            | 1024 Spalten       |
| Tabelle             | Bytes pro Spalte                             | Abhängig von der Spalte [Datentyp][].  Grenzwert ist 8000 für Zeichen Datentypen, 4000 für Nvarchar oder 2 GB für MAX-Datentypen.|
| Tabelle             | Bytes pro Zeile definierte Größe                  | 8060 Byte<br/><br/>Die Anzahl von Bytes pro Zeile wird auf die gleiche Weise berechnet, wie es für SQL Server bei Seite Komprimierung. SQL Data Warehouse unterstützt beispielsweise SQL Server Zeile Überlauf Speicher der **Spalten variabler Länge** außerhalb von Zeilen abgelegt werden kann. Wenn Zeilen variabler Länge außerhalb von Zeilen abgelegt werden, wird nur 24-Byte-Zeichen Stamm im Hauptfenster Datensatz gespeichert. Weitere Informationen finden Sie im MSDN-Artikel [Zeile-Überlauf Daten von mehr als 8 KB][] .|
| Tabelle             | Partitionen pro Tabelle                    | 15.000<br/><br/>Hohe Leistung, wir empfehlen Minimieren der Anzahl von Partitionen Sie benötigen während er sich weiterhin Ihre Bedürfnisse unterstützen. Zunehmender die Anzahl der Partitionen, den Verwaltungsaufwand für Datendefinitionssprache (Data Definition Language, DDL) und Data Manipulation Language (DML) Vorgänge wächst und bewirkt, dass langsamer.|
| Tabelle             | Zeichen pro Begrenzungslinie Partitionswert.| 4000 |
| Index             | Nicht gruppierte Indizes pro Tabelle.        | 999<br/><br/>Gilt nur für Rowstore Tabellen ein.|
| Index             | Gruppierte Indizes pro Tabelle.            | 1<br><br/>Gilt für sowohl Rowstore und Columnstore Tabellen.|
| Index             | Index Key Größe.                          | 900 Byte.<br/><br/>Gilt nur für Rowstore Indizes.<br/><br/>Indizes Varchar Spalten mit einer Größe von mehr als 900 Byte können erstellt werden, wenn die vorhandenen Daten in den Spalten 900 Byte nicht überschreitet, wenn der Index erstellt wird. Jedoch später einfügen oder tritt ein UPDATE Aktionen auf die Spalten, die dazu führen, die Gesamtgröße dass zu 900 Byte nicht überschreiten.|
| Index             | Key Spalten pro Index.                   | 16<br/><br/>Gilt nur für Rowstore Indizes. Gruppierte Columnstore Indizes werden alle Spalten enthält.|
| Statistik        | Die Größe der kombinierten Spaltenwerte.      | 900 Byte.         |
| Statistik        | Spalten pro Statistik-Objekt.           | 32                 |
| Statistik        | Statistik für Spalten pro Tabelle erstellt. | 30.000            |
| Gespeicherten Prozeduren | Maximale Schachtelungsebenen.               | 8                 |
| Ansicht              | Spalten pro Ansicht                         | 1.024             |


## <a name="loads"></a>Lädt

| Kategorie          | Beschreibung                                  | Maximum            |
| :---------------- | :------------------------------------------- | :----------------- |
| Polybase lädt    | Bytes pro Zeile                                | 32.768<br/><br/>Polybase lädt maximal beide Zeilen kleiner als 32K geladen und können nicht VARCHR(MAX), NVARCHAR(MAX) oder VARBINARY(MAX) geladen.  Während dieser Grenzwert heute vorhanden ist, es entfernt werden relativ bald.<br/><br/>


## <a name="queries"></a>Abfragen

| Kategorie          | Beschreibung                                  | Maximum            |
| :---------------- | :------------------------------------------- | :----------------- |
| Abfrage             | In der Warteschlange Abfragen Benutzer Tabellen.               | 1000               |
| Abfrage             | Gleichzeitige Abfragen auf Systemansichten.          | 100                |
| Abfrage             | Klicken Sie auf Systemansichten in der Warteschlange Abfragen               | 1000               |
| Abfrage             | Maximale Parameter                           | 2098               |
| Stapelverarbeitung             | Maximale Größe                                 | 65, 536 * 4096        |
| SELECT-Ergebnisse    | Spalten pro Zeile                              | 4096<br/><br/>Sie können mehr als 4096 Spalten pro Zeile in der SELECT-Ergebnis haben. Es gibt keine Garantie, dass Sie immer 4096 haben können. Wenn der Abfrage-Plan eine temporäre Tabelle erforderlich ist, können die 1024 Spalten pro Tabelle maximale anfallen.|
| WÄHLEN SIE AUS            | Verschachtelte Unterabfragen                            | 32<br/><br/>Sie können mehr als 32 verschachtelte Unterabfragen in einer SELECT-Anweisung haben. Es gibt keine Garantie, dass Sie immer 32 enthalten können. Beispielsweise können Sie eine Verknüpfung Unterabfragen in den Abfrageplan einbringen. Die Anzahl der Unterabfragen kann auch durch verfügbaren Arbeitsspeicher beschränkt.|
| WÄHLEN SIE AUS            | Spalten pro Verknüpfung                             | 1024 Spalten<br/><br/>Sie können die Verknüpfung nie mehr als 1024 Spalten haben. Es gibt keine Garantie, dass Sie immer 1024 haben können. Wenn der Verknüpfung Plan eine temporäre Tabelle mit mehr Spalten als das Ergebnis der Verknüpfung erforderlich ist, gilt die Beschränkung 1024 die temporäre Tabelle. |
| WÄHLEN SIE AUS            | Bytes pro GROUP BY-Spalten.                  | 8060<br/><br/>Die Spalten in die GROUP BY-Klausel können maximal 8.060 haben.|
| WÄHLEN SIE AUS            | Bytes pro ORDER BY-Spalten                   | 8060 Byte.<br/><br/>Die Spalten in der ORDER BY-Klausel können maximal 8.060 haben.|
| Bezeichner und Konstanten pro-Anweisung | Anzahl der referenzierte Bezeichnern und Konstanten. | 65.535<br/><br/>SQL Data Warehouse schränkt die Anzahl der Bezeichner und Konstanten, die in einem Ausdruck einer Abfrage enthalten sein können. Dieser Grenzwert ist 65.535. Überschreiten Sie SQL Server-Fehler 8632 dieser Zahl ergibt. Weitere Informationen finden Sie unter [Interner Fehler: ein Ausdruck Services wurde erreicht][].|


## <a name="metadata"></a>Metadaten

| Ansicht "System"                        | Maximale Anzahl Zeilen |
| :--------------------------------- | :------------|
| Sys.dm_pdw_component_health_alerts | 10.000       |
| Sys.dm_pdw_dms_cores               | 100          |
| Sys.dm_pdw_dms_workers             | Gesamtzahl der DMS Worker für die letzte 1000 fordert SQL. |
| Sys.dm_pdw_errors                  | 10.000       |
| Sys.dm_pdw_exec_requests           | 10.000       |
| Sys.dm_pdw_exec_sessions           | 10.000       |
| Sys.dm_pdw_request_steps           | Die Gesamtzahl der Schritte für die letzten 1000 SQL-Anfragen, die in sys.dm_pdw_exec_requests gespeichert sind. |
| Sys.dm_pdw_os_event_logs           | 10.000       |
| Sys.dm_pdw_sql_requests            | Die neuesten 1000 SQL-Anfragen, die in sys.dm_pdw_exec_requests gespeichert sind. |


## <a name="next-steps"></a>Nächste Schritte
Weitere Referenzinformationen finden Sie unter [SQL Data Warehouse-Referenz (Übersicht)][].

<!--Image references-->

<!--Article references-->
[Datawarehouse Einheiten (DWU)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Die Data Warehouse SQL-Referenz (Übersicht)]: ./sql-data-warehouse-overview-reference.md
[Arbeitsbelastung management]: ./sql-data-warehouse-develop-concurrency.md
[Tempdb]: ./sql-data-warehouse-tables-temporary.md
[Datentyp]: ./sql-data-warehouse-tables-data-types.md
[Erstellen einer Support-ticket]: /sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->
[Zeile-Überlauf Daten über 8 KB]: https://msdn.microsoft.com/library/ms186981.aspx
[Interner Fehler: ein Ausdruck Services wurde erreicht]: https://support.microsoft.com/kb/913050
