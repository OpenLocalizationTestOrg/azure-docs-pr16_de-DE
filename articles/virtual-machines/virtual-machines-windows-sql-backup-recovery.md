<properties
    pageTitle="Sicherung und Wiederherstellung für SQLServer | Microsoft Azure"
    description="Beschreibt die Sicherung und Wiederherstellung Hinweise für SQL Server-Datenbanken, die auf Azure virtuellen Computern ausgeführt."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Sichern und Wiederherstellen für SQLServer in Azure-virtuellen Computern

## <a name="overview"></a>(Übersicht)

Sichern von Daten in SQL Server-Datenbanken ist ein wichtiger Bestandteil der Strategie zum Schutz vor Datenverlust aufgrund von Fehlern bei der Anwendung oder Benutzer. Dies ist gleich WAHR für SQL Server auf Azure virtuellen Computern (virtuelle Computer) ausgeführt.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Für SQL Server in Azure-virtuellen Computern ausgeführt wird können Sie native Sicherung verwenden und Verfahren unter Verwendung von verbundenen Laufwerke für das Ziel der Sicherungsdateien wiederherstellen. Es gibt jedoch eine hinsichtlich der Anzahl der Datenträger, die Sie an einer Azure-virtuellen Computern, basierend auf der [Größe des virtuellen Computers](virtual-machines-linux-sizes.md)anfügen können. Es gibt auch den Verwaltungsaufwand der Datenträger Verwaltung zu berücksichtigen ist.

