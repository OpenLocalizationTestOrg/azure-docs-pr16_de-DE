<properties
    pageTitle="Erweiterte Ereignisse in SQL-Datenbank | Microsoft Azure"
    description="Beschreibt erweiterte Ereignisse (XEvents) in Azure SQL-Datenbank, und wie Ereignis Sitzungen geringfügig Ereignis Sitzungen in Microsoft SQL Server variieren."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""
    tags=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="genemi"/>


# <a name="extended-events-in-sql-database"></a>Erweiterte Ereignisse in SQL-Datenbank

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

In diesem Thema wird erläutert, wie die Implementierung von erweiterten Ereignisse in Azure SQL-Datenbank weicht ist im Vergleich zum erweiterten Ereignisse in Microsoft SQL Server.


- SQL-Datenbank V12 gewonnenen dem Feature "Erweiterte Ereignisse" in der zweiten Hälfte des Kalenders 2015.
- SQL Server verfügt über erweiterte Ereignisse seit 2008 haben.
- Festlegen der Features von erweiterten Ereignissen auf SQL-Datenbank ist eine umfangreiche Teilmenge der Features auf SQL Server.


*XEvents* ist eine informelle Spitzname, die für 'erweiterte Ereignisse' manchmal verwendet wird in Blogs und anderen Speicherorten informell gestaltet werden kann.


