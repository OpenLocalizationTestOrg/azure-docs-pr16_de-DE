<properties
   pageTitle="SQL Data Warehouse Sicherungskopien | Microsoft Azure"
   description="Informationen Sie zu SQL Data Warehouse integrierten Datenbanksicherungskopien, mit die Sie eine Azure SQL-Data Warehouse auf einen Wiederherstellungspunkt oder ein anderes geografische Region wiederherstellen können."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lakshmi1812"
   manager="barbkess"
   editor="monicar"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="lakshmir;barbkess"/>

# <a name="sql-data-warehouse-backups"></a>SQL Data Warehouse Sicherungskopien

SQL Data Warehouse bietet lokale und geografische Sicherungskopien als Teil der Datawarehouse zusätzliche Funktionen. Hierzu gehören BLOB-Speicher von Azure Momentaufnahmen und Geo redundante Speicherung. Formular mit Datawarehouse Sicherungskopien wiederherstellen Ihr Datawarehouse bis zu einem Wiederherstellen der primären Region oder in ein anderes geografische Region wiederherstellen. In diesem Artikel wird erläutert, die Einzelheiten Sicherungskopien in SQL Data Warehouse.

## <a name="what-is-a-data-warehouse-backup"></a>Was ist eine Sicherungskopie der Datawarehouse?

Eine Sicherungskopie der Datawarehouse ist die Daten, die Sie verwenden können, um ein Datawarehouse zu einem bestimmten Zeitpunkt wiederherstellen.  Da SQL Data Warehouse ein System verteilt ist, besteht aus Datawarehouse Sicherung viele Dateien, die in Azure Blobs gespeichert werden. 

Datenbanksicherungskopien sind ein wesentlicher Bestandteil jeder Strategie Business Continuity- und Disaster Wiederherstellung, da Ihre Daten aus unbeabsichtigtes Beschädigung oder Löschung zu schützen. Weitere Informationen finden Sie unter [Übersicht über die Business Continuity](../sql-database/sql-database-business-continuity.md).

## <a name="data-redundancy"></a>Datenredundanz

SQL Data Warehouse Loss von Daten durch Speichern von Daten in lokal redundante (LRS) Azure Premium Storage. Dieses Feature Azure-Speicher speichert mehrere synchrone Kopien der Daten in der Mitte lokaler Daten zu transparenten Datenschutz sichergestellt ist, wenn es lokalisierte Fehlern gibt. Datenredundanz wird sichergestellt, dass Ihre Daten Infrastrukturprobleme, wie z. B. Fehlern der Datenträger überstehen können. Datenredundanz sorgt für Geschäftskontinuität mit einer Fehlertoleranz und Infrastruktur mit hoher Verfügbarkeit.

Weitere Informationen zu:

