<properties
    pageTitle="Hybrid Verbindungen Übersicht | Microsoft Azure"
    description="Informationen Sie zu Hybrid Verbindungen, Sicherheit, TCP- und unterstützte Konfigurationen. MABS, WABS."
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ccompy"/>


# <a name="hybrid-connections-overview"></a>Hybrid Verbindungen (Übersicht)
Einführung in Hybrid Verbindungen, führt die unterstützten Konfigurationen und enthält die erforderlichen TCP Ports.


## <a name="what-is-a-hybrid-connection"></a>Was ist eine Verbindung Hybrid

Hybrid Verbindungen sind ein Feature von Azure BizTalk-Dienste. Hybrid-Verbindungen bieten eine einfache und bequeme Möglichkeit, das Feature Web Apps in Azure-App-Verwaltungsdienst (vormals Websites) und das Feature Mobile-Apps in Azure-App-Dienst (früher Mobile Services) auf lokale Ressourcen hinter der Firewall zu verbinden.

![Hybrid-Verbindungen][HCImage]

Hybrid Verbindungen bietet folgende Vorteile:

- Web Apps und Mobile-Apps können vorhandene lokaler Daten und Dienste sicheren Zugriff auf.
- Mehrere Web Apps oder Mobile-Apps können eine Verbindung Hybrid Zugriff auf eine lokale Ressource freigeben.
- Minimale TCP-sind erforderlich, auf Ihr Netzwerk zuzugreifen.
- Applikationen Hybrid Verbindungen mit Zugriff nur bestimmte lokalen Ressource, die veröffentlicht wird über die Verbindung Hybrid.
- Können auf eine beliebige lokale Ressource verbinden, die ein statischer TCP Port, wie etwa SQL Server, MySQL, HTTP-Web-APIs und die meisten benutzerdefinierten Webdienste verwendet.

    > [AZURE.NOTE] TCP basierte Dienste, mit denen dynamische Ports (z. B. FTP-Passivmodus oder erweiterten Passivmodus) werden derzeit nicht unterstützt. LDAP wird auch nicht unterstützt. LDAP verwendet eine statische TCP-, aber es konnte auch UDP verwenden. Es wird daher nicht unterstützt.

- Kann für alle von Web Apps (.NET, PHP, Java, Python, Node.js) und Mobile-Apps (Node.js, .NET) unterstützt Framework verwendet werden.
- Web Apps und Mobile-Apps können lokale Ressourcen auf genau die gleiche Weise zugreifen, als wäre das Web oder Mobile-App in Ihrem lokalen Netzwerk befindet. Beispielsweise können die gleichen Verbindung verwendete Zeichenfolge lokalen auch auf Azure verwendet werden.


Hybrid-Verbindungen bieten auch Unternehmensadministratoren mit Steuerelement und Sichtbarkeit in den Ihres Unternehmens Ressourcen zugegriffen von Hybrid-Anwendungen, einschließlich:

- Gruppenrichtlinieneinstellungen von verwenden, können Administratoren Hybrid Verbindungen im Netzwerk zulassen und auch bestimmen Ressourcen, die von Applications Hybrid zugegriffen werden können.
- Ereignis und Audit Protokollen auf dem Firmennetzwerk können Einblick in den Ressourcen durch Hybrid Verbindungen zugegriffen werden.


## <a name="example-scenarios"></a>Beispielszenarien

Hybrid-Verbindungen unterstützen die folgenden Kombinationen aus Framework und der Anwendung:

- .NET Framework Zugriff auf SQL Server
- .NET Framework Zugriff auf HTTP-/HTTPS-Diensten mit WebClient
- Zugriff von PHP mit SQL Server, MySQL
- Java-Zugriff auf SQL Server, MySQL und Oracle
- Java-Zugriff auf HTTP-/HTTPS-Dienste

Wenn Sie Hybrid Verbindungen mit lokalen SQL Server zugreifen, beachten Sie Folgendes:

- Mit dem Namen SQL Server-Instanzen Express muss konfiguriert sein, um statische Ports verwenden. Standardmäßig benannten SQL Express Instanzen dynamischen Ports verwenden.
- SQL Express standardmäßig Instanzen verwenden einen statischen Port TCP muss aktiviert sein. TCP ist standardmäßig nicht aktiviert.
- Bei Verwendung von Cluster oder Verfügbarkeit Gruppen, die `MultiSubnetFailover=true` Modus wird derzeit nicht unterstützt.
- Die `ApplicationIntent=ReadOnly` wird derzeit nicht unterstützt.
- SQL-Authentifizierung möglicherweise erforderlich ist, wie die End-to-End-Autorisierungsmethode, die durch die Azure-Anwendung und der lokalen SQLServer unterstützt.