> [AZURE.NOTE] Ab Oktober 2015 ist das erweiterte Ereignis Sitzung-Feature in SQL Azure-Datenbank Ebene der Vorschau aktiviert. Das Datum der allgemeinen Verfügbarkeit (GA) ist noch nicht festgelegt.
>
> Die Seite Azure [Dienstupdates](https://azure.microsoft.com/updates/?service=sql-database) hat Beiträge an, wenn GA Ankündigungen vorgenommen werden.


Weitere Informationen zum erweiterten Ereignissen für Azure SQL-Datenbank und Microsoft SQL Server ist bei verfügbar:

- [Schnellstart: Erweiterte Ereignisse in SQL Server](http://msdn.microsoft.com/library/mt733217.aspx)
- [Erweiterte Ereignisse](http://msdn.microsoft.com/library/bb630282.aspx)


## <a name="prerequisites"></a>Erforderliche Komponenten


In diesem Thema wird davon ausgegangen, dass Ihnen bereits einige bekannt ist:


- [Azure SQL-Datenbank-Dienst](https://azure.microsoft.com/services/sql-database/).


- [Erweiterte Ereignisse](http://msdn.microsoft.com/library/bb630282.aspx) in Microsoft SQL Server.
 - Der Großteil unserer Dokumentation zur erweiterten Ereignissen gilt für SQL Server und SQL-Datenbank.


Vorherige anzeigen, um die folgenden Elemente ist hilfreich, wenn die Datei Ereignis als [Ziel](#AzureXEventsTargets)auswählen:


- [Azure-Speicherdienst](https://azure.microsoft.com/services/storage/)


- PowerShell
 - [Verwenden von Azure PowerShell mit Azure-Speicher](../storage/storage-powershell-guide-full.md) - bietet umfassende Informationen zu PowerShell und der Azure-Speicherdienst.


## <a name="code-samples"></a>Codebeispielen


Verwandte Themen zwei Codebeispielen:


- [Anrufen Sie Puffer Zielcode für erweiterte Ereignisse in SQL-Datenbank](sql-database-xevent-code-ring-buffer.md)
 - Kurze einfaches Transact-SQL-Skript.
 - Wir hervorheben im Thema Stichprobe Code werden, wenn Sie mit einem Ziel anrufen Puffer fertig sind, Sie ihre Ressourcen freigeben sollten, indem Sie Alter Drag & Drop ausführen `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` Anweisung. Sie können später Hinzufügen einer anderen Instanz von Anrufen Puffer durch `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.


- [Ereignis Datei Zielcode für erweiterte Ereignisse in SQL-Datenbank](sql-database-xevent-code-event-file.md)
 - Phase 1 ist PowerShell zum Erstellen eines Containers Azure-Speicher.
 - Phase 2 ist Transact-SQL, die den Container Azure-Speicher verwendet.


## <a name="transact-sql-differences"></a>Transact-SQL-Unterschiede


- Wenn Sie den Befehl [CREATE EVENT SITZUNG](http://msdn.microsoft.com/library/bb677289.aspx) in SQL Server ausführen, verwenden Sie die **Auf SERVER** -Klausel. Aber auf SQL-Datenbank Sie die **Datenbank auf** -Klausel stattdessen verwenden.


- Die **Datenbank auf** -Klausel gilt auch für das [ALTER EVENT SITZUNG](http://msdn.microsoft.com/library/bb630368.aspx) und [DROP EVENT SITZUNG](http://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL-Befehle.


- Es wird empfohlen, die Ereignis Sitzung Möglichkeit aufnehmen möchten **STARTUP_STATE = ON** in Ihrem **CREATE EVENT SITZUNG** oder **ALTER EVENT SESSION** -Anweisung.
 - Der Wert **= ON** unterstützt einen automatischen Neustart nach einer neu konfiguriert die logische Datenbank wegen eines Failovers.


## <a name="new-catalog-views"></a>Neue Katalogsansichten


Das Feature "Erweiterte Ereignisse" wird von mehreren [Katalogsansichten](http://msdn.microsoft.com/library/ms174365.aspx)unterstützt. Katalogsansichten informieren Sie über *Metadaten oder Definitionen* von Benutzern erstellte Ereignis Sitzungen in der aktuellen Datenbank. Informationen zu Instanzen von aktiven Ereignis Sitzungen zurück Ansichten keine.


| Name des<br/>Katalogansicht | Beschreibung |
| :-- | :-- |
| **Sys.database_event_session_actions** | Eine Zeile für jede Aktion zurückgegeben auf jede-Ereignis einer Veranstaltung. |
| **Sys.database_event_session_events** | Eine Zeile für jedes Ereignis zurückgegeben in einer Veranstaltung. |
| **Sys.database_event_session_fields** | Gibt eine Zeile für jede anpassen können Spalte, die auf Ereignisse und Ziele explizit festgelegt wurde. |
| **Sys.database_event_session_targets** | Eine Zeile für jedes Ereignisziel zurückgegeben für ein Ereignis Sitzung. |
| **Sys.database_event_sessions** | Gibt eine Zeile für jede Sitzung Ereignis in der Datenbank SQL-Datenbank. |


Microsoft SQL Server, ähnlich wie Katalogsansichten haben Namen, in denen *.server\_ * anstelle von *.database\_*. Der Name eines Musters ist wie **sys.server_event_%**.


## <a name="new-dynamic-management-views-dmvshttpmsdnmicrosoftcomlibraryms188754aspx"></a>Neue dynamische Management Ansichten [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx)


Azure SQL-Datenbank weist [dynamische Management Ansichten (DMVs)](http://msdn.microsoft.com/library/bb677293.aspx) , die erweiterte Ereignisse unterstützen. DMVs informieren Sie über *Sitzungen Ereignis* .


| Namen der DMV | Beschreibung |
| :-- | :-- |
| **Sys.dm_xe_database_session_event_actions** | Gibt Informationen zur Sitzung Aktionen des Ereignisses. |
| **Sys.dm_xe_database_session_events** | Gibt Informationen zur Sitzungsereignisse zurück. |
| **Sys.dm_xe_database_session_object_columns** | Zeigt die Konfigurationswerte für Objekte, die an eine Sitzung gebunden sind. |
| **Sys.dm_xe_database_session_targets** | Gibt Informationen zur Sitzung Ziele zurück. |
| **Sys.dm_xe_database_sessions** | Eine Zeile zurückgegeben für jedes Ereignis Sitzung, die ausgelegte ist mit der aktuellen Datenbank. |


Ähnliche Katalogsansichten sind benannte in Microsoft SQL Server, ohne die * \_Datenbank* Teil des Namens, z. B.:


- **sys.dm_xe_sessions**anstelle von Namen<br/>**sys.dm_xe_database_sessions**.


### <a name="dmvs-common-to-both"></a>DMVs beiden gemeinsam


Für erweiterte Ereignisse sind zusätzliche DMVs, die sowohl Azure SQL-Datenbank auf Microsoft SQL Server auf allgemeine sind:


- **Sys.dm_xe_map_values**
- **Sys.dm_xe_object_columns**
- **Sys.dm_xe_objects**
- **Sys.dm_xe_packages**



 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-the-available-extended-events-actions-and-targets"></a>Suchen der verfügbaren erweiterten Ereignisse, Aktionen und Ziele


Sie können eine einfache SQL- **Wählen Sie** eine Liste der verfügbaren Ereignisse, Aktionen und Ziel erhalten ausführen.


```
SELECT
        o.object_type,
        p.name         AS [package_name],
        o.name         AS [db_object_name],
        o.description  AS [db_obj_description]
    FROM
                   sys.dm_xe_objects  AS o
        INNER JOIN sys.dm_xe_packages AS p  ON p.guid = o.package_guid
    WHERE
        o.object_type in
            (
            'action',  'event',  'target'
            )
    ORDER BY
        o.object_type,
        p.name,
        o.name;
```



<a name="AzureXEventsTargets" id="AzureXEventsTargets"></a>

&nbsp;

## <a name="targets-for-your-sql-database-event-sessions"></a>Ziele für Ihre SQL-Datenbank Ereignis Sitzungen


Es folgen einige Ziele, die Ergebnisse von Ihrem Ereignis Sitzungen auf SQL-Datenbank aufnehmen können:


- [Anrufen Puffer Ziel](http://msdn.microsoft.com/library/ff878182.aspx) : kurz enthält Daten im Speicher ein.
- [Ereigniszähler Ziel](http://msdn.microsoft.com/library/ff878025.aspx) : zählt alle Ereignisse, die während einer Sitzung erweiterte Ereignisse auftreten.
- [Ereignisdatei Ziel](http://msdn.microsoft.com/library/ff878115.aspx) : schreibt abgeschlossen Puffer zu einem Container Azure-Speicher.


[Event Tracing for Windows (ETW)](http://msdn.microsoft.com/library/ms751538.aspx) API ist nicht verfügbar für erweiterte Ereignisse auf SQL-Datenbank.


## <a name="restrictions"></a>Einschränkungen


Es gibt ein paar Sicherheits-Unterschiede befitting der Cloud-Umgebung, die SQL-Datenbank:


- Erweiterte Ereignisse sind auf dem Isolationsmodell Single-Mandanten begründet. Eine Ereignis Sitzung in einer Datenbank kann keine Daten oder Ereignisse aus einer anderen Datenbank zugreifen.

- Sie können keine **CREATE EVENT SESSION** -Anweisung im Kontext der **master** -Datenbank ausgeben.


## <a name="permission-model"></a>Berechtigungsmodell


Sie müssen die **Kontrolle** über die Berechtigung der Datenbank eine **CREATE EVENT SESSION** -Anweisung aus. Der Datenbankbesitzer (Dbo) Stufe **steuern** .


### <a name="storage-container-authorizations"></a>Speicher Container Autorisierungs


Das SAS Token, die, das Sie für Ihre Azure-Speicher Container generieren, muss **Rwl** für Berechtigungen angeben. Der Wert **Rwl** bietet folgenden Berechtigungen:


- Lesen
- Schreiben
- Liste


## <a name="performance-considerations"></a>Leistungsaspekte


Es gibt Szenarien, stark mit erweiterten Ereignissen aktiver Arbeitsspeicher als für insgesamt System fehlerfrei ist gesammelt kann. Daher, das System Azure SQL-Datenbank dynamisch legt und passt Grenzwerte für die Größe des aktiven Speichers, die von einer Sitzung Ereignis kumuliert werden kann. Viele Faktoren wechseln in die dynamische Berechnung.


Wenn Sie eine Fehlermeldung, die besagt, dass ein maximaler Arbeitsspeicher erzwungen wurde erhalten, werden einige Maßnahmen, die Sie ergreifen können:


- Führen Sie weniger gleichzeitige Ereignis Sitzungen.


- Verringern Sie die Speichermenge, die Sie angeben können, klicken Sie auf durch Ihre **Erstellen** und **Ändern** der Anweisungen für Ereignis Sitzungen, die **MAX\_Arbeitsspeicher** Klausel.


### <a name="network-latency"></a>Netzwerkwartezeit


Das Ziel des **Ereignisses Datei** möglicherweise Netzwerkwartezeit oder Fehlern beim Beibehalten von Daten in Azure-Speicher Blobs erzielen. Andere Ereignisse in SQL-Datenbank möglicherweise verzögert werden, während sie warten, bis die Netzwerkkommunikation ausführen. Diese Verzögerung kann Ihre Arbeitsbelastung verlangsamen.

- Um diese Leistung Risiken vermeiden Sie Einstellung die Option **EVENT_RETENTION_MODE** **NO_EVENT_LOSS** in das Ereignis Sitzung Definitionen.


## <a name="related-links"></a>Links zu verwandten Themen


- [Mithilfe der Azure PowerShell mit Azure-Speicher](../storage/storage-powershell-guide-full.md).
- [Cmdlets Azure-Speicher](http://msdn.microsoft.com/library/dn806401.aspx)


- [Verwenden von Azure PowerShell mit Azure-Speicher](../storage/storage-powershell-guide-full.md) - bietet umfassende Informationen zu PowerShell und der Azure-Speicherdienst.
- [So verwenden Sie BLOB-Speicher von .NET](../storage/storage-dotnet-how-to-use-blobs.md)


- [Erstellen von Anmeldeinformationen (Transact-SQL)](http://msdn.microsoft.com/library/ms189522.aspx)
- [Erstellen der Veranstaltung (Transact-SQL)](http://msdn.microsoft.com/library/bb677289.aspx)


- [Jonathan Kehayias Blogbeiträge zu erweiterten Ereignissen in Microsoft SQL Server](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


Andere Themen Code Stichprobe erweiterte Ereignisse stehen unter den folgenden Links. Müssen prüfen Sie regelmäßig Beispiele, um festzustellen, ob die Stichprobe Microsoft SQL Server im Vergleich zu Azure SQL-Datenbank ausgerichtet. Dann können Sie entscheiden, ob kleinere Änderungen erforderlich sind, um das Beispiel auszuführen.


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
