<properties
   pageTitle="Geschäftskontinuität Cloud - Datenbank Wiederherstellung - SQL-Datenbank | Microsoft Azure"
   description="Erfahren Sie, wie SQL Azure-Datenbank Cloud Geschäftskontinuität und Wiederherstellung der Datenbank unterstützt, und hilft beibehalten wichtiger Cloud-Anwendung, ausgeführt."
   keywords="Geschäftskontinuität, Cloud Geschäftskontinuität, Datenbank Wiederherstellung, Wiederherstellung der Datenbank"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Übersicht über Geschäftskontinuität mit Azure SQL-Datenbank

In dieser Übersicht werden die Funktionen, die SQL Azure-Datenbank für Geschäftskontinuität und Wiederherstellung enthält. Es stellt Optionen, Empfehlungen und Lernprogramme zum Wiederherstellen von Unterbrechung Ereignisse, die zu Datenverlust oder dazu führen, dass Ihre Datenbank und die Anwendung nicht mehr verfügbar sein können. Die Diskussion enthält Vorgehensweise, wenn ein Benutzer Anwendungsfehler wirkt sich auf die Datenintegrität, eine Azure Region weist einen Ausfall oder Ihrer Anwendung erfordert Wartung. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>SQL-Datenbank-Features, die Sie verwenden können, um Geschäftskontinuität bereitzustellen.

SQL-Datenbank bietet mehrere Business Continuity-Funktionen, einschließlich automatische Sicherungskopien und optional Datenbankreplikation. Jede weist verschiedene Merkmale für geschätzte Wiederherstellungszeit (Einfügen) und Datenverluste für zuletzt verwendete Transaktionen. Nachdem Sie diese Optionen verstehen, können Sie zwischen ihnen - wählen und in den meisten Szenarien zusammen verwenden, für die verschiedenen Szenarios. Bei der Entwicklung Ihrer Business Continuity-Plan, müssen Sie die zulässige maximale Zeit vor die Anwendung vollständig, nach dem das Ereignis Unterbrechung wiederhergestellt – dies der Wiederherstellung Zeit Ziel (RTO ist) zu verstehen. Müssen Sie auch die maximale Größe des zuletzt verwendete Datenupdates (Zeitintervall) kennen, die Anwendung tolerieren kann, verloren gehen, wenn nach dem Unterbrechung Ereignis - Wiederherstellung Punkt Ziel (RPO) wiederherstellen. 

In der folgenden Tabelle vergleicht die einfügen und RPO für die am häufigsten verwendeten drei Szenarien an.

| Funktion |  Grundlegende Ebene | Standard-Ebene  | Premium Ebene |
|---|---|---|---|
| Zeigen Sie im Feld Zeit wiederherstellen aus einer Sicherung | Wiederherstellen eines Punkt, innerhalb von 7 Tagen   | Einen Punkt in 35 Tagen wiederherstellen  | Einen Punkt in 35 Tagen wiederherstellen |
Geo-Wiederherstellen von Sicherungskopien Geo repliziert | Einfügen < 12h, RPO < 1h   | Einfügen < 12h, RPO < 1h   | Einfügen < 12h, RPO < 1h |
|Aktive Geo-Replikation | Einfügen < 30s, RPO < 5 s   | Einfügen < 30s, RPO < 5 s | Einfügen < 30s, RPO < 5 s |


### <a name="use-database-backups-to-recover-a-database"></a>Verwenden Sie die Datenbanksicherungskopien zum Wiederherstellen einer Datenbank

SQL-Datenbank führt automatisch eine Kombination aus vollständigen Datenbanksicherungskopien wöchentlich, Differenz-Datenbank stündlich, und Transaktion Log Sicherungen fünf Minuten an Ihr Unternehmen vor Datenverlust schützen. Diese Sicherungskopien werden in lokal redundante Speicherung für 35 Tagen für Datenbanken, die in der Standard- und Premium Service-Stufen und sieben Tage für Datenbanken in der Standard-Ebene gespeichert – finden Sie unter [Service Ebenen](sql-database-service-tiers.md) Weitere Details auf Dienst-Kategorien. Wenn der Aufbewahrungszeitraum für Ihre Service-Leiste nicht Ihren geschäftlichen Anforderungen erfüllt, können Sie durch [Ändern der Dienstebene](sql-database-scale-up.md)den Aufbewahrungszeitraum erhöhen. Die vollständige und Differenz-Datenbank, die Sicherungskopien auch eines [gepaarten Datacenter](../best-practices-availability-paired-regions.md) für Schutz vor einem Data Center Ausfall - repliziert werden finden Sie unter [Automatische Datenbanksicherungskopien](sql-database-automated-backups.md) für weitere Details.

