<properties 
    pageTitle="Konfigurieren einer Web-Anwendung-Firewall (WAF) für App-Service-Umgebung" 
    description="Erfahren Sie, wie Sie eine Web-Anwendung Firewall vor der App-Service-Umgebung konfigurieren." 
    services="app-service\web" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="naziml"/>    

# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Konfigurieren einer Web-Anwendung-Firewall (WAF) für App-Service-Umgebung

## <a name="overview"></a>(Übersicht) ##
Web-Anwendung Firewalls wie der [Barracuda WAF für Azure](https://www.barracuda.com/programs/azure) , die auf der [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) hilft Ihrer Webanwendungen durch Prüfen von Web-Verkehr SQL-Injektionen, websiteübergreifende Scripting, Schadsoftware Uploads und Anwendung DDoS und anderen Angriffen blockieren eingehende secure verfügbar ist. Es untersucht auch die Antworten von den Back-End-Webservern für Data Loss Prevention (DLP). In Kombination mit der Isolation und zusätzliche Skalierung von App-Service-Umgebungen bereitgestellt, bietet dies eine ideale Umgebung zu Host Business kritische Webanwendungen, die bösartige Besprechungsanfragen und hohe Volumen an Netzwerkverkehr aufnehmen müssen.

+[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Setup ##
Unsere App-Service-Umgebung hinter mehreren laden gleichmäßig Instanzen von Barracuda WAF für dieses Dokument, das wir konfiguriert werden kann, dass nur Daten aus der WAF die App-Service-Umgebung erreichen kann und nicht Verfügung, von der DMZ zur stehen verteilt. Wir haben auch Azure Datenverkehr Manager vor unserer Barracuda WAF Instanzen für den Lastenausgleich über Azure Data Center und Regionen. Die Einrichtung auf hoher Ebene Diagramm würde so aussehen wie nachfolgend gezeigt.

![Architektur][Architecture] 

> Hinweis: Mit der Einführung von [ILB für App-Service-Umgebung unterstützt](app-service-environment-with-internal-load-balancer.md)wird, können Sie die ASE, um von der DMZ nicht zugegriffen werden und nur zur Verfügung, mit dem privaten Netzwerk konfigurieren. 

## <a name="configuring-your-app-service-environment"></a>Konfigurieren der App-Service-Umgebung ##
Zum Konfigurieren einer App-Service-Umgebung finden Sie in [unseren Dokumentation](app-service-web-how-to-create-an-app-service-environment.md) zu diesem Thema. Nachdem Sie eine App-Service-Umgebung erstellt haben, können Sie [Web Apps](app-service-web-overview.md), [Apps API](../app-service-api/app-service-api-apps-why-best-platform.md) und [Mobile-Apps](../app-service-mobile/app-service-mobile-value-prop.md) in dieser Umgebung erstellen, die alle hinter der WAF geschützt werden, den, die wir im nächsten Abschnitt konfigurieren.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Konfigurieren von Ihrem Barracuda WAF Cloud-Dienst ##
Barracuda verfügt über eine [ausführliche Artikel](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) zum Bereitstellen von deren WAF auf einem virtuellen Computer in Azure. Aber da wir Redundanz möchten und keinen einzelnen Punkt des Fehlers vorstellen, mindestens 2 WAF Instanz virtuellen Computern in der gleichen Cloud-Dienst bereitstellen, wenn diese Anweisungen folgen soll.

### <a name="adding-endpoints-to-cloud-service"></a>Hinzufügen von Endpunkten in die Cloud-Dienst ###
Nachdem Sie in der Cloud-Dienst 2 oder mehr WAF virtueller Computer Instanzen haben können das [Azure-Portal](https://portal.azure.com/) Sie HTTP und HTTPS Endpunkte hinzufügen, die von der Anwendung verwendet werden, wie in der nachstehenden Abbildung gezeigt.

![Konfigurieren von Endpunkt][ConfigureEndpoint]

Wenn Ihre Anwendung anderen Endpunkten verwenden, stellen Sie sicher, die diese Liste auch hinzufügen. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Konfigurieren von Barracuda WAF über deren Verwaltungsportal ###
Barracuda WAF verwendet TCP Port 8000 für Konfiguration über deren Verwaltungsportal an. Da wir mehrerer Instanzen von der WAF virtuellen Computern haben, müssen Sie hier die Schritte für jede Instanz der virtueller Computer wiederholen. 


> Hinweis: Nachdem Sie WAF Konfiguration abgeschlossen haben, entfernen Sie den Endpunkt TCP/8000 aus allen Ihrer WAF virtuellen Computer, um Ihre WAF schützen.

Fügen Sie den Endpunkt Management wie in der nachstehenden Abbildung gezeigt zum Konfigurieren Ihrer Barracuda WAF hinzu.

![Fügen Sie Management Endpunkt hinzu][AddManagementEndpoint]
 
Verwenden Sie einen Browser, um den Management-Endpunkt in der Cloud-Dienst zu suchen. Wenn Ihre Cloud-Dienst test.cloudapp.net aufgerufen wird und, greifen Sie diesen Endpunkt durch Http://test.cloudapp.net:8000 suchen. Sie sollten eine Anmeldeseite finden Sie unter unter zufrieden sind, dass Sie anmelden können, verwenden Sie in der Phase der Installation WAF virtueller Computer angegebenen Anmeldeinformationen.

![Management-Anmeldeseite][ManagementLoginPage]

Einmal sollte Sie Login ein Dashboard als in der nachstehenden Abbildung angezeigt werden, die grundlegende Statistiken über den Schutz WAF präsentieren können.

![Projektmanagement-Dashboard][ManagementDashboard]

Klicken auf der Registerkarte Dienste, ermöglichen Ihnen die Konfiguration von Ihrer WAF für Dienste, die es geschützt wird. Weitere Details zur Konfiguration Ihrer Barracuda WAF finden Sie in [der Dokumentation](https://techlib.barracuda.com/waf/getstarted1). Im folgenden Beispiel wird anhand einer Azure Web App wurde erstellen Verkehr auf HTTP und HTTPS konfiguriert.

![Hinzufügen von Management Services][ManagementAddServices]

> Hinweis: Je nachdem wie die Anwendung konfiguriert sind, und welche Features in Ihrer Umgebung der App-Dienst verwendet werden, Sie müssen für TCP Ports als 80 und 443, z. B. weiterleiten, wenn Sie SSL IP-Setup für eine Web App haben. Für eine Liste der Network Ports, die in der App-Service-Umgebungen Näheres [Steuerelement eingehender Datenverkehr-Dokumentation](app-service-app-service-environment-control-inbound-traffic.md) Netzwerk-Ports Abschnitt.

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Konfigurieren von Microsoft Azure Datenverkehr Manager (OPTIONAL) ##
Wenn die Anwendung in mehreren Regionen verfügbar ist, sollten Sie laden gegeneinander Wägen sie hinter [Azure Datenverkehr Manager](../traffic-manager/traffic-manager-overview.md)ein ab. Dazu können Sie einen Endpunkt im [Azure klassischen-Portal](https://manage.azure.com) unter Verwendung des Cloud-Dienst für Ihre WAF im Profil Datenverkehr Manager wie in der nachstehenden Abbildung gezeigt hinzufügen. 

![Datenverkehr Manager Endpunkt][TrafficManagerEndpoint]

Wenn die Anwendung Authentifizierung erforderlich ist, stellen Sie sicher, dass Sie einige Ressourcen verfügen, die Authentifizierung für den Datenverkehr Manager Signal an für die Verfügbarkeit der Anwendung erfordern nicht. Sie können die URL, unter dem Abschnitt Konfigurieren der [Azure klassischen Portal](https://manage.azure.com) konfigurieren, wie unten dargestellt.

![Konfigurieren Sie den Datenverkehr-Manager][ConfigureTrafficManager]

Um den Datenverkehr Manager Pings aus Ihrer WAF an Ihrer Anwendung weitergeleitet werden sollen, müssen Sie Setup Website von Übersetzungen auf Ihre Barracuda WAF für die Weiterleitung von Datenverkehr an Ihrer Anwendung wie im folgenden Beispiel dargestellt.

![Website-Übersetzung][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Schützen von Datenverkehr an der App-Service-Umgebung mithilfe von Netzwerk-Sicherheitsgruppen (NSG)##
Führen Sie das [Steuerelement eingehender Datenverkehr Dokumentation](app-service-app-service-environment-control-inbound-traffic.md) Details auf Einschränkung des Datenverkehrs an Ihre App-Service-Umgebung aus der WAF nur mithilfe der Adresse VIP Ihre Cloud-Dienst ein. Hier ist ein Beispiel für Powershell-Befehl zur Durchführung dieser Aufgabe für TCP-80 ein.


    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

Ersetzen Sie die SourceAddressPrefix durch die virtuelle IP-Adresse von Ihrem WAF des Cloud-Dienst an.

> Hinweis: Die VIP von Ihrem Cloud-Dienst wird beim Löschen und neu Cloud-Dienst erstellen ändern. Stellen Sie sicher, dass die IP-Adresse in das Netzwerk Ressourcengruppe aktualisiert, nachdem Sie das tun. 
 
<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
