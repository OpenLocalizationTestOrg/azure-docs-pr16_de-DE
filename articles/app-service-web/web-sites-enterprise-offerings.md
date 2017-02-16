<properties 
    pageTitle="App-Verwaltungsdienst Azure Web Apps Angebote für Unternehmen" 
    description="Zeigt, wie Azure App Dienst Web Apps Enterprise Website-Lösungen für Ihr Unternehmen erstellen" 
    services="app-service\web" 
    documentationCenter="" 
    authors="apwestgarth" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/29/2016" 
    ms.author="anwestg"/>

# <a name="azure-app-service-web-apps-offerings-for-enterprise-whitepaper"></a>App-Verwaltungsdienst Azure Web Apps Angebote für Enterprise-Whitepaper #

Die Notwendigkeit senken und Vorführen von IT-Lösungen schneller in einer raschen Entwicklung-Umgebung erstellt neue Herausforderung für Entwickler, IT-Profis und -Manager. Benutzer suchen zunehmend für die Zeile des Business BRANCHENSPEZIFISCHE Webanwendungen schnelle, reagiert und von einem beliebigen Gerät verfügbar sein. Zur gleichen Zeit Unternehmen versuchen, ausschöpfen des Potenzials die Produktivität und Effizienz, die aus der Integration in die Cloud und mobile-Diensten stammen, dies ist möglicherweise über Geräte mithilfe von Active Directory für die Zusammenarbeit in Office 365 von Daten aus eine interne LOB-Anwendung, die Daten aus der Implementierung Unternehmen Vertrieb wiederum zieht an eines Beitrags so einfach wie das einmaliges Anmelden. [Azure App Dienst Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) ist ein Enterprise-Klasse Cloud-Dienst für entwickeln, testen und Web- und Windows-Dienste, Web-APIs und generische Websites ausgeführt. Zum Ausführen von Unternehmenswebsites, Intranet-Websites, Geschäfts-apps und digitale Marketingkampagnen in einem globalen Netzwerk von Rechenzentren optimiert für Maßstab und Verfügbarkeit mit Unterstützung für fortlaufende Integration verwendet werden, und moderne DevOps Vorgehensweisen.  

Dieses Whitepaper hervorgehoben, die Funktionen von der [Web Apps](/services/app-service/web/) -Dienst speziell dienten LOB Webanwendungen darüber liegendem Migration von vorhandenen Webanwendungen und Bereitstellung von Webanwendungen für neue LOB auf der Plattform ausgeführt wird. 

## <a name="audience"></a>Zielgruppe ##

IT-Profis, Architekten und Projektmanager, die gefunden werden, in der Cloud Web Auslastung migrieren, die aktuell lokal ausgeführt werden. Web Auslastung können entweder Business Mitarbeiter oder Business auf Partner Webanwendungen umfassen.

## <a name="introduction"></a>Einführung ##

App-Dienst Web Apps ist eine ideale Plattform für die sowohl internen und externen Webanwendungen und Services gehostet werden, wie es eine kostengünstigere, hoch skalierbare, verwaltete Lösung ermöglicht es Ihnen bietet, konzentrieren auf geschäftlicher Nutzen für Ihre Benutzer statt Ausgaben nehmen viel Zeit und Geld Wartung und Unterstützung Umgebungen zu trennen. Web Apps bietet eine flexible Plattform welche Bereitstellen Ihrer Enterprise-Webanwendungen bietet die Möglichkeit, fahren Sie mit der lokalen Active Directory über die Integration mit Microsoft Azure Active Directory authentifizieren unterstützen von einfach und schnell Bereitstellungen ausführenden verwenden Ihrer internen fortlaufender Integration und Bereitstellung des angegebenen Methoden, während Sie automatisch skalieren, um mit der geschäftliche Anforderungen - alle auf einer verwalteten Plattform wächst, die sich auf Ihrer Anwendung und nicht Ihre Infrastruktur konzentrieren können. 

## <a name="problem-definition"></a>Problemdefinition ##

