<properties
    pageTitle="Sichern der Datenbanken gedehnt aktiviert | Microsoft Azure"
    description="Erfahren Sie, wie gedehnt Sichern\-Datenbanken aktiviert."
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
    ms.date="10/14/2016"
    ms.author="douglasl"/>

# <a name="backup-stretch-enabled-databases"></a>Zusätzliche gedehnt aktiviert Datenbanken

Datenbanksicherungskopien können Sie viele Arten von Fehlern, Fehlern und Schäden wiederherstellen.  

-   Sie haben Ihre gedehnt Sichern\-SQL Server-Datenbanken aktiviert.  

-   Microsoft Azure sichert automatisch auf der remote-Daten, die gedehnt Datenbank aus SQL Server in Azure migriert hat.  

>    [AZURE.NOTE] Sicherung ist nur einen Teil einer abgeschlossen hohen Verfügbarkeit und Business Continuity-Lösung. Weitere Informationen zur hohen Verfügbarkeit finden Sie unter [Hohen Verfügbarkeit von Lösungen](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="back-up-your-sql-server-data"></a>Sichern Sie Ihrer SQL Server-Daten  

Sichern Ihrer gedehnt\-aktiviert SQL Server-Datenbanken, können Sie weiterhin die Sicherung SQL Server-Methoden verwenden, die Sie zurzeit verwenden. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen von SQL Server-Datenbanken](https://msdn.microsoft.com/library/ms187048.aspx).

Sicherungen gedehnt aktiviert SQL Server-Datenbank enthalten nur lokaler Daten und Qualifizieren von Daten für die Migration an dem Punkt in der Uhrzeit, wann die Sicherung ausgeführt wird. \(Daten berechtigt sind, die noch nicht migriert, aber in Azure basierend auf der Migrations-Einstellungen der Tabellen migriert werden.\) Dies wird als eine **flache** Sicherung bezeichnet und umfasst die Daten nicht bereits in Azure migriert.  

## <a name="back-up-your-remote-azure-data"></a>Sichern Sie Ihrer remote Azure-Daten   

Microsoft Azure sichert automatisch auf der remote-Daten, die gedehnt Datenbank aus SQL Server in Azure migriert hat.  

### <a name="azure-reduces-the-risk-of-data-loss-with-automatic-backup"></a>Azure reduziert das Risiko von Datenverlusten mit automatischen Sicherung  
Der Dienst SQL Server gedehnt Datenbank auf Azure Loss Ihrer entfernten Datenbanken mit der automatischen Speicherung Momentaufnahmen mindestens alle 8 Stunden. Es behält jeder Snapshot für sieben Tage, damit Sie einen Bereich von Wiederherstellungspunkten möglich können.  

### <a name="azure-reduces-the-risk-of-data-loss-with-geo-redundancy"></a>Azure reduziert das Risiko von Datenverlusten mit Geo\-Redundanz  
Azure-Datenbanksicherungskopien auf Geo gespeichert sind\-redundante Azure-Speicher (RAS\-GRS) und können daher Geo\-redundante standardmäßig. Geo\-redundante Speicherung repliziert von Daten für einen sekundären Bereich, der hundert Meilen von der primären Region ist. In primären und sekundären Regionen werden Ihre Daten jedes dreimal auf separaten Fehlerstrukturanalyse- und Upgrade Domänen repliziert. Dadurch wird sichergestellt, dass Ihre Daten dauerhaften sogar im Fall einer abgeschlossen regionalen Ausfall oder, die einer der Azure Regionen nicht verfügbar gibt wieder.

### <a name="a-namestretchrpoastretch-database-reduces-the-risk-of-data-loss-for-your-azure-data-by-retaining-migrated-rows-temporarily"></a><a name="stretchRPO"></a>Dehnen Datenbank reduziert das Risiko von Datenverlusten für Ihre Azure-Daten durch migrierte Zeilen vorübergehend Aufbewahrung
Nachdem gedehnt Datenbank in Azure berechtigte Zeilen aus SQL Server migriert werden, wird diese Zeilen in der Tabelle staging für mindestens 8 Stunden beibehalten. Wenn Sie eine Sicherungskopie Ihrer Azure-Datenbank wiederherstellen, verwendet gedehnt Datenbank die Zeilen in der Tabelle staging gespeichert, SQL Server und Azure-Datenbanken abzustimmen.

Nachdem Sie eine Sicherungskopie Ihrer Azure-Daten wiederherstellen, müssen Sie die gespeicherte Prozedur [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) , um die Verbindung der Strecken wiederherzustellen ausführen\-SQL Server-Datenbank auf die Remotedatenbank Azure aktiviert. Wenn Sie **sys.sp_rda_reauthorize_db**ausführen, gleicht gedehnt Datenbank automatisch SQL Server und Azure-Datenbanken.

Zum Erhöhen der Anzahl von Stunden für migrierte Daten, die Datenbank gedehnt vorübergehend in der Tabelle staging behält führen Sie der gespeicherten Prozedur [sys.sp_rda_set_rpo_duration aus](https://msdn.microsoft.com/library/mt707766.aspx) , und geben Sie eine Zahl größer als 8 Stunden. Berücksichtigen Sie die folgenden Faktoren, um zu entscheiden, wie viele Daten beibehalten:
-   Die Häufigkeit der automatischen Azure Sicherung (mindestens alle 8 Stunden).
-   Der Zeitaufwand nach ein Problem aufgetreten, das Problem zu erkennen und um zu entscheiden, eine Sicherungskopie wiederherzustellen.
-   Die Dauer des Wiederherstellungsvorgangs Azure.

> [AZURE.NOTE] Erhöhen die Menge der Daten, die Datenbank gedehnt vorübergehend in der Tabelle staging behält verlängert den Speicherplatz auf dem SQL Server erforderlich.

Um die Anzahl der Stunden der Daten zu überprüfen, die Datenbank gedehnt vorübergehend derzeit in der Tabelle staging behält, führen Sie die gespeicherte Prozedur [sys.sp_rda_get_rpo_duration](https://msdn.microsoft.com/library/mt707767.aspx)aus.

## <a name="see-also"></a>Siehe auch

[Verwalten von und Problembehandlung bei gedehnt Datenbank](sql-server-stretch-database-manage.md)

[Sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Sichern und Wiederherstellen von SQL Server-Datenbanken](https://msdn.microsoft.com/library/ms187048.aspx)
