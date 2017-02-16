<properties
    pageTitle="SQL (PaaS) im Vergleich zu SQL-Datenbankserver in der Cloud auf virtuellen Computern (IaaS) | Microsoft Azure"
    description="Erfahren Sie, welche SQLServer-Option Cloud Ihrer Anwendung passt: Azure SQL (PaaS)-Datenbank oder SQL Server in der Cloud auf Azure virtuellen Computern."
    services="sql-database, virtual-machines"
    keywords="SQL Server-Cloud, SQL Server in der Cloud, PaaS-Datenbank cloud SQL Server, DBaaS"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="cjgronlund"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Wählen Sie eine Cloud SQL Server-Option aus: Azure SQL (PaaS)-Datenbank oder SQL Server auf Azure-virtuellen Computern (IaaS)

Azure besteht aus zwei Optionen für das Hosten von SQL Server-Auslastung in Microsoft Azure:

* [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/): eine SQL-Datenbank systemeigenen in der Cloud, auch bekannt als eine Plattform als einer Service (PaaS) oder einer Datenbank als Service (DBaaS), die für die app-Entwicklung von Software als Service (SaaS) optimiert ist. Kompatibilität mit SQL Server-Features, die meisten vergleichbar. Weitere Informationen zum PaaS finden Sie unter [Was ist PaaS](https://azure.microsoft.com/overview/what-is-paas/).
* [SQL Server auf Azure virtuellen Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server installiert ist, und klicken Sie in der Cloud auf Windows Server virtuellen Computern (virtuellen Computern) Azure, auch bekannt als eine Infrastruktur als Service (IaaS) gehostet wird.
SQL Server auf Azure-virtuellen Computern ist für Migrieren von vorhandenen SQL Server-Clientanwendungen optimiert. Alle Versionen und Editionen von SQL Server zur Verfügung stehen. 100 % Kompatibilität mit SQL Server verschoben, sodass Sie so viele Datenbanken als erforderlich und ausgeführten Cross-Datenbank Transaktionen hosten vergleichbar. Vollzugriff auf SQL Server und Windows vergleichbar.

Erfahren Sie, wie jede Option in der Microsoft-Datenplattform passt und Anfordern von Hilfe übereinstimmenden die richtige Option auf Ihr Unternehmen. Ob Sie Kosten sparen oder minimale Administration vor allen weiteren Aufgaben priorisieren, kann in diesem Artikel helfen Ihnen bei der Entscheidung, welcher Ansatz bietet gegen die geschäftliche Anforderungen, dass Sie wichtig sind.


## <a name="microsofts-data-platform"></a>Microsoft Datenplattform

Eines der ersten Dinge zu verstehen, in allen Diskussionen über Azure im Vergleich zu lokalen SQL Server-Datenbanken ist, dass Sie alle verwendet werden kann. Microsoft Datenplattform nutzt SQL Server-Technologie und über die lokale physische Maschinen, private Cloud-Umgebungen, gehostete private Cloud Drittanbieter-Umgebungen und öffentliche Cloud verfügbar gemacht werden. SQL Server auf Azure virtuelle Mchines können Sie durch eine Kombination aus lokalen und Cloud gehosteten Bereitstellungen während mit demselben Satz von Serverprodukte, Development Tools und Know-how in dieser Umgebungen eindeutige sowie weitere Business Anforderungen.

   ![SQL Server-Optionen Cloud: SQLServer auf IaaS oder SaaS SQL-Datenbank in der Cloud.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Wie im Diagramm angezeigt wird, kann jede Angebot charakterisiert werden das Ausmaß der Verwaltungskonsole, die Sie über die Infrastruktur (klicken Sie auf die X-Achse) installiert haben, und durch den Grad der Kosteneffizienz erzielten Datenbank Ebene Konsolidierung und Automatisierung (auf der y-Achse).

Beim Entwerfen einer Anwendung, stehen vier grundlegende Optionen für das Hosten von des SQL Server-Teils der Anwendung:

- SQL Server auf physischen Computern nicht virtualisierten
- SQL Server lokal virtualisierten virtuellen (private Cloud)
- SQL Server in Azure virtuellen Computern (öffentlichen Microsoft-Cloud)
- SQL Azure-Datenbank (Microsoft öffentliche Cloud)

In den folgenden Abschnitten erfahren Sie SQL Server in der öffentlichen Microsoft-Cloud: Azure SQL-Datenbank und SQL Server auf Azure-virtuellen Computern. Darüber hinaus untersuchen Sie allgemeine Business Motivatoren zum bestimmen, welche Option am besten für eine Anwendung.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Detail Azure SQL-Datenbank und SQL Server auf Azure-virtuellen Computern

**Azure SQL-Datenbank** ist eine relationale Datenbank-as-a-Service (DBaaS) in der Azure Cloud, der in die Branche Kategorien von *Software-als-Service (SaaS)* und *Plattform-als-Service (PaaS)*gehostet wird. [SQL-Datenbank](sql-database-technical-overview.md) basiert auf standardisierten Hard- und Software, die im Besitz, gehostet und von Microsoft verwaltet wird. Mit SQL-Datenbank können Sie direkt auf den Dienst mit integrierten Features und Funktionen entwickeln. Wenn Sie je nach Bedarf berechnet mit Optionen zum nach oben oder sich für höhere Potenz ohne Unterbrechung skalieren SQL-Datenbank zu verwenden.

**SQL Server auf Azure virtuellen Computern (virtuelle Computer)** in der Kategorie Industry *Infrastruktur-als-Service (IaaS)* fällt und ermöglicht Ihnen, SQL Server innerhalb eines virtuellen Computers in der Cloud ausgeführt werden. Ähnlich wie mit SQL-Datenbank, wird es auf standardisierte Hardware erstellt, die im Besitz, gehostet und von Microsoft verwaltet wird. Wenn SQL Server auf einem virtuellen Computer verwenden, können Sie entweder Bezahlung-wie Sie-auf Start für eine SQL Server-Lizenz, die bereits in einer SQL Server-Bild enthalten, oder verwenden Sie einfach eine vorhandene Lizenz. Sie können auch auf einfache Weise Maßstab-nach-oben/unten und den virtuellen Computer nach Bedarf anhalten/fortsetzen.

In der Regel werden diese beiden SQL-Optionen für verschiedene Zwecke optimiert:

- **SQL-Datenbank** wurde optimiert, klicken Sie zum Verringern der Gesamtkosten der minimalen für die Bereitstellung und viele Datenbanken verwalten. Reduzierung der laufenden Verwaltungskosten, da Sie nicht zum Verwalten von virtuellen Computern, Betriebssystem oder eine Datenbank-Software verfügen. Sie müssen keinen Upgrades, hohen Verfügbarkeit oder [Sicherungen](sql-database-automated-backups.md)verwalten zu können. Im Allgemeinen Azure SQL-Datenbank kann die Anzahl der Datenbanken, die von einem einzelnen verwaltete beträchtlich erhöhen IT oder Entwicklungsressource.
- **SQL Server ausgeführt wird, auf Azure-virtuellen Computern** ist für vorhandene Applications in Azure migrieren oder erweitern vorhandenen lokalen Applications in der Cloud auf hybridbereitstellungen optimiert. Darüber hinaus können Sie SQL Server auf einem virtuellen Computer zu entwickeln und Testen der herkömmliche SQL Server-Clientanwendungen. Mit SQL Server auf Azure-virtuellen Computern müssen Sie die vollständige Administratorrechte über eine dedizierte SQL Server-Instanz und eines cloudbasierten virtuellen Computers. Es ist eine perfekten Wahl, wenn bereits ein Unternehmen IT-Ressourcen für den virtuellen Computern verwalten verfügt. Diese Funktionen können Sie ein hochgradig angepasste System Leistung und Verfügbarkeit Anforderungen Ihrer Anwendungs Adresse erstellen.

In der folgenden Tabelle werden die Hauptmerkmale der SQL-Datenbank und SQL Server auf Azure-virtuellen Computern zusammengefasst:

|       | SQL-Datenbank | SQLServer in einer Azure-virtuellen Computern|
| -------------- | ------------ | ---------------------- |
| **Optimal für:** | Neue Cloud gestalteter Applications, die Uhrzeit Einschränkungen in Entwicklung und Marketing aufweisen. |Vorhandene Anwendungen, die in der Cloud mit minimalen Änderungen schnelle migriert werden müssen. Schnelle Entwicklung und Test Szenarien, wenn Sie nicht lokalen nicht Herstellung SQL Server Hardware kaufen möchten. |
|| Teams, die für die Datenbank integrierte hohe Verfügbarkeit, Wiederherstellung und Upgrade benötigen. |Teams für die konfigurieren und hohe Verfügbarkeit, Wiederherstellung und Patch für SQL Server verwalten können. Einige sofern dies erheblich automatisierte Funktionen vereinfachen. |
||Teams, die nicht die zugrunde liegenden Betriebssystem und Konfiguration Einstellungen verwalten möchten.| Wenn Sie eine angepasste Umgebung mit vollen Administratorrechten benötigen.|
||Datenbanken von bis zu 1 TB oder größere Datenbanken, die unter Verwendung eines Musters Skalierung [horizontal oder vertikal aufgeteilt](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) werden können.|SQL Server-Instanzen mit bis zu 64 TB-Speicher. Die Instanz kann so viele Datenbanken Bedarf unterstützen. |
||[Gebäude Software-als-Service (SaaS) Applications](sql-database-design-patterns-multi-tenancy-saas-applications.md).| Migrieren und Enterprise und Hybrid Applications erstellen.|
|||||
|**Ressourcen:**|Sie möchten nicht IT-Ressourcen zum Konfigurieren und Verwalten der zugrunde liegenden Infrastruktur einsetzen, aber auf den Layer Anwendung konzentrieren möchten.|Sie müssen einige IT-Ressourcen zum Konfigurieren und verwalten. Einige sofern dies erheblich automatisierte Funktionen vereinfachen.|
|**Gesamtkosten:**|Hardware-Kosten beseitigt und Verwaltungskosten verringert.|Nicht mehr Hardware-Kosten aus.|
|**Geschäftskontinuität:**|Zusätzlich zu den integrierten Infrastruktur Fehlertoleranzfunktionen bietet Azure SQL-Datenbank, zum Beispiel [automatisierte Sicherungskopien](sql-database-automated-backups.md), [Point-In-Time wiederherstellen](sql-database-recovery-using-backups.md#point-in-time-restore), [Geo-wiederherstellen](sql-database-recovery-using-backups.md#geo-restore)und [Aktiven Geo-Replikation](sql-database-geo-replication-overview.md) Geschäftskontinuität erhöhen. Weitere Informationen finden Sie unter [Übersicht über die SQL-Datenbank Business Continuity](sql-database-business-continuity.md).|SQL Server auf Azure-virtuellen Computern können Sie eine hohe Verfügbarkeit und Disaster Wiederherstellung-Lösung, für bestimmte Anforderungen Ihrer Datenbank einrichten. Verwenden Sie ein System, die optimiert ist daher für eine Anwendung. Sie können testen und Failovers selbst bei Bedarf auszuführen. Weitere Informationen finden Sie unter [hohe Verfügbarkeit und Wiederherstellung für SQL Server auf Azure virtuellen Computern](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).|
|**Hybriden Cloud:**|Eine lokale Anwendung kann Daten in SQL Azure-Datenbank zugreifen.|Mit SQL Server auf Azure-virtuellen Computern haben Sie Applications aus, die teilweise in der Cloud ausgeführt und teilweise lokalen. Beispielsweise können Sie Ihre lokalen Netzwerk und Active Directory-Domäne in der Cloud über [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)erweitern. Darüber hinaus können Sie lokale Datendateien in Azure-Speicher mit [SQL Server-Datendateien in Azure](http://msdn.microsoft.com/library/dn385720.aspx)speichern. Weitere Informationen finden Sie unter [Einführung in SQL Server 2014 hybriden Cloud](http://msdn.microsoft.com/library/dn606154.aspx).|
||Unterstützt die [SQL Server Transaktionen Replikation](https://msdn.microsoft.com/library/mt589530.aspx) als Abonnent Daten repliziert.|Vollständig unterstützt [Transaktionen SQL Server-Replikation](https://msdn.microsoft.com/library/mt589530.aspx), [AlwaysOn Verfügbarkeit Gruppen](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md), Integration Services und Log Liefer-, um die Replikation von Daten. Darüber hinaus herkömmliche SQL Server-Sicherungen vollständig unterstützt|
|||||
|||||

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Business Gründe für die Auswahl Azure SQL-Datenbank oder SQL Server auf Azure-virtuellen Computern

### <a name="cost"></a>Kosten

Ob Sie einen Start, der für Cashflow oder ein Team in eine definierte Unternehmen, die Budgetrestriktionen arbeitet strapped ist befinden, ist eingeschränkt Bereitstellung von Mitteln häufig der primären Treiber bei der Entscheidung, wie Ihre Datenbanken gehostet. In diesem Abschnitt lernen Sie die Abrechnung und Lizenzierung Grundlagen in Azure im Hinblick auf diese beiden Optionen für den relationalen Datenbank: SQL-Datenbank und SQL Server auf Azure-virtuellen Computern. Sie erfahren außerdem zum Berechnen der Summe Anwendung Kosten.

#### <a name="billing-and-licensing-basics"></a>Abrechnung und Lizenzierung Grundlagen

**SQL-Datenbank** wird als Dienst, nicht mit einer Lizenz für Kunden verkauft.  [SQL Server auf Azure-virtuellen Computern](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md) verkauft wird mit einer darin enthaltenen Lizenz, die Sie Zahlen pro Minute. Wenn Sie eine vorhandene Lizenz verfügen, können Sie auch verwenden.  

**SQL-Datenbank** steht derzeit in verschiedene Dienst Ebenen, die alle stündlich mit einer festen Rate basierend auf der Dienstebene in Rechnung gestellt werden und Leistungsstufe ausgewählt haben. Darüber hinaus werden Sie für ausgehenden Datenverkehr im Internet bei normalen [Datenübertragung Sätzen](https://azure.microsoft.com/pricing/details/data-transfers/)Abrechnung. Die Ebenen der Dienst Basic, Standard und Premium dienen vorhersehbar Leistung mit mehreren Leistungsmerkmale Ihrer Anwendung Höchstwert Anforderungen entsprechend zu übermitteln. Sie können zwischen Dienst Ebenen und Leistung Ebenen Ihrer Anwendung verfügt über Durchsatz Anforderungen entsprechend zu ändern. Wenn Ihre Datenbank umfangreicher Transaktionen und Anforderungen zur Unterstützung von vielen gleichzeitiger Benutzern verfügt, empfehlen wir die Premium-Service-Ebene an. Die neuesten Informationen zu den aktuellen unterstützten Service Ebenen werden soll finden Sie unter [Azure SQL-Datenbank Dienst Ebenen](sql-database-service-tiers.md). Sie können auch [flexible Datenbank Pools](sql-database-elastic-pool.md) zum Freigeben von Ressourcen der Leistung zwischen Datenbankinstanzen erstellen.

Mit **SQL-Datenbank**wird die Datenbanksoftware automatisch konfiguriert, korrigiert und Upgrade von Microsoft, die Ihre Verwaltungskosten reduziert. Darüber hinaus helfen Ihnen, deren [Sicherung integrierte](sql-database-automated-backups.md) Funktionen deutlich zu erreichen, besonders, wenn Sie eine große Anzahl von Datenbanken haben.

Mit **SQL Server auf Azure-virtuellen Computern**können Sie die Plattform bereitgestellten SQL Server-Bilder (enthält eine Lizenz) verwenden oder bringen Ihre SQL Server-Lizenz. Unterstützten SQL Server-Versionen (2008 R2, 2012, 2014, 2016) und Editionen (Entwickler, Express, Web, Standard, Enterprise) verfügbar sind. Darüber hinaus stehen bringen-Your-Besitzer-Lizenz Versionen (BYOL) der Bilder. Wenn Bilder mithilfe der Azure bereitgestellt werden, hängt die Kosten der virtueller Speicher und die Version von SQL Server, die Sie auswählen. Unabhängig davon virtueller Speicher oder SQL Server-Edition Zahlen Sie pro Minute zur Lizenzierung Kosten von SQL Server und Windows Server, zusammen mit den Azure-Speicher Kosten für die virtuellen Computer Datenträger aus. Die Abrechnung pro Minute-Option können Sie SQL Server für verwenden, solange Sie Lizenzen ohne Kauf Erweiterung SQL Server benötigen. Wenn Sie Ihre eigenen SQL Server-Lizenz zu Azure schalten, unterliegen Sie für Windows Server und nur die Kosten für Speicher. Weitere Informationen zur Lizenzierung bringen eigener finden Sie unter [Lizenz Mobilität über Software Assurance-Azure](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="calculating-the-total-application-cost"></a>Berechnen der Summe Anwendung Kosten

Wenn Sie eine Cloud verwenden, enthält die Kosten für die Anwendung ausführen der Entwicklung und Verwaltungskosten sowie die öffentliche Cloud-Plattform Dienst Kosten.

So sieht die Kostenberechnung detaillierte für eine Anwendung, die in der SQL-Datenbank und SQL Server auf Azure-virtuellen Computern ausführen:

**Bei Verwendung von Azure SQL-Datenbank:**

*Gesamtkosten der Anwendung hochgradig minimierten Verwaltungskosten + Kosten für die Entwicklung + SQL-Datenbank-Service-Kosten =*

**Wenn SQL Server auf Azure-virtuellen Computern verwenden zu können:**

*Gesamtkosten der Anwendung hochgradig minimierten Software Development Kosten + Verwaltungskosten SQL Server und Windows Server zur Lizenzierung Kosten + Azure-Speicherkosten =*

Weitere Informationen zu Preisen finden Sie unter den folgenden Ressourcen:

- [Die Preise der SQL-Datenbank](https://azure.microsoft.com/pricing/details/sql-database/)
- Für [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) und [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows) [Preise virtuellen Computern](https://azure.microsoft.com/pricing/details/virtual-machines/)
- [Azure Preise Taschenrechner](https://azure.microsoft.com/pricing/calculator/)

> [AZURE.NOTE] Es gibt eine kleine Gruppe von Features in SQL Server, die nicht verfügbar oder mit SQL-Datenbank nicht verfügbar sind. Weitere Informationen finden Sie unter [Allgemein der SQL-Datenbank Einschränkungen und Richtlinien](sql-database-general-limitations.md) und [SQL-Datenbank Transact-SQL-Informationen](sql-database-transact-sql-information.md) . Wenn Sie eine vorhandene SQL Server-Lösung in der Cloud verschieben, finden Sie unter [Migrieren einer SQL Server-Datenbank mit Azure SQL-Datenbank](sql-database-cloud-migrate.md). Wenn Sie eine vorhandene lokalen SQL Server-Anwendung mit SQL-Datenbank migrieren, sollten Sie die Aktualisierung der Anwendung, um die Funktionen nutzen, die cloud Services-Angebot. Beispielsweise könnten Sie [Azure Web App-Verwaltungsdienst](https://azure.microsoft.com/services/app-service/web/) oder [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) Hosten Ihrer Anwendung Layer zum Kostenvorteile zu erhöhen.

### <a name="administration"></a>Verwaltung

Für viele Unternehmen ist die Entscheidung für den Übergang in einen Cloud-Service wie viel zu Verschiebung Komplexität der Administration dargestelltes Kosten. Mit **SQL-Datenbank**verwaltet Microsoft die zugrunde liegenden Hardware. Microsoft automatisch repliziert alle Daten, um hohe Verfügbarkeit zu gewährleisten, konfiguriert und upgrades die Datenbanksoftware, verwaltet werden, den Lastenausgleich und transparentes Failover bedeutet, es ist ein Serverfehler. Können Sie weiterhin Ihre Datenbank zu verwalten, müssen Sie jedoch nicht mehr zum Verwalten der Datenbank-Engine, Server-Betriebssystem oder Hardware.  Beispiele für Elemente, die Sie verwalten fortsetzen können, sind Datenbanken und Benutzernamen, Index und Optimieren von Abfragen und Überwachung und Sicherheit.

Mit **SQL Server auf Azure-virtuellen Computern**müssen Sie Vollzugriff auf das Betriebssystem und die Konfiguration von SQL Server-Instanz aus. Mit eines virtuellen Computers ist es Ihnen zu entscheiden, wann oder das Betriebssystem und die Datenbank-Software Aktualisierung und wann z. B. Anti-Virus Software installieren. Einige automatisierten Funktionen sind verfügbar, um seine Patch, Sicherung und hohen Verfügbarkeit zu vereinfachen. Darüber hinaus können Sie die Größe der virtuellen Computer, die Anzahl der Datenträger und deren Speicherkonfigurationen steuern. Azure ermöglicht das Ändern der Größe eines virtuellen Computers, je nach Bedarf. Informationen finden Sie unter [virtuellen Computern und Cloud-Dienst Größen für Azure](../virtual-machines/virtual-machines-linux-sizes.md). 

### <a name="service-level-agreement-sla"></a>Vereinbarung zum Servicelevel (Vereinbarung zum SERVICELEVEL)

Für viele IT-Abteilung ist das nach-oben-Time Verpflichtung von einem Dienst Ebene Vertrag SERVICELEVEL Besprechung höchste Priorität aus. In diesem Abschnitt Prüfen was Vereinbarung zum SERVICELEVEL für jede Option Hostinganbieter Datenbank gilt.

Für **SQL-Datenbank** Basic, Standard und Premium Service Ebenen bietet Microsoft eine Verfügbarkeit Vereinbarung zum SERVICELEVEL von 99,99 %. Die neuesten Informationen finden Sie unter [Ebene Servicevertrag](https://azure.microsoft.com/support/legal/sla/sql-database/). Die neuesten Informationen zu SQL-Datenbank-Dienst Ebenen und den unterstützten Business Continuity-Pläne finden Sie unter [Service Ebenen](sql-database-service-tiers.md).

Für **SQL Server ausgeführt wird, auf Azure-virtuellen Computern**bietet Microsoft eine Verfügbarkeit Vereinbarung zum SERVICELEVEL von 99,95 %, die nur die virtuellen Computern behandelt. Diese Vereinbarung zum SERVICELEVEL befasst sich nicht auf dem virtuellen Computer ausgeführten Prozesse (z. B. SQL Server) und setzt voraus, dass Sie mindestens zwei Instanzen von virtuellen Computer in einem Satz Verfügbarkeit hosten. Die neuesten Informationen finden Sie unter [Virtueller Computer Vereinbarung zum SERVICELEVEL](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Für die Datenbank hohen Verfügbarkeit in virtuellen Computern sollten Sie eine der Optionen unterstützte hohe Verfügbarkeit in SQL Server, wie etwa [AlwaysOn Verfügbarkeit Gruppen](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx)konfigurieren. Verwenden einer Option unterstützten hohen Verfügbarkeit nicht zur Verfügung, eine weitere Vereinbarung zum SERVICELEVEL, jedoch können Sie erreichen > Datenbank Verfügbarkeit von 99,99 %.

### <a name="a-namemarketatime-to-market"></a><a name="market"></a>Zeit bis zur Veröffentlichung

**SQL-Datenbank** ist das richtige für Applikationen Cloud entwickelt, wenn Entwicklertools Produktivität und schnelle Time-to-Market wichtig sind. Mit den programmgesteuerten DBA vergleichbare Funktionen ist es für Cloud Architekten und Entwickler, wie es müssen für die Verwaltung der zugrunde liegenden Betriebssystem und Datenbank Grundlinie. Beispielsweise können Sie die [REST-API](http://msdn.microsoft.com/library/azure/dn505719.aspx) und [PowerShell-Cmdlets](http://msdn.microsoft.com/library/azure/dn546726.aspx) zum Automatisieren und administrative Vorgänge für Tausende von Datenbanken verwalten. Features wie [Flexible Datenbank Pools](sql-database-elastic-pool.md) gestattet Ihnen Fokussierung auf den Layer Anwendung und Ihre Lösung schneller zu Market vorführen.

**SQL Server ausgeführt wird, auf Azure-virtuellen Computern** eignet sich optimal, wenn Ihre vorhandenen oder neuen Applikationen große Datenbanken, zusammenhängende Datenbanken oder den Zugriff auf alle Funktionen in SQL Server oder Windows erforderlich. Es ist auch geeignet, wenn Sie vorhandene migrieren möchten lokalen Applikationen und als Azure-Datenbanken – ist. Da Sie nicht benötigen, so ändern Sie die Präsentation, Anwendung und Ebenen Daten, sparen Sie Zeit und Ihr Budget auf Ihre vorhandene Lösung Neuentwurf an. Stattdessen können Sie sich auf alle Ihre Lösungen migrieren, Azure und auf diese Weise einige Leistung Optimierungen, die möglicherweise von der Azure-Plattform erforderlich konzentrieren. Weitere Informationen finden Sie unter [Leistung bewährte Methoden für SQL Server auf Azure virtuellen Computern](../virtual-machines/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel untersucht SQL-Datenbank und SQL Server auf Azure virtuellen Computern (virtuelle Computer) und erläutert allgemeine Business Motivatoren, die sich möglicherweise auf Ihre Entscheidung auswirken. Im folgenden finden eine Zusammenfassung der Vorschläge für Sie zu berücksichtigen ist:

Wählen Sie **SQL Azure-Datenbank** aus, wenn:

- Sie erstellen neue cloudbasierten Applikationen die Kosten sparen nutzen, und zur Optimierung der Leistung, die cloud-Dienste bereitzustellen. Dieser Ansatz bietet die Vorteile von einem vollständig verwaltete Cloud-Dienst, hilft unteren initial Time-to-Market und langfristiges Kosten optimieren kann bereitstellen.

- Sie möchten, dass Microsoft common Management Operationen auf Ihrer Datenbanken und sicheres Verfügbarkeit SLAs für Datenbanken erforderlich.



Wählen Sie **SQLServer auf Azure-virtuellen Computern** an, wenn:

- Sie haben die vorhandene lokale Anwendungen, die Sie migrieren oder in der Cloud erweitern möchten, oder wenn Sie Enterprise-Anwendungen, die größer als 1 TB erstellen möchten. Dieser Ansatz bietet die Vorteile von 100 % SQL-Kompatibilität, große Datenbankkapazität, Vollzugriff auf SQL Server und Windows und zu lokalen Tunnel abzusichern. Dieser Ansatz minimiert Kosten für die Entwicklung und Änderungen der vorhandenen Applikationen.

- Sie verfügen über vorhandener IT-Ressourcen und können schließlich Patch, Sicherungskopien und hohen Verfügbarkeit der Datenbank besitzen. Beachten Sie, dass einige Features automatisierte dieser Vorgänge erheblich vereinfachen. 


## <a name="next-steps"></a>Nächste Schritte
- Finden Sie unter [SQL-Datenbank-Lernprogramm: erstellen eine SQL-Datenbank in Minuten über das Azure-Portal](sql-database-get-started.md) erste Schritte mit SQL-Datenbank.
- Finden Sie unter [SQL-Datenbank Preise] (https://azure.microsoft.com/pricing/details/sql-database/).
- Finden Sie unter [Bereitstellen einer SQL Server-virtuellen Computern in Azure](../virtual-machines/virtual-machines-windows-portal-sql-server-provision.md) erste Schritte mit SQL Server auf Azure-virtuellen Computern.
- Finden Sie unter [SQLServer auf eine Azure-virtuellen Computern: Learning Path](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/).