Beginnend mit SQL Server-2014, können Sie sichern und Wiederherstellen in Microsoft Azure Blob-Speicher. SQL Server 2016 enthält auch Erweiterungen für diese Option aus. Darüber hinaus bietet für Datenbankdateien in Microsoft Azure Blob Storage gespeichert, SQL Server 2016 eine Option für nahezu erfolgt Sicherungskopien und für schnelle Wiederherstellung Azure Momentaufnahmen verwenden. Dieser Artikel bietet einen Überblick über die folgenden Optionen, und Weitere Informationen finden Sie unter [SQL Server-Sicherung und Wiederherstellung mit Microsoft Azure Blob Storage Service](https://msdn.microsoft.com/library/jj919148.aspx).

>[AZURE.NOTE] Eine Beschreibung der Optionen zum Sichern sehr großer Datenbanken finden Sie unter [Multi-TB SQL Server-Datenbank Sicherungsstrategien für Azure virtuellen Computern](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).

In den folgenden Abschnitten enthalten spezifische Informationen zu den verschiedenen Versionen von SQL Server in einer Azure-virtuellen Computern unterstützt.

## <a name="sql-server-virtual-machines"></a>SQL Server-virtuellen Computern

Wenn Ihre SQL Server-Instanz auf eine Azure-virtuellen Computern ausgeführt wird, können die Datenbankdateien bereits auf Daten vorliegen in Azure. Diese Datenträger live in Azure Blob-Speicher. Daher dauern die Gründe für das Sichern der Datenbank und die Ansätze Sie etwas ändern aus. Beachten Sie Folgendes aus. 

- Sie müssen nicht mehr Datenbanksicherungskopien zum Schutz vor Hardware oder Medien bieten, da Microsoft Azure diesen Schutz als Teil der Microsoft Azure-Diensts bietet durchführen.

- Sie müssen immer noch Datenbanksicherungskopien zum Schutz vor Benutzerfehlern oder für zu archivieren, rechtliche Gründe oder administrative Zwecke bieten ausführen.

- Sie können die Sicherungsdatei direkt in Azure speichern. Weitere Informationen finden Sie in den folgenden Abschnitten, die Anleitung für die verschiedenen Versionen von SQL Server bereitstellen.

## <a name="sql-server-2016"></a>SQLServer 2016

Microsoft SQL Server 2016 unterstützt [Sichern und Wiederherstellen mit Azure Blobs](https://msdn.microsoft.com/library/jj919148.aspx) Features in SQL Server 2014 gefunden. Umfasst aber auch die folgenden Erweiterungen:

| Verbesserung der 2016               | Details                          |
|---------------------|-------------------------------|
| **Striping**              | Wenn Microsoft Azure BLOB-Speicher sichern, unterstützt SQL Server 2016 Sichern auf mehrere Blobs zu aktivieren, bis zu einem Maximum von 12,8 TB großen Datenbanken sichern.      |
| **Snapshot-Sicherung**                | Durch die Verwendung von Momentaufnahmen Azure bietet SQL Server-Datei-Snapshot Sicherung nahezu erfolgt Sicherungskopien und schnelle Wiederherstellung für Datenbankdateien gespeichert mithilfe des Diensts für Azure BLOB-Speicher. Diese Funktion können Sie die Sicherung zu vereinfachen und Richtlinien wiederherstellen. Datei-Snapshot-Sicherung unterstützt auch Punkt in Zeit wiederherstellen. Weitere Informationen finden Sie unter [Snapshot Sicherungskopien Datenbankdateien in Azure](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx).   |
| **Verwaltete Sicherung Planung**            | SQL Server verwaltet Sicherung in Azure unterstützt jetzt benutzerdefinierter Terminpläne aus. Weitere Informationen finden Sie unter [SQL Server verwaltete Sicherung in Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx).   |

Ein Lernprogramm der Leistungsfähigkeit von SQL Server 2016 Wenn Azure Blob-Speicher verwenden, finden Sie unter [Lernprogramm: verwenden den Dienst Microsoft Azure Blob-Speicher mit SQL Server 2016 Datenbanken](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQLServer 2014

SQL Server 2014 umfasst die folgenden Erweiterungen:

1. **Sichern und Wiederherstellen in Azure**:

 - *SQL Server-Sicherung-URL* verfügt jetzt über Unterstützung in SQL Server Management Studio. Sichern oder Wiederherstellen einer Aufgabe oder Wartung Plan-Assistenten in SQL Server Management Studio verwenden, ist die Option zum Sichern in Azure jetzt verfügbar. Weitere Informationen finden Sie unter [SQL Server-Sicherung-URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
 - *SQL Server verwaltet Sicherung in Azure* verfügt über neue Funktionen, der Automatisches Sicherung Management ermöglicht. Dies eignet sich besonders zum Automatisieren von Sicherung Management für SQL Server 2014 Instanzen auf einem Computer Azure ausgeführt. Weitere Informationen finden Sie unter [SQL Server verwaltete Sicherung in Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
 - *Automatische Sicherung* bietet zusätzliche Automatisierung um *SQL Server verwaltete Sicherung in Azure* automatisch auf alle vorhandenen und neuen Datenbanken für einen SQL Server virtuellen Computer Azure zu aktivieren. Weitere Informationen finden Sie unter [Automatische Sicherung für SQL Server in Azure virtuellen Computern](virtual-machines-windows-sql-automated-backup.md).
 - Eine Übersicht über alle Optionen für SQL Server-2014 Sicherung in Azure finden Sie unter [SQL Server-Sicherung und Wiederherstellung mit Microsoft Azure Blob Storage Service](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).

1. **Verschlüsselung**: SQLServer 2014 unterstützt die Verschlüsselung von Daten, wenn Sie eine Sicherungskopie zu erstellen. Unterstützt mehrere Algorithmen für die Verschlüsselung und die Verwendung osf ein Zertifikat oder asymmetrische Schlüssel. Weitere Informationen finden Sie unter [Sicherung Verschlüsselung](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>SQLServer 2012

Ausführliche Informationen zu SQL Server-Sicherung und Wiederherstellung in SQL Server 2012 finden Sie unter [Sichern und Wiederherstellen von SQL Server-Datenbanken (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

Starten in SQL Server 2012 SP1 kumulativen Update 2, können Sie bis zu sichern und Wiederherstellen aus dem Dienst Azure BLOB-Speicher. Sichern von SQL Server-Datenbanken auf einem SQL Server auf eine Azure-virtuellen Computern oder eine lokale Instanz ausgeführt, kann diese Verbesserung verwendet werden. Weitere Informationen finden Sie unter [SQL Server-Sicherung und Wiederherstellung mit Azure Blob Storage Service](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Einige der Vorteile der Verwendung der Azure Blob-Speicherdienst gehören der Möglichkeit, die Beschränkung 16 Datenträger für verbundenen Laufwerke erleichterte Verwaltung, die direkte Verfügbarkeit der Sicherungsdatei zu einer anderen Instanz von SQL Server-Instanz für eine Azure-virtuellen Computern ausgeführt, oder eine lokale Instanz für die Migration – oder Ausfall umgehen. Eine vollständige Liste der Vorteile bei der Verwendung einer Azure Blob-Speicherdienst für SQL Server-Sicherungskopien finden Sie unter Abschnitt *Vorteile* in [SQL Server-Sicherung und Wiederherstellung mit Azure Blob Storage Service](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Bewährte Methode Empfehlungen und Informationen zur Problembehandlung finden Sie unter [Sichern und Wiederherstellen von Best Practices (Azure Blob Storage Service)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQLServer 2008

SQL Server-Sicherung und Wiederherstellung in SQL Server 2008 R2 finden Sie unter [Sichern und Wiederherstellen von Datenbanken in SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

SQL Server-Sicherung und Wiederherstellung in SQL Server 2008 finden Sie unter [Sichern und Wiederherstellen von Datenbanken in SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie bei der Planung der bereitstellungs von SQL Server auf eine Azure-virtuellen Computer finden Sie Anleitungen provisioning in den folgenden Lernprogramm: [Bereitstellung eines SQL Server virtuellen Computers auf Azure Azure Ressourcenmanager](virtual-machines-windows-portal-sql-server-provision.md).

Obwohl Sicherung und Wiederherstellung können zum Migrieren der Daten verwendet werden, gibt es potenziell einfacher Daten Migrations-Pfade für SQL Server ein Azure-virtuellen Computers. Eine umfassende Erläuterung der Migrationsoptionen und Empfehlungen finden Sie unter [Migrieren einer Datenbank mit SQL Server ein Azure-virtuellen Computers](virtual-machines-windows-migrate-sql.md).

Überprüfen Sie weitere [Ressourcen für SQL Server in Azure virtuellen Computern ausgeführt](virtual-machines-windows-sql-server-iaas-overview.md).