Sie können diese automatische Datenbanksicherungskopien zum Wiederherstellen einer Datenbank aus verschiedenen Unterbrechung Ereignisse, innerhalb der Data Center sowohl auf einem anderen Datacenter verwenden. Verwenden die automatische Datenbanksicherungskopien, hängt von geschätzten Zeitpunkt der Wiederherstellung verschiedene Faktoren, einschließlich der Gesamtzahl von Datenbanken, die in der gleichen Region bei gleichzeitig, die Größe der Datenbank, die Größe der Protokolldatei Transaktionen und Bandbreite wiederherstellen. In den meisten Fällen ist die Wiederherstellungszeit weniger als 12 Stunden. Wenn auf einem anderen Datenbereich wiederherstellen, ist Datenverluste auf 1 Stunde durch die Geo redundante Speicherung stündlich Differenz Datenbanksicherungskopien beschränkt. 

> [AZURE.IMPORTANT] Zum Wiederherstellen automatisierte Sicherungskopien verwenden, müssen Sie Mitglied der Teilnehmerrolle für SQL Server oder der Abonnementbesitzer - finden Sie unter [RBAC: Standardrollen](../active-directory/role-based-access-built-in-roles.md). Sie können die von der Azure-Portal, PowerShell oder die REST-API wiederherstellen. Sie können keine Transact-SQL verwenden.

Automatische Sicherungskopien Ihrer Business Continuity und Wiederherstellung dafür verwenden, wenn eine Anwendung:

- Aufgabe kritische gilt nicht.
- Eine Bindung Vereinbarung zum SERVICELEVEL verfügt nicht über daher der Ausfall von 24 Stunden oder mehr nicht finanzielle Haftung dazu führen wird.
- Hat eine niedrige Anzahl Daten ändern (niedrig Transaktionen pro Stunde) und verlieren bis zu eine Stunde nach der Änderung ist ein akzeptabler Datenverlust. 
- Sind die Kosten vertrauliche. 

Wenn Sie schneller Wiederherstellung benötigen, verwenden Sie die [Aktive Geo-Replikation](sql-database-geo-replication-overview.md) (Nächstes beschrieben). Wenn Sie zum Wiederherstellen von Daten aus einer Periode, die älter als 35 Tage, sollten die Datenbank regelmäßig, um eine Datei BACPAC Archivierung kann müssen gespeichert (eine komprimierte Datei, die Ihre Datenbankschema und die zugeordneten Daten enthält) in Azure Blob-Speicher oder in einem anderen Speicherort Ihrer Wahl. Weitere Informationen zum Erstellen einer konsistent Datenbankarchiv finden Sie unter [Erstellen einer Datenbankkopie](sql-database-copy.md) , und [die Datenbankkopie exportieren](sql-database-export.md). 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Verwenden von aktiven Geo-Replikation schnellere Wiederherstellung und eine Wiederherstellung zugeordnet Datenverluste einzuschränken

Zusätzlich zur Verwendung von Datenbanksicherungskopien für die Datenbank Wiederherstellung im Falle einer Unterbrechung Business, können Sie [Aktiven Geo-Replikation](sql-database-geo-replication-overview.md) zum Konfigurieren einer Datenbank um bis zu vier lesbare sekundäre Datenbanken in die Bereiche Ihrer Wahl haben. Diese sekundären Datenbanken bleiben mit der primären Datenbank mit einem Verfahren asynchrone Replikation synchronisiert. Dieses Feature wird verwendet, um Business Unterbrechung bei einer Data Center Ausfall oder während der Aktualisierung Anwendung gescannt wird. Aktive Geo-Replikation kann auch verwendet werden, eine bessere abfrageleistung für schreibgeschützte Abfragen weit auseinander liegenden Benutzern zur Verfügung gestellt.