Die IT-Querformat ist schnell ändern, mit einer Verschiebung Weg Hosten auf traditionelle Servern mit ihrer hohen großes Kosten auf den langen Lead Zeiten hinzu, die bei Bedarf verwendet verwenden der Dienste, die automatisch skalieren laden verarbeitet. IT-Abteilung sind die Kosten verringern abgefragt wird, und Platzbedarf Infrastruktur und Wartung aufwenden mit Schwerpunkt auf Reduzierung CAPEX gleichzeitig Flexibilität erhöht. Das Ende älteren Infrastruktur Plattformen an, z. B. Windows Server 2003, ist beibehalten von führenden IT-Abteilung, um zu überprüfen, Cloud Migration als eine mögliche Methode zum neuen langfristig großes Kosten zu vermeiden. In der Vergangenheit würde IT Einkauf Entscheidungen für andere Abteilungen stellen. jedoch abzubrechen zunehmend CMOs und anderen Business Unit Spitzen Rolle in deren Budget wie entfällt, und was deren Rendite ist. Unternehmen benötigen zunehmend, ihre Mitarbeiter ganz mobiler als je zuvor mit Mitarbeitern Remote arbeiten, mehr Zeit mit Kunden benötigen Zugriff auf Systeme problemlos Ausgaben sein.

Unternehmen benötigt ändern, monatlich, wöchentlich, täglich. Unternehmen sind für Chat globale Skala mit regulären aktualisierten vollständige der neuen Features, die von einem dritten oder intern bereitgestellten Dienste gefunden.  In einigen Fällen sind auch Unternehmen für die Funktionen, um ihre Applikationen isolieren und den Zugriff auf Ressourcen und gleichzeitig auch ausführenden Suchen des Fertigungsanlagen öffentlichen Cloud verwenden. Benutzer können höhere vorgeschlagen, viele unter Verwendung der Dienste in ihr eigenes privaten Leben wie Office 365 an. Sie erwartet haben Zugriff auf ähnliche, auf dem neuesten Stand Feature Rich-Dienste in ihre Arbeit Nutzungsdauer. Diese bei Bedarf, bewältigen IT muss aussehen helfen Unternehmen dies durch Auswahl und Integration mit Drittanbieter aktivieren services, sorgfältige Auswahl der Plattformen, die an die geschäftliche Anforderungen, anpassen können, aber auch mit einer reduzierten Gesamtkosten zuverlässigen wird.

Sofortige geschäftlicher Nutzen, Vorführung der neuen Features in regelmäßigen Abständen Vorführung suchen Entwicklungsteams. Für eine effektive Kosten, zuverlässigen Plattform die Integration mit ihren vorhandenen Tools und Methoden – Entwicklung, testen sie suchen, lassen Sie wieder los. und Arbeiten mit IT-Abteilung Automatisierung, Bereitstellung, Verwaltung und warnen, alle mit dem Ziel NULL Ausfall.

<a href="highlevel" />
## <a name="high-level-solution"></a>Lösung mit hoher Ebene ##

Webplattformen und Framework werden zunehmend verwendet entwickeln, testen und branchenanwendungen hosten.  Mit einer normalen branchenspezifische Anwendung, wie eine interne Mitarbeiter Fußball-System besteht häufig ausschließlich aus einer Web app mit einer Datenbank sichern zum Speichern der Daten, die mit der Anwendung verbunden.

App-Dienst Web Apps ist eine gute Option für das Hosten der Antrag, bietet eine skalierbar und zuverlässig Infrastruktur die verwaltete und den in der Nähe NULL manueller Eingriff und Ausfallzeiten Patch ist. Die Microsoft Azure-Plattform bietet viele Datenspeicheroptionen zur Unterstützung von Webanwendungen auf Web Apps aus Microsoft Azure SQL-Datenbank, eine verwaltete skalierbare relationalen Datenbank-als-Service, beliebte Dienste von unseren Partnern wie ClearDB MySQL-Datenbank und MongoDB gehostet.

Eine weitere Möglichkeit besteht darin, Ihre bestehende Investition lokal verwenden. Im Beispielszenario eines Mitarbeiters Fußball System, möchten Sie möglicherweise Ihre Datenspeicher innerhalb Ihrer eigenen internen Infrastruktur verwalten. Dies kann für die Integration mit internen Systeme (reporting, Lohn-und, Abrechnung usw.) oder die Anforderung einer IT-Governance erfüllen sein.  Web Apps bietet eine Reihe von Methoden, aktivieren Sie die Verbindung zu Ihrem auf lokale Infrastruktur:

