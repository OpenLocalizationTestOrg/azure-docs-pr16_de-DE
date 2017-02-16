<properties
   pageTitle="SQL Data Warehouse wiederherstellen | Microsoft Azure"
   description="Übersicht über die Datenbank Wiederherstellungsoptionen zum Wiederherstellen einer Datenbank in Azure SQL-Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/29/2016"
   ms.author="lakshmir;barbkess;sonyama"/>


# <a name="sql-data-warehouse-restore"></a>SQL Data Warehouse wiederherstellen

> [AZURE.SELECTOR]
- [(Übersicht)][]
- [Portal][]
- [PowerShell][]
- [REST][]

SQL Data Warehouse bietet lokalen und geografische stellt als Teil der Datawarehouse Disaster Wiederherstellungsfunktionen. Verwenden Sie Datawarehouse Sicherungskopien wiederherstellen Ihr Datawarehouse bis zu einem Wiederherstellen der primären Region, oder verwenden Sie Geo redundante Sicherungskopien in ein anderes geografische Region wiederherstellen. In diesem Artikel wird erläutert, die Einzelheiten ein Datawarehouse wiederherstellen.

## <a name="what-is-a-data-warehouse-restore"></a>Was ist ein Datawarehouse wiederherstellen?

Ein Datawarehouse Wiederherstellen ist einer neuen Datawarehouse, das aus einer Sicherung eines vorhandenen erstellt wird oder die gelöschten Datawarehouse. Wiederhergestellte Datawarehouse erstellt gesicherte Datawarehouse zu einem bestimmten Zeitpunkt neu. Da SQL Data Warehouse verteilten Systems ist, wird ein Datawarehouse Wiederherstellen von vielen Sicherungsdateien erstellt, die in Azure Blobs gespeichert werden. 

Datenbank wiederherstellen ist ein wesentlicher Bestandteil jeder Strategie Business Continuity- und Disaster Wiederherstellung, weil sie Ihre Daten erneut nach unbeabsichtigtes Beschädigung oder Löschung erstellt.

Weitere Informationen finden Sie unter:

-  [SQL Data Warehouse Sicherungskopien](sql-data-warehouse-backups.md)
-  [Business Continuity (Übersicht)](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a>Warehouse Wiederherstellen von Datenpunkten

Als ein Vorteil der Verwendung von Azure Premium Speicher verwendet SQL Data Warehouse BLOB-Speicher von Azure Momentaufnahmen, um das primäre Datawarehouse sichern. Jeder Snapshot enthält einen Wiederherstellungspunkt, der die Uhrzeit darstellt, der Snapshot gestartet. Um ein Datawarehouse wiederhergestellt werden soll, wählen Sie einen Wiederherstellungspunkt aus und geben Sie einen Wiederherstellungsbefehl.  

SQL Data Warehouse stellt immer die Sicherung in einer neuen Datawarehouse aus. Sie können entweder das wiederhergestellte Datawarehouse und der aktuellen Zeile beibehalten oder Löschen einer von ihnen. Wenn Sie das aktuelle Datawarehouse mit dem wiederhergestellten Datawarehouse ersetzen möchten, können Sie es umbenennen.

Wenn Sie zum Wiederherstellen eines gelöschten oder angehalten Datawarehouse benötigen, können Sie [eine Support-Ticket zu erstellen](sql-data-warehouse-get-started-create-support-ticket.md). 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore the last available restore point.

Yes, for the next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps the data warehouse and its snapshots for seven days just in case you need the data. After seven days, you won't be able to restore to any of the restore points. -->

## <a name="geo-redundant-restore"></a>Geo redundante wiederherstellen

Wenn Sie den Geo redundante Speicher verwenden, können Sie das [Datencenter für](../best-practices-availability-paired-regions.md) in einem anderen geografische Region Datawarehouse wiederherstellen. Das Datawarehouse wird von der letzten tägliche Sicherung wiederhergestellt. 

## <a name="restore-timeline"></a>Wiederherstellen der Zeitachse

Sie können eine Datenbank zu einem beliebigen verfügbaren wiederherstellen innerhalb der letzten sieben Tage wiederherstellen. Momentaufnahmen starten Sie alle vier bis acht Stunden und stehen sieben Tage lang. Wenn eine Momentaufnahme älter als sieben Tage ist, es abgelaufen ist und seine Wiederherstellungspunkt ist nicht mehr verfügbar.

## <a name="restore-costs"></a>Wiederherstellen von Kosten

Der Speicher Gebühr für das wiederhergestellte Datawarehouse ist in der Geschwindigkeit Azure Premium Speicher in Rechnung gestellt. 

Wenn Sie ein wiederhergestellten Datawarehouse anhalten, unterliegen Sie für die Speicherung Rate Azure Premium Storage. Der Vorteil von anhalten ist, dass Sie nicht die DWU computing-Ressourcen berechnet werden.

Weitere Informationen zur Preisgestaltung SQL Data Warehouse finden Sie unter [SQL Data Warehouse Preise](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="uses-for-restore"></a>Verwendungsmöglichkeiten für wiederherstellen

Zum Wiederherstellen von Daten nach Daten versehentlich Verlust oder Beschädigung wird vor allem verwendet für Datawarehouse wiederherstellen.

Datawarehouse wiederherstellen können Sie auch eine Sicherungskopie für mehr als sieben Tage beibehalten. Sobald die Sicherung wiederhergestellt wird, Sie haben das Datawarehouse online und können er endlos speichern berechnen Kosten zeigen. Die Datenbank angehaltene budgetgerecht Speicher Gebühren Rate Azure Premium Storage. 

## <a name="related-topics"></a>Verwandte Themen

### <a name="scenarios"></a>Szenarien

- Eine Übersicht über Business Continuity finden Sie unter [Übersicht über die Business continuity](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

Zum Ausführen einer Datawarehouse wiederherstellen Wiederherstellen Sie mit:

- Azure-Portal finden Sie unter [Wiederherstellen ein Datawarehouse über das Azure-Portal](sql-data-warehouse-restore-database-portal.md)
- PowerShell-Cmdlets finden Sie unter [Wiederherstellen ein Datawarehouse mithilfe der PowerShell-cmdlets](sql-data-warehouse-restore-database-powershell.md)
- Zeigen Sie mit APIs, finden Sie unter [Wiederherstellen ein Datawarehouse mithilfe der REST-APIs](sql-data-warehouse-restore-database-rest-api.md)

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[(Übersicht)]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
