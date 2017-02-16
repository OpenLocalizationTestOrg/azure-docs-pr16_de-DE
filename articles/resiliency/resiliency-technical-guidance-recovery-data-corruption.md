<properties
   pageTitle="Stabilität technischen Leitfaden zum Wiederherstellen von Beschädigung der Daten oder unbeabsichtigtes löschen | Microsoft Azure"
   description="Artikel auf verstehen, wie Daten einer Beschädigung der Daten oder Daten versehentlich löschen auf und Vergleich robuste, hohen Verfügbarkeit Fehlerstrukturanalyse-Anwendungen entwerfen sowie Planung für die Wiederherstellung"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-data-corruption-or-accidental-deletion"></a>Azure Stabilität technischen Leitfaden: Wiederherstellung Beschädigung der Daten oder unbeabsichtigtes löschen

Teil einer robuste Business Continuity-Plan hat einen Plan Probleme, wenn Ihre Daten beschädigt oder versehentlich gelöscht werden. Im folgenden finden Informationen zur Wiederherstellung nach Daten beschädigt oder aufgrund von Anwendungsfehler oder Operator Fehler versehentlich gelöscht wurde.

##<a name="virtual-machines"></a>Virtuellen Computern

Verwenden Sie zum Schutz Ihrer Azure-virtuellen Computern (auch als Infrastruktur als Dienst virtuellen Computern bezeichnet) Anwendungsfehler oder unbeabsichtigtes Löschvorgang [Azure Sicherung](https://azure.microsoft.com/services/backup/)aus. Sicherung Azure ermöglicht die Erstellung von Sicherungskopien, die auf mehrere virtueller Computer Datenträger konsistent sind. Darüber hinaus können die Sicherung Tresor in Regionen Wiederherstellung von Region Verlust bereitstellen repliziert werden.

##<a name="storage"></a>Speicher

Beachten Sie, dass während Azure-Speicher Daten Stabilität durch automatisierte Replikate bereitstellt, dies nicht der Anwendungscode (oder Entwickler/Benutzer) von Daten zu beschädigen verhindert durch unbeabsichtigtes oder unbeabsichtigte löschen, aktualisieren und usw. ein. Verwalten von Daten Genauigkeit unter Anwendung oder Benutzer Fehler erfordert komplexere Verfahren, wie beispielsweise Kopieren der Daten in einer sekundären Speicherposition mit ein Überwachungsprotokoll. Entwickler können Blob [Snapshot-Funktion](https://msdn.microsoft.com/library/azure/ee691971.aspx), nutzen die schreibgeschützte Point-in-Time Momentaufnahmen Blob Inhaltsverzeichnis erstellen können. Dies kann als Grundlage für eine Lösung Daten Genauigkeit für Azure-Speicher Blobs verwendet werden.

###<a name="blob-and-table-storage-backup"></a>BLOB und Tabelle Speicher sichern.

Während Blobs und Tabellen hochgradig dauerhaften sind, geben sie stets den aktuellen Status der Daten aus. Wiederherstellung von unerwünschten ändern oder Löschen von Daten möglicherweise wiederherstellen von Daten in einem vorherigen Zustand erforderlich. Dies kann erreicht werden, indem Nutzen der Funktionen von Azure zum Speichern und Point-in-Time Kopien beibehalten.

Für Azure-Blobs kann Point-in-Time Sicherungskopien mit dem [Blob Snapshot-Funktion](https://msdn.microsoft.com/library/ee691971.aspx)ausgeführt werden. Für jeden Snapshot unterliegen Sie nur für den Speicher zum Speichern der Unterschiede innerhalb der Blob, seit der letzten Zustand snapshot erforderlich. Die Momentaufnahmen hängen das Vorhandensein des ursprünglichen Blob, die, dem Sie auf, basieren, damit ein Kopiervorgang zu einem anderen Blob oder sogar ein anderes Speicherkonto ratsam ist. Dadurch wird sichergestellt, dass die Sicherung Daten ordnungsgemäß gegen unbeabsichtigtes Löschen geschützt ist. Für Azure-Tabellen können Sie zu einer anderen Tabelle oder Azure Blobs Point-in-Time Kopien machen. Weitere detaillierte Leitfäden und Beispiele für die Anwendung Ebene von Sicherungskopien von Tabellen und Blobs finden Sie hier:

  * [Schützen von Tabellen gegen Anwendungsfehler](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/05/03/protecting-your-tables-against-application-errors/)
  * [Schützen von Ihrem Blobs gegen Anwendungsfehler](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/04/29/protecting-your-blobs-against-application-errors/)

##<a name="database"></a>Datenbank

Mehrere [Geschäftskontinuität](../sql-database/sql-database-business-continuity.md) (Wiederherstellen) Optionen stehen zur Verfügung für Azure SQL-Datenbank. Datenbanken können mithilfe der Funktion [Datenbank kopieren](../sql-database/sql-database-copy.md) oder durch [Exportieren](../sql-database/sql-database-export.md) und [Importieren](https://msdn.microsoft.com/library/hh710052.aspx) von einer SQL Server-Datei Bacpac kopiert werden. Datenbankkopie bietet überführen konsistente Ergebnisse, während eine Bacpac (über den Import/Export-Dienst) nicht der Fall ist. Beide der folgenden Optionen als Warteschlange-basierte Dienste innerhalb des Data Center ausführen, und sie bieten keine aktuell eine Uhrzeit-zu-Abschluss Vereinbarung zum SERVICELEVEL.

>[AZURE.NOTE]Die Datenbankoptionen kopieren und importieren/exportieren platzieren ein hohes Maß Laden auf der Quelldatenbank an. Sie können Ressourcenkonflikten oder begrenzungsebene Ereignisse auslösen.

###<a name="sql-database-backup"></a>SQL-Datenbank sichern

Point-in-Time Sicherungskopien für Microsoft Azure SQL-Datenbank, die durch [Kopieren der SQL Azure-Datenbank](../sql-database/sql-database-copy.md)erzielt werden. Sie können diesen Befehl verwenden, um eine konsistent Kopie einer Datenbank auf dem gleichen logischen Datenbankserver oder auf einem anderen Server erstellen. In beiden Fällen wird die Datenbankkopie voll funktionsfähig und vollständig unabhängig von der Quelldatenbank. Jede erstellte Kopie, stellt eine Point-in-Time-Wiederherstellungsoption dar. Sie können den Zustand der Datenbank vollständig wiederherstellen, durch die neue Datenbank mit der Name der Quelldatenbank umbenennen. Alternativ können Sie eine bestimmte Teilmenge der Daten aus der neuen Datenbank wiederherstellen, mithilfe von Transact-SQL-Abfragen. Weitere Details zu SQL-Datenbank finden Sie unter [Übersicht über Geschäftskontinuität mit Azure SQL-Datenbank](../sql-database/sql-database-business-continuity.md).

###<a name="sql-server-on-virtual-machines-backup"></a>SQLServer auf virtuellen Computern Sicherung

Für SQL Server als ein Dienst virtuellen Computern (oft IaaS oder IaaS virtuellen Computern bezeichnet) mit Azure-Infrastruktur verwendet, es gibt zwei Optionen: traditionelle Sicherungskopien und Log Versand. Traditionelle Sicherungskopien verwenden, können Sie bis zu einem bestimmten Zeitpunkt wiederherstellen, aber langsamer des Wiederherstellungsvorgangs. Traditionelle Sicherungskopien wiederherstellen erfordert, beginnend mit Beginn eine vollständige Sicherung, und klicken Sie dann anwenden Sicherungskopien anschließend geöffnet. Die zweite Option ist so konfigurieren Sie ein Protokoll Liefer-Sitzung zum Verzögern der Wiederherstellung für zur Verfügung (z. B., indem Sie zwei Stunden). Dadurch wird ein Fenster zum Wiederherstellen von Fehlern, die auf dem primären vorgenommen.

##<a name="other-azure-platform-services"></a>Andere Dienste Azure-Plattform

Einige Dienste Azure-Plattform Informationen in einer benutzergesteuerte Speicher-Konto oder Azure SQL-Datenbank gespeichert. Wenn die Ressource Firmen- oder Speicher gelöscht oder beschädigt ist, kann dies mit dem Dienst Probleme verursachen. In diesen Fällen ist es wichtig, Sicherungskopien beizubehalten, mit denen Sie diese Ressourcen neu zu erstellen, wenn sie gelöscht oder beschädigt wurden würden.

Für Azure-Websites und Azure Mobile Dienste müssen Sie sichern und die zugehörigen Datenbanken verwalten. Für Azure Media-Dienst und virtuelle Maschinen müssen Sie das zugeordnete Speicherplatz Azure-Konto und alle Ressourcen, die in diesem Konto warten. Beispielsweise müssen Sie für virtuelle Maschinen, Sichern und Verwalten von virtuellen Computer Datenträger im Azure Blob-Speicher.

##<a name="checklists-for-data-corruption-or-accidental-deletion"></a>Checklisten zum Beschädigung der Daten oder unbeabsichtigtes löschen

##<a name="virtual-machines-checklist"></a>Checkliste virtuellen Computern

  1. Überprüfen Sie im Abschnitt virtuellen Computern dieses Dokuments.
  2. Sichern Sie und verwalten Sie der Datenträger virtueller Computer mit Azure Sicherung (oder eigene zusätzliche System mithilfe von Azure Blob-Speicher sowie virtuelle Festplatte Momentaufnahmen).

##<a name="storage-checklist"></a>Checkliste für Speicher

  1. Überprüfen Sie im Abschnitt Speicher dieses Dokuments.
  2. Sichern Sie regelmäßig kritischen Speicherressourcen ein.
  3. Erwägen Sie die Snapshot-Funktion für Blobs.

##<a name="database-checklist"></a>Checkliste für die Datenbank

  1. Überprüfen Sie im Abschnitt Datenbank dieses Dokuments.
  2. Erstellen Sie Point-in-Time Sicherungskopien mithilfe des Befehls Kopie der Datenbank ein.

##<a name="sql-server-on-virtual-machines-backup-checklist"></a>SQL Server auf virtuellen Computern Sicherung Checkliste

  1. Überprüfen von SQL Server auf virtuellen Computern Sicherung Abschnitt dieses Dokuments.
  2. Sollten Sie herkömmliche Sicherung und Wiederherstellung Techniken.
  3. Erstellen Sie ein verzögerte Protokoll Liefer-Sitzung ein.

##<a name="web-apps-checklist"></a>Web Apps-Checkliste

  1. Sichern und Verwalten von zugeordneten Datenbank, sofern vorhanden.

##<a name="media-services-checklist"></a>Checkliste für Media-Dienste

  1. Sichern und Verwalten von zugeordneten Speicherressourcen.

##<a name="more-information"></a>Weitere Informationen

Weitere Informationen zu Funktionen zum Sichern und Wiederherstellen in Azure finden Sie unter [Speicher, Sicherung und Wiederherstellung Szenarien](https://azure.microsoft.com/documentation/scenarios/storage-backup-recovery/).

##<a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist Teil einer Serie [Azure Stabilität technischen Leitfaden](./resiliency-technical-guidance.md)dienten. Wenn Sie weitere Stabilität, Wiederherstellung und hohen Verfügbarkeit Ressourcen suchen, finden Sie in Azure Stabilität technischen Leitfaden [zusätzlichen Ressourcen](./resiliency-technical-guidance.md#additional-resources).