- [App-Service-Umgebungen](app-service-app-service-environment-intro.md) - App Dienst Umgebungen (ASE) sind eine neue Premium-Funktion, die das Angebot Microsoft Azure-App-Verwaltungsdienst zuletzt hinzugefügt wurde.  ASEs bieten eine vollständig isolierte und dedizierte Umgebung für die Ausführung von App-Verwaltungsdienst Azure apps sicher bei hoher und bietet Isolation und sicheren Netzwerkzugriff   
- [Hybrid Verbindungen](../biztalk-services/integration-hybrid-connection-overview.md) – Hybrid Verbindungen sind ein Feature von Microsoft Azure BizTalk Services und Web Apps Verbindung zum lokal Ressourcen sicher, beispielsweise SQL Server, MySQL, Web-APIs und benutzerdefinierte Web Services aktivieren. 
- [Virtuelle Netzwerk-Integration](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/) – Web Apps-Integration in Azure-virtuellen Netzwerk können Sie Web app mit einer Azure-virtuellen Netzwerk herzustellen, die wiederum für Ihre auf lokale Infrastruktur über ein VPN zwischen Standorten verbunden werden können. 

Die folgenden Diagramme sind eine Lösung Beispiel auf hoher Ebene, mit der Konnektivitätsoptionen für lokale Ressourcen dargestellt.  Das erste Beispiel zeigt, wie dies kann mithilfe der Standardfeatures des App-Verwaltungsdienst Azure erreicht werden, und das zweite zeigt, wie dies sein kann auch mithilfe der Premium-Angebot, App Dienst Umgebungen erzielt.

Verwenden von Standard-App-Verwaltungsdienst-Funktionen:![](./media/web-sites-enterprise-offerings/on-premise-connectivity-solutions.png "Using Standard App Service Features")

Verwenden einer App-Service-Umgebung an:![](./media/web-sites-enterprise-offerings/on-premise-connectivity-solutions-ASE.png "Using an App Service Environment")

## <a name="business-benefits"></a>Geschäftliche Vorteile ##

App-Dienst Web Apps stellt eine Vielzahl geschäftliche Vorteile, die aktivieren die Funktion wesentlich kostengünstiger und in die Vorführung für die geschäftliche Anforderungen agiles sein. 

### <a name="paas-model"></a>PaaS Modell ###

App-Dienst Web Apps ist auf einer Plattform als Service-Modell erstellt, das eine Reihe von Kosten und Effizienz Spareinlagen bietet.  Sie müssen nicht mehr Stunden Verwalten von virtuellen Computern, Betriebssystemen und Framework Patch verbringen. Web Apps ist eine automatisch korrigierte-Umgebung, die womit Sie Fokussierung auf Verwalten Ihrer Webanwendungen und nicht virtuellen Computern, verlassen Teams kostenlosen größerem geschäftlichen bereitstellen kann.

PaaS Modell Unterstützung in den Bereichen Web Apps ermöglicht mehr als Experten der DevOps-Methode, um ihre Ziele zu erfüllen. Als ein Unternehmen, bedeutet dies, vollständige Verwaltung und Integration in der gesamten Lebenszyklus, einschließlich Entwicklung, testen, Version, für die Überwachung und Verwaltung und Support. 

Für die Entwicklungsteams können fortlaufender Integration und Bereitstellung Workflows konfiguriert werden, in Visual Studio Team Services, GitHub, TeamCity, Hudson oder BitBucket, automatisierte erstellen, testen und Bereitstellung versetzt schneller Release Zyklen und Verringern der Reibung beteiligt in vorhandene Infrastruktur freigeben aktivieren. Web Apps unterstützt auch die Erstellung mehrerer testen und staging-Umgebungen für den Workflow Release, nicht mehr benötigen Sie reservieren oder Hardware für folgende Zwecke zuweisen, können Sie beliebig viele Umgebungen wie soll und Definieren eigener Werbeaktion auf Workflow lassen erstellen. Als Unternehmen, die Sie auf ein Slot Testen von Datenquellen-Steuerelements, müssen Sie eine Reihe von Tests und nach dem erfolgreichen Abschluss doch konnte zu einer Phase Slot heraufstufen und schließlich auf Herstellung ohne Ausfallzeiten, mit dem zusätzlichen Vorteil, den Webanwendungen auf Web Apps gehostet sind vorinstalliert und Tastaturkürzel, um die bestmögliche Kundenzufriedenheit bereitzustellen austauschen.  Darüber hinaus können Unternehmen stellen Sie von der Testen in der App-Dienst Web Apps-Funktionen, die Herstellung direkte eines Abschnitts des Datenverkehrs an einen anderen Slot, überprüfen die Änderungen vor dem Wechsel alle Datenverkehr in die neue Bereitstellung oder Wiederherstellen alle den Datenverkehr in die vorherige Bereitstellung verwenden. 

