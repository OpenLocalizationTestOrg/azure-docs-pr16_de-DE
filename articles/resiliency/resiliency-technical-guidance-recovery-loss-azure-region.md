<properties
   pageTitle="Stabilität für Wiederherstellung Verlust von einer Azure Region technischen Leitfaden | Microsoft Azure"
   description="Artikel auf Grundlegendes zu und Vergleich robuste, hohen Verfügbarkeit Fehlerstrukturanalyse-Anwendungen entwerfen sowie für die Planung für die Wiederherstellung"
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

#<a name="azure-resiliency-technical-guidance-recovery-from-a-region-wide-service-disruption"></a>Azure Stabilität technischen Leitfaden: Wiederherstellung aus einer Region organisationsweite dienststörung

Azure ist physisch und logisch in Einheiten mit dem Namen Regionen unterteilt. Eine oder mehrere Rechenzentren in der Nähe besteht eine Region aus. Zum Zeitpunkt der Erstellung dieses Dokuments weist Azure 24 Regionen auf der ganzen Welt an.

Klicken Sie unter Ausnahmefällen ist es möglich, dass die Fertigungsanlagen in einer gesamten Region nicht zugegriffen werden kann, beispielsweise aufgrund von Netzwerkfehlern verwendet werden können. Oder Fertigungsanlagen verloren vollständig, beispielsweise aufgrund einer Naturkatastrophe handeln. Dieser Abschnitt erläutert die Funktionen von Azure zum Erstellen von Anwendungen, die in Regionen verteilt werden. Abgabe können Sie um die Möglichkeit zu verringern, dass ein Fehler in einer Region anderen Regionen auswirken kann.

##<a name="cloud-services"></a>Cloud-Dienste

###<a name="resource-management"></a>Ressourcenverwaltung

Sie können Computinginstanzen auf Regionen verteilen, indem Sie einen separate Cloud-Dienst in jeder Region Ziel erstellen und dann das Bereitstellungspaket in jedem Clouddienst veröffentlichen. Beachten Sie aber, dass Verteilen des Datenverkehrs über Cloud Services in unterschiedlichen Regionen vom Anwendungsentwickler oder mit einer Datenverkehr Verwaltungsdienst implementiert werden muss.

Ermitteln der Anzahl der Rolleninstanzen von freie im Voraus bereitstellen für die Wiederherstellung ist ein wichtiger Aspekt bei der Planung von Kapazität. Gibt es eine vollständige sekundäre Bereitstellung wird sichergestellt, dass Kapazität bereits bei Bedarf zur Verfügung; Dies verdoppelt jedoch effektiv die Kosten. Ein gemeinsames Muster besteht darin, eine kleine, sekundäre Bereitstellung, nur groß genug ist, um wichtige Dienste ausgeführt haben. Diese kleine sekundäre Bereitstellung ist eine gute Idee beide Kapazität, und zum Testen der Konfigurations der Umgebung sekundäre reservieren.

>[AZURE.NOTE]Das Kontingent Abonnement ist keine Garantie für Kapazität. Das Kontingent ist einfach Kreditkarte beschränkt. Damit Kapazität sichergestellt ist, muss die erforderliche Anzahl von Rollen in das Service-Modell definiert sein, und die Rollen bereitgestellt werden müssen.

###<a name="load-balancing"></a>Lastenausgleich

