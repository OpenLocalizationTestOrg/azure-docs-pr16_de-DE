<properties
   pageTitle="Stapel und HPC Lösungen in der Cloud | Microsoft Azure"
   description="Erfahren Sie mehr über den Stapel und High Performance computing (HPC und große berechnen) Szenarien und Lösungsmöglichkeiten in Azure"
   services="batch, virtual-machines, cloud-services"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-compute"
   ms.date="07/27/2016"
   ms.author="danlep"/>

# <a name="batch-and-hpc-solutions-in-the-azure-cloud"></a>Stapel und HPC Lösungen in der Cloud Azure

Azure bietet effiziente und skalierbare Cloudlösungen für Stapel und High Performance computing (HPC) – die Abkürzung *Groß zu berechnen*. Erfahren Sie hier Auslastung große berechnen und Azure Dienste zur Verfügung, oder direkt zu [Lösung Szenarien](#scenarios) weiter unten in diesem Artikel springen. In diesem Artikel wird hauptsächlich für technische Entscheidungsträger, IT-Manager und unabhängige Softwarelieferanten, aber andere IT-Profis und Entwickler können mit den folgenden Lösungsvorschlägen vertraut machen.

Organisationen haben Probleme mit großen computing: Entwurf und Analyse, Bild rendern, komplexe Modellierung, Monte Carlo-Simulationen, finanzielle Risiken Berechnungen und andere Technik. Azure hilft Organisationen, die zur Lösung dieser Probleme mit den Ressourcen, Dezimalstellen und Terminplan benötigten. Mit Azure können sich an Organisationen:

* Erstellen von Hybrid Lösungen, Erweitern eines lokalen HPC Clusters zum Auslagern Höchstwert Auslastung in der cloud

* HPC Cluster Tools und Auslastung vollständig in Azure ausführen

* Verwenden von verwalteten und skalierbare Azure-Diensten wie [Stapel](https://azure.microsoft.com/documentation/services/batch/) zum Ausführen von rechenintensiven Auslastung ohne bereitstellen und Verwalten von berechnen Infrastruktur

Obwohl Gegenstand dieses Artikels, Azure auch Entwicklern bietet und Partnern sämtlicher Funktionen, Architektur Auswahlmöglichkeiten und Entwicklungstools umfangreiche, benutzerdefinierte große berechnen Workflows erstellen. Und eine wachsende Partnerökosystem bereit sind, hilft Ihnen der Auslastung große berechnen in der Cloud Azure produktiv treffen können.


## <a name="batch-and-hpc-applications"></a>Stapel und HPC Applikationen

Im Gegensatz zu Web Applications und viele Linie branchenanwendungen, Stapel und HPC Applikationen aufweisen, einen definierten Anfang und ein Ende und sie können auf einen Zeitplan oder bei Bedarf, manchmal für Stunden oder mehr ausführen. Die meisten in zwei Hauptfenster Kategorien fallen: *systembedingt parallele* (bezeichnet "sehr parallel", da sie lösen Probleme, die sich für die Ausführung von parallel auf mehreren Computern oder Prozessoren eignen) und *eng verknüpft*. Finden Sie in der folgenden Tabelle werden weitere Informationen zu dieser Anwendungstypen. Einige Azure Lösung Ansätze für einen Typ oder anderen verbessern.

>[AZURE.NOTE] In Stapel und HPC Lösungen eine laufende Instanz der Anwendung ist eine *Position*in der Regel bezeichnet, und jedes Projekt möglicherweise in *Aufgaben*unterteilt erhalten. Und die gruppierten berechnen Ressourcen für die Anwendung werden häufig bezeichnet *Knoten zu berechnen*.

Typ | Merkmale | Beispiele
------------- | ----------- | ---------------
**Parallele systembedingt**<br/><br/>![Parallele systembedingt][parallel] |• Einzelne Computer Anwendungslogik unabhängig voneinander ausführen<br/><br/> • Hinzufügen Computern kann die Anwendung skalieren und Zeitaufwand für die Berechnung<br/><br/>• Anwendung umfasst separate Programmdateien oder ist eine Gruppe von Diensten, die von einem Client (SOA, Anwendung oder eine Service-orientierte Architektur) aufgerufen aufgeteilte |• Finanzielle Risiken Modellierung<br/><br/>• Bild rendern und Bild Verarbeitung<br/><br/>• Medien Codierung und Umcodierung<br/><br/>• Monte Carlo-Simulationen<br/><br/>• Testen von software
**Eng verknüpft**<br/><br/>![Eng verknüpft][coupled] |• Anwendung erfordert Datenverarbeitungsknoten zu interagieren oder exchange Zwischenergebnisse<br/><br/>• Berechnen Knoten möglicherweise kommunizieren mit der Nachricht übergeben Interface (MPI), ein häufig verwendetes Kommunikationsprotokoll für parallele Datenverarbeitung<br/><br/>• Die Anwendung vertrauliche Netzwerkwartezeit und Bandbreite ist<br/><br/>• Leistung der Anwendung kann verbessert werden, indem Sie schnelle Netzwerke Technologien wie InfiniBand und remote direkte Arbeitsspeicher Access (RDMA) |• Oil und Gas Wasserspeichermodelle<br/><br/>• Technisch Entwurf und Analyse, wie etwa berechnete Pneumatik / dynamics<br/><br/>• Physische Simulationen beispielsweise Auto abstürzen und nuklearen Reaktionen<br/><br/>• Wetter Planung

### <a name="considerations-for-running-batch-and-hpc-applications-in-the-cloud"></a>Aspekte, die für die Ausführung von Stapel und HPC Applications in der cloud

Sie können leicht viele Clientanwendungen migrieren, die entwickelt wurden, auf lokale HPC Cluster Azure oder einer hybridumgebung (Cross lokale) ausgeführt werden. Es kann jedoch sein einige Einschränkungen oder Aspekte, einschließlich:


* **Verfügbarkeit von Cloudressourcen** – je nach Typ der Cloud berechnen Ressourcen, die Sie verwenden möchten, die können Sie nicht möglicherweise auf Verfügbarkeit fortlaufender Maschinen zu verlassen, während der Ausführung eines Auftrags. Behandlung von Bundesland und den Fortschritt überprüfen auf allgemeine Techniken in der Cloudressourcen mit möglichen vorübergehende Fehler und weitere erforderlichen behandelt werden.


* **Daten-Access** - Daten Techniken für Access gängiger in Enterprise Cluster, z. B. NFS, erfordern spezielle Konfiguration in der Cloud. Oder Sie müssen unterschiedliche Daten Access Methoden und Muster für die Cloud bringt.

* **Verschieben von Daten** - For Applications, dass Prozess große Datenmengen, Strategien zum Verschieben der Daten in der Cloud-Speicher und zum Berechnen von Ressourcen benötigt werden. Sie müssen schnelle Cross lokale wie [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/)networking gegebenenfalls. Erwägen Sie auch Legal, behördliche oder Richtlinie Einschränkungen für speichern oder den Zugriff auf die Daten ein.


* **Lizenzierung** : Kontrollkästchen mit dem Lieferanten jeden kommerzielle Antrag zur Lizenzierung oder eine andere Einschränkungen für die Ausführung von in der Cloud. Nicht alle Hersteller bieten Lizenzierung je nach Bedarf berechnet. Möglicherweise müssen einen Lizenzierungsserver in der Cloud für Ihre Lösung planen, oder Verbinden mit einem lokalen Lizenz-Server.


### <a name="big-compute-or-big-data"></a>Große berechnen oder Big Data?

Die Trennlinie zwischen Groß berechnen und Big Data Applications nicht immer löschen, und möglicherweise müssen Sie einige Applications Merkmale beider. Beide umfassen umfangreiche Berechnungen, in der Regel für Cluster aus Computern ausgeführt werden. Die Lösungsansätze und unterstützende Tools können jedoch abweichen.

• **Berechnen Groß** ist normalerweise Applikationen (Variablen) haben, die auf CPU-Leistung und Speicherkapazität, z. B. engineering Simulationen, finanzielle Risiken modeling und digitale Rendern aufsetzen. Die Infrastruktur für eine große berechnen Lösung, enthalten möglicherweise Computern mit speziellen Mehrkernprozessoren unformatierten Berechnung und spezialisierte, schnelle Netzwerke Hardware zum Verbinden der Computer ausführen.

• **Big Data** löst Data Analysis Probleme, die große Datenmengen betreffen, die von einem einzelnen Computer oder die Datenbank Managementsystem verwaltet werden kann. Beispiele für große Datenmengen Webprotokolle oder andere Business Intelligence-Daten. Big Data in der Regel mehr auf der Festplatte und e/a-Leistung als auf CPU Power verlassen. Es gibt auch speziellen Big Data Tools, wie z. B. Apache Hadoop die Cluster und Partition die Daten verwalten. (Informationen zu Azure HDInsight und anderen Azure Hadoop-Lösungen enthalten, finden Sie unter [Hadoop](https://azure.microsoft.com/solutions/hadoop/).)

## <a name="compute-management-and-job-scheduling"></a>Berechnen von Verwaltungs- und Planung von Aufträgen

Stapel und HPC Applikationen häufig ausgeführt enthält eine *Cluster-Manager* und eine *Aufgabe* besser verwalten Ressourcen gruppierten berechnen und Zuweisen der Anwendungen, die die Einzelvorgänge ausgeführt werden. Diese Funktionen möglicherweise von separaten Tools oder integriertes Tool oder-Diensten erfolgen.

* **Cluster-Manager** - Vorschriften, frei und verwaltet Ressourcen zu berechnen (oder Knoten berechnen). A Cluster-Manager möglicherweise automatisieren die Installation von Bildern Betriebssystem und Applikationen auf Datenverarbeitungsknoten, Berechnen von Ressourcen nach Bedarf skalieren und Überwachen der Leistung der Knoten.

* **Job Scheduler** - gibt an, den Ressourcen (z. B. Prozessoren oder Speicher) eine Anwendung Anforderungen, und die Bedingungen, wenn es ausgeführt wird. Eine Position Scheduler verwaltet eine Reihe von Aufträgen und weist Ressourcen auf Grundlage einer zugewiesene Priorität oder andere Eigenschaften fest.

Cluster und Auftrag Terminplanungssoftware für Windows- und Linux-basierten Cluster können auch auf Azure migrieren. Angenommen, bietet [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029), Microsoft kostenlosen berechnen Clusterlösung für Windows und Linux HPC Auslastung mehrere Optionen für die Ausführung in Azure. Sie können auch Linux Cluster zum Ausführen von Open Source-Tools, wie z. B. Drehmoment und SLURM erstellen. Sie können auch Lösungen für kommerzielle Raster in Azure, z. B. [TIBCO DataSynapse GridServer](http://www.tibco.com/company/news/releases/2016/tibco-to-accelerate-cloud-adoption-of-banking-and-capital-markets-customers-via-microsoft-collaboration), [IBM Plattform Symphony](http://www-01.ibm.com/support/docview.wss?uid=isg3T1023592)und [Univa Raster-Engine](http://www.univa.com/products/grid-engine)bringen.

Wie in den folgenden Abschnitten dargestellt wird, können Sie auch nutzen berechnen Ressourcen verwalten und Planen von Aufträgen ohne (oder zusätzlich zu) traditionellen Cluster Verwaltungstools Azure-Dienste.


## <a name="scenarios"></a>Szenarien

Hier sind drei häufige Szenarien auf große berechnen Auslastung Azure ausgeführt werden mithilfe von vorhandenen HPC Cluster-Lösungen, Azure Services oder eine Kombination aus beiden. Wichtige Faktoren für die Auswahl der einzelnen Szenarien aufgelistet werden, aber nicht vollständig sind. Informationen zu die verfügbaren Azure-Diensten, die Sie eventuell in Ihre Lösung verwenden liegt mehr im Artikel nach.

  | Szenario | Warum wählen Sie es aus?
------------- | ----------- | ---------------
**Einen HPC Cluster in Azure Spitzen**<br/><br/>[! [Cluster Burst] [Burst_cluster]](./media/batch-hpc-solutions/burst_cluster.png) <br/><br/> Weitere Informationen:<br/>• [Azure Worker Instanzen mit HPC Pack Spitzen](https://technet.microsoft.com/library/gg481749.aspx)<br/><br/>[Einrichten einer Hybrid berechnen Cluster mit HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) •<br/><br/>• [Azure Stapel mit HPC Pack Spitzen](https://technet.microsoft.com/library/mt612877.aspx)<br/><br/>|• Kombinieren Ihre [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) oder anderen lokalen Cluster mit zusätzlichen Azure Ressourcen in einer Hybrid-Lösung.<br/><br/>• Erweitern Ihrer Auslastung große berechnen auf Plattform als Service (PaaS) virtuellen Computern Instanzen (aktuell nur Windows Server) ausgeführt.<br/><br/>• Zugriff auf eine lokale Lizenz Server oder einen benutzerdefinierten Datenspeicher mithilfe eines optionalen Azure virtuellen Netzwerks|• Sie haben einen vorhandenen HPC Cluster und benötigen weitere Ressourcen <br/><br/>• Nicht zum Kauf und zur zusätzliche HPC Cluster Infrastruktur verwalten möchten<br/><br/>• Stehen Ihnen vorübergehende Höchstwert bei Bedarf Perioden oder Sonderprojekte
**Erstellen eines HPC Clusters vollständig in Azure**<br/><br/>[! [Cluster in IaaS] [Iaas_cluster]](./media/batch-hpc-solutions/iaas_cluster.png)<br/><br/>Weitere Informationen:<br/>• [HPC Cluster-Lösungen in Azure](./big-compute-resources.md)<br/><br/>|• Schnell und konsistente Bereitstellen Ihrer Programme und Tools standardmäßige oder benutzerdefinierte Infrastruktur für Windows oder Linux Cluster als eine Service (IaaS) virtuellen Computern.<br/><br/>• Führen Sie verschiedene große berechnen Auslastung mit den Auftrag Planung Lösung Ihrer Wahl.<br/><br/>• Mit zusätzlichen Azure Dienste wie Netzwerk- und Speicher können umfassende Cloud-basierte Lösung erstellen. |• Nicht zum Kauf und zur zusätzliche Linux oder Windows HPC Cluster Infrastruktur verwalten möchten<br/><br/>• Stehen Ihnen vorübergehende Höchstwert bei Bedarf Perioden oder Sonderprojekte<br/><br/>Sie benötigen einen zusätzlichen • cluster für eine Uhrzeit, aber nicht möchten, in Computern und Speicherplatz bereitzustellen, investieren.<br/><br/>• Zu Ihrer Anwendung berechnen ankommt auslagern, damit er als vollständig in der Cloud-Dienst ausgeführt wird.
**Auschecken einer parallelen Anwendung in Azure skalieren**<br/><br/>[! [Azure Stapel] [Batch_proc]](./media/batch-hpc-solutions/batch_proc.png)<br/><br/>Weitere Informationen:<br/>• [Grundlagen des Blatts Azure](./batch-technical-overview.md)<br/><br/>[Erste Schritte mit der Bibliothek Azure Stapel für .NET](./batch-dotnet-get-started.md) •|• Entwickeln mit [Azure Stapel](https://azure.microsoft.com/documentation/services/batch/) skalieren verschiedene große berechnen Auslastung für Windows oder Linux-virtuellen Computern Speicherpools ausgeführt.<br/><br/>• Mit einer Azure-Plattform-Dienst können Bereitstellung und automatische Skalierung des virtuellen Computern, Planung von Aufträgen, Wiederherstellung, Verschieben von Daten, Abhängigkeit Management und Bereitstellung der Anwendung verwalten.|•, Die Sie verwalten möchten, nicht berechnen von Ressourcen oder ein Projekt Planer; In diesem Fall Konzentration auf Ihre Programme ausgeführt werden soll<br/><br/>• Zu Ihrer Anwendung berechnen ankommt auslagern, damit er als in der Cloud-Dienst ausgeführt wird.<br/><br/>• Berechnen Ressourcen entsprechend die Arbeitsbelastung berechnen automatisch skalieren soll.


## <a name="azure-services-for-big-compute"></a>Azure Services für große zu berechnen.

Das geht zu berechnen, Daten, Netzwerke und verwandte Dienste, die Sie kombinieren können für große berechnen Lösungen und Workflows. Genaue Anleitungen Azure Services finden Sie unter der Azure Services- [Dokumentation](https://azure.microsoft.com/documentation/). Die [Szenarien](#scenarios) in diesem Artikel anzeigen nur einige Methoden für die Verwendung dieser Dienste

>[AZURE.NOTE] Azure werden regelmäßig neue Dienste, die für Ihr Szenario hilfreich sein können. Wenn Sie Fragen haben, wenden Sie sich an einer [Azure Partner](https://pinpoint.microsoft.com/en-US/search?keyword=azure) oder die e-Mail- *bigcompute@microsoft.com*.

### <a name="compute-services"></a>Berechnen von services

Berechnen von Azure Services bilden das Herzstück einer Lösung groß zu berechnen und die anderen berechnen Services Angebot Vorteile für unterschiedliche Szenarien. Grundlegende Ebene bieten diese Dienste verschiedene Modi für Applikationen auf virtuellen Computern-basierten Computinginstanzen ausführen, die Azure bietet mithilfe von Windows Server Hyper-V-Technologie. Diese Instanzen können Standard- und benutzerdefinierte Linux und Windows-Betriebssysteme und Tools ausgeführt werden. Azure Ihnen eine Auswahl [Instanz](../virtual-machines/virtual-machines-windows-sizes.md) Größen mit unterschiedlichen Konfigurationen CPU-Kerne, Speicher, Speicherkapazität und andere Eigenschaften fest. Je nach Ihren Anforderungen können Sie die Instanzen für Tausende von Kerne skalieren und dann abwärts skalieren, wenn Sie weniger Ressourcen benötigen.

>[AZURE.NOTE] Nutzen Sie die Vorteile der Azure rechenintensiven Instanzen steigern die Leistung und Skalierbarkeit von HPC Auslastung einschließlich parallele MPI-Anwendungen, die ein wenig Wartezeit und hohen Durchsatz Anwendung Netzwerk erforderlich sind. [H-Serie und berechnen ankommt A-Serie virtuellen Computern](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md)finden Sie unter.  

Dienst | Beschreibung
------------- | -----------
**[Virtuellen Computern](https://azure.microsoft.com/documentation/services/virtual-machines/)**<br/><br/> |• Bieten berechnen Infrastruktur als Service (IaaS) mit Microsoft Hyper-V-Technologie<br/><br/>• Ermöglichen es Ihnen, flexible bereitstellen und Verwalten von beständigen Cloud-Computern von Windows Server oder Linux Standardbilder aus dem [Azure Marketplace](https://azure.microsoft.com/marketplace/), oder Bilder und Daten Datenträger, die Sie angeben<br/><br/>• Bereitgestellt und als [Virtueller Computer Maßstab Sätze](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/) umfangreiche Services aus identischen virtuellen Computern, mit Automatische Skalierung zum Vergrößern oder Verkleinern von Kapazität automatisch zu entwickeln verwaltet werden können<br/><br/>• Ausführen der lokalen Cluster-Tools und Anwendungen vollständig in der Cloud zu berechnen.<br/><br/>
**[Cloud-Dienste](https://azure.microsoft.com/documentation/services/cloud-services/)**<br/><br/> |• Große berechnen von Applications in Worker Rolleninstanzen, ausgeführt werden kann, die vollständig von Azure verwaltet werden und werden von virtuellen Computern, die eine Version von Windows Server<br/><br/>• Aktivieren skalierbare, zuverlässige Applikationen mit niedriger Verwaltungsaufwand in einer Plattform als (PaaS) Servicemodell ausgeführt<br/><br/>• Erfordern möglicherweise zusätzliche Tools oder Entwicklung mit lokalen HPC Cluster-Lösungen integriert werden soll.
**[Stapelverarbeitung](https://azure.microsoft.com/documentation/services/batch/)**<br/><br/> |• Wird umfangreiche Parallel und Stapel Auslastung eine vollständig verwaltete Dienst ausgeführt<br/><br/>• Bietet Planung von Aufträgen und automatische Skalierung aus einem verwalteten Pool von virtuellen Computern<br/><br/>• Ermöglicht Entwicklern das Erstellen und Ausführen von Applications als Dienst oder vorhandene Applikationen Cloud-aktivieren<br/>

### <a name="storage-services"></a>Speicher-services

Eine große berechnen-Lösung in der Regel wirkt sich auf eine Reihe von Eingabedaten, und Daten für ihre Ergebnisse generiert. Die verwendeten in große berechnen Lösungen Azure-Speicher-Dienste gehören:

* [Blob, Tabelle und Warteschlangenspeicher](https://azure.microsoft.com/documentation/services/storage/) - große Mengen von unstrukturierten Daten, NoSQL Daten und Nachrichten für Workflows und Kommunikation, Hilfethemas verwalten. Beispielsweise können Blob-Speicher für große technische Datasets oder für die Eingabewerte Bilder oder Mediendateien Ihrer Anwendungsprozesse. Sie können Warteschlangen für die asynchrone Kommunikation in einer Lösung verwenden. Finden Sie unter [Einführung in Microsoft Azure-Speicher](../storage/storage-introduction.md).

* [Dateispeicher Azure](https://azure.microsoft.com/services/storage/files/) - Freigaben gemeinsame Dateien und Daten in Azure mit dem standard SMB-Protokoll, die für einige HPC Cluster-Lösungen benötigt wird.

* [Dem Datenspeicher](https://azure.microsoft.com/services/data-lake-store/) - bietet eine Hyperscale Apache Hadoop Distributed File System für die Cloud, nützlich für Stapel, in Echtzeit, und interaktive Analytics.

### <a name="data-and-analysis-services"></a>Daten und Analysis services

Einige Szenarien große berechnen umfangreiche Daten Zahlungen umfassen, oder Generieren von Daten, die weitere Verarbeitung oder Analyse erledigen. Azure bietet mehrere Daten und Analysis Services, einschließlich:

* [Daten Factory](https://azure.microsoft.com/documentation/services/data-factory/) - Builds Daten basierende Workflows Datenpipelines () die Verknüpfung, aggregieren und Transformieren von Daten aus lokalen-Cloud-basierte und Internet-Datenspeicher.

* [SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/) - stellt die wichtigsten Features von einem Microsoft SQL Server relationalen Datenbank Management-System in einen verwalteten Dienst.

* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/) - bereitstellt und Vorschriften Windows Server oder Linux-basierten Apache Hadoop Cluster in der Cloud zu verwalten, analysieren und Erstellen von Berichten zu groß Daten.

* [Computer-Schulung](https://azure.microsoft.com/documentation/services/machine-learning/) – hilft Ihnen der erstellen, testen, Steuerung und Vorhersage analytische Lösungen in einem vollständig verwaltete Dienst verwalten.

### <a name="additional-services"></a>Zusätzliche Dienste

Ihre Lösung große berechnen möglicherweise weiteren Azure Dienste Verbindung zu Ressourcen lokal oder in anderen Umgebungen benötigen. Beispiele für:

* [Virtuelle Netzwerk](https://azure.microsoft.com/documentation/services/virtual-network/) - erstellt einen logisch isolierten Abschnitt in Azure Azure Ressourcen miteinander oder auf Ihrem lokalen Data Center verbinden. Mit einem Cross lokale virtuelle Netzwerk können große berechnen von Applications lokaler Daten, Active Directory-Dienste und Lizenzservern zugreifen.

* [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) - erstellt eine private Verbindung zwischen Microsoft Data Center und Infrastruktur, die lokal ist oder in einer Umgebung für die gemeinsame Speicherort. ExpressRoute bietet höhere Sicherheit, mehr Zuverlässigkeit, höhere Geschwindigkeit und unteren Wartezeiten als normalen Verbindungen über das Internet an.

* [Dienstbus](https://azure.microsoft.com/documentation/services/service-bus/) - bietet verschiedene Verfahren für Applikationen zu vermitteln oder exchange-Daten, ob sie auf Azure auf einem anderen Cloud-Plattform oder in einem Data Center befinden.

## <a name="next-steps"></a>Nächste Schritte

* Finden Sie [Technische Ressourcen für Stapel und HPC](big-compute-resources.md) technischen Leitfaden zum Erstellen einer Lösung zu finden.

* Diskutieren Sie Azure Optionen für Partner, einschließlich Zyklus Computing und UberCloud an.

* Weitere Informationen zum Azure große berechnen Lösungen durch [Towers Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222), [Altair](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/), [ANSYS](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)und [d3VIEW](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088)übermittelt.

* Die neuesten Ankündigungen finden Sie unter [Microsoft HPC und Stapel Teamblog](http://blogs.technet.com/b/windowshpc/) und den [Azure-Blog](https://azure.microsoft.com/blog/tag/hpc/).

<!--Image references-->
[parallel]: ./media/batch-hpc-solutions/parallel.png
[coupled]: ./media/batch-hpc-solutions/coupled.png
[iaas_cluster]: ./media/batch-hpc-solutions/iaas_cluster.png
[burst_cluster]: ./media/batch-hpc-solutions/burst_cluster.png
[batch_proc]: ./media/batch-hpc-solutions/batch_proc.png