Vorgänge Teams können überzeugt sind, dass sie die am besten Positionen auf Probleme mit einem der deren gehostet auf Web Apps mit der integrierten Webanwendungen reagieren können in die Überwachung und Benachrichtigungen Features. Operationen Teams sollte in Analytics und solche von Microsoft Visual Studio-Anwendung Einblicken, neue Relic und AppDynamics Überwachung Solutions bereits investiert haben. Diese werden auf Web Apps aktivieren Continuity und eine vertraute Umgebung aus dem Überwachen Ihrer Webanwendungen auch vollständig unterstützt.

Schließlich Web Apps stellt Funktionalität automatisch Sichern Ihrer app(s) und gehostete Datenbanken direkte zu einem Container Azure BLOB-Speicher. Mit dem Sie eine einfache Weise und sehr kostengünstiger Methode zum nach Datenverlust, wodurch sich komplexen auf lokale Hard- und Software wiederherstellen.

### <a name="ease-of-migration"></a>Steigern der Migration ###

Hardware-Wartung und Drehung ist ein wichtiges Problem für Unternehmen, wie die Release-wechselt für Hardware und Betriebssystemen beschleunigen. Vielleicht sind eine Anzahl von Windows Server 2003 R2-Server, die an das Ende des Supports in 2015 stammen, doch diese werden weiterhin Hostinganbieter Key Webanwendungen für Ihr Unternehmen? App-Dienst Web Apps sind gute Kandidaten Grundlage für diese Webanwendungen hosten und Ihnen die Rationalisierung der Business Hardware Platz. Web Apps erhalten Sie zentralen Zugriff auf einen Bereich von Hardwarespezifikationen die verwaltet und als Teil der Dienst, die Ersetzung und die Verwaltungskosten als Teil des Budgets Infrastruktur berücksichtigen überflüssig verwaltet werden.  Migration kann werden so einfach wie das Kopieren und Einfügen aus Ihrer vorhandenen Bereitstellung Web Apps oder eine komplexere Migration der wird mit der Migrations-Assistent von Web Apps Wert hinzufügen. Migrierte Webanwendungen genießen das gesamte Spektrum der Azure zusätzliche Services können Sie die Webanwendungen Integration Services. Beispielsweise können Sie berücksichtigen Azure Active Directory zum Steuern des Zugriffs auf eine Anwendung basierend auf Benutzer-Zuordnung zu Sicherheitsgruppen hinzufügen. Ein weiteres Beispiel kann, dass Cache Services zum Verbessern der Leistung und Verringern der Wartezeit, sofern insgesamt Benutzerfunktionalität besser hinzufügen. 

### <a name="enterprise-class-hosting"></a>Enterprise-Klasse Hosting ###