Um Lastenausgleich erfordert Datenverkehr über alle Regionen eine Lösung für den Datenverkehr an. Azure bietet [Azure Datenverkehr-Manager](https://azure.microsoft.com/services/traffic-manager/). Sie können auch Dienste von Drittanbietern nutzen, die ähnlichen Datenverkehr Verwaltungsfunktionen bereitstellen.

###<a name="strategies"></a>Strategien

Viele alternative Strategien stehen für das Berechnen verteilt über Regionen implementieren. Diese müssen an die Anforderungen für bestimmte Business und die Umstände der Anwendung angepasst werden. Auf hoher Ebene können die Vorgehensweisen in den folgenden Kategorien unterteilt werden:

  * __Erneut bereitstellen auf Disaster__: In dieser Ansatz die Anwendung zum Zeitpunkt der Disaster Seitenvorlage bereitgestellt. Dies ist für nicht kritische Applikationen, die jeweils garantiert Wiederherstellung erfordern nicht geeignet.

  * __Warm freie (aktiv/passiv)__: ein sekundärer verwalteter Service wird in eine alternative Region erstellt und Rollen werden bereitgestellt, damit minimale Kapazität; sichergestellt ist jedoch empfangen nicht die Rollen Herstellung Datenverkehr. Dieser Ansatz empfiehlt sich für Applikationen, die nicht zum Verteilen von den Datenverkehr auf Regionen vorgesehen sind.

  * __Wichtiges freie (aktiv/aktiv)__: die Anwendung Herstellung Laden in mehreren Regionen erhalten soll. Die Clouddienste in jeder Region möglicherweise für größere Speicherkapazität als erforderlichen Ausfall konfiguriert sein. Sie können auch die Clouddienste skalieren möglicherweise out-gleichzeitig Disaster-Failover nach Bedarf. Dieser Ansatz erfordert umfangreiche Investitionen in Anwendungsentwurf, aber es signifikante Vorteile hat. Hierzu gehören niedrig und garantiert Wiederherstellungszeit fortlaufender aller Wiederherstellung Speicherorte und effiziente Nutzung von Kapazität testen.

Eine vollständige Erläuterung des verteilten Entwurf befindet sich außerhalb dieses Dokuments. Weitere Informationen finden Sie unter [Wiederherstellung und hohe Verfügbarkeit für Azure Applications](https://aka.ms/drtechguide).

##<a name="virtual-machines"></a>Virtuellen Computern

Wiederherstellung der Infrastruktur als eine Service (IaaS) virtuellen Computern (virtuellen Computern) ähnelt Plattform als ein Service (PaaS) Wiederherstellung weitgehend zu berechnen. Wichtige Unterschiede bestehen, jedoch, eine IaaS VM sowohl den virtuellen Computer und den Datenträger virtueller Computer besteht aus.

  * __Verwenden von Azure Sicherung Cross Region Sicherungskopien erstellen, die Anwendung konsistent sind__.
  [Sicherung Azure](https://azure.microsoft.com/services/backup/) ermöglicht Kunden Anwendung konsistent Sicherungskopien auf mehrere virtueller Computer Datenträger erstellen und Replikation von Sicherungskopien über Regionen unterstützen. Dies ist möglich, indem Sie sich Geo Replikation den Sicherung Tresor zum Zeitpunkt der Erstellung. Beachten Sie, dass die Replikation der Sicherung Tresor muss zum Zeitpunkt der Erstellung konfiguriert werden. Es kann später festgelegt werden. Wenn Sie ein Bereich verloren geht, wird Microsoft die Sicherungskopien Kunden zur Verfügung stellen. Kunden werden können an ihren Points konfigurierten wiederherstellen wiederhergestellt.

  * __Den Datenträger Daten vom Betriebssystem Datenträger zu trennen__. Ein wichtiger Aspekt für IaaS virtuelle Computer ist, dass Sie den Datenträger Betriebssystem nicht ändern können, ohne den virtuellen Computer neu zu erstellen. Dies ist kein Problem, wenn Ihre Wiederherstellungsstrategie nach Datenverlust erneut bereitstellen. Jedoch möglicherweise es ein Problem, wenn Sie den Warm freie Ansatz Kapazität reservieren verwenden. Um dies zu ordnungsgemäß implementieren, benötigen Sie den richtige Betriebssystem Datenträger auf dem primären und sekundären Speicherorte bereitgestellt, und die Anwendungsdaten auf einem separaten Laufwerk gespeichert werden müssen. Verwenden Sie, falls möglich eine standard Betriebssystem-Konfiguration, die auf beiden Orten bereitgestellt werden kann. Nach einem Failover müssen Sie dann das Datenlaufwerk an Ihre vorhandene IaaS virtuellen Computern in der sekundäre DC anfügen. Verwenden Sie zum Kopieren von Momentaufnahmen der die Daten Laufwerke an einem entfernten Standort AzCopy.

  * __Achten Sie potenzielle Konsistenzprobleme nach einem Geo-Failover von mehreren virtuellen Computer Festplatten__. Virtueller Computer Festplatten werden als Azure-Speicher Blobs implementiert und die dieselben Geo-Replikation Merkmale haben. Sofern [Azure Sicherung](https://azure.microsoft.com/services/backup/) verwendet wird, gibt es keine Garantie der Konsistenz über Festplatten, da Geo-Replikation asynchrone ist und unabhängig voneinander repliziert. Einzelne virtueller Computer Datenträger sind unbedingt in einem Absturz konsistent nach einem Geo-Failover staatliche, aber nicht konsistent über Festplatten. Dies kann in einigen Fällen (beispielsweise im Falle Festplattenstriping) Probleme verursachen.

##<a name="storage"></a>Speicher

###<a name="recovery-by-using-geo-redundant-storage-of-blob-table-queue-and-vm-disk-storage"></a>Wiederherstellung mithilfe von Geo redundante Speicherung Blob, Tabellen, Warteschlange und virtueller Computer Festplattenspeicher

In Azure, Blobs, Tabellen, Warteschlangen und virtuellen Computer sind Laufwerke alle als Geo standardmäßig repliziert. Dies wird als Geo redundante Speicher (GRS) bezeichnet. GRS repliziert Speicherdaten für ein gepaarten Datencenter Hunderte von Meilen innerhalb einer bestimmten geografischen Region auseinander. GRS soll zusätzliche Zuverlässigkeit bereitgestellt wird, falls es einem Haupt-Datacenter Datenverlust ist. Microsoft-steuert, wann ausgeführt wird und Failover beträgt Haupt-Datenverluste in der ursprünglichen primären Standort nicht behebbarer in einem angemessenen Zeitraum gilt. In einigen Szenarien kann dies mehrere Tage lang sein. Daten werden in der Regel innerhalb weniger Minuten repliziert, obwohl Synchronisierungsintervall noch nicht über eine Vereinbarung zum Servicelevel sind.

Fällt eine Geo-, werden keine Änderung wie das Konto zugegriffen wird (die Taste URL und Konto nicht geändert wird). Das Speicherkonto, jedoch in einem anderen Bereich nach Failover werden. Dies konnte Applikationen beeinflussen, die regionalen Zugehörigkeit zu ihrem Speicherkonto erfordern. Auch für Diensten und Anwendungen, die nicht über ein Speicherkonto im gleichen Datencenter erfordern möglicherweise die Cross-Datacenter Wartezeit und Bandbreite Gebühren Gründe für den Datenverkehr in der Region Failover vorübergehend zu verschieben ansprechenden werden. Dies kann in einer gesamten Wiederherstellen nach Datenverlusten berücksichtigt werden.

Zusätzlich zu den automatischen Failover von GRS bereitgestellt hat Azure einen Dienst eingeführt, der Sie auf die Kopie der Daten in der sekundäre Speicherort gelesen zugreifen können. Dies wird Lesezugriff Geo redundante Speicher (RAS-GRS) bezeichnet.

Weitere Informationen zu sowohl GRS und RAS GRS Speicher finden Sie unter [Replikation Azure-Speicher](../storage/storage-redundancy.md).

###<a name="geo-replication-region-mappings"></a>Geo-Replikation Region Zuordnungen:

Es ist wichtig, wissen, wo Ihre Daten Geo repliziert, damit Sie wissen, wo Sie die anderen Instanzen der Daten bereitstellen, die regionalen Zugehörigkeit zu Ihrem Storage erfordern. Die folgende Tabelle zeigt die Kombinationen primären und sekundären Speicherort:

[AZURE.INCLUDE [paired-region-list](../../includes/paired-region-list.md)]

###<a name="geo-replication-pricing"></a>Geo-Replikation Preise:

Geo-Replikation ist in der aktuellen Preise für Azure-Speicher enthalten. Dies wird Geo redundante Speicher (GRS) bezeichnet. Wenn Sie nicht die Daten Geo repliziert werden sollen, können Sie Geo-Replikation für Ihr Konto deaktivieren. Dies wird lokal redundante Speicherung bezeichnet, und es zu einem reduzierten Preis im Vergleich zu GRS belastet wird.

###<a name="determining-if-a-geo-failover-has-occurred"></a>Bestimmen, ob ein Geo-Failover stattgefunden hat

Bei einem Geo-Failover, wird dies in der [Azure-Dienststatus-Dashboard](https://azure.microsoft.com/status/)bereitgestellt. Applikationen können eine automatisierte Methode erkennen, jedoch durch die Überwachung des Geo-Region für ihr Speicherkonto implementieren. Dies kann zu anderen Wiederherstellungsvorgängen, wie etwa die Aktivierung von berechnen von Ressourcen in der Geo-Region auslösen, wo ihre Speicher aufgerufen, verwendet werden. Sie können mithilfe von [Speicher Kontoeigenschaften abrufen](https://msdn.microsoft.com/library/ee460802.aspx)eine Abfrage für diese von der Servicemanagement API, durchführen. Die relevanten Eigenschaften sind:

    <GeoPrimaryRegion>primary-region</GeoPrimaryRegion>
    <StatusOfPrimary>[Available|Unavailable]</StatusOfPrimary>
    <LastGeoFailoverTime>DateTime</LastGeoFailoverTime>
    <GeoSecondaryRegion>secondary-region</GeoSecondaryRegion>
    <StatusOfSecondary>[Available|Unavailable]</StatusOfSecondary>

###<a name="vm-disks-and-geo-failover"></a>Virtueller Computer Datenträger und Geo-failover

Wie im Abschnitt auf virtuellen Computer Festplatten erläutert, gibt es keine Garantie für Konsistenz der Daten auf virtuellen Computer Festplatten nach einem Failover. Um die Richtigkeit der Sicherungskopien sicherzustellen, sollte eine Sicherung Produkt wie Data Protection Manager zum Sichern und Wiederherstellen von Anwendungsdaten verwendet werden.

##<a name="database"></a>Datenbank

###<a name="sql-database"></a>SQL-Datenbank

Azure SQL-Datenbank bietet zwei Typen von Wiederherstellung: Geo-wiederherstellen und aktiven Geo-Replikation.

####<a name="geo-restore"></a>Geo-wiederherstellen

[Geo-wiederherstellen](../sql-database/sql-database-recovery-using-backups.md#geo-restore) steht auch mit Basic, Standard und Premium Datenbanken. Es stellt die Wiederherstellung Standardoption, wenn die Datenbank aufgrund von ein Vorfall in der Region nicht verfügbar ist, in dem die Datenbank gehostet wird. Ähnlich wie Point-In-Time wiederherstellen, Geo-wiederherstellen Datenbanksicherungskopien in Geo redundante Azure-Speicher benötigt. Es stellt aus der Geo repliziert Sicherungskopie wieder her, und daher ist flexibel in Bezug auf die Speicherung Ausfall in der primären Region. Weitere Informationen hierzu finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank oder Failover zu einem sekundären](../sql-database/sql-database-disaster-recovery.md).

####<a name="active-geo-replication"></a>Aktive Geo-Replikation

[Aktive Geo-Replikation](../sql-database/sql-database-geo-replication-overview.md) ist für alle Ebenen der Datenbank verfügbar. Es ist für Applikationen ausgelegt, die haben kürzere Wiederherstellung Anforderungen als Geo-wiederherstellen anbieten kann. Aktive Geo-Replikation können Sie bis zu vier lesbare sekundäre auf Servern in unterschiedlichen Regionen erstellen. Sie können eine sekundären Instanzen Failover initiieren. Darüber hinaus kann aktive Geo-Replikation verwendet werden, die Anwendung Upgrade oder Verlagerung Szenarien unterstützen, als auch Lastenausgleich für schreibgeschützt Auslastung. Weitere Informationen finden Sie unter [Geo-Replikation konfigurieren](../sql-database/sql-database-geo-replication-portal.md) und tritt ein Fehler [auf die sekundäre Datenbank auf](../sql-database/sql-database-geo-replication-failover-portal.md). Einzelheiten zum Entwerfen und implementieren Applikationen und Applikationen Upgrades ohne Ausfallzeiten finden Sie unter Entwurf eine Anwendung für die Cloud Wiederherstellung aktiven Geo-Replikation in SQL-Datenbank und [verwenden](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md) [Verwaltung parallele Updates von Applications Cloud SQL-aktive Geo Datenbankreplikation](../sql-database/sql-database-manage-application-rolling-upgrade.md) .

###<a name="sql-server-on-virtual-machines"></a>SQLServer auf virtuellen Computern

Eine Vielzahl von Optionen stehen für Wiederherstellung und hohe Verfügbarkeit für SQLServer 2012 (und höher) in Azure virtuellen Computern ausgeführt. Weitere Informationen finden Sie unter [hohe Verfügbarkeit und Wiederherstellung für SQL Server in Azure virtuellen Computern](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

##<a name="other-azure-platform-services"></a>Andere Dienste Azure-Plattform

Wenn Sie versuchen, Ihre Cloud-Dienst in mehreren Azure Regionen ausführen, müssen Sie die Auswirkungen für jede Ihrer Abhängigkeiten berücksichtigen. In den folgenden Abschnitten wird davon ausgegangen Service-spezifische Anleitung, müssen Sie den gleichen Azure-Dienst in einer alternativen Azure Datacenter verwenden. Dies umfasst sowohl Konfiguration und Daten-Replikation Aufgaben.

>[AZURE.NOTE]In einigen Fällen können dazu beitragen folgendermaßen um ein Service-spezifische Ausfall statt eines gesamten Datacenter Ereignisses zu verringern. Hinsichtlich der Anwendung eines Service-spezifische Ausfall möglicherweise nur als beschränken und migrieren vorübergehend den Dienst zu einer alternativen Azure Region müssten.

###<a name="service-bus"></a>Dienstbus

Azure Service Bus verwendet einen eindeutigen Namespace, der Azure Regionen erstreckt sich nicht. Somit ist die erste Anforderung die erforderlichen Service Bus Namespaces in der Region alternative einrichten. Es gibt jedoch auch Aspekte für die Zuverlässigkeit der in der Warteschlange angezeigt. Es gibt mehrere Strategien für die Replikation von Nachrichten Azure Regionen ein. Die Details für diese Replikationsstrategien und anderen Strategien für zur Wiederherstellung finden Sie unter [bewährte Methoden für Hohlkörperelementen Applikationen gegen Dienstbus Ausfall und Schäden](../service-bus-messaging/service-bus-outages-disasters.md). Andere Aspekte Verfügbarkeit finden Sie unter [Dienstbus (Verfügbarkeit)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="app-service"></a>App-Dienst

Um eine App-Verwaltungsdienst Azure-Anwendung wie Web Apps oder Mobile-Apps, sekundäre Azure Region migrieren, müssen Sie eine Sicherungskopie der Website für die Veröffentlichung verfügbare verfügen. Wenn der Ausfall keine das gesamte Azure Datencenter verbunden ist, möglicherweise es möglich, eine Sicherungskopie der den Websiteinhalt Herunterladen FTP zu verwenden. Erstellen Sie eine neue app klicken Sie dann in der alternativen Region, es sei denn, Sie hier, um die Kapazität reservieren zuvor getan haben. Veröffentlichen der Website in die neue Region, und nehmen Sie alle erforderlichen Konfigurationsschritte Änderungen vor. Diese Änderungen konnte Datenbank Verbindungszeichenfolgen oder andere Einstellungen regionsspezifische einbezogen. Falls erforderlich, fügen Sie das Zertifikat der Website SSL hinzu und ändern Sie den DNS-CNAME-Eintrag aus, damit der benutzerdefinierten Domänennamen auf erneut bereitgestellte Azure Web App-URL verweist.

###<a name="hdinsight"></a>HDInsight

Die Daten, die HDInsight zugeordnet werden standardmäßig in Azure BLOB-Speicher gespeichert. HDInsight erfordert, dass ein Hadoop Cluster Verarbeitung MapReduce Aufträge in derselben Region als das Speicherkonto gemeinsame befinden muss, die die analysierten Daten enthält. Verwendet das Feature "Geo-Replikation" Azure-Speicher zur Verfügung, können Sie Ihre Daten in der Region sekundäre zugreifen, in dem die Daten repliziert wurde, wenn aus irgendeinem Grund die primäre Region nicht mehr verfügbar ist. Sie können einen neuen Hadoop Cluster in der Region erstellen, in dem die Daten repliziert wurden und weiter verarbeitet. Andere Aspekte Verfügbarkeit finden Sie unter [HDInsight (Verfügbarkeit)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="sql-reporting"></a>SQL-Berichterstattung

Zu diesem Zeitpunkt müssen bei der Wiederherstellung Verlust von einer Region Azure SQL-Berichterstattung mehrmals in unterschiedlichen Azure Regionen. Diese Instanzen SQL-Berichterstattung sollten dieselben Daten zugreifen und diese Daten sollte eine eigene Wiederherstellung nach einem Ausfall planen haben. Sie können auch externe Sicherungskopien der RDL-Datei für jeden Bericht beibehalten.

###<a name="media-services"></a>Media-Dienste

Azure Media Services weist einen anderen Wiederherstellung Ansatz für Codierung und streaming. In der Regel ist streaming wichtigere während einer regionalen Ausfall. Um dies vorzubereiten, sollten Sie ein Konto Media-Dienste in zwei verschiedenen Azure Regionen verfügen. Der codierte Inhalt sollte in beiden Regionen befinden. Während ein Fehler auftritt können Sie die Region ein anderes alternative streaming Datenverkehr umleiten. Codierung kann in einer beliebigen Azure Region durchgeführt werden. Ist Codierung dringende, beispielsweise während live Ereignisse zu verarbeiten, müssen Sie auf Aufträge zu einer alternativen Datacenter übermitteln, die bei Fehlern vorbereitet sein.

###<a name="virtual-network"></a>Virtuelles Netzwerk

Konfigurationsdateien enthalten die schnellste Möglichkeit zum Einrichten eines virtuellen Netzwerks in einer alternativen Azure Region. Nach dem Konfigurieren des virtuellen Netzwerks in der primären Azure Region, für das aktuelle Netzwerk in einem Netzwerk Konfigurationsdatei [exportieren die virtuelle Netzwerkeinstellungen](../virtual-network/virtual-networks-create-vnet-classic-portal.md) . Im Falle einer Ausfall in der primären Region, aus der gespeicherten Konfigurationsdatei [das virtuelle Netzwerk wiederherstellen](../virtual-network/virtual-networks-create-vnet-classic-portal.md) . Konfigurieren Sie andere Cloud Services, virtuellen Computern oder übergreifend lokale Einstellungen für die Arbeit mit dem neuen virtuellen Netzwerk.

##<a name="checklists-for-disaster-recovery"></a>Checklisten für die Wiederherstellung

##<a name="cloud-services-checklist"></a>Cloud Services-Checkliste

  1. Überprüfen Sie im Abschnitt Cloud Services dieses Dokuments.
  2. Ein Kreuz-Region Wiederherstellen nach Datenverlusten zu erstellen.
  3. Grundlegendes zu vor-und Nachteile in reserviert werden Kapazität in alternativen Regionen aus.
  4. Verwenden Sie den Datenverkehr Weiterleitung Tools, wie etwa Azure Datenverkehr Manager ein.

##<a name="virtual-machines-checklist"></a>Checkliste virtuellen Computern

  1. Überprüfen Sie im Abschnitt virtuellen Computern dieses Dokuments.
  2. Verwenden Sie [Azure Sicherung](https://azure.microsoft.com/services/backup/) , um Anwendung konsistent Sicherungen über Regionen erstellen.

##<a name="storage-checklist"></a>Checkliste für Speicher

  1. Überprüfen Sie im Abschnitt Speicher dieses Dokuments.
  2. Geo-Replikation Speicher Ressourcen nicht deaktiviert werden.
  3. Grundlegendes zu alternativen Region Geo-Replikation fällt aus.
  4. Erstellen Sie benutzerdefinierte Strategien für benutzergesteuerte Failoverstrategien an.

##<a name="sql-database-checklist"></a>Checkliste für SQL-Datenbank

  1. Überprüfen des SQL-Datenbank im Abschnitts dieses Dokuments.
  2. Verwenden Sie [Geo-wiederherstellen](../sql-database/sql-database-recovery-using-backups.md#geo-restore) oder [Geo-Replikation](../sql-database/sql-database-geo-replication-overview.md) je nach Bedarf an.

##<a name="sql-server-on-virtual-machines-checklist"></a>SQL Server auf virtuellen Computern Checkliste

  1. Überprüfen von SQL Server auf virtuellen Computern Abschnitt dieses Dokuments.
  2. Verwenden Sie Cross-Region AlwaysOn Verfügbarkeit Gruppen oder Spiegeln von Datenbanken.
  3. Alternativ können verwenden Sie sichern und Wiederherstellen Sie, um BLOB-Speicher.

##<a name="service-bus-checklist"></a>Dienstbus Checkliste

  1. Überprüfen Sie im Abschnitt Dienstbus dieses Dokuments.
  2. Konfigurieren Sie einen Namespace Dienstbus in einer alternativen Region ein.
  3. Erwägen Sie benutzerdefinierte Replikationsstrategien für Nachrichten über Regionen ein.

##<a name="app-service-checklist"></a>App-Checkliste

  1. Überprüfen Sie den App-Dienst Abschnitt dieses Dokuments.
  2. Verwalten Sie Website Sicherungen außerhalb der primären Region ein.
  3. Wenn einem Dienstausfall teilweise ist, versuchen Sie zum Abrufen der aktuellen Website mit FTP.
  4. Planen Sie die Website auf neuen oder vorhandenen Website in einer alternativen Region bereitstellen.
  5. Planen der Konfiguration Änderungen für Updates sowohl DNS CNAME-Einträge.

##<a name="hdinsight-checklist"></a>HDInsight Checkliste

  1. Überprüfen Sie im Abschnitt HDInsight dieses Dokuments.
  2. Erstellen Sie einen neuen Hadoop Cluster in der Region mit repliziert Daten ein.

##<a name="sql-reporting-checklist"></a>SQL-Berichterstattung Checkliste

  1. Überprüfen Sie im Abschnitt SQL Reporting dieses Dokuments.
  2. Verwalten einer alternativen Reporting SQL-Instanz in einem anderen Bereich.
  3. Verwalten Sie einen eigenen Plan repliziert das Ziel die Region ein.

##<a name="media-services-checklist"></a>Checkliste für Media-Dienste

  1. Überprüfen Sie im Abschnitt Media-Dienste dieses Dokuments.
  2. Erstellen eines Media Services-Kontos in eine alternative Region.
  3. Codieren Sie, die denselben Inhalt in beiden Regionen zur Unterstützung von streaming Failover.
  4. Einreichen Sie Codierung Aufträge zu einer alternativen Region im Falle einer dienststörung an.

##<a name="virtual-network-checklist"></a>Virtuelle Netzwerk Checkliste

  1. Überprüfen Sie im Abschnitt virtuelles Netzwerk dieses Dokuments.
  2. Verwenden Sie exportiert virtuelle Netzwerkeinstellungen erneut in einem anderen Region erstellen.

##<a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist Teil einer Serie [Azure Stabilität technischen Leitfaden](./resiliency-technical-guidance.md)dienten. Im nächsten Artikel in dieser Reihe befasst sich [Wiederherstellung aus einem lokalen Rechenzentrum in Azure](./resiliency-technical-guidance-recovery-on-premises-azure.md).
