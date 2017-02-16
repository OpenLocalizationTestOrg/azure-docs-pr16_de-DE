<properties
   pageTitle="SQL-Datenbank Wiederherstellung | Microsoft Azure"
   description="Informationen Sie zum Wiederherstellen einer Datenbank aus einer regionale Datenzentrum Ausfall oder eines Fehlers mit Azure SQL-Datenbank aktiv Geo-Replikation und Funktionen Geo-wiederherstellen."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Wiederherstellen einer Azure SQL-Datenbank oder Failover zu einem sekundären

Azure SQL-Datenbank bietet die folgenden Funktionen zum Wiederherstellen nach einem Ausfall:

- [Aktive Geo-Replikation](sql-database-geo-replication-overview.md)
- [Geo-wiederherstellen](sql-database-recovery-using-backups.md#point-in-time-restore)

Weitere Informationen zu Business Continuity Szenarien und die Funktionen zur Unterstützung von diese Szenarios finden Sie unter [Geschäftskontinuität](sql-database-business-continuity.md).

### <a name="prepare-for-the-event-of-an-outage"></a>Bereiten Sie für das Ereignis von einer einem Dienstausfall vor

Bei Erfolg mit Wiederherstellung auf einem anderen Datenbereich mit aktiven Geo-Replikation oder Geo redundante Sicherungskopien, müssen Sie Vorbereiten eines Servers in einer anderen Datacenter sollte Ausfall der neue primäre Server vorgesehen ist die Notwendigkeit entstehen als auch Schritte dokumentierten und eine reibungslose Wiederherstellung sichergestellt gut definiert haben. Diese vorbereitenden Schritte umfassen:

- Identifizieren Sie den logischen Server in einem anderen Bereich den neuen primären Server vorgesehen ist. Mit aktiven Geo-Replikation, wird dies mindestens ein und vielleicht jeden sekundären Server sein. Für Geo-wiederherstellen wird dies in der Regel einem Server in der [Region für](../best-practices-availability-paired-regions.md) die Region sein, in dem die Datenbank gespeichert ist.
- Identifizieren Sie und optional definieren Sie, die Server Ebene Firewallregeln auf für Benutzer für den neuen primären Datenbankzugriff erforderlich.
- Stellen Sie fest, wie Sie Benutzer auf den neuen primären Server, beispielsweise indem Verbindungszeichenfolgen ändern oder Ändern von DNS-Einträge umleiten abgelegt werden.
- Identifizieren Sie und optional erstellen Sie, die Benutzernamen, die in der master-Datenbank auf dem neuen primären Server vorhanden sein müssen, und stellen Sie sicher, dass diese Benutzernamen geeignete Berechtigungen in der master-Datenbank, wenn alle haben. Weitere Informationen finden Sie unter [Sicherheit der SQL-Datenbank nach der Wiederherstellung](sql-database-geo-replication-security-config.md)
- Identifizieren von Warnungsregeln, die aktualisiert werden, um die neue primäre Datenbank zuzuordnen.
- Dokument in der aktuellen primären Datenbank die ü Konfiguration
- Führen Sie eine [Wiederherstellung ausführen von Drilldowns oder](sql-database-disaster-recovery-drills.md). Wenn Sie einen Ausfall für Geo-Wiederherstellung simulieren, können Sie löschen oder umbenennen die Quelldatenbank zum Auslösen des Connectivity Anwendungsfehler. Um einem Ausfall für aktive Geo-Replikation simulieren möchten, können Sie die Web-Anwendung oder virtuellen Computern, die auf die Datenbank oder Failover der Datenbank verbunden, um Connectity auftretende Anwendungsfehler verursachen deaktivieren.

## <a name="when-to-initiate-recovery"></a>Wann Wiederherstellung initiieren

Der Wiederherstellungsvorgang wirkt sich auf die Anwendung. Ändern der SQL-Verbindungszeichenfolge oder Umleitung mit DNS erfordert und permanent Datenverlust ergeben. Daher sollte durchgeführt werden nur, wenn der Ausfall vermutlich länger als Ziel der Ihrer Anwendung Wiederherstellung Zeit dauern wird. Wenn die Anwendung, um die Herstellung bereitgestellt wird sollten Sie durchführen von regelmäßigen Überwachung der Anwendung Gesundheit und verwenden die folgenden Datenpunkte um zu bestätigen, dass die Wiederherstellung erforderlich ist:

1.  Permanenter Konnektivitätsfehler von der Anwendungsebene in der Datenbank.
2.  Azure-Portal zeigt eine Benachrichtigung über ein Vorfall in der Region mit wesentlichen Einfluss.
3.  Als heruntergestuft, wird der Azure SQL-Datenbankserver markiert.

Je nach Ihrer Anwendung Fehlertoleranz Ausfallzeiten und möglich Business Haftung können Sie die folgenden Wiederherstellungsoptionen berücksichtigen.

Verwenden Sie die [Erste wiederhergestellt Datenbank](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*), um den neuesten Geo repliziert Wiederherstellungspunkt abzurufen.

## <a name="wait-for-service-recovery"></a>Warten auf Dienst Wiederherstellung

Die Arbeit Azure Teams sorgfältig zum Wiederherstellen der Verfügbarkeit von Diensten wie schnell wie möglich, aber abhängig von der Stammwebsite es verursachen kann Stunden oder Tage dauern.  Wenn eine Anwendung längere Downtime tolerierbar können Sie einfach der Wiederherstellung durchzuführen warten. In diesem Fall ist keine Aktion Ihrerseits erforderlich. Sie können den aktuellen Dienststatus auf unsere [Azure-Dienststatus-Dashboard](https://azure.microsoft.com/status/)anzeigen. Die Verfügbarkeit der Anwendung wird nach der Wiederherstellung des Bereichs wiederhergestellt werden.

## <a name="failover-to-geo-replicated-secondary-database"></a>Failover auf sekundäre Datenbank Geo repliziert

Wenn Ihre Anwendungsausfallzeiten des Business Haftung führen kann sollten Sie Geo repliziert Datenbanken in Ihrer Anwendung verwenden. Es wird die Anwendung Verfügbarkeit in einem anderen Bereich bei einem Ausfall schnell wiederherstellen aktivieren. Erfahren Sie, wie [Geo-Replikation konfigurieren](sql-database-geo-replication-portal.md).

Zum Wiederherstellen der Verfügbarkeit von Datenbanken müssen Sie die Failovers zum Geo repliziert Secondary mithilfe einer der unterstützten Methoden einleiten.

Verwenden Sie eine der folgenden Führungslinien Failover auf eine Geo repliziert sekundäre Datenbank aus:

- [Failover auf einem Geo repliziert sekundären mithilfe von Azure-Portal](sql-database-geo-replication-portal.md)
- [Failover zu einem sekundären Geo repliziert mithilfe der PowerShell](sql-database-geo-replication-powershell.md)
- [Failover auf einem Geo repliziert sekundären mithilfe von T-SQL](sql-database-geo-replication-transact-sql.md)

## <a name="recover-using-geo-restore"></a>Wiederherstellen von einem Geo-wiederherstellen

Wenn keine Ihrer Anwendung Ausfallzeiten in Business Haftung ergibt können Geo-wiederherstellen als eine Methode Sie Ihrer Anwendung Datenbanken wiederherstellen. Eine Kopie der Datenbank erstellt aus der neuesten Geo redundante Sicherungskopie.

Verwenden Sie eine der folgenden Führungslinien auf Geo-Wiederherstellen einer Datenbank in einer neuen Region:

- [Geo-Wiederherstellen einer Datenbank zu einer neuen Region mit Azure-Portal](sql-database-geo-restore-portal.md)
- [Geo-Wiederherstellen einer Datenbank mithilfe der PowerShell neue Region](sql-database-geo-restore-powershell.md)

## <a name="configure-your-database-after-recovery"></a>Konfigurieren Sie die Datenbank nach der Wiederherstellung

Wenn Sie entweder Geo-Replikation Failover oder Geo-wiederherstellen für die Wiederherstellen nach einem Ausfall verwenden, müssen Sie sicherstellen, dass die Verbindung zu den neuen Datenbanken ordnungsgemäß konfiguriert ist, damit die Funktion normalen Anwendung fortgesetzt werden kann. Hierbei handelt es sich um eine Checkliste für Aufgaben Vorbereiten der Herstellung wiederhergestellte Datenbank.

### <a name="update-connection-strings"></a>Aktualisieren der Verbindungszeichenfolgen

Da Ihre wiederhergestellte Datenbank in einem anderen Server gespeichert werden soll, müssen Sie Ihrer Anwendung Verbindungszeichenfolge verweisen auf diesem Server zu aktualisieren.

Weitere Informationen zum Ändern von Verbindungszeichenfolgen finden Sie unter der entsprechenden Entwicklungssprache für Ihre [Verbindungsbibliothek](sql-database-libraries.md).

### <a name="configure-firewall-rules"></a>Konfigurieren der Firewall-Regeln

Sie müssen, um sicherzustellen, dass die Firewallregeln, die so konfiguriert, dass auf dem Server, und klicken Sie auf die Datenbank Etikettenlayout entsprechen, die auf dem primären Server und die primäre Datenbank konfiguriert wurden. Weitere Informationen finden Sie unter [wie: Konfigurieren der Firewall-Einstellungen (Azure SQL-Datenbank)](sql-database-configure-firewall-settings.md).


### <a name="configure-logins-and-database-users"></a>Konfigurieren von Benutzernamen und Benutzer-Datenbank

Sie müssen, um sicherzustellen, dass alle Benutzernamen, die von der Anwendung verwendeten auf dem Server vorhanden sein, das die wiederhergestellte Datenbank hostet. Weitere Informationen finden Sie unter [Sicherheitskonfiguration für Geo-Replikation](sql-database-geo-replication-security-config.md).

>[AZURE.NOTE] Konfigurieren und Testen Ihre Server-Firewall-Regeln und Benutzernamen (und seine Berechtigungen) während einer Disaster Wiederherstellung Drillup. Dieser Server Ebene Objekte und deren Konfiguration möglicherweise nicht zur Verfügung während der Ausfall.

### <a name="setup-telemetry-alerts"></a>Setup werden Benachrichtigungen

Sie müssen, um sicherzustellen, dass Ihre vorhandenen Einstellungen für die Regel zum Zuordnen der wiederhergestellte Datenbank und anderen Server aktualisiert werden.

Weitere Informationen zu Datenbank Warnungsregeln finden Sie unter [Benachrichtigung Benachrichtigungen erhalten](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) und [Nachverfolgen Dienststatus](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Aktivieren Sie die Überwachung

Wenn Überwachung Zugriff auf Ihre Datenbank erforderlich ist, müssen Sie nach der Wiederherstellung Datenbank Überwachung zu aktivieren. Ein guter Hinweis, dass die Überwachung erforderlich ist besteht darin, dass Clientanwendungen sichere Verbindungszeichenfolgen in einem Muster von *. database.secure.windows.net. Weitere Informationen finden Sie unter [Erste Schritte mit SQL-Datenbank Überwachung](sql-database-auditing-get-started.md).


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure SQL-Datenbank automatische Sicherungskopien finden Sie unter [SQL-Datenbank automatische Sicherungskopien](sql-database-automated-backups.md)
- Weitere Informationen zu Business Continuity entwerfen und Wiederherstellung Szenarien finden Sie unter [Continuity-Szenarien](sql-database-business-continuity.md)
- Weitere Informationen zum automatische Sicherungskopien für Wiederherstellung verwenden, finden Sie unter [Wiederherstellen einer Datenbank aus den Dienst initiiert Sicherungskopien](sql-database-recovery-using-backups.md)