App-Dienst Web Apps bietet eine unveränderliche, zuverlässige Plattform die kann eine Vielzahl von Business Behandeln von kleinen internen Entwicklung und Test Auslastung mit hoher Auslastung der hochgradig skalierten Websites muss bewährt hat. Mithilfe von Web Apps, legen Sie fest, die Microsoft der gleichen Enterprise Class Hostinganbieter Plattform verwenden, wie ein Unternehmen für hohen Wert Web Auslastung verwendet. Web Apps, sowie alle Dienste auf der Azure-Plattform werden mit Sicherheit und Konformität mit gesetzlichen Vorschriften, wie etwa ISO (ISO/IEC 27001:2005); erstellt. SOC1 und SOC2 SSAE 16/ISAE 3402 Datenschutzpolitik, HIPAA BAA, PCI und Fedramp, Herzstück für jedes Element und Features für Weitere Informationen finden Sie unter Bitte [http://aka.ms/azurecompliance](/support/trust-center/compliance/). 

Microsoft Azure-Plattform ermöglicht Rolle basierte Autorisierung Steuerelemente aktivieren Enterprise Ebenen des Steuerelements nach Ressourcen innerhalb der Web Apps. RBAC bietet Unternehmen die Möglichkeit, eigene Access Informationsverwaltungsrichtlinien für alle von ihren Ressourcen in der Azure-Umgebung implementieren, indem Sie Zuweisen von Benutzern zu Gruppen und diesen Gruppen gegen die Ressource wie eine Web app wiederum die erforderlichen Berechtigungen zuweisen. Weitere Informationen zu RBAC in Azure finden Sie unter [http://aka.ms/azurerbac](../active-directory/role-based-access-control-configure.md). Durch die Nutzung von Web Apps, können Sie sicher, dass Ihre Webanwendungen in einer sicheren Umgebung bereitgestellt werden und Sie haben Vollzugriff, in welche Gebiet Ihre Bestände jederzeit bereitgestellt werden, sein. 

Azure Service-Umgebungen App [http://aka.ms/aseintro](http://aka.ms/aseintro) sind eine neue Premium Service Plan Option für Enterprise-Kunden, die App-Verwaltungsdienst Azure verwenden möchten, und diese bieten eine vollständig isolierte und dedizierte Umgebung.  Auf diese Weise können Unternehmenskunden Applikationen bereitgestellt, die sehr hoch Maßstab nutzen können aber auch Vollzugriff auf eingehende und ausgehende Netzwerkverkehr Probleme und ASEs können Anwendungen hochgeschwindigkeit sichere Verbindungen über virtuelle Netzwerke auf lokale Ressourcen angezeigt.

App-Dienst Web Apps sind auch vollständige verwenden Ihrer auf lokale Investitionen vorzunehmen, indem Sie die Möglichkeit, die auf Ihre internen Ressourcen, wie Ihre Datawarehouse oder SharePoint-Umgebung wieder eine Verbindung herstellen. Wie in [hoher Ebene Lösung](#highlevel) erläutert umso Hybrid Verbindungen und virtuelle Netzwerkkonnektivität herstellen von Verbindungen mit auf lokale Infrastruktur und alle Dienste verwenden.

### <a name="global-scale"></a>Globaler Ebene ###

App-Dienst Web Apps ist eine globale und skalierbare Plattform Ihrer Webanwendungen vergrößern und Anpassen an den Anforderungen eine wachsende Unternehmen schnell und mit minimalen langfristig zu planen und Kosten aktivieren. In normalen auf lokale Infrastruktur Szenarien, erläuterten Nachfrage sowohl lokal und geografischen würde erfordern eine große Datenmenge Management, Planen und Bereitstellen von Ausgaben und zusätzliche Infrastruktur verwalten. Web Apps bietet die Möglichkeit, Ihre Webanwendungen mit den Kriegsverlaufs und den Ablauf von Ihren Anforderungen skalieren. Beispielsweise die Anwendung Ausgaben z. B., für die meisten des Monats mit Ihre Benutzer können sich light Benutzer der Anwendung, aber als Termin für die monatlichen Ausgaben Übermittlungen eingegeben werden und Nutzung erhöht sich auf Ihrer Anwendung, Web Apps verfügt über eine Funktion zum automatisch Weitere Infrastruktur für die Anwendung bereitgestellt und dann nach die Verwendung nachgelassen hat erneut es kann Skalierung wieder in die geplante Infrastruktur, die Sie definieren.

Web Apps ist global in 24 Rechenzentren weltweit und wachsende verfügbar. Der am häufigsten aktualisierten Liste der Regionen und Speicherort finden Sie unter [http://aka.ms/azlocations](http://aka.ms/azlocations). Ihr Unternehmen kann einfach mit Web Apps globale Reichweite und Maßstab erzielen. Zunehmender Ihres Unternehmens in neue Regionen, die reporting Anwendung Dashboards, die Sie verwenden-Host auf Web Apps einfach in zusätzliche Rechenzentren bereitgestellt werden kann und dienen lokale Benutzer wesentlich mehr über die Kombination von Web Apps und Azure Datenverkehr Manager alle mit dem zusätzlichen Vorteil der skalierbare Infrastruktur unterhalb zu verkleinern und erweitern als den Anforderungen der regionalen Büros ändern.
 
## <a name="solution-details"></a>Lösungsdetails ##

Ein Beispiel für eine Anwendung Migrationsszenario wollen. Dadurch werden die Details der wie App Dienst Web Apps-Features zusammen hier hervorragende Lösung und Business Wert angeben.
 
In diesem Beispiel wird die Zeile des Business-Anwendung, die wir erörtern werden Aufwand reporting-Anwendung, und übermitteln Sie ihre Ausgaben für Rückerstattung Mitarbeiter können. Die Anwendung wird auf einem Windows Server 2003 R2 ausführen IIS6 gehostet, und die Datenbank ist ein SQL Server 2005-Datenbank. Der Grund wir wählen Sie ältere Server liegt mit den kommenden Ende der Dienst für Windows Server 2003 R2 und SQL Server 2005, und wir haben [Tools](http://aka.ms/websitesmigration) und [Leitfäden](http://aka.ms/websitesmigrationresources) Auslastung in Azure automatisch migrieren. Denken Sie daran das in diesem Beispiel verwendete Muster gelten für eine Breite Migrationsszenarien zutreffen. 

### <a name="migrate-existing-application"></a>Migrieren von vorhandenen Anwendung ###

Schritt 1 der gesamten Lösung für das Verschieben einer Line-of-Business-Anwendungs zum Web Apps ist zum Identifizieren der vorhandenen Anwendung Posten und Architektur. Das Beispiel in diesem Dokument ist eine ASP.NET Web-Anwendung, die auf einem einzelnen IIS-Server mit der Datenbank auf einem separaten SQL Server gehostet gehostet wird, wie in der folgenden Abbildung gezeigt. Mitarbeiter-Anmeldung an das System mit einer Kombination von Benutzername und Kennwort, Ausgaben Details ein- und gescannte Kopien Rezepte, in der Datenbank für jedes Element der Fußball hochladen. 
 
![](./media/web-sites-enterprise-offerings/on-premise-app-example.png)

#### <a name="items-to-consider"></a>Elemente zu berücksichtigen ist ####

Wenn aus einer lokalen Umgebung Migrations-Anwendung, sollten Sie beachten müssen, einige Einschränkungen der Web Apps. Hier sind einige wichtigen Themen bei der Migration von Webanwendungen zum Web Apps ([http://aka.ms/websitesmigrationresources](http://aka.ms/websitesmigrationresources)) berücksichtigt werden:

-   Port Bindungen – unterstützt Web Apps nur Port 80 für HTTP und 443 für HTTPS-Datenverkehr. Wenn eine Anwendung einen anderen Anschluss verwendet, und klicken Sie dann einmal migriert werden die Anwendung machen, Verwendung von Port 80 für HTTP und port 443 für HTTPS-Datenverkehr. Dies ist oft ein Problem harmlos, wie es in auf lokale Bereitstellungen vornehmen üblich ist mit verschiedenen Ports und die Verwendung von Domänennamen, insbesondere nicht im Entwicklung und Test-Umgebungen zu umgehen
-   Authentifizierung – unterstützt Web Apps anonyme Authentifizierung standardmäßig und formularbasierte Authentifizierung durch eine Anwendung bezeichnet. Web Apps kann Windows-Authentifizierung anbieten, wenn die Anwendung nur mit Azure Active Directory und ADFS integriert ist. Dies ist ein Feature der ausführlicher erläutert wird [hier](http://aka.ms/azurebizapp) 
-   GAC Grundlage Assemblys – Web Apps lässt sich nicht auf die Bereitstellung von Assemblys in der globalen Cache Assemblycache (). Daher, wenn die Anwendung migriert werden nutzt diesen lokalen bereitstellen, sollten Sie die Assemblys in den Ordner Bin der Anwendung verschieben.
-   IIS5 Kompatibilitätsmodus – Web Apps unterstützt keine IIS5 Kompatibilitätsmodus und als solche jede Instanz Web Apps und alle Webanwendungen unter übergeordneten Web Apps-Instanz in der gleichen Worker-Prozess innerhalb einer einzelnen Anwendung Ressourcenpool ausführen.
-   Verwenden von COM-Bibliotheken – Web Apps lässt sich nicht auf die Registrierung von COM-Komponenten auf der Plattform aus. Daher Wenn die Anwendung von einem beliebigen COM-Komponenten verwenden, diese in verwaltetem Code neu geschrieben werden müssten und mit der Anwendung bereitgestellt.
-   Klicken Sie auf Web Apps können Anforderungsmonitor – Anforderungsmonitor unterstützt werden. Als Teil der Anwendung bereitgestellt werden müssen, und der Web-Anwendung Verbindungszeichenfolgeneigenschaft registriert. Weitere Informationen finden Sie unter [http://aka.ms/azurewebsitesxdt](web-sites-transform-extend.md). 

Nachdem werden folgende Themen angesehen, sollte der Webanwendung für die Cloud bereitstehen. Keine Sorge, und wenn einige Themen nicht vollständig erfüllt ist, gibt Ihnen das Migrationstool optimale Leistung bis zur Migration hinzuzufügen. 

Die nächsten Schritte des Migrationsvorgangs sind, eine App-Dienst Web app und einer SQL Azure-Datenbank zu erstellen. Es gibt mehrere Größen der Web Apps Instanzen mit unterschiedlichen Anzahl von CPUs und RAM Beträge verfügbar ist, wählen Sie auf Grundlage Ihrer Anforderung aus Web Applications. Weitere Informationen und Preise, finden Sie unter [http://aka.ms/azurewebsitesskus](/pricing/details/websites/). Ebenso eignet sich Microsoft Azure SQL-Datenbank auf alle eines Unternehmens muss mit verschiedenen Ebenen Dienst und Leistung Ebenen Vorschriften zu erfüllen. Weitere Informationen finden Sie unter [http://aka.ms/azuresqldbskus](/pricing/details/sql-database/)werden. Nachdem erstellt, die Anwendung zu App Dienst Web Apps, entweder über FTP oder WebDeploy hochgeladen wird, und klicken Sie dann auf die Datenbank verschieben.

Bei dieser Migration verwendet die Lösung Azure SQL-Datenbank, aber das ist nicht die einzige Datenbank, die auf Azure unterstützt wird. Unternehmen können auch machen, verwenden von MySQL, MongoDB, Azure DocumentDB und viele weitere über Add-ons im [Azure-Store](/marketplace/partner-program/)erworben werden können. 

Beim Erstellen einer SQL Azure-Datenbank stehen eine Reihe von Optionen zum Importieren von einer vorhandenen Datenbank aus einer lokalen Server aus ein Skript für die Verwendung der [Datenebene Anwendung exportieren und importieren](http://aka.ms/dacpac)einer vorhandenen Datenbank generieren. 

Die Anwendungsdatenbank Ausgaben durch Erstellen einer neuen Azure SQL-Datenbank erstellt wurde, Herstellen einer Verbindung mit der Datenbank mithilfe von SQL Server Management Studio und anschließende Ausführen eines Skripts auf das Schema der Datenbank erstellen, und füllen Sie es mit Daten aus der lokalen Datenbank.

Der letzte Schritt in dieser ersten Phase der Migration erfordert die Aktualisierung der Verbindungszeichenfolgen in der Datenbank für die Anwendung. Dies kann über das Azure-Portal erreicht werden. Für jede Web app können Sie bestimmte Anwendungseinstellungen, einschließlich jede beliebige durch die Anwendung verwendet wird, die Verbindung zu einer beliebigen Datenbank verwendeten Verbindungszeichenfolge ändern.

### <a name="alternatives-to-using-azure-sql-database"></a>Alternativen zur Verwendung von Azure SQL-Datenbank ###

Die Azure-Plattform bietet eine Reihe von Alternativen zur Verwendung von Azure SQL-Datenbank einer primären Applikationen Webdatenbank, handelt es sich verschiedene Auslastung h. aktivieren Verwenden einer Lösung NoSQL oder die Plattform eines Unternehmens Daten nach Bedarf zu aktivieren. Beispielsweise ein Unternehmens am effektivsten halten möglicherweise Daten, die nicht gespeichert werden müssen, Ort oder in einer öffentlichen Cloud-Umgebung, und daher zum Verwalten der Verwendung von deren lokale Datenbank suchen möchten.

#### <a name="connectivity-to-on-premises-resources"></a>Verbindung zu auf lokale Ressourcen ####
App-Dienst Web Apps bietet mehrere Möglichkeiten zum Herstellen einer Verbindung mit auf lokale Ressourcen, wie z. B. Datenbanken Wiederverwendung vorhandene Infrastruktur von hoher Wert. Die Optionen sind wie unten aufgelistet:

- App-Service-Umgebungen isoliert sind und erstellt in einem Subnetz eines virtual Network, ermöglichen somit der Umgebung zur Kommunikation mit privaten Endpunkte befindet sich innerhalb der gleichen virtuellen Netzwerk - [http://aka.ms/appserviceasenetworking](http://aka.ms/appserviceasenetworking)
- Web Apps virtuelle Netzwerkintegration unterstützt die Integration zwischen Web Apps und ein virtuelles Netzwerk Azure, Zulassen des Zugriffs auf Ressourcen im Netzwerk, die Ihre auf lokale mit Standort-zu-Standort VPN bestehender Konnektivität direkt an Ihre auf lokale Systeme können virtuelle ausgeführt.
- Hybrid-Verbindungen sind ein Feature von Azure BizTalk-Dienste und bieten eine einfache Möglichkeit, eine einzelne Verbindung Ressourcen wie SQL Server, MySQL, HTTP-Web-APIs und am häufigsten benutzerdefinierte Webdienste lokalen.

#### <a name="scale-and-resiliency"></a>Maßstab und Stabilität ####

Wächst ein Unternehmen seine Mitarbeiter, über Akquisitionen oder natürlich organisch Wachstum, also auch müssen Applikationen skalieren, um diese neuer Bedarf entsprechen web. Tatsächlich um heute ist zu einer noch größeren verteilt am selben Standort Teams und Mitarbeiter remote finden Sie unter Allgemeine, beispielsweise Unternehmen mit Büros in den Vereinigten Staaten, Europa und Asien, mit einem mobilen Sales erzwingen, dass in vielen Weitere Gebiete. Web Apps verfügt über eine Funktion, flexible geänderten Skalierung bequem und automatisch zu behandeln.

App-Dienst Web Apps ermöglicht Webanwendungen so konfiguriert werden, dass automatisch über das Azure-Portal, je nach zwei Vektoren – geplante Zeiten oder durch CPU-Auslastung skalieren. Web Apps automatische Skalierung ermöglicht das kostengünstiger und extrem flexible berücksichtigen größer Änderungen Verwendung für alle branchenanwendungen von Webanwendungen wie unsere Fußball reporting-System in der marketing-Websites, die eine hohe Burst des Datenverkehrs für eine kurze Dauer der Werbeaktion auftreten. Weitere Informationen und Anleitungen Skalierung Ihrer Webanwendungen mit Web Apps finden Sie unter [Skalieren Websites](web-sites-scale.md).

Zusätzlich zu der Skalierung Flexibilität Web Apps ermöglicht die Gesamtkosten für die Plattform Geschäftskontinuität und Stabilität durch die möglichen Verteilung von Webanwendungen und ihren Ressourcen über mehrere Rechenzentren und geographischen Regionen an.

## <a name="summary"></a>Zusammenfassung ##
App-Dienst Web Apps bietet eine flexible, kostengünstige, reagiert Lösung für die dynamischen Anforderungen ein Geschäft in einer raschen Entwicklung-Umgebung. Erhöhen der Produktivität der Web Apps hilft Unternehmen und Effizienz durch Erstellen einer verwalteten Plattform mit modernen DevOps Funktionen und reduzierte Hände auf Verwaltung verwenden, während Sie Enterprise-Funktionen in skalieren, Stabilität, Sicherheit und Integration in lokalen Posten.

## <a name="call-to-action"></a>Rufen Sie in Aktion ##
Weitere Informationen zu dem Azure App Dienst Web Apps Dienst, besuchen [http://aka.ms/enterprisewebsites](/services/websites/enterprise/) , in dem Informationen kann als Quelle stammen, und melden Sie sich für eine Testversion heute bei [http://aka.ms/azuretrial](/pricing/free-trial/) Dienst auswerten und entdecken Sie die Vorteile für Ihr Unternehmen.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 
 