## <a name="security-and-ports"></a>Sicherheit und ports

Hybrid-Verbindungen verwenden Autorisierung freigegeben Access Signatur (SAS), um die Verbindungen aus Azure Applications und der lokalen Hybrid-Verbindungs-Manager, um die in Verbindung zu sichern. Separate Verbindungsschlüssel sind für die Anwendung und der lokalen Hybrid-Verbindungs-Manager erstellt. Diese Verbindungsschlüssel können über bereitgestellt und unabhängig voneinander gesperrt werden.

Hybrid-Verbindungen bieten für nahtlose und sichere Verteilung der Schlüssel der Anwendungen und der lokalen Hybrid-Verbindungs-Manager.

Finden Sie unter [Erstellen und Verwalten von Hybrid Verbindungen](integration-hybrid-connection-create-manage.md).

Die *Anwendung Autorisierung unterscheidet sich von der Hybrid Verbindung*. Alle entsprechenden Autorisierungsmethode kann verwendet werden. Die Autorisierungsmethode hängt von über der Azure Cloud und der lokalen Komponenten unterstützten Autorisierungsmethoden-durchgehende ab. Z. B. greift Azure-Anwendung einer lokalen SQL Server. In diesem Szenario möglicherweise SQL-Autorisierung, dass die Autorisierungsmethode, die ist die End-to-End unterstützt.

#### <a name="tcp-ports"></a>TCP-
Hybrid Verbindungen erfordern nur ausgehende TCP- oder HTTP-Verbindungen aus Ihrem privaten Netzwerk. Sie müssen keine Firewallports öffnen oder ändern Ihre Netzwerkkonfiguration Shape, um alle eingehenden Verbindungen in Ihrem Netzwerk zulassen.

Die folgenden TCP-werden von Hybrid Verbindungen verwendet:

Port | Warum Sie benötigen
--- | ---
9350 - 9354 | Diese Ports werden für die Datenübertragung verwendet. Der Manager Dienstbus Relay untersucht Port 9350, um festzustellen, ob TCP-Konnektivität verfügbar ist. Wenn sie verfügbar ist, wird angenommen klicken Sie dann, dass Port 9352 auch verfügbar ist. Datenverkehr geht über den Port 9352. <br/><br/>Ausgehende Verbindungen auf diese Ports zulassen.
5671 | Wenn Port 9352 für Datenverkehr verwendet wird, wird als Steuerelement Kanal Port 5671 verwendet. <br/><br/>Ausgehende Verbindungen zu diesem Port zulassen
80, 443 | Diese Ports werden für einige Daten Besprechungsanfragen in Azure verwendet. Auch wenn Ports 9352 und 5671 nicht verwendet werden können, sind *dann* Ports 80 und 443 fallbackabfragen Ports, die für die Datenübertragung und dem Steuerelement-Kanal verwendet.<br/><br/>Ausgehende Verbindungen auf diese Ports zulassen. <br/><br/>**Notiz** Es wird nicht empfohlen, das diese als die fallbackabfragen Ports anstelle der anderen TCP-verwendet. Die HTTP/WebSocket wird als das Protokoll anstelle von systemeigenen TCP für Datenkanäle verwendet. Es kann geringere Leistung führen.



## <a name="next-steps"></a>Nächste Schritte

[Erstellen und Verwalten von Verbindungen Hybrid](integration-hybrid-connection-create-manage.md)<br/>
[Herstellen einer Verbindung eine lokale Ressource mit Azure Web Apps](../app-service-web/web-sites-hybrid-connection-get-started.md)<br/>
[Verbinden Sie mit lokalen SQL Server aus einer Azure Web app](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/>


## <a name="see-also"></a>Siehe auch

[REST-API für die Verwaltung von BizTalk-Dienste auf Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)
[BizTalk-Dienste: Editionen Diagramm](biztalk-editions-feature-chart.md)<br/>
[Erstellen einer BizTalk Service mit Azure-portal](biztalk-provision-services.md)<br/>
[BizTalk-Dienste: Dashboard, überwachen und Dezimalstellen Registerkarten](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png