Wenn die primäre Datenbank unerwartet offline geht oder Sie es offline für Wartungsaktivitäten durchführen müssen, können Sie schnell eine sekundäre um Konvertierung in der primären (auch einen Failover genannt) und Konfigurieren von Applications Verbindung mit dem neu heraufgestufte primären heraufstufen. Mit einem geplanten Failover gibt es keine Daten verloren. Mit einer ungeplanten Failover möglicherweise einige wenige Datenverlust für sehr jüngsten Transaktionen aufgrund der Art der asynchrone Replikation. Nach einem Failover können Sie später Failback - entsprechend einen Plan oder, wenn das Data Center wieder online ist. In allen Fällen Benutzer erzielen Sie einigen wenigen Ausfall und erneut eine Verbindung herstellen müssen. 

> [AZURE.IMPORTANT] Wenn aktive Geo-Replikation verwenden möchten, müssen Sie Abonnementbesitzer sein oder Administratorberechtigungen in SQL Server. Sie können konfigurieren und System durch Verwendung von Azure-Portal, PowerShell oder die REST-API Berechtigungen für das Abonnement oder indem Transact-SQL-Berechtigungen in SQL Server verwenden.

Verwenden Sie aktiven Geo-Replikation aus, wenn eine Anwendung eine der folgenden Kriterien erfüllt:

- Ist die Aufgabe entscheidend.
- Verfügt über eine Vereinbarung zum Servicelevel (Vereinbarung zum SERVICELEVEL), die nicht mindestens 24 Stunden Ausfall zulässt.
- Ausfallzeiten führt finanzielle Haftung.
- Verfügt über eine hohe Rate Ändern von Daten ist ausreichend und eine Stunde nach Daten verlieren ist nicht zulässig.
- Der Mehrkosten aktiven Geo-Replikation ist kleiner als der potenziellen finanzielle Haftung und zugehörigen Verlust der Business.

## <a name="recover-a-database-after-a-user-or-application-error"></a>Wiederherstellen einer Datenbank nach einem Benutzer oder einer Anwendung Fehler

* Niemand ist perfekten! Ein Benutzer möglicherweise versehentlich löschen einiger Daten, versehentlich ziehen eine Tabelle wichtige, oder sogar eine gesamte Datenbank. Oder eine Anwendung möglicherweise gute Daten mit ungültige Daten aufgrund einen Anwendungsfehler versehentlich überschreiben. 

In diesem Szenario sind folgende Optionen bei der Wiederherstellung.

### <a name="perform-a-point-in-time-restore"></a>Führen Sie eine Point-in-Time-Wiederherstellung

Die automatische Sicherung können Sie eine Kopie der Datenbank zu einem bekannten guten Zeitpunkte wiederherstellen, vorausgesetzt, dass innerhalb der Datenbank Aufbewahrungszeitraum ist. Nachdem Sie die Datenbank wiederhergestellt wird, können Sie ersetzen die ursprüngliche Datenbank durch die wiederhergestellte Datenbank oder kopieren die benötigten Daten aus den wiederhergestellten Daten in der ursprünglichen Datenbank. Wenn die Datenbank aktiv Geo-Replikation verwendet, wird empfohlen, kopieren die benötigten Daten aus der wiederhergestellte Kopie in der ursprünglichen Datenbank. Wenn Sie die ursprüngliche Datenbank durch die wiederhergestellte Datenbank zu ersetzen, müssen Sie neu zu konfigurieren und erneut synchronisieren aktiven Geo-Replikation (was einige Zeit für eine große Datenbank in Anspruch nehmen kann). 

