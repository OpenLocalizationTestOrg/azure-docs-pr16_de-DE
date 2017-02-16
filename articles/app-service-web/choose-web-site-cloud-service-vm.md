<properties
    pageTitle="Azure-App-Dienst, virtuellen Computern, Dienst Fabric und Cloud Services-Vergleich | Microsoft Azure"
    description="Erfahren Sie, wie zwischen Azure-App-Verwaltungsdienst, virtuellen Computern, Dienst Fabric und Cloud-Dienste für das Hosten von Webanwendungen auswählen."
    services="app-service\web, virtual-machines, cloud-services"
    documentationCenter=""
    authors="tdykstra"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2016"
    ms.author="tdykstra"/>

# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Vergleich der Azure-App-Dienst, virtuellen Computern, Dienst Fabric und Cloud Services

## <a name="overview"></a>(Übersicht)

Azure bietet mehrere Methoden zum Hosten von Websites: [Azure-App-Verwaltungsdienst][], [virtuellen Computern][], [Dienst Fabric][]und [Cloud Services][]. In diesem Artikel können Sie die grundlegende Informationen zu den Optionen, und nehmen Sie die richtige Wahl für eine Webanwendung.

App-Verwaltungsdienst Azure ist die beste Wahl für die meisten Web apps. Bereitstellung und Verwaltung sind in der Plattform integriert, Websites können schnell skalieren, um hohe Datenverkehr verarbeitet und der integrierten Auslastung zu verteilen und den Datenverkehr-Manager bieten hohe Verfügbarkeit. Sie können vorhandene Websites Azure-App-Dienst problemlos mit einem [online Migrationstool](https://www.migratetoazure.net/)verschieben, verwenden Sie eine Open Source-app aus dem Katalog der Web-Anwendung oder Erstellen einer neuen Website mit Framework und Tools Ihrer Wahl. Das Feature [WebJobs][] erleichtert Hintergrundauftrag Verarbeitung bei der App-Dienst Web app hinzufügen.

Dienst Fabric ist eine gute Wahl, wenn Sie eine neue app erstellen oder erneutes Schreiben eine vorhandene app, um eine Microservice Architektur verwenden. Apps, die in einem freigegebenen Pool Maschinen ausgeführt wird, können Sie klein anfangen und vergrößern, um umfangreiche Skala mit Hunderte oder Tausende von Autos Bedarf an. Dynamische Services ganz einfach konsistent und zuverlässig app Zustand gespeichert und Dienst Fabric verwaltet automatisch Dienst Partitionierung, skalieren und Verfügbarkeit für Sie.  Dienst Fabric unterstützt auch WebAPI mit geöffneten Web-Oberfläche für .NET (OWIN) und ASP.NET Core.  Im Vergleich zu App-Dienst, bietet Service Fabric auch mehr Kontrolle über oder direkten Zugriff auf die zugrunde liegende Infrastruktur. Können in Ihren Servern remote oder Server Startaufgaben konfigurieren. Cloud Services ähnelt dem Dienst Fabric in Grad der Kontrolle im Vergleich zu Center für erleichterte Bedienung, aber es ist jetzt ein legacy-Dienst und Fabric Service wird empfohlen, für die Entwicklung neuer.

Wenn Sie eine vorhandene Anwendung, die wesentliche Änderungen in der App-Dienst oder Dienst Fabric Ausführen benötigen verfügen, können Sie virtuellen Computern auswählen, um die Migration in der Cloud zu vereinfachen. Ordnungsgemäß erfordert konfigurieren, Sichern und Verwalten von virtuellen Computern jedoch viel mehr Zeit und IT-Fachwissen im Vergleich zu Azure-App-Verwaltungsdienst und Dienst Fabric. Wenn Sie, Azure-virtuellen Computern erwägen, stellen Sie sicher, dass Sie berücksichtigen den laufenden Wartungsaufwand patch, aktualisieren und Verwalten Ihrer Umgebung virtueller Computer. Azure-virtuellen Computern ist Infrastructure-as-a-Service (IaaS), während App-Dienst und Dienst Fabric Platform-as-a-Service (Paas) ausführen. 

## <a name="a-namefeaturesafeature-comparison"></a><a name="features"></a>Vergleich der Features

In der folgenden Tabelle werden die Funktionen von App-Dienst, Cloud Services, virtuellen Computern und Dienst Fabric die beste Wahl hilft Ihnen verglichen. Aktuelle Informationen zu der Vereinbarung zum SERVICELEVEL für jede Option finden Sie unter [Azure Service Level Agreements](/support/legal/sla/).

Feature|App-Verwaltungsdienst (Web apps)|Cloud-Dienste (Webrollen)|Virtuellen Computern|Dienst Fabric|Notizen
---|---|---|---|---|---
Nahezu sofortige Bereitstellung|X|||X|Bereitstellen einer Anwendung oder einer Anwendungsupdate auf einen Clouddienst, oder Erstellen eines virtuellen Computers dauert einige Minuten mindestens; Bereitstellen einer Anwendung mit einem Web-app dauert Sekunden.
Skalieren Sie später auf größeren Computern ohne die erneute Bereitstellung|X|||X|
Web Serverinstanzen freigeben Inhalt und Konfiguration, d. h., Sie müssen nicht erneut bereitstellen oder beim Skalieren neu zu konfigurieren.|X|||X|
Umgebung mit mehreren Bereitstellung (Herstellung und Staging)|X|X||X|Dienst Fabric ermöglicht es Ihnen, dass mehrere Umgebungen für Ihre apps oder unterschiedliche Versionen von der app-nebeneinander bereitstellen.
Automatische Verwaltung von OS|X|X|||Automatische Updates für OS geplant für eine zukünftige Service Struktur lassen Sie wieder los.
Nahtlose Plattform wechseln (einfach wechseln zwischen 32-Bit- und 64-Bit)|X|X|||
Bereitstellen von Code mit GIT, FTP|X||X||
Bereitstellen von Code mit Web bereitstellen|X||X||Cloud Services unterstützt die Verwendung von Web zum Bereitstellen von Updates für einzelne Rolleninstanzen bereitstellen. Allerdings werden, können Sie es für anfängliche Bereitstellung von einer Rolle verwenden, und wenn Sie Web bereitstellen für ein Update verwenden, müssen Sie separat für jede Instanz von einer Rolle bereitstellen. Mehrere Instanzen sind für die Inanspruchnahme der Cloud-Dienst Vereinbarung zum SERVICELEVEL für die Herstellung Umgebungen erforderlich.
WebMatrix-support|X||X||
Zugriff auf Dienste wie Dienstbus, Speicher, SQL-Datenbank|X|X|X|X|
Host Web oder Web Services Ebene einer Architektur mit mehreren Ebenen|X|X|X|X|
Host mittleren Ebene einer Architektur mit mehreren Ebenen|X|X|X|X|App-Dienst Web apps können einfach REST-API mittlere Ebene hosten, und das Feature [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) kann im Hintergrund verarbeitenden Aufträge hosten. Sie können eine dedizierte Website unabhängig Skalierbarkeit für die Leiste erzielen WebJobs ausführen. Die Vorschau- [API apps](../app-service-api/app-service-api-apps-why-best-platform.md) -Funktion bietet noch mehr Features für die restlichen Dienste verwalten.
Integrierte MySQL-als-Service-Unterstützung|X|X|X||Cloud Services können MySQL-als-Service durch ClearDBs Angebote, aber nicht als Teil des Workflows Azure-Portal integrieren.
Unterstützung für ASP.NET, klassischen ASP, Node.js, PHP, Python|X|X|X|X|Dienst Fabric unterstützt die Erstellung einer Front-End-Web mithilfe von [ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md) , oder Sie können jede Art von Anwendung (Node.js, Java usw.) als [Gast ausführbare](../service-fabric/service-fabric-deploy-existing-app.md)bereitstellen.
Skalieren Sie auf mehrerer Instanzen ohne die erneute Bereitstellung|X|X|X|X|Virtuellen Computern skalieren können, auf mehrere Instanzen, aber die Dienste, die ausgeführt werden müssen, diese Skalierung behandeln geschrieben werden. Sie müssen ein Lastenausgleich zum Weiterleiten von Besprechungsanfragen über den Computer, und erstellen Sie eine Gruppe Zugehörigkeit, um zu verhindern, dass gleichzeitige Neustarten aller Instanzen aufgrund von Fehlern Wartung oder Hardware konfigurieren.
Unterstützung von SSL|X|X|X|X|Für App-Dienst Web apps wird für den benutzerdefinierten Domänennamen SSL nur Basic- und Standard-Modus unterstützt. Informationen zur Verwendung von SSL mit Web apps finden Sie unter [konfigurieren ein SSL-Zertifikat für eine Website Azure](../app-service-web/web-sites-configure-ssl-certificate.md).
Integration von Visual Studio|X|X|X|X|
Remote-Debuggen|X|X|X||
Bereitstellen von Code mit TFS|X|X|X|X|
Netzwerkisolation mit [Azure-virtuellen Netzwerk](/services/virtual-network/)|X|X|X|X|Siehe auch [Azure Websites virtuelles Netzwerk-Integration](/blog/2014/09/15/azure-websites-virtual-network-integration/)
Unterstützung für [Azure Datenverkehr-Manager](/services/traffic-manager/)|X|X|X|X|
Integrierte Endpunkt für die Überwachung|X|X|X||
Remote desktop-Zugriff auf Servern||X|X|X|
Installieren von einem beliebigen benutzerdefinierten MSI||X|X|X|Dienst Fabric ermöglicht es Ihnen, eine ausführbare Datei als [Gast ausführbare](../service-fabric/service-fabric-deploy-existing-app.md) hosten, oder Sie können eine beliebige app installieren, auf den virtuellen Computern.
Möglichkeit zum Definieren/Start Aufgaben ausführen||X|X|X|
ETW-Ereignisse können hören möchten||X|X|X|

## <a name="a-namescenariosascenarios-and-recommendations"></a><a name="scenarios"></a>Szenarien und Empfehlungen

Hier sind einige häufige Anwendungsszenarien mit anderem Ort tätig welche Azure Web-hosting Option möglicherweise geeigneter für jede Empfehlungen ein.

- [Ich benötige ein Web-front-End mit Hintergrund Verarbeitung und Datenbank Back-End-branchenanwendungen mit integriert auf lokale Anlagen ausführen.](#onprem)
- [Ich benötige sehr zuverlässig Meine corporate Website gehostet, die gut skaliert und Angebote globale erreicht haben.](#corp)
- [Ich habe eine IIS6-Anwendung unter Windows Server 2003.](#iis6)
- [Ich bin Besitzer einer small Business, und ich benötige eine preisgünstige Möglichkeit, Meine Website host jedoch mit zukünftiges Wachstum berücksichtigen.](#smallbusiness)
- [Ich versuche, ein Web oder die Grafik-Designer, und ich möchte entwerfen und Erstellen von Websites für meine Kunden.](#designer)
- [Ich bin mit einem Web-Front-End-Anwendung mit mehreren Ebenen in der Cloud migrieren.](#multitier)
- [Meine Anwendung hängt stark angepasste Windows oder Linux-Umgebungen und in der Cloud verschoben werden soll.](#custom)
- [Meine Website verwendet open Source-Software, und ich möchte in Azure gehostet.](#oss)
- [Ich habe eine Line-of-Business-Anwendung, die Verbindung mit dem Firmennetzwerk muss.](#lob)
- [Ich möchte ein REST-API oder Webdienst für mobile Clients hosten.](#mobile)


### <a name="a-idonprema-i-need-a-web-front-end-with-background-processing-and-database-backend-to-run-business-applications-integrated-with-on-premise-assets"></a><a id="onprem"></a>Ich benötige ein Web-front-End mit Hintergrund Verarbeitung und Datenbank Back-End-branchenanwendungen mit integriert auf lokale Anlagen ausführen.

App-Verwaltungsdienst Azure ist eine großartige Lösung für komplexe branchenanwendungen an. Hiermit können Sie entwickeln Sie apps, die automatisch auf einer laden angeglichene Plattform skalieren, sind mit Active Directory gesichert und Verbinden mit Ihrem lokalen Ressourcen. Es ist diese einfach durch eine hervorragende Portal und APIs apps verwalten, und ermöglicht Ihnen, Einblick wie Kunden diese mit app Einblicke Tools verwenden. Das Feature [Webjobs][] kann Hintergrundprozesse ausgeführt und Aufgaben als Teil der Web-Kategorie und beim Hybrid Konnektivität und VNET Features erleichtern es Verbindung wieder auf lokale Ressourcen. Azure-App-Dienst bietet drei 9 Vereinbarung zum SERVICELEVEL von Web apps und ermöglicht Ihnen das:

* Führen Sie Ihre Applikationen zuverlässig auf eine Cloud selbstreparierendes, Auto-Patch aus.
* Skalieren Sie Automatisches in einem globalen Netzwerk von Rechenzentren.
* Sichern Sie und Wiederherstellen Sie, für die Wiederherstellung.
* ISO, SOC2 und PCI kompatibel sein.
* Nahtlose in Active Directory

### <a name="a-idcorpa-i-need-a-reliable-way-to-host-my-corporate-website-that-scales-well-and-offers-global-reach"></a><a id="corp"></a>Ich benötige sehr zuverlässig Meine corporate Website gehostet, die gut skaliert und Angebote globale erreicht haben.

App-Verwaltungsdienst Azure ist eine großartige Lösung für Unternehmenswebsites hosten. Web apps zu skalieren schnell und einfach in einem globalen Netzwerk von Rechenzentren Nachfrage zu können. Lokalen Reichweite, Fehlertoleranz und Management mit intelligenten Datenverkehr vergleichbar. Alle auf einer Plattform, die hervorragende Verwaltungstools bereitstellt, so dass Sie Website Gesundheit und Datenverkehr Website schnell und einfach Einblick in aus. Azure-App-Dienst bietet drei 9 Vereinbarung zum SERVICELEVEL von Web apps und ermöglicht Ihnen das:

* Führen Sie Ihren Websites zuverlässig auf eine Cloud selbstreparierendes, Auto-Patch aus.
* Skalieren Sie Automatisches in einem globalen Netzwerk von Rechenzentren.
* Sichern Sie und Wiederherstellen Sie, für die Wiederherstellung.
* Verwalten von Protokollen und Datenverkehr mit integrierten Tools.
* ISO, SOC2 und PCI kompatibel sein.
* Nahtlose in Active Directory

### <a name="a-idiis6a-i-have-an-iis6-application-running-on-windows-server-2003"></a><a id="iis6"></a>Ich habe eine IIS6-Anwendung unter Windows Server 2003.

App-Verwaltungsdienst Azure erleichtert die Infrastrukturkosten im Zusammenhang mit der Migration von älteren IIS6 Applications zu vermeiden. Microsoft hat erstellt [benutzerfreundliche Migrations-Tools und Hinweise zur detaillierte Migration](https://www.movemetowebsites.net/) , mit denen Sie Kompatibilität prüfen und identifizieren alle Änderungen, die vorgenommen werden müssen. Integration in Visual Studio und TFS allgemeine CMS Tools erleichtert die IIS6 Applikationen direkt in der Cloud bereitstellen. Nach der Bereitstellung bietet im Azur Portal robuste Management-Tools, mit denen Sie nach unten zum Verwalten der Kosten und Anfordern von bis zu besprechen nach Bedarf skalieren. Mit dem Migrationstool können Sie folgende Aktionen ausführen:

* Migrieren von schnell und einfach Ihre legacy Windows Server 2003-Webanwendung in der Cloud.
* Entscheiden Sie sich um Ihre angefügten SQL-Datenbank lokal zum Erstellen einer Hybrid-Anwendung zu verlassen.
* Automatisch verschieben Ihrer SQL-Datenbank mit der legacy-Anwendung.

### <a name="a-idsmallbusinessaim-a-small-business-owner-and-i-need-an-inexpensive-way-to-host-my-site-but-with-future-growth-in-mind"></a><a id="smallbusiness"></a>Ich bin Besitzer einer small Business, und ich benötige eine preisgünstige Möglichkeit, Meine Website host jedoch mit zukünftiges Wachstum berücksichtigen.

Azure App-Verwaltungsdienst ist eine großartige Lösung für dieses Szenario, da Sie ihn kostenlos verwenden können, und fügen Sie bei Bedarf weitere Funktionen hinzu. Jeder kostenlose Web app im Lieferumfang von Azure bereitgestellten Domäne (*IHRE_FIRMA*. azurewebsites.net), und die Plattform umfasst integrierte Bereitstellung und Verwaltungstools ebenso wie eine Anwendung Katalog, die für den Einstieg zu erleichtern. Es gibt viele andere Dienste und Skalierungsoptionen, mit denen die Website mit erhöhter Benutzer bei Bedarf weiterentwickelt. Mit Azure-App-Verwaltungsdienst können Sie folgende Aktionen ausführen:

- Mit der kostenlosen Ebene beginnen Sie, und klicken Sie dann oben skalieren Sie, je nach Bedarf.
- Mithilfe der Anwendung beliebte Webanwendungen, wie z. B. WordPress schnell einrichten.
- Fügen Sie zusätzliche Azure Dienste und Features an Ihrer Anwendung nach Bedarf hinzu.
- Secure Web app mit HTTPS an.

### <a name="a-iddesignera-im-a-web-or-graphic-designer-and-i-want-to-design-and-build-websites-for-my-customers"></a><a id="designer"></a>Ich versuche, ein Web oder die Grafik-Designer, und ich möchte entwerfen und Erstellen von Websites für meine Kunden

Azure-App-Verwaltungsdienst für Webentwickler und Designer kann problemlos mit einer Vielzahl von Tools und Framework integriert, umfasst Unterstützung Git und FTP-Bereitstellung und bietet enger Integration mit Tools und Diensten wie Visual Studio und SQL-Datenbank. Mit App-Verwaltungsdienst können Sie folgende Aktionen ausführen:

- Verwenden Sie die Befehlszeile Tools für [automatisierte Vorgänge][scripting].
- Arbeiten mit beliebten Sprachen wie [.net][dotnet], [PHP][], [Node.js][nodejs], und [Python][].
- Wählen Sie drei verschiedene Skalierung Stufen für durch zentrales Skalieren mit sehr hoher Kapazität aus.
- Integrieren mit anderen Azure-Diensten, wie etwa [SQL-Datenbank][sqldatabase], [Dienstbus] [ servicebus] und [Speicher][]oder Partner-Angebote aus dem [Azure Store][azurestore], z. B. MySQL und MongoDB.
- Integration mit Tools wie Visual Studio, Git, WebMatrix, WebDeploy, TFS und FTP.

### <a name="a-idmultitieraim-migrating-my-multi-tier-application-with-a-web-front-end-to-the-cloud"></a><a id="multitier"></a>Ich bin mit einem Web-Front-End-Anwendung mit mehreren Ebenen in der Cloud migrieren.

Wenn Sie eine Anwendung mit mehreren Ebene, wie etwa einen Webserver ausführen, die mit einer Datenbank, verbunden ist Azure-App-Verwaltungsdienst eine gute Option, die enger Integration in SQL Azure-Datenbank bietet. Und Sie können das Feature WebJobs für Back-End-Prozesse ausgeführt.

Wenn Sie mehr über die Server-Umgebung, wie etwa die Möglichkeit, remote in Ihrem Server Kontrolle oder Server Startaufgaben konfigurieren benötigen, wählen Sie für eine oder mehrere der Ebenen Dienst Fabric.

Wählen Sie für eine oder mehrere der Ebenen virtuellen Computern, verwenden Sie ein eigenes Computerabbild oder Server-Software oder Dienste, die Sie auf Dienst Fabric konfigurieren können nicht ausgeführt werden soll.

### <a name="a-idcustomamy-application-depends-on-highly-customized-windows-or-linux-environments-and-i-want-to-move-it-to-the-cloud"></a><a id="custom"></a>Meine Anwendung hängt stark angepasste Windows oder Linux-Umgebungen und in der Cloud verschoben werden soll.

Wenn die Anwendung komplexe Installation oder Konfiguration von Software und das Betriebssystem erfordert, ist virtuellen Computern wahrscheinlich die bestmögliche Lösung. Mit virtuellen Computern können Sie folgende Aktionen ausführen:

- Verwenden Sie des virtuellen Computers-Katalogs zum beginnen mit einem Betriebssystem, wie etwa Windows oder Linux, und passen Sie es für Ihren Anforderungen Anwendung.
- Erstellen und Hochladen ein benutzerdefiniertes Bild von einem vorhandenen lokalen Server auf einem virtuellen Computer in Azure ausführen.

### <a name="a-idossamy-site-uses-open-source-software-and-i-want-to-host-it-in-azure"></a><a id="oss"></a>Meine Website verwendet open Source-Software und in Azure gehostet werden möchte.

Wenn der open-Source-Framework App-Diensts unterstützt wird, werden die Sprachen und die Anwendung erforderlichen Framework automatisch konfiguriert. App-Dienst können Sie:

- Verwenden Sie viele beliebten öffnen Quelle Sprachen, wie etwa [.NET][dotnet], [PHP][], [Node.js][nodejs], und [Python][].
- Einrichten von WordPress, Drupal, Umbraco, DNN und viele andere Drittanbieter-Webanwendungen.
- Migrieren einer vorhandenen Anwendungs oder Erstellen eines neuen Kontos aus dem Katalog der Anwendung.

Wenn der open-Source-Framework App-Dienst nicht unterstützt wird, können Sie es auf eine der anderen Optionen Hostinganbieter Azure Web ausführen. Mit virtuellen Computern, installieren und konfigurieren die Software auf dem Computerabbild, das Windows sein kann oder Linux-basierten.

### <a name="a-idlobai-have-a-line-of-business-application-that-needs-to-connect-to-the-corporate-network"></a><a id="lob"></a>Ich habe eine Line-of-Business-Anwendung, die Verbindung mit dem Firmennetzwerk muss

Wenn Sie eine Linie-of-Business-Anwendung erstellen möchten, möglicherweise Ihrer Website direkten Zugriff auf Dienste oder Daten auf dem Firmennetzwerk erforderlich. Dies ist auf App-Dienst, Dienst Fabric und virtuellen Computern mithilfe von [Azure virtuelle Netzwerkdienst](/services/virtual-network/)möglich. App-Dienst können Sie das [VNET Integrationsfeature](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), wodurch Ihre Azure Applications ausführen, als wären sie in Ihrem Netzwerk Ihres Unternehmens.

### <a name="a-idmobileai-want-to-host-a-rest-api-or-web-service-for-mobile-clients"></a><a id="mobile"></a>Ich möchte ein REST-API oder Webdienst für mobile Clients hosten

HTTP-basierten Web Services ermöglichen Ihnen eine Vielzahl von Clients, einschließlich der mobilen Clients unterstützen. Wie ASP.NET Web API Integrieren von Visual Studio, damit es einfacher zu erstellen und die restlichen Dienste nutzen.  Diese Dienste werden von einem Web-Endpunkt verfügbar gemacht, daher es möglich ist, eine Web-hosting auf Azure Methode zum unterstützen dieses Szenario zu verwenden. App-Dienst ist jedoch eine gute Wahl für die Verwaltung von REST-APIs. Mit App-Verwaltungsdienst können Sie folgende Aktionen ausführen:

- Eine [mobile-app](../app-service-mobile/app-service-mobile-value-prop.md) oder [API-app](../app-service-api/app-service-api-apps-why-best-platform.md) , die HTTP-Webdiensts in einem Azure des Global verteilten Rechenzentren zu hosten, schnell zu erstellen.
- Migrieren von vorhandenen Services oder neue erstellen.
- Vereinbarung zum SERVICELEVEL für Verfügbarkeit mit einer einzelnen Instanz erzielen oder Skalieren auf mehreren dedizierten Computern.
- Verwenden Sie die veröffentlichte Site REST-APIs auf eine beliebige HTTP-Clients, einschließlich der mobilen Clients bereit.

> [AZURE.NOTE]
> Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Konto anmelden möchten, wechseln Sie zu <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, wo können Sie sofort eine kurzlebige Starter-app in Azure-App-Verwaltungsdienst kostenlos erstellen. Keine Kreditkarte erforderlich, keine Zusagen.

## <a name="a-idnextstepsa-next-steps"></a><a id="nextsteps"></a>Nächste Schritte

Weitere Informationen zu den drei Web Hostinganbieter Optionen finden Sie unter [Einführung in Azure](../fundamentals-introduction-to-azure.md).

Um die Optionen Einstieg in für eine Anwendung ausgewählt haben, finden Sie unter den folgenden Ressourcen:

* [Azure App-Verwaltungsdienst](/documentation/services/app-service/)
* [Azure Cloud Services](/documentation/services/cloud-services/)
* [Azure-virtuellen Computern](/documentation/services/virtual-machines/)
* [Dienst Fabric](/documentation/services/service-fabric)

<!-- URL List -->

[Azure App-Verwaltungsdienst]: /services/app-service/
[Cloud-Dienste]: http://go.microsoft.com/fwlink/?LinkId=306052
[Virtuellen Computern]: http://go.microsoft.com/fwlink/?LinkID=306053
[Dienst Fabric]: /services/service-fabric
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: http://www.windowsazure.com/develop/net/common-tasks/enable-ssl-web-site/
[azurestore]: http://www.windowsazure.com/gallery/store/
[scripting]: http://www.windowsazure.com/documentation/scripts/?services=web-sites
[dotnet]: http://www.windowsazure.com/develop/net/
[nodejs]: http://www.windowsazure.com/develop/nodejs/
[PHP]: http://www.windowsazure.com/develop/php/
[Python]: http://www.windowsazure.com/develop/python/
[servicebus]: http://www.windowsazure.com/documentation/services/service-bus/
[sqldatabase]: http://www.windowsazure.com/documentation/services/sql-database/
[Speicher]: http://www.windowsazure.com/documentation/services/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