- Azure Premium-Speicher finden Sie unter [Einführung in Azure Premium Storage](../storage/storage-premium-storage.md).
- Lokal redundante Speicherung, finden Sie unter [Replikation Azure-Speicher](../storage/storage-redundancy.md#locally-redundant-storage).


## <a name="azure-storage-blob-snapshots"></a>Azure Blob-Speicher Momentaufnahmen

Als ein Vorteil der Verwendung von Azure Premium Speicher verwendet SQL Data Warehouse BLOB-Speicher von Azure Momentaufnahmen, um das Datawarehouse lokal sichern. Sie können ein Datawarehouse auf Wiederherstellen, zeigen Sie eine Momentaufnahme wiederherstellen. Momentaufnahmen mindestens alle acht Stunden starten, und es stehen sieben Tage lang.  

Weitere Informationen zu:

- Azure Blob Momentaufnahmen, finden Sie unter [Erstellen eines Blob Snapshot](../storage/storage-blob-snapshots.md).


## <a name="geo-redundant-backups"></a>Geo redundante Sicherungskopien

Alle 24 Stunden, speichert SQL Data Warehouse vollständige Datawarehouse in Standard-Speicher. Vollständige Datawarehouse wird erstellt, um die Uhrzeit der letzten Momentaufnahme entsprechen. Der standard-Speicher gehört mit einem Konto Geo redundante Speicherung mit Lesezugriff (RAS-GRS). Das Feature Azure Speicher RAS-GRS repliziert die Sicherungsdateien eines [gepaarten Datacenter](../best-practices-availability-paired-regions.md). Diese Geo-Replikation wird sichergestellt, dass Sie Datawarehouse wiederherstellen können, falls Sie nicht die Momentaufnahmen in Ihrem primären Region zugreifen können. 

Dieses Feature ist standardmäßig aktiviert. Wenn Sie nicht Geo redundante Sicherungskopien verwenden möchten, können Sie Abonnement kündigen. 

>[AZURE.NOTE] In Azure-Speicher bezieht sich der Ausdruck *Replikation* zum Kopieren von Dateien von einem Ort zu einem anderen ein. Der SQL- *Datenbankreplikation* bezieht sich auf planmäßigen auf mehrere sekundäre Datenbanken mit einer primären Datenbank synchronisiert. 

Weitere Informationen zu:
- Geo redundante Speicher, finden Sie unter [Replikation Azure-Speicher](../storage/storage-redundancy.md).
- RAS-GRS-Speicher finden Sie unter [Geo redundante Speicher Lese-Zugriff](../storage/storage-redundancy.md#read-access-geo-redundant-storage).

## <a name="data-warehouse-backup-schedule-and-retention-period"></a>Data warehouse-Sicherung planen und Aufbewahrungszeitraum

SQL Data Warehouse erstellt Momentaufnahmen Ihre online-Daten Lager alle vier bis acht Stunden und behält jeder Snapshot sieben Tage lang. Sie können Ihre online-Datenbank in der letzten sieben Tage an einem Punkt wiederherstellen wiederherstellen. 

Führen Sie zum Anzeigen der letzte Snapshot gestartet, diese Abfrage auf Ihr online SQL Data Warehouse ein. 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Wenn Sie eine Momentaufnahme für mehr als sieben Tage beibehalten müssen, können Sie ein neues Datawarehouse ein Wiederherstellungspunkts wiederherstellen. Nach Abschluss die Wiederherstellung beginnt SQL Data Warehouse mit der Erstellung von Momentaufnahmen auf neuen Datawarehouse. Wenn Sie Änderungen an der neuen Datawarehouse vornehmen nicht, die Momentaufnahmen leer bleiben und daher die Kosten Momentaufnahme ist minimal. Sie könnten auch anhalten, die Datenbank aus, um zu verhindern, dass SQL Data Warehouse Momentaufnahmen erstellen.


### <a name="what-happens-to-my-backup-retention-while-my-data-warehouse-is-paused"></a>Was geschieht mit meiner Sicherung Aufbewahrungsrichtlinien, während mein Datawarehouse angehalten ist?

SQL Data Warehouse erstellt keine Momentaufnahmen und Momentaufnahmen während ein Datawarehouse angehalten ist nicht abläuft. Das Alter Momentaufnahme wird nicht geändert, während das Datawarehouse angehalten ist. Snapshot Aufbewahrung basiert auf die Anzahl der Tage, die das Datawarehouse nicht Kalendertage online ist.

Wenn eine Momentaufnahme Oktober 1: 00 Uhr beginnt und das Datawarehouse Oktober 3: 00 Uhr angehalten ist, ist die Momentaufnahme zwei Tage. Wenn das Datawarehouse wieder online ist ist der Snapshot zwei Tage aus. Wenn das Datawarehouse online Oktober 5: 00 Uhr ist, wird der Snapshot, zwei Tage alt ist und fünf weitere Tage lang bleibt.

Wenn das Datawarehouse wieder online ist, wird SQL Data Warehouse Lebensläufen neue Momentaufnahmen und Momentaufnahmen läuft ab, wenn sie mehr als sieben Tage Daten haben.

### <a name="how-long-is-the-retention-period-for-a-dropped-data-warehouse"></a>Wie lange dauert der Aufbewahrungszeitraum für ein abgelegten Datawarehouse?
Wenn ein Datawarehouse abgelegt wird, werden das Datawarehouse als auch die Momentaufnahmen sieben Tage lang gespeichert und dann entfernt. Sie können das Datawarehouse auf einen der Wiederherstellungspunkte gespeicherten wiederherstellen.

> [AZURE.IMPORTANT] Wenn Sie eine logische SQL Server-Instanz löschen, werden alle Datenbanken, die die Instanz gehören ebenfalls gelöscht werden und nicht wiederhergestellt werden. Sie können keine Wiederherstellen eines gelöschten Servers.

## <a name="data-warehouse-backup-costs"></a>Datawarehouse zusätzliche Kosten

Die Gesamtkosten für Ihre primäre Datawarehouse und sieben Tage von Momentaufnahmen Azure Blob wird auf die nächste TB gerundet. Beispielsweise werden, wenn Ihr Datawarehouse 1,5 TB ist, und verwenden Sie die Momentaufnahmen 100 GB, Sie für 2 TB Daten zu Azure Premium Speicher Sätzen Abrechnung. 

>[AZURE.NOTE] Jeder Snapshot Anfangs leer und vorgenommenen Änderungen an der primären Datawarehouse hinzufügen. Alle Momentaufnahmen vergrößern Größe als die Datawarehouse Änderungen. Daher wächst die Speicherkosten für Momentaufnahmen gemäß der Rendite ändern.

Wenn Sie Geo redundante Speicherung verwenden, erhalten Sie eine separate Speicher aufzulisten. Die Geo redundante Speicherung ist Standardsatz Lesezugriff geografischen redundante Speicherung (RAS-GRS) in Rechnung gestellt.

Weitere Informationen zur Preisgestaltung SQL Data Warehouse finden Sie unter [SQL Data Warehouse Preise](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="using-database-backups"></a>Verwenden der Datenbanksicherungskopien

Die primäre SQL Datawarehouse Sicherungskopien wird zum Datawarehouse zu einem Punkt wiederherstellen innerhalb der Aufbewahrungszeitraum wiederherstellen.  

- Zum Wiederherstellen eines SQL Datawarehouse finden Sie unter [einem SQL Datawarehouse wiederherstellen](sql-data-warehouse-restore-database-overview.md).


## <a name="related-topics"></a>Verwandte Themen

### <a name="scenarios"></a>Szenarien

- Eine Übersicht über Business Continuity finden Sie unter [Übersicht über die Business continuity](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

- Zum Wiederherstellen eines Datawarehouse finden Sie unter [einem SQL Datawarehouse wiederherstellen](sql-data-warehouse-restore-database-overview.md)

<!-- ### Tutorials -->