Weitere Informationen und detaillierte Anweisungen zum Wiederherstellen einer Datenbank zu einem Punkt Zeitpunkt mithilfe des Azure-Portals oder mithilfe der PowerShell finden Sie unter [- Zeitpunkt wiederherstellen](sql-database-recovery-using-backups.md#point-in-time-restore). Mit Transact-SQL können nicht wiederhergestellt werden.

### <a name="restore-a-deleted-database"></a>Wiederherstellen einer gelöschten Datenbank

Wenn die Datenbank wird gelöscht, aber nicht der logische Server gelöscht wurde, können Sie die gelöschte Datenbank bis zu dem Punkt wiederherstellen, an denen sie gelöscht wurde. Dies stellt eine Sicherungskopie der Datenbank auf demselben logischen SQLServer aus dem sie gelöscht wurde. Sie mit dem ursprünglichen Namen wiederherstellen können oder geben Sie einen neuen Namen oder die wiederhergestellte Datenbank.

Weitere Informationen und detaillierte Anweisungen zum Wiederherstellen einer gelöschten Datenbank mithilfe des Azure-Portals oder mithilfe der PowerShell finden Sie unter [Wiederherstellen einer gelöschte Datenbank](sql-database-recovery-using-backups.md#deleted-database-restore). Mit Transact-SQL können nicht wiederhergestellt werden.

> [AZURE.IMPORTANT] Wenn der logische Server gelöscht wird, kann keine gelöschte Datenbank wiederhergestellt werden. 

### <a name="import-from-a-database-archive"></a>Importieren Sie aus einer Datenbankarchiv

Wenn der Verlust von Daten, die außerhalb der aktuellen Aufbewahrungszeitraum für automatische Sicherungskopien aufgetreten ist, und Sie die Datenbank Archivierung wurde haben, können Sie in einer neuen Datenbank [Archivierte BACPAC Datei importieren](sql-database-import.md) . An diesem Punkt können Sie entweder ersetzen die ursprüngliche Datenbank durch die importierten Datenbank, oder kopieren die benötigten Daten aus den importierten Daten in die ursprüngliche Datenbank. 

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Wiederherstellen einer Datenbank in eine andere Region nach einem Azure Regionsdaten Center Ausfall

<!-- Explain this scenario -->

Obwohl seltene, kann ein Azure Data Center einen Ausfall haben. Bei einem Ausfall, bewirkt, dass sie eine Business Unterbrechung, die möglicherweise nur ein paar Minuten dauern oder möglicherweise für Stunden dauern. 

- Eine Möglichkeit ist für die Datenbank wieder online ist, wenn der Data Center Ausfall über warten. Dies funktioniert für Applikationen, die sich, dass die Datenbank offline leisten können. Ein Entwicklungsprojekt oder eine kostenlose Testversion brauchen Sie beispielsweise ständig arbeiten. Bei einem Data Center einen Ausfall aufweist, wissen Sie nicht wie lange der Ausfall dauert, damit diese Option nur, funktioniert Wenn Sie Ihre Datenbank für eine Weile benötigen.
- Eine weitere Möglichkeit ist entweder Failover auf ein anderes Datenbereich, wenn mithilfe von aktiven Geo-Replikation oder das Wiederherstellen Geo redundante Datenbanksicherungskopien (Geo-wiederherstellen) verwenden. Nur ein paar Sekunden dauert ein Failover während der Wiederherstellung von Sicherungskopien Stunden dauert.

Wenn Sie die Aktion ausführen, wie lange Sie wiederherstellen dauert, und wie viel Sie bei einem Ausfall der Data Center anfallen Datenverlust hängt davon ab, wie Sie die Business Continuity-Funktionen, die in der Anwendung oben beschriebenen verwenden möchten. Sie können tatsächlich um, verwenden Sie eine Kombination aus Datenbanksicherungskopien und aktiven Geo-Replikation, je nach Ihren Anforderungen Anwendung. Eine Erläuterung Anwendung Entwurf Aspekten für eigenständige Datenbanken und flexible PoolsVerwendung Business Continuity-Funktionen finden Sie unter [Entwurf eine Anwendung für die Cloud Wiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md) und [Strategien für flexible Ressourcenpool zur Wiederherstellung](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

Die folgenden Abschnitte bieten einen Überblick über die Schritte zum Wiederherstellen mit Datenbanksicherungskopien oder aktiven Geo-Replikation. Detaillierten Schritte, einschließlich der Planung Anforderungen, Beitrag Wiederherstellungsschritte und Informationen dazu, wie Sie einen Ausfall simulieren eines Disaster Wiederherstellung Drillup ausführen finden Sie unter [Wiederherstellen einer SQL-Datenbank nach einem Ausfall](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Bereiten für eine einem Dienstausfall vor

Unabhängig von der Business Continuity-Features, die Sie verwenden, müssen Sie folgende Aktionen ausführen:

- Identifizieren Sie und bereiten Sie der Ziel-Server, einschließlich Server Ebene Firewall-Regeln, Benutzernamen und Berechtigungen der Websitesammlungsebene master-Datenbank vor.
- Bestimmen Sie, wie Kunden und Clientanwendungen auf den neuen Server umleiten
- Dokument anderen Abhängigkeiten an, wie z. B. Überwachung Einstellungen und Benachrichtigungen 
 
Wenn Sie nicht planen und ordnungsgemäß vorbereiten, bringen Ihre Programme online nach einem Failover oder einer Wiederherstellung zusätzliche Zeit in Anspruch nimmt und wahrscheinlich auch erfordern Sie betonen - eine fehlerhafte Kombination nacheinander Problembehandlung.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>Failover zu einer sekundären Geo repliziert-Datenbank 

Wenn Sie Ihre Wiederherstellung dafür, [erzwingen Sie ein Failover zu einem sekundären Geo repliziert](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database)Active Geo-Replikation verwenden. Innerhalb von Sekunden des sekundären wird zur neuen primären höher gestuft und neue Transaktionen aufzeichnen und Antworten auf Abfragen - mit nur ein paar Sekunden Datenverlust für die Daten, die noch nicht repliziert hatten bereit ist. Informationen zum Automatisieren des Failovervorgangs finden Sie unter [Entwurf eine Anwendung für die Cloud Wiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [AZURE.NOTE] Wenn das Data Center wieder online ist, können Sie mit dem ursprünglichen primären Failback (oder nicht).

### <a name="perform-a-geo-restore"></a>Führen Sie eine Geo-Wiederherstellung 

Bei Verwendung von Automatische Sicherungskopien mit Replikation Geo redundante Speicherung Ihrer Wiederherstellung dafür, [Initiieren der Wiederherstellung einer Datenbank mit Geo-wiederherstellen](sql-database-disaster-recovery.md#recover-using-geo-restore). Wiederherstellung wird in der Regel findet, innerhalb von 12 Stunden – mit Datenverlust von bis zu einer Stunde durch wann bestimmt die letzte stündlich Differenz Sicherung mit ergriffen und repliziert. Bis die Wiederherstellung abgeschlossen ist, ist die Datenbank keine Transaktionen aufzeichnen oder Antworten auf Abfragen. 

> [AZURE.NOTE] Wenn das Data Center wieder online ist, bevor Sie Ihrer Anwendung die wiederhergestellte Datenbank zu wechseln, können Sie einfach die Wiederherstellung Abbrechen.  

### <a name="perform-post-failover--recovery-tasks"></a>Führen Sie die abschließenden Failover / Aufgaben für die Wiederherstellung 

Nach der Wiederherstellung aus beiden Wiederherstellungsverfahren müssen Sie die folgenden zusätzlichen Aufgaben ausführen, bevor Sie Ihre Benutzer und Applikationen sind wieder einsatzbereit:

- Redirect-Clients und -Clientanwendungen auf dem neuen Server, und wiederhergestellte Datenbank
- Sicherstellen Sie, dass die entsprechenden Server Ebene Firewall-Regeln sind für Benutzer eine Verbindung herstellen (oder verwenden die [Datenbank Ebene Firewalls](sql-database-firewall-configure.md#creating-database-level-firewall-rules))
- Sicherstellen Sie, dass die entsprechenden Benutzernamen und Berechtigungen der Websitesammlungsebene master-Datenbank vorhanden sind (oder verwenden Sie die [enthaltenen Benutzer](https://msdn.microsoft.com/library/ff929188.aspx))
- Konfigurieren von ü, je nach Bedarf
- Konfigurieren von Benachrichtigungen, je nach Bedarf

## <a name="upgrade-an-application-with-minimal-downtime"></a>Aktualisieren einer Anwendung mit minimalen Ausfallzeiten

In einigen Fällen muss eine Anwendung aufgrund der geplanten Wartung wie ein Anwendungsupgrade offline verfügbar gemacht werden. [Verwalten Anwendungsupgrades](sql-database-manage-application-rolling-upgrade.md) beschrieben, wie Sie die aktiven Geo-Replikation verwenden, um parallele Updates Ihrer Anwendung "Cloud" Ausfallzeiten bei Upgrades reduzieren, und geben Sie einen Pfad für die Wiederherstellungsdatei ein, für den Fall ein Problem auftritt, zu ermöglichen. Dieser Artikel befasst sich mit zwei verschiedenen Verfahren den Upgradevorgang orchestriert und erläutert die vor- und Nachteile der einzelnen Optionen.

## <a name="next-steps"></a>Nächste Schritte

Eine Erläuterung Anwendung Entwurf Aspekten für eigenständige Datenbanken und flexible Pools finden Sie unter [Entwurf eine Anwendung für die Cloud Wiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md) und [Strategien für flexible Ressourcenpool zur Wiederherstellung](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).






