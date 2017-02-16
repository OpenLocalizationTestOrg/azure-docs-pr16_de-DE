<properties 
   pageTitle="SQL-Datenbank Disaster Wiederherstellung einen Drilldown | Microsoft Azure" 
   description="Leitfäden und bewährte Methoden für die Verwendung von Azure SQL-Datenbank Disaster Wiederherstellung einen Drilldown ausführen, mit dem Beibehalten der Aufgabe kritische Informationen zu Fehlern und Ausfall robuste branchenanwendungen." 
   services="sql-database" 
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="07/31/2016"
   ms.author="sstein; sashan"/>

#<a name="performing-disaster-recovery-drill"></a>Disaster Wiederherstellung Drillup ausführen

Es wird empfohlen, dass die Überprüfung der Anwendungsbereitschaft für Wiederherstellung Workflow regelmäßig durchgeführt werden. Das Verhalten der Anwendung und der Auswirkungen Überprüfen von Datenverlust und/oder die Unterbrechung umfasst die Failover empfiehlt sich engineering. Es ist auch eine Vorbedingung durch die meisten Branchenstandards als Teil des Business Continuity Zertifizierung.

Durchführen einer Disaster Wiederherstellung Drillup umfasst:

- Nachbilden einer Daten Ebene einem Dienstausfall
- Wiederherstellen von 
- Überprüfen Sie die Anwendung Integrität Beitrag wiederherstellen

Abhängig, wie Sie [Ihrer Anwendung für Geschäftskontinuität ausgelegt](sql-database-business-continuity.md), der Drillup ausführen der Workflow variieren kann. Im folgenden beschreiben wir die bewährten Methoden Durchführen einer Disaster Wiederherstellung Drillup im Kontext Azure SQL-Datenbank. 

##<a name="geo-restore"></a>Geo-wiederherstellen

Es wird empfohlen, um Datenverluste zu verhindern, dass beim Durchführen eines Disaster Wiederherstellung Drillup, der mit einer testumgebung durch Erstellen einer Kopie der Herstellung Umgebung und verwenden es zur Überprüfung der Anwendung Failover Workflow Drillup ausführen.
 
####<a name="outage-simulation"></a>Simulation von einem Dienstausfall

Um die einem Dienstausfall simulieren können Sie löschen oder umbenennen die Quelldatenbank. Dadurch wird Connectivity Anwendungsfehler. 

####<a name="recovery"></a>Wiederherstellung

- Führen Sie die Geo-Wiederherstellen der Datenbank in einem anderen Server als beschrieben [hier](sql-database-disaster-recovery.md)ein. 
- Ändern der Anwendungskonfiguration so, dass eine Verbindung mit den wiederhergestellten Datenbanken und folgen Sie den Leitfaden [Konfigurieren einer Datenbank nach der Wiederherstellung](sql-database-disaster-recovery.md) , um die Wiederherstellung abzuschließen.

####<a name="validation"></a>Überprüfung

- Führen Sie die Drillup, indem Sie die Anwendung Integrität Beitrag wiederherstellen (d. h. Verbindungszeichenfolgen, Benutzernamen, Grundfunktionen testen oder andere Validierungen Teil der standard-Anwendung Signoffs Verfahren) zu bestätigen.

##<a name="geo-replication"></a>Geo-Replikation

Für eine Datenbank, die mithilfe von Geo-Replikation geschützt ist wird die Übung Drillup Geplantes Failover auf die sekundäre Datenbank (Variablen) haben. Geplante Failover wird sichergestellt, dass der primären und sekundären Datenbanken synchron bleiben, wenn die Rollen aktiviert sind. Im Gegensatz zu den ungeplanten Failover führt dieser Vorgang nicht Datenverlust, zu, damit die Drillup in dieser Umgebung ausgeführt werden kann. 

####<a name="outage-simulation"></a>Simulation von einem Dienstausfall

Um die einem Dienstausfall simulieren können Sie die Webanwendung oder virtuellen Computern, die mit der Datenbank verbunden deaktivieren. Dies wird in der Konnektivitätsfehlern für die Webclients führen.

####<a name="recovery"></a>Wiederherstellung

- Stellen Sie sicher, dass die Anwendungskonfiguration in der Region DR auf die ehemalige sekundäre zeigt die vollständigen Zugriff auf neue primär verwendet werden soll. 
- Führen Sie [Geplanter Failover](sql-database-geo-replication-powershell.md#initiate-a-planned-failover) , um die sekundären Datenbank eine neue primäre machen aus
- Folgen Sie den Leitfaden [Konfigurieren einer Datenbank nach der Wiederherstellung](sql-database-disaster-recovery.md) , um die Wiederherstellung abzuschließen.

####<a name="validation"></a>Überprüfung

- Führen Sie die Drillup, indem Sie die Anwendung Integrität Beitrag wiederherstellen (d. h. Verbindungszeichenfolgen, Benutzernamen, Grundfunktionen testen oder andere Validierungen Teil der standard-Anwendung Signoffs Verfahren) zu bestätigen.


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Business Continuity Szenarien finden Sie unter [Continuity-Szenarien](sql-database-business-continuity.md)
- Weitere Informationen zu Azure SQL-Datenbank automatische Sicherungskopien finden Sie unter [SQL-Datenbank automatische Sicherungskopien](sql-database-automated-backups.md)
- Weitere Informationen zum automatische Sicherungskopien für Wiederherstellung verwenden, finden Sie unter [Wiederherstellen einer Datenbank aus den Dienst initiiert Sicherungskopien](sql-database-recovery-using-backups.md)
- Weitere Informationen zu schneller Wiederherstellungsoptionen finden Sie unter [Aktiv-Geo-Replikation](sql-database-geo-replication-overview.md)  
