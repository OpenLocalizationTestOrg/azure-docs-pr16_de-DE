<properties
    pageTitle="So verwenden Sie Azure-Speicher für SQL Server sichern und Wiederherstellen | Microsoft Azure"
    description="Informationen Sie zum Sichern von SQL Server auf Azure-Speicher. Es wird erläutert, die Vorteile von SQL-Datenbanken sichern, um Azure-Speicher."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="MikeRayMSFT"
    manager="jhubbard"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="mikeray"/>

# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Verwenden von Azure-Speicher für SQL Server-Sicherung und Wiederherstellung

## <a name="overview"></a>(Übersicht)

Beginnend mit SQL Server 2012 SP1 CU2, können Sie jetzt SQL Server Sicherungskopien direkt auf den Dienst Azure Blob-Speicher schreiben. Sie können dieses Feature verwenden, bis zu sichern und Wiederherstellen von Azure BLOB-Dienst mit einer lokalen SQL Server-Datenbank oder einer SQL Server-Datenbank in eine Azure-virtuellen Computern. Sichern in die cloud bietet Vorteile der Verfügbarkeit, unbegrenzte Geo repliziert Aufbewahrung und steigern der Migration von Daten in die und aus der Cloud. Sie können mithilfe von Transact-SQL oder SMO Sicherung oder Wiederherstellung Anweisungen ausgestellt.

