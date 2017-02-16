<properties
    pageTitle="Fehlercodes für SQL - Verbindung Datenbankfehler | Microsoft Azure"
    description="Informationen Sie zu SQL-Fehlercodes für SQL-Datenbank in Clientanwendungen, z. B. häufigen Datenbank Verbindung, Probleme beim Kopieren der Datenbank und allgemeine Fehler. "
    keywords="SQL-Fehlercode, Access Sql, Verbindung Datenbankfehler, Sql-Fehlercodes"
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="annemill"/>


# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-error-and-other-issues"></a>SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Verbindungsfehler und andere Probleme-Datenbank


<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


In diesem Artikel sind SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen, einschließlich Verbindung Datenbankfehler, vorübergehende Fehler (auch als vorübergehenden Fehler bezeichnet), Ressourcen Governance Fehler, Probleme beim Kopieren der Datenbank, flexible Ressourcenpool und andere Fehler aufgeführt. Die meisten Kategorien sind bestimmte mit Azure SQL-Datenbank, und auf Microsoft SQL Server nicht angewendet.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Verbindung Datenbankfehler, vorübergehende Fehler und andere temporäre Fehler

In der folgenden Tabelle werden die SQL-Fehlercodes für Verbindung Verlust Fehler und andere vorübergehende Fehler, die möglicherweise auftreten, wenn eine Anwendung versucht, den Zugriff auf SQL-Datenbank behandelt. Erste Schritte-Lernprogramme zum zu Azure SQL-Datenbank herstellen, finden Sie unter [Verbinden mit Azure SQL-Datenbank](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Am häufigsten verwendeten Datenbank Verbindung oder vorübergehende Fehlerstrukturanalyse-Fehlern

Die Azure-Infrastruktur ist in der Lage, Servern dynamisch neu zu konfigurieren, bei hoher Auslastung in der SQL-Datenbank-Dienst auftreten.  Dieses Verhalten dynamische möglicherweise Ihr Clientprogramm, dessen Verbindung mit SQL-Datenbank zu verlieren. Diese Art des Fehlers ist eine *vorübergehende Fehlerstrukturanalyse*bezeichnet.

Wenn Ihr Clientprogramm Logik Wiederholungsversuche aufweist, können sie versuchen Sie, eine Verbindung nach der vorübergehenden Fehlerstrukturanalyse-Time to alleine behoben zugewiesen wiederherzustellen.  Es empfiehlt sich, dass Sie für 5 Sekunden, bevor Sie Ihre erste "Wiederholen verzögern". Wiederholen nach einer kurzen Verzögerung kürzer als 5 Sekunden Risiken überfluten Cloud-Dienst. Bei jeder nachfolgenden Wiederholung die Verzögerung sollte wesentlich, wächst bis zu einem Maximum von 60 Sekunden.

Vorübergehende Fehler darstellen in der Regel als eine der folgenden Fehlermeldungen aus Ihrer Clientprogramme:

- Datenbank < Db_name > auf dem Server < Azure_instance > ist zurzeit nicht verfügbar. Wiederholen Sie die Verbindung zu einem späteren Zeitpunkt ein. Wenn das Problem weiterhin besteht, wenden Sie sich an Kundensupport und ermöglichen sie die Sitzung Tracing-ID < Nummer >

- Datenbank < Db_name > auf dem Server < Azure_instance > ist zurzeit nicht verfügbar. Wiederholen Sie die Verbindung zu einem späteren Zeitpunkt ein. Wenn das Problem weiterhin besteht, wenden Sie sich an Kundensupport, und teilen Sie sie die Sitzung Tracing-ID < Nummer >. (Microsoft SQL Server, Fehler: 40613)

- Eine vorhandene Verbindung wurde vom entfernten Host geschlossen.

- System.Data.Entity.Core.EntityCommandExecutionException: Fehler beim Ausführen der Befehlsdefinition. Details finden Sie die innere Ausnahme. ---> System.Data.SqlClient.SqlException: ein Transport Ebene Fehler ist aufgetreten, wenn die Ergebnisse vom Server empfangen. (Anbieter: Sitzungsanbieter, Fehler: 19 - physische Verbindung kann nicht verwendet werden)

Codebeispiele Logik Wiederholungsversuche finden Sie unter:

- [Datenverbindungsbibliotheken für SQL-Datenbank und SQLServer](sql-database-libraries.md) 
- [Beheben von Verbindungsfehlern bei der und vorübergehende Fehler in SQL-Datenbank Aktionen](sql-database-connectivity-issues.md)

Eine Beschreibung des *Zeitraums blockieren* für Clients, die ADO.NET verwenden ist in [SQL Server Verbindung Verbindungspooling (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)verfügbar.

### <a name="transient-fault-error-codes"></a>Vorübergehende Fehlerstrukturanalyse-Fehlercodes

Die folgenden Fehler sind vorübergehend, und in der Anwendungslogik wiederholt werden soll 

| Fehlercode | Schwere | Beschreibung |
| ---: | ---: | :--- |
| 4060 | 16 | Datenbank kann nicht geöffnet werden "% & #x2a; ls" von der Anmeldung angeforderte. Fehler bei der Anmeldung. |
|40197|17|Der Dienst ist einen Fehler bei der Bearbeitung Ihrer Anfrage aufgetreten. Versuchen Sie es erneut. Fehlercode %d.<br/><br/>Sobald der Dienst aufgrund von Software oder Hardware-Upgrades, Hardware-Fehlern oder andere Probleme Failover ausgefallen ist, erhalten Sie folgende Fehler angezeigt. Die Meldung Fehler 40197 eingebettet Fehlercode: (%d) enthält weitere Informationen zu den Typ des Fehlers oder Failover, die aufgetreten sind. Einige Beispiele für des Fehlers, den Codes in die Meldung Fehler 40197 eingebettet werden, sind 40020, 40143, 40166 und 40540.<br/><br/>Wiederherstellen der Verbindung mit der SQL-Datenbankserver verbinden Sie automatisch eine fehlerfrei Kopie der Datenbank. Die Anwendung 40197, Fehlerprotokoll eingebettete Fehlercode: (%d) in der Nachricht zur Behandlung dieses Problems abzufangen, und erneut eine Verbindung mit SQL-Datenbank herstellen, bis die Ressourcen verfügbar sind, und die Verbindung wird erneut hergestellt.|
|40501|20|Der Dienst ist ausgelastet. Wiederholen Sie die Anforderung nach 10 Sekunden ein. Vorfall ID: %ls. Code: %d.<br/><br/>*Hinweis:* Weitere Informationen finden Sie unter:<br/>• [Azure SQL-Datenbank Ressource Grenzwerte](sql-database-resource-limits.md).
|40613|17|Datenbank '%. & #x2a; ls' auf dem Server '% & #x2a; ls' ist zurzeit nicht verfügbar. Wiederholen Sie die Verbindung zu einem späteren Zeitpunkt ein. Wenn das Problem weiterhin besteht, wenden Sie sich an Kundensupport und ermöglichen sie die Sitzung Tracing-ID des '% & #x2a; ls'.|
|49918|16|Anforderung nicht verarbeiten. Nicht genügend Ressourcen zum Verarbeiten anfordern.<br/><br/>Der Dienst ist ausgelastet. Wiederholen Sie die Anforderung später. |
|49919|16|Nicht Prozess erstellen oder aktualisieren die Anfrage. Zu viele erstellen oder aktualisieren Sie Vorgänge für Abonnements "% ld".<br/><br/>Der Dienst ist ausgelastet Verarbeitung mehrerer erstellen oder Aktualisieren von Besprechungsanfragen für Ihr Abonnement oder Server. Anfragen werden derzeit für Ressourcen Optimierung blockiert. Abfrage [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) für anstehende Vorgänge an. Warten Sie bis ausstehend erstellen oder Aktualisierungsabfragen abgeschlossen sind oder Löschen eines ausstehenden Anfragen und wiederholen Sie die Anforderung später. |
|49920|16|Anforderung nicht verarbeiten. Zu viele Vorgänge für Abonnements "% ld".<br/><br/>Der Dienst ist ausgelastet Verarbeitung mehrere Anforderungen für dieses Abonnement. Anfragen werden derzeit für Ressourcen Optimierung blockiert. Abfrage [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) Antrag auf Vorgang an. Warten, bis alle ausstehenden Anfragen abgeschlossen sind oder löschen Sie eine der ausstehenden Anfragen und wiederholen Sie die Anforderung später. |

## <a name="database-copy-errors"></a>Kopieren von Datenbankfehler

Die folgenden Fehler können beim Kopieren von einer Datenbank in Azure SQL-Datenbank gefunden werden. Weitere Informationen finden Sie unter [Kopieren einer SQL Azure-Datenbank](sql-database-copy.md).


|Fehlercode|Schwere|Beschreibung|
|---:|---:|:---|
|40635|16|Client mit IP-Adresse '% & #x2a; ls' ist vorübergehend deaktiviert.|
|40637|16|Erstellen Sie Datenbankkopie ist derzeit deaktiviert.|
|40561|16|Fehler beim Kopieren der Datenbank. Die Quell-oder Zielliste ist nicht vorhanden.|
|40562|16|Fehler beim Kopieren der Datenbank. Die Quelldatenbank wurde gelöscht.|
|40563|16|Fehler beim Kopieren der Datenbank. Die Zieldatenbank wurde gelöscht.|
|40564|16|Datenbankkopie ist aufgrund eines internen Fehlers. Legen Sie die Zieldatenbank, und versuchen Sie es erneut.|
|40565|16|Fehler beim Kopieren der Datenbank. Es ist nicht mehr als 1 gleichzeitige Datenbank kopieren aus derselben Quelle zulässig. Legen Sie die Zieldatenbank, und versuchen Sie es später erneut.|
|40566|16|Datenbankkopie ist aufgrund eines internen Fehlers. Legen Sie die Zieldatenbank, und versuchen Sie es erneut.|
|40567|16|Datenbankkopie ist aufgrund eines internen Fehlers. Legen Sie die Zieldatenbank, und versuchen Sie es erneut.|
|40568|16|Fehler beim Kopieren der Datenbank. Quelldatenbank ist nicht mehr verfügbar sind. Legen Sie die Zieldatenbank, und versuchen Sie es erneut.|
|40569|16|Fehler beim Kopieren der Datenbank. Zieldatenbank weist nicht mehr verfügbar sind. Legen Sie die Zieldatenbank, und versuchen Sie es erneut.|
|40570|16|Datenbankkopie ist aufgrund eines internen Fehlers. Legen Sie die Zieldatenbank, und versuchen Sie es später erneut.|
|40571|16|Datenbankkopie ist aufgrund eines internen Fehlers. Legen Sie die Zieldatenbank, und versuchen Sie es später erneut.|

## <a name="resource-governance-errors"></a>Fehler bei der Ressource governance

Die folgenden Fehler werden durch übermäßige Verwendung von Ressourcen während der Arbeit mit Azure SQL-Datenbank verursacht. Beispiel:

- Eine Transaktion wurde zu lange geöffnet.
- Eine Transaktion ist zu viele Sperren enthalten.
- Eine Anwendung ist zu viel Speicher in Anspruch.
- Eine Anwendung ist zu viel Verarbeitung `TempDb` Leerzeichen.

Verwandte Themen:

* Ausführlichere Informationen finden Sie hier: [Azure SQL-Datenbank Ressource Beschränkungen](sql-database-resource-limits.md)

|Fehlercode|Schwere|Beschreibung|
|---:|---:|:---|
|10928|20|Ressourcen-ID: %d. Der %s-Grenzwert für die Datenbank beträgt %d und erreicht wurde. Weitere Informationen finden Sie unter [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637).<br/><br/>Enthält die Kennnummer, die die Ressource, die die erreicht hat. Für Arbeitsthreads, die Ressourcen-ID = 1. Für Sitzungen, die Ressourcen-ID = 2.<br/><br/>*Hinweis:* Weitere Informationen zu diesen Fehler und wie Sie es beheben finden Sie unter:<br/>• [Azure SQL-Datenbank Ressource Grenzwerte](sql-database-resource-limits.md). |
|10929|20|Ressourcen-ID: %d. Die minimale %s-Garantie ist %d, Obergrenze beträgt %d und die aktuelle Verwendung für die Datenbank ist %d. Der Server ist jedoch derzeit ausgelastet und kann Anfragen größer als %d für diese Datenbank zu unterstützen. Weitere Informationen finden Sie unter [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637). Andernfalls versuchen Sie es später erneut.<br/><br/>Enthält die Kennnummer, die die Ressource, die die erreicht hat. Für Arbeitsthreads, die Ressourcen-ID = 1. Für Sitzungen, die Ressourcen-ID = 2.<br/><br/>*Hinweis:* Weitere Informationen zu diesen Fehler und wie Sie es beheben finden Sie unter:<br/>• [Azure SQL-Datenbank Ressource Grenzwerte](sql-database-resource-limits.md).|
|40544|20|Die Datenbank hat seine Größenkontingent erreicht. Partition oder Löschen von Daten, Indizes löschen oder finden Sie in der Dokumentation mögliche Lösungen.|
|40549|16|Sitzung wird beendet, da Sie eine Transaktion langer haben. Versuchen Sie, Ihre Transaktion verkürzen.|
|40550|16|Die Sitzung wurde beendet, da es zu viele Sperren eingerichtet wurde. Versuchen Sie Lese- oder weniger Zeilen in einer einzigen Transaktion ändern.|
|40551|16|Die Sitzung wurde aufgrund übermäßiger beendet `TEMPDB` Verwendung. Versuchen Sie, ändern Ihre Abfrage, um die Verwendung von temporären Tabellen Speicherplatz zu verringern.<br/><br/>*Tipp:* Wenn Sie temporäre Objekte verwendet werden, zu sparen Speicherplatz in der `TEMPDB` Datenbank durch temporäre Objekte ablegen, nachdem sie von der Sitzung nicht mehr benötigt werden.|
|40552|16|Aufgrund der Verwendung von übermäßige Transaktion Log Speicherplatz wurde die Sitzung beendet. Versuchen Sie es weniger Zeilen in einer einzigen Transaktion ändern.<br/><br/>*Tipp:* Wenn Sie ausführen Massen fügt mithilfe der `bcp.exe` Programm oder die `System.Data.SqlClient.SqlBulkCopy` Klasse, versuchen Sie es mit den `-b batchsize` oder `BatchSize` Optionen zum Einschränken der Anzahl von Zeilen, die auf dem Server in jede Transaktion kopiert haben. Wenn Sie einen Index mit neu aufbauen der `ALTER INDEX` Anweisung, versuchen Sie es mit der `REBUILD WITH ONLINE = ON` Option.|
|40553|16|Da eine übermäßige arbeitsspeicherauslastung wurde die Sitzung beendet. Versuchen Sie, Ihre Abfrage zum Verarbeiten von weniger Zeilen zu ändern.<br/><br/>*Tipp:* Verringern die Anzahl der `ORDER BY` und `GROUP BY` Operationen in Transact-SQL-Code benötigt weniger Arbeitsspeicher der Abfrage.|

## <a name="elastic-pool-errors"></a>Flexible Pool-Fehlern

Die folgenden Fehler beziehen sich auf das Erstellen und Verwenden von Elastics Pools.

| Fehlernummer | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1132 | EX_RESOURCE | Die flexible Ressourcenpool hat Speichergrenzwert erreicht. Die Speicherplatzverwendung für den flexible Pool darf (%d) MB nicht überschreiten. | Flexible Ressourcenpool Grenzwert für Speicherplatz in MB. | Beim Versuch, Schreiben von Daten in einer Datenbank, wenn die Speichergrenze die flexible Ressourcenpool erreicht wurde. | Bitte sollten Sie die DTUs die flexible Ressourcenpool falls möglich um Speichergrenzwert erhöhen, indem Sie die Speicherung einzelner Datenbanken innerhalb der flexible Ressourcenpool untersuchten verringern erhöhen oder entfernen Sie Datenbanken aus dem Pool flexible. |
| 10929 | EX_USER | Die minimale %s-Garantie ist %d, Obergrenze beträgt %d und die aktuelle Verwendung für die Datenbank ist %d. Der Server ist jedoch derzeit ausgelastet und kann Anfragen größer als %d für diese Datenbank zu unterstützen. Finden Sie unter [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) , um Unterstützung zu erhalten. Andernfalls versuchen Sie es später erneut. | DTU min pro Datenbank; DTU Max pro Datenbank | Die Gesamtzahl der gleichzeitigen Kollegen (Anfragen) in allen Datenbanken im flexible Pool versucht, den Ressourcenpool Grenzwert überschreiten. | Bitte sollten Sie die DTUs möglich, wenn die flexible Ressourcenpool akzeptieren, um ihre Arbeitskollegen maximale vergrößern erhöhen oder entfernen Sie Datenbanken aus dem Pool flexible. |
| 40844 | EX_USER | Datenbank '%ls' auf dem Server "%ls" ist eine "%ls" Edition-Datenbank in eine flexible Ressourcenpool und kann keine Geschäftsbeziehung fortlaufender kopieren. | Datenbankname, Datenbankedition, -Servernamens | Eine StartDatabaseCopy für ein nicht-Premium Db in einem flexible Pool ausgegeben wird. | Demnächst |
| 40857 | EX_USER | Flexible Ressourcenpool für Server wurde nicht gefunden: "%ls", flexible Ressourcenpool Namen: "%ls". | Name des Servers; Flexible Ressourcenpool Namen | Angegebene flexible Pool ist auf dem angegebenen Server nicht vorhanden. | Geben Sie einen gültigen flexible Pool-Namen ein. |
| 40858 | EX_USER | Flexible Ressourcenpool '%ls' auf dem Server bereits vorhanden: '%ls' | Servernamens flexible Ressourcenpool Namen | Angegebenen flexible Ressourcenpool bereits in der angegebenen logischen Server. | Geben Sie neue flexible Ressourcenpool an. |
| 40859 | EX_USER | Flexible Ressourcenpool unterstützt Dienstebene '%ls' nicht. | Flexible Ressourcenpool Dienstebene | Ebene angegebenen Dienst wird für die Bereitstellung von flexible Ressourcenpool nicht unterstützt. | Geben Sie die richtige Edition oder nichts Dienstebene die Ebene der Standard-Dienst verwenden. |
| 40860 | EX_USER | Flexible Ressourcenpool '%ls' und Dienst Ziel '%ls' Kombination ist ungültig. | Flexible Ressourcenpool Name; Dienst Ebene Ziel Namen | Flexible Ressourcenpool und Dienst Ziel kann zusammen angegeben werden, wenn Dienst Ziel als 'ElasticPool' angegeben ist. | Geben Sie die richtige Kombination von flexible Ressourcenpool und Dienst Ziel. |
| 40861 | EX_USER | Die Datenbankedition ' %. *! können nicht anders als die flexible Ressourcenpool Dienstebene also sein ' %.* !-Tabelle. | Datenbankedition, flexible Ressourcenpool Dienstebene | Die Datenbankedition unterscheidet sich von der flexible Ressourcenpool Dienstebene zur Verfügung. | Geben Sie eine Datenbankedition anders als die flexible Ressourcenpool Dienstebene also nicht an.  Beachten Sie, dass die Datenbankedition nicht angegeben werden muss. |
| 40862 | EX_USER | Flexible Ressourcenpool Name muss flexible Ressourcenpool Dienst Ziel angegeben werden. | Keine | Flexible Ressourcenpool Dienst Ziel bestimmt eine flexible Ressourcenpool nicht eindeutig. | Geben Sie die flexible Ressourcenpool an, wenn das Ziel des flexible Ressourcenpool Dienst verwenden. |
| 40864 | EX_USER | Die DTUs für den flexible Pool muss mindestens (%d) DTUs für Dienstebene ' % *!-Tabelle. | DTUs für flexible Ressourcenpool; Flexible Ressourcenpool Dienstebene. | Versuch der DTUs für den flexible Pool unter die untere Grenze festzulegen. | Wiederholen Sie die Einstellung der DTUs, die für die elastisch pool zu mindestens die Untergrenze. |
| 40865 | EX_USER | Die DTUs für den flexible Pool nicht überschreiten (%d) DTUs für Dienstebene ' % *!-Tabelle. | DTUs für flexible Ressourcenpool; Flexible Ressourcenpool Dienstebene. | Versuch der DTUs für den flexible Pool über das maximale Limit festzulegen. | Wiederholen Sie die Einstellung der DTUs für den flexible Pool nicht größer als die Obergrenze an. |
| 40867 | EX_USER | Der max pro Datenbank DTU muss bei mindestens (%d) für den Dienstebene ' % *!-Tabelle. | DTU Max pro Datenbank; Flexible Ressourcenpool Dienstebene | Versuch der Max DTU pro Datenbank unter den unterstützten Grenzwert festzulegen. | Erwägen Sie die flexible Ressourcenpool Dienstebene, die die gewünschte Einstellung unterstützt. |
| 40868 | EX_USER | Der max pro Datenbank DTU darf nicht überschreiten, (%d) für den Dienstebene ' % *!-Tabelle. | DTU Max pro Datenbank; Flexible Ressourcenpool Dienstebene. | Versuch der Max DTU pro Datenbank die unterstützten Beschränkung festzulegen. | Erwägen Sie die flexible Ressourcenpool Dienstebene, die die gewünschte Einstellung unterstützt. |
| 40870 | EX_USER | Die min DTU pro Datenbank darf nicht überschreiten, (%d) für den Dienstebene ' % *!-Tabelle. | DTU min pro Datenbank; Flexible Ressourcenpool Dienstebene. | Versuch der DTU min pro Datenbank die unterstützten Beschränkung festzulegen. | Erwägen Sie die flexible Ressourcenpool Dienstebene, die die gewünschte Einstellung unterstützt. |
| 40873 | EX_USER | Die Anzahl der Datenbanken (%d) und DTU min pro Datenbank (%d) darf der DTUs die flexible Ressourcenpool (%d) nicht überschreiten. | Anzahl der Datenbanken in flexible Ressourcenpool; DTU min pro Datenbank; DTUs flexible Ressourcenpool. | Beim Versuch, DTU min für Datenbanken im flexible Pool angeben, die die DTUs der flexible Ressourcenpool überschreitet. | Sollten Sie die DTUs der flexible Ressourcenpool erhöhen oder Verringern der min DTU pro Datenbank oder Verringern der Anzahl der Datenbanken im flexible Pool. |
| 40877 | EX_USER | Eine flexible Ressourcenpool kann nicht gelöscht werden, es sei denn, sie keine Datenbanken enthält. | Keine | Flexible Pool enthält eine oder mehrere Datenbanken und können daher nicht gelöscht werden. | Entfernen Sie Datenbanken aus dem Pool flexible, um sie zu löschen. |
| 40881 | EX_USER | Flexible Pool ' % *!-Tabelle hat das Datenbank zählen überschritten hat.  Der Datenbank zählen Grenzwert für den flexible Pool darf (%d) für eine flexible Ressourcenpool mit (%d) DTUs nicht überschreiten. | Name des flexible Ressourcenpool; Flexible Ressourcenpool Datenbank zählen maximal; e DTUs für Ressourcenpool. | Versucht, erstellen oder Hinzufügen von Datenbank zum flexible Ressourcenpool, wenn die Datenbank zählen maximal flexible Pool erreicht wurde. | Bitte sollten Sie die DTUs der flexible Ressourcenpool falls möglich erhöhen, um seine maximale Datenbankgröße erhöhen oder entfernen Sie Datenbanken aus dem Pool flexible. |
| 40889 | EX_USER | Die DTUs oder Grenzwert für Speicherplatz für den flexible Pool ' % *!-Tabelle können nicht verringert werden, da, die nicht genügend Speicherplatz für die Datenbanken bereitstellen möchten. | Name des flexible Ressourcenpool. | Beim Versuch, die Speichergrenze die flexible Ressourcenpool unter seine Speicherplatzverwendung zu verkleinern. | Verringern Sie die speichernutzung der einzelnen Datenbanken im flexible Pool, oder entfernen Sie Datenbanken aus dem Pool, um deren DTUs oder Grenzwert für Speicherplatz zu verringern. |
| 40891 | EX_USER | Die min DTU pro Datenbank (%d) kann die DTU Max pro Datenbank (%d) nicht überschreiten. | DTU min pro Datenbank; DTU max pro Datenbank. | Beim Versuch, die min DTU pro Datenbank höher ist als der Max DTU pro Datenbank festlegen. | Stellen Sie sicher, dass die min DTU pro Datenbanken der Max DTU pro Datenbank nicht überschreiten. |
| TBD | EX_USER | Die Größe des Speichers für eine einzelne Datenbank in einem flexible Pool kann nicht überschreiten die zulässige maximale Größe ' % *!-Tabelle service flexible Ressourcenpool Ebene. | Flexible Ressourcenpool Dienstebene | Die maximale Größe für die Datenbank überschreitet die maximale Größe durch die flexible Ressourcenpool Dienstebene. | Legen Sie die maximale Größe der Datenbank im Rahmen der die maximale Größe, die durch die flexible Ressourcenpool Dienstebene zulässig. |

Verwandte Themen:

* [Erstellen einer Datenbank flexible Ressourcenpool (c#)](sql-database-elastic-pool-create-csharp.md) 
* [Verwalten einer Datenbank flexible Ressourcenpool (c#)](sql-database-elastic-pool-manage-csharp.md). 
* [Erstellen einer Datenbank flexible Ressourcenpool (PowerShell)](sql-database-elastic-pool-create-powershell.md) 
* [Überwachen und Verwalten einer Datenbank flexible Ressourcenpool (PowerShell)](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Allgemeine Fehler

Folgender Fehler nicht in vorherigen Kategorien fallen.

|Fehlercode|Schwere|Beschreibung|
|---:|---:|:---|
|15006|16|<AdministratorLogin>ist kein gültiger Name, da er ungültige Zeichen enthält.|
|18452|14|Fehler bei der Anmeldung. Der Benutzername aus einer nicht vertrauenswürdigen Domäne ist, und kann nicht verwendet werden, mit Windows authentication.%. & #x2a; ls (Windows-Benutzernamen werden in dieser Version von SQL Server nicht unterstützt.)|
|18456|14|Fehler bei der Anmeldung für Benutzer ' % & #x2a;ls'.%. & #x2a; ls %. & #x2a; ls (Fehler bei der Anmeldung für Benutzer "% & #x2a; ls". Fehler bei die Änderung des Kennworts Änderung des Kennworts bei der Anmeldung wird nicht in dieser Version von SQL Server unterstützt.)|
|18470|14|Fehler bei der Anmeldung für Benutzer '% & #x2a; ls'. Ursache: Das Konto ist disabled.%. & #x2a; ls|
|40014|16|Mehrere Datenbanken können nicht in derselben Transaktion verwendet werden.|
|40054|16|Tabellen ohne einen gruppierten Index werden in dieser Version von SQL Server nicht unterstützt. Erstellen eines gruppierten Indexes, und versuchen Sie es erneut.|
|40133|15|Dieser Vorgang wird in dieser Version von SQL Server nicht unterstützt.|
|40506|16|Angegebenen SID ist ungültig für diese Version von SQL Server.|
|40507|16|' % & #x2a;!-Tabelle nicht mit den Parametern in dieser Version von SQL Server aufgerufen werden.|
|40508|16|USE-Anweisung wird zum Umschalten zwischen Datenbanken nicht unterstützt. Verwenden Sie eine neue Verbindung für die Verbindung zu einer anderen Datenbank ein.|
|40510|16|Anweisung '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt|
|40511|16|Integrierte Funktion '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40512|16|Veraltete Feature "%ls" wird in dieser Version von SQL Server nicht unterstützt.|
|40513|16|Server Variable '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40514|16|"%ls" wird in dieser Version von SQL Server nicht unterstützt.|
|40515|16|Verweis auf Datenbank und/oder Server Namen in '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40516|16|Globale temporäre Objekte werden in dieser Version von SQL Server nicht unterstützt.|
|40517|16|Stichwort oder Anweisung Option '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40518|16|DBCC-Befehl '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40520|16|Sicherungsfähige Class "% S_MSG" wird in dieser Version von SQL Server nicht unterstützt.|
|40521|16|Sicherungsfähige Class '% S_MSG' werden im Bereich in dieser Version von SQL Server nicht unterstützt.|
|40522|16|Datenbanktyp Hauptbenutzer '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40523|16|Erstellen des implizit Benutzers '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt. Erstellen Sie den Benutzer explizit vor der Verwendung.|
|40524|16|Datentyp '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40525|16|MIT '%.ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40526|16|' % & #x2a;!-Tabelle Rowsetprovider in dieser Version von SQL Server nicht unterstützt.|
|40527|16|Verknüpfte Server werden in dieser Version von SQL Server nicht unterstützt.|
|40528|16|Benutzer können keine Zertifikate, asymmetrische Schlüssel oder Windows-Anmeldung in dieser Version von SQL Server zugeordnet werden.|
|40529|16|Integrierte Funktion '% & #x2a; ls' in Identitätswechsel Kontext nicht in dieser Version von SQL Server unterstützt wird.|
|40532|11|Server kann nicht geöffnet werden "% & #x2a; ls" von der Anmeldung angeforderte. Fehler bei der Anmeldung.|
|40553|16|Da eine übermäßige arbeitsspeicherauslastung wurde die Sitzung beendet. Versuchen Sie, Ihre Abfrage zum Verarbeiten von weniger Zeilen zu ändern.<br/><br/>*Hinweis:* Verringern die Anzahl der `ORDER BY` und `GROUP BY` Operationen in Transact-SQL-Code wird die Arbeitsspeicher Anforderungen Ihrer Abfrage verringert.|
|40604|16|Konnte nicht CREATE/ALTER DATABASE, da es das Speicherkontingent des Servers überschreiten würde.|
|40606|16|Anfügen von Datenbanken wird in dieser Version von SQL Server nicht unterstützt.|
|40607|16|Windows-Benutzernamen werden in dieser Version von SQL Server nicht unterstützt.|
|40611|16|Server können maximal 128 Firewall-Regeln definiert haben.|
|40614|16|Start-IP-Adresse der Firewall-Regel darf End-IP-Adresse nicht überschreiten.|
|40615|16|Server "{0}" von der Anmeldung angeforderte kann nicht geöffnet werden. Client mit IP-Adresse "{1}" ist nicht zulässig, auf den Server zugreifen.  Zum Aktivieren des Zugriffs Verwenden des SQL-Datenbank-Portals oder der master-Datenbank zum Erstellen einer Firewallregel für diese IP-Adresse oder die Adresse Zellbereich Sp_set_firewall_rule ausgeführt.  Es kann bis zu fünf Minuten, damit diese Änderung wirksam dauern.|
|40617|16|Den Namen der Firewall-Regel, die mit beginnt <rule name> ist zu lang. Maximale Länge beträgt 128.|
|40618|16|Der Firewallregelname darf nicht leer sein.|
|40620|16|Fehler bei der Anmeldung für Benutzer "% & #x2a; ls". Fehler bei die Änderung des Kennworts Änderung des Kennworts bei der Anmeldung wird in dieser Version von SQL Server nicht unterstützt.|
|40627|20|Vorgang auf dem Server "{0}" und die Datenbank "{1}" wird ausgeführt. Warten Sie einige Minuten, bevor Sie es erneut versuchen.|
|40630|16|Fehler bei der Überprüfung des Kennworts. Das Kennwort erfüllt nicht Richtlinie Anforderungen, da diese zu kurz ist.|
|40631|16|Das Kennwort, das Sie angegeben haben, ist zu lang. Das Kennwort sollte maximal 128 Zeichen umfassen.|
|40632|16|Fehler bei der Überprüfung des Kennworts. Das Kennwort erfüllt nicht Richtlinie Anforderungen, da es nicht derart komplex ist.|
|40636|16|Reservierte Datenbanknamen kann nicht verwendet '% & #x2a; ls' in diesen Vorgang.|
|40638|16|Ungültige Abonnement-Id < Abonnement-Id >. Abonnement ist nicht vorhanden.|
|40639|16|Anforderung Schema nicht entsprechen: <schema error>.|
|40640|20|Der Server hat eine unerwartete Ausnahme.|
|40641|16|Der angegebene Speicherort ist ungültig.|
|40642|17|Der Server ist zurzeit ausgelastet. Versuchen Sie es später erneut.|
|40643|16|Der Wert der angegebenen X-ms-Version Kopfzeile ist ungültig.|
|40644|14|Fehler beim Zugriff auf das angegebene Abonnement zu autorisieren.|
|40645|16|Servername <servername> nicht leer oder null sein. Sie können nur von vorgenommen werden, der Kleinbuchstaben 'a'-'Z', die Zahlen 0-9 und Bindestriche. Der Bindestrich möglicherweise nicht führen oder aufgenommen wird, in den Namen.|
|40646|16|Abonnement-ID darf nicht leer sein.|
|40647|16|Abonnements < Abonnement-Id hat keinen Server Servername.|
|40648|17|Zu viele Anfragen wurden durchgeführt. Versuchen Sie es später.|
|40649|16|Ungültiger Inhaltstyp angegeben ist. Nur Anwendung/Xml unterstützt wird.|
|40650|16|Abonnement < Abonnement-Id > ist nicht vorhanden oder ist nicht für den Vorgang bereit.|
|40651|16|Fehler beim Server erstellen, da das Abonnement < Abonnement-Id > deaktiviert ist.|
|40652|16|Kann nicht verschoben oder Server erstellen. Abonnements < Abonnement-Id > wird Server Kontingent überschritten.|
|40671|17|Fehler bei der Kommunikation zwischen dem Gateway und der Verwaltungsdienst. Versuchen Sie es später.|
|40852|16|Datenbank kann nicht geöffnet werden ' %. *! auf dem Server ' %.* von der Anmeldung angeforderte!-Tabelle auf. Zugriff auf die Datenbank darf nur mithilfe einer mit aktivierter Sicherheit Verbindungszeichenfolge. Zugriff auf die Datenbank, ändern Sie die Verbindungszeichenfolgen enthalten sichere Server den vollqualifizierten Domänennamen - 'Servername'.database.windows .net sollten zu 'Servername'.database geändert werden. `secure`. Windows.|
|45168|16|Das SQL Azure-System Auslastung ist, und ist eine Obergrenze auf gleichzeitige DB Vorgänge für einen einzelnen Server platzieren (z. B. Datenbank erstellen). In der Fehlermeldung angegebene Server hat die maximale Anzahl der aktiven Verbindungen überschritten. Versuchen Sie es später erneut.|
|45169|16|SQL Azure-Systems Auslastung ist, und platzieren Sie die Obergrenze ist die Anzahl der parallele Servervorgänge für ein einzelnes Abonnement (z. B. Server erstellen). Das Abonnement in der Fehlermeldung angegeben hat die maximale Anzahl der aktiven Verbindungen überschritten, und die Anforderung wurde zurückgewiesen. Versuchen Sie es später erneut.|

## <a name="related-links"></a>Links zu verwandten Themen

- [SQL Azure-Datenbank Allgemein Einschränkungen und Richtlinien](sql-database-general-limitations.md)
- [Beschränkungen der Ressource für Azure SQL-Datenbank](sql-database-resource-limits.md)