SQL Server 2016 wird die neue Funktionen; [Datei-Snapshot Sicherung](http://msdn.microsoft.com/library/mt169363.aspx) können Sie fast erfolgt Sicherungskopien und äußerst schnelle Wiederherstellung ausführen.

In diesem Thema wird erläutert, warum Sie könnten Sie entscheiden für Sicherungskopien SQL Azure-Speicher zu verwenden und dann werden die beteiligten Komponenten. Die Ressourcen finden Sie am Ende dieses Artikels können Zugriff auf Vorgehensweisen und Weitere Informationen zu diesen Dienst mit Ihrer SQL Server-Sicherungen arbeiten beginnen.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>Vorteile der Verwendung des Azure Blob-Diensts für SQL Server Sicherungskopien

Es gibt mehrere Probleme, die beim Sichern von SQL Server bestehen. Dazu zählen unter anderem Risiko von Speicher Fehler, Zugriff auf Aufbewahrung und Hardwarekonfiguration Speicher Verwaltung. Viele dieser Herausforderung werden adressiert mithilfe des Azure Blob Store Service für SQL Server-Sicherungskopien. Berücksichtigen Sie die folgenden Vorteile:

- **Benutzerfreundliche**: Speicherung der Sicherungskopien in Azure Blobs ein praktisches werden kann, flexible und einfach zu externen Option zuzugreifen. Erstellen von Aufbewahrung für Ihre SQL Server-Sicherungskopien kann so einfach wie das Ändern der vorhandenen Skripts/Aufträge die **Sicherung zu URL** -Syntax verwenden sein. Aufbewahrung sollten in der Regel weit genug aus den Speicherort für die Herstellung Datenbank auf einem einzelnen Ausfall zu verhindern, dass die beeinflussen, die externen und Fertigung Datenbank-Speicherorte möglicherweise. Indem Sie sich [Geo Replikation Ihrer Azure-blobs](../storage/storage-redundancy.md), müssen Sie eine zusätzliche Schutzebene bei einem Ausfall, die die gesamte Region auswirken kann.
- **Sicherung Archiv**: der Azure BLOB-Speicher-Dienst bietet eine bessere Alternative zu den häufig verwendeten Band-Option zum Archivieren von Sicherungskopien. Bandspeicher möglicherweise physische Verkehrs eine externe Einrichtung und Maßnahmen zum Schutz der Medien erforderlich. Speicherung der Sicherungskopien in Azure BLOB-Speicher bietet eine Chat hochgradig verfügbar ist, und eine permanente archivieren Option.
- **Verwaltete Hardware**: Es gibt keine Aufwand für die Verwaltung von Hardware mit Azure Services. Azure Services verwalten die Hardware und Geo-Replikation für Redundanz und Schutz vor Hardware-Fehlern bereitstellen.
- **Unbegrenzte Speicher**: durch das Aktivieren einer direkten Sicherung auf Azure Blobs, haben Sie Zugriff auf nahezu unbeschränkte. Sie können auch weist Sicherung auf ein Datenträger Azure-virtuellen Computern Grenzwerte basierend auf dem Computer Größe. Es gibt eine hinsichtlich der Anzahl der Datenträger, die Sie an einer Azure-virtuellen Computern nach Sicherungskopien anfügen können. Dieser Grenzwert ist und weniger für kleinere Instanzen Berechtigungen für eine zusätzliche großen Instanz 16 Festplatten.
- **Verfügbarkeit von Sicherung**: Sicherungskopien in Azure Blobs gespeichert sind überall und jederzeit verfügbar und können für stellt auf einem lokalen SQL Server oder eine andere SQL Server ausgeführt wird in einer Azure virtuellen Computern, ohne die Notwendigkeit der Datenbank anfügen/trennen oder herunterladen und die virtuelle Festplatte anfügen einfach zur Verfügung.
- **Kosten**: bezahlen nur für den Dienst, der verwendet wird. Kann als externen und Sicherung Archivierungsoption kostengünstiger sein. Finden Sie unter der [Azure Preisgestaltung Rechner](http://go.microsoft.com/fwlink/?LinkId=277060 "Rechner Preise")und die [Preise Azure Artikel](http://go.microsoft.com/fwlink/?LinkId=277059 "Artikel Preise") für Weitere Informationen.
- **Speicher Momentaufnahmen**: Wenn in einer Azure Blob-Datenbankdateien gespeichert werden sollen und Sie die SQL Server 2016 verwenden können [Datei-Snapshot Sicherung](http://msdn.microsoft.com/library/mt169363.aspx) nahezu erfolgt Sicherungskopien und äußerst schnelle Wiederherstellung ausführen.

Weitere Informationen hierzu finden Sie unter [SQL Server-Sicherung und Wiederherstellung mit Azure Blob Storage Service](http://go.microsoft.com/fwlink/?LinkId=271617).

In den folgenden zwei Abschnitten stellen Sie vor der Azure Blob-Speicherdienst, einschließlich der erforderlichen SQL Server-Komponenten. Es ist wichtig zu verstehen, die Komponenten und ihre Interaktion erfolgreich sichern und Wiederherstellen aus der Dienst Azure Blob-Speicher.

## <a name="azure-blob-storage-service-components"></a>Azure Blob Storage Service-Komponenten

Die folgenden Azure Komponenten verwendeten beim Sichern auf den Dienst Azure Blob-Speicher.

| Komponente               | Beschreibung                          |
|---------------------|-------------------------------|
| **Speicher-Konto** | Das Speicherkonto ist der Ausgangspunkt für alle Speicherdienste. Um einen Dienst Azure BLOB-Speicher zuzugreifen, erstellen Sie zuerst ein Konto Azure-Speicher. Weitere Informationen zu Azure Blob-Speicherdienst finden Sie unter [So verwenden Sie den Dienst Azure BLOB-Speicher](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Container** | Ein Containers bietet eine Gruppierung einer Reihe von Blobs, und Sie können eine unbegrenzte Anzahl von Blobs speichern. Zum Schreiben einer SQL Server Sicherungsdatei mit einem Azure Blob-Dienst Sie mindestens den Container Root erstellt müssen. |
| **BLOB** | Eine Datei von einem beliebigen Typ und Größe. BLOBs in folgendem Format ein URL adressiert sind: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Weitere Informationen zu Seitenblobs finden Sie unter [Grundlegendes zu blockieren und Seitenblobs](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>SQL Server-Komponenten

Die folgenden SQL Server-Komponenten werden verwendet, wenn zum Dienst Azure BLOB-Speicher sichern.

| Komponente               | Beschreibung                          |
|---------------------|-------------------------------|
| **URL** | Eine URL gibt eine URI Uniform Resource Identifier () in eine eindeutige Sicherungsdatei an. Die URL wird verwendet, um den Speicherort und den Namen der Sicherungsdatei SQL Server bereitzustellen. Die URL muss zu einem tatsächlichen Blob, nicht nur einen Container verweisen. Wenn das Blob nicht vorhanden ist, wird sie erstellt. Wenn Sie eine vorhandene Blob angegeben ist, Sicherung schlägt fehl, es sei denn, die > mit Formatoption angegeben ist. Im folgenden ist ein Beispiel für die URL, Sie in den Befehl Sicherung geben: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS wird empfohlen, jedoch nicht erforderlich. |
| **Anmeldeinformationen** | Die Informationen, die erforderlich ist, herstellen und Azure BLOB-Speicher Dienst authentifizieren wird als Referenz gespeichert.  Damit SQL Server Sicherungskopien mit einer Azure Blob oder Wiederherstellen daraus zu schreiben muss eine SQL Server-Anmeldeinformationen erstellt werden. Weitere Informationen finden Sie unter [SQL Server-Anmeldeinformationen](https://msdn.microsoft.com/library/ms189522.aspx). |

> [AZURE.NOTE] Wenn Sie auswählen, kopieren und eine Sicherungsdatei zum Dienst Azure BLOB-Speicher hochladen, müssen Sie, wenn Sie planen, verwenden Sie diese Datei für Wiederherstellungsvorgänge einen Seite BLOB-Typ als Speicheroption verwenden. Wiederherstellen von einem Block Blob-Typ tritt ein Fehler auf.

## <a name="next-steps"></a>Nächste Schritte

1. Erstellen Sie ein Azure-Konto, wenn Sie eine bereits besitzen. Wenn Sie Azure bewerten möchten, sollten Sie die [kostenlose Testversion](https://azure.microsoft.com/free/).

1. Wechseln Sie dann über eine der folgenden Lernprogramme, die Sie durch Erstellen eines Kontos Speicher und Durchführen einer Wiederherstellung begleiten.

    - **SQLServer 2014**: [Lernprogramm: SQL Server 2014 Sicherung und Wiederherstellung in Microsoft Azure BLOB-Speicherdienst](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
    - **SQL Server 2016**: [Lernprogramm: verwenden den Dienst Microsoft Azure Blob-Speicher mit SQL Server 2016 Datenbanken](https://msdn.microsoft.com/library/dn466438.aspx)

1. Überprüfen Sie zusätzlichen Dokumentation mit [SQL Server-Sicherung und Wiederherstellung mit Microsoft Azure Blob-Speicher-Dienst](https://msdn.microsoft.com/library/jj919148.aspx)starten.

Wenn Probleme auftreten, überprüfen Sie im Thema [SQL Server-Sicherung URL Best Practices und Problembehandlung](https://msdn.microsoft.com/library/jj919149.aspx).

Für andere SQL Server sichern und Wiederherstellen von Optionen, finden Sie unter [Sichern und Wiederherstellen für SQL Server in Azure virtuellen Computern](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
