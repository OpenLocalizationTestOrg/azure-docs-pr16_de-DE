<properties
    pageTitle="Erstellen und verwenden eine interne Lastenausgleich mit einer App-Service-Umgebung | Microsoft Azure"
    description="Erstellen und Verwenden einer ASE mit einer ILB"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Verwenden eine interne Lastenausgleich mit einer App-Service-Umgebung #

Das Feature für die App-Dienst Environments(ASE) ist Premium Service Option der Azure-App-Dienst, der eine erweiterte Konfiguration Videofunktionen übermittelt, die in der Stempel mit mehreren Mandanten ist nicht verfügbar.  Das Feature ASE bereitstellt im Wesentlichen die Azure-App-Verwaltungsdienst in Ihrer Azure-virtuellen Network(VNet).  Um ein besseres Verständnis der Funktionen App Service-Umgebungen, lesen Sie die [Was ist eine App-Service-Umgebung] zu erhalten[ WhatisASE] Dokumentation.  Wenn Sie nicht wissen, die Vorteile des Betriebs in einem VNet lesen Sie den [Azure-virtuellen Netzwerk FAQ][virtualnetwork].  


## <a name="overview"></a>(Übersicht) ##


Eine ASE kann mit einem barrierefreien Internet-Endpunkt oder eine IP-Adresse in Ihrer VNet bereitgestellt werden.  Um die IP-Adresse an eine Adresse VNet festlegen müssen Sie Ihre ASE mit einem internen laden Balancer(ILB) bereitstellen.  Wenn Ihre ASE, mit einer ILB konfiguriert ist bieten Ihnen:

- eigene Domäne oder Unterdomäne.  Um zu vereinfachen, wird dieses Dokument Unterdomäne vorausgesetzt, aber Sie können sie manuell konfigurieren.  
- das Zertifikat für HTTPS
- DNS Management für Ihre Subdomain.  


Im Gegenzug können Sie Folgendes:

- Host Intranet Applications, wie branchenanwendungen, sicher in der Cloud, die Sie über eine Website auf Website oder ein ExpressRoute VPN zugreifen
- Host apps in der Cloud, die in Öffentliche DNS-Server nicht aufgelistet werden
- Erstellen von Internet isoliert Back-End-apps Ihrer front-End-apps mit sichere Integration können den


#### <a name="disabled-functionality"></a>Deaktivierte Funktionen ####

Es gibt einige Aktionen, die Sie nicht möglich, wenn Sie eine ILB ASE verwenden.  Diese Elemente umfassen:

- Verwenden von IPSSL
- Zuweisen von IP-Adressen zu bestimmten apps
- kaufen, und verwenden ein Zertifikat mit einer app über das Portal.  Natürlich weiterhin Zertifikate direkt mit einer Zertifizierungsstelle abrufen und verwenden Sie es mit Ihrer apps, nicht nur über das Azure-Portal.


## <a name="creating-an-ilb-ase"></a>Erstellen einer ILB ASE ##

Erstellen einer ILB ASE unterscheidet sich nicht viel von einer ASE wie gewohnt erstellt.  Weitere tieferen Informationen über das Erstellen einer ASE, lesen Sie [zum Erstellen einer App-Service-Umgebung][HowtoCreateASE].  Der Vorgang zum Erstellen einer ILB ASE ist zwischen einer VNet während der Erstellung ASE erstellen oder Auswählen einer bereits vorhandenen VNet identisch.  So erstellen Sie eine ILB ASE 

1.  Wählen Sie in der Azure-Portal **Neu -> Web + Mobile-App-Service-Umgebung >**
2.  Wählen Sie Ihr Abonnement
3.  Wählen Sie aus, oder erstellen Sie eine Ressourcengruppe
4.  Wählen Sie aus, oder erstellen Sie eine VNet
5.  Erstellen Sie ein Subnetz ein, wenn eine VNet auswählen
6.  **Virtuelle Netzwerkspeicherort-Konfiguration VNet >** wählen Sie aus, und legen Sie den Typ des VIP Internal
7.  Geben Sie Unterdomäne an (dies die Unterdomäne für apps erstellt in dieser ASE verwendet werden)
8.  Wählen Sie Ok aus, und klicken Sie dann erstellen


![][1]


Innerhalb der virtuelle Netzwerk Blade wird eine Option VNet Konfiguration.  Diese Berechtigung ermöglicht, die Sie zwischen einem externen VIP oder internen VIP auswählen.  Die Standardeinstellung ist externe.  Wenn Sie auf externe festgelegt wurden, wird Ihrer ASE ein Internet zugänglich VIP verwendet.  Wenn Sie die Internal auswählen, wird Ihre ASE mit einer ILB auf eine IP-Adresse in Ihrer VNet konfiguriert sein.  


Nach Auswahl der Internal, die Möglichkeit, mehr IP-Adressen in Ihrer ASE entfernt, und stattdessen müssen Sie die Unterdomäne von der ASE bereitstellen.  In einer ASE mit einer externen VIP wird der Namen der ASE für apps, die in dieser ASE erstellt in die Unterdomäne verwendet.  
Wenn Ihre ASE ***Contosotest*** und Ihre app aufgerufen wurde, in der betreffenden ASE aufgerufen wurde ***Mytest*** klicken Sie dann auf die Unterdomäne wäre von Format, ***contosotest.p.azurewebsites.net*** und die URL für die app wäre ***mytest.contosotest.p.azurewebsites.net***.  
Wenn Sie den Typ des VIP Internal festlegen, wird Ihr Name ASE nicht in die Unterdomäne die ASE verwendet.  Sie haben die Unterdomäne explizit angeben.  Wenn Sie Ihre Subdomain wurde ***contoso.corp.net*** und vorgenommenen app dieses ASE ***Timereporting*** benannte wäre die URL für die app ***timereporting.contoso.corp.net***.


## <a name="apps-in-an-ilb-ase"></a>In einer ILB ASE Apps ##

Erstellen einer app in einer ILB ASE entspricht dem Erstellen einer app in einer ASE Normal.  

1. Wählen Sie in der Azure-Portal **Neu -> Web + Mobile -> Web** oder **Mobile** oder **API-App**
2. Geben Sie die Namen der app
2. Wählen Sie Abonnements aus.
3. Wählen Sie aus, oder erstellen Sie Ressourcengruppe
4. Wählen Sie aus, oder erstellen Sie App-Dienst Plan(ASP).  Wenn das Erstellen einer neuen ASP wählen Ihrer ASE als Speicherort, und wählen Sie den Worker Pool, den Sie Ihre ASP in erstellt werden soll.  Beim Erstellen der ASP wählen Sie Ihre ASE als den Speicherort und den Worker Pool aus.  Wenn Sie den Namen der app angeben, sehen Sie, dass die Unterdomäne unter Ihrem Namen app wird durch die Unterdomäne für Ihre ASE ersetzt.   
5. Wählen Sie erstellen.  Sie sollten das Kontrollkästchen **Pin zum Dashboard** auswählen, wenn Sie möchten, dass die app auf dem Dashboard angezeigt.  

![][2]


Unter den Namen der Anwendung erhält der Unterdomänennamen die Unterdomäne mit Ihrer ASE entsprechend aktualisiert.  


## <a name="post-ilb-ase-creation-validation"></a>Beitrag ILB ASE Creation Überprüfung ##

Eine ILB ASE ist etwas anders als die nicht ILB ASE.  Wie bereits erwähnt, müssen Sie ein eigenes DNS verwalten und müssen Sie auch Ihr eigenes Zertifikat für HTTPS-Verbindungen angeben.  


Nachdem Sie Ihre ASE erstellt haben, werden Sie feststellen, dass die Unterdomäne der Unterdomäne zeigt Sie angegeben haben, und es wird ein neues Element im Menü " **Einstellungen** " **ILB Zertifikat**bezeichnet.  Die ASE wird mit ein selbst signiertes Zertifikat erstellt, HTTPS Testen erleichtert.  Im Portal erfahren Sie, die Sie benötigen, um Ihr eigenes Zertifikat für HTTPS bereitzustellen, aber dies ist zum Steuern, dass Sie ein Zertifikat besitzen, die mit Ihrem Unterdomäne wechselt.  

![][3]


Wenn Sie einfach Dinge ausprobieren und nicht wissen, wie Sie ein Zertifikat erstellen, können Sie die IIS-MMC Console-Anwendung verwenden, um ein selbst signiertes Zertifikat erstellen.  Nachdem sie erstellt wurde können Sie diese als PFX-Datei exportieren und Laden Sie es in der Benutzeroberfläche ILB Zertifikat. Wenn Sie eine Website mit ein selbst signiertes Zertifikat gesicherte zugreifen, erhalten Ihr Browser eine Warnung Sie, dass die Website, die Sie den Zugriff auf sind nicht sicher, weil nicht möglich, das Zertifikat überprüfen ist.  Wenn Sie, dass die Warnung zu vermeiden möchten, benötigen Sie ein ordnungsgemäß signiertes Zertifikat, das Ihre Subdomain entspricht und hat eine Kette von Vertrauensstellung, die vom Browser erkannt wird.

![][6]

Wenn Sie versuchen den Flow mit eigener Zertifikate und HTTP- und HTTPS-Zugriff auf Ihre ASE testen möchten:

1.  Wechseln Sie zu ASE Benutzeroberfläche nach der Erstellung ASE **ASE-Einstellungen > -> ILB Zertifikate**
2.  Legen Sie ILB Zertifikat, indem Sie Zertifikat Pfx-Datei auswählen, und geben Sie Ihr Kennwort ein.  In dieser Phase wird ein wenig Zeit in Anspruch und es werden die Nachricht, die ein Skalierung Vorgang ausgeführt wird angezeigt.
3.  Holen Sie sich die Adresse ILB für Ihre ASE (**ASE-Eigenschaften > -> virtuelle IP-Adresse**)
4.  Erstellen Sie eine Web-app in ASE nach der Erstellung 
5.  Erstellen eines virtuellen Computers aus, wenn Sie eine in dieser VNET (nicht in demselben Subnetz wie der Umbruch ASE oder Dinge) besitzen
6.  Legen Sie DNS-Einträge für Ihre Subdomain ein.  Verwenden Sie ein Platzhalterzeichen in Ihrer DNS-Einträge für Ihre Subdomain können oder wenn Sie einige einfache Tests, Aufgaben ausführen möchten, bearbeiten Sie die Hosts-Datei Ihrer virtuellen Computers Web app-Name VIP IP-Adresse festlegen.  Wenn Ihre ASE den Unterdomänennamen hatte. ilbase.com und Sie vorgenommen Mytestapp der Web-app, sodass würden am mytestapp.ilbase.com berücksichtigt dann, die in der Hostsdatei festlegen.  (Unter Windows ist die Hostsdatei am C:\Windows\System32\drivers\etc\ verwenden)
7.  Verwenden Sie einen Browser auf diesen virtuellen Computer, und wechseln Sie zu http://mytestapp.ilbase.com (oder welchen Ihren Web app-Namen für Ihre Subdomain ist)
8.  Verwenden Sie einen Browser auf diesen virtuellen Computer, und wechseln Sie zu https://mytestapp.ilbase.com können, Sie das Fehlen der Sicherheit zu übernehmen, wenn ein selbst signiertes Zertifikat verwenden müssen.  


Die IP-Adresse für Ihr ILB wird als die virtuelle IP-Adresse in Ihre Eigenschaften aufgeführt.

![][4]


## <a name="using-an-ilb-ase"></a>Verwenden einer ILB ASE ##

#### <a name="network-security-groups"></a>Netzwerk-Sicherheitsgruppen ####

Eine ILB ASE ermöglicht Netzwerkisolation für Ihre apps, während der apps nicht zugegriffen werden oder im Internet auch bekannt sind.  Dies ist hervorragend für Hostinganbieter Intranet-Websites, z. B. branchenanwendungen.  Wenn Sie zum Einschränken des Zugriffs auch müssen weiteren können weiterhin Netzwerk Sicherheit Groups(NSGs) Sie zum Steuern des Zugriffs auf das Netzwerkebene. 


Wenn Sie NSGs zum Einschränken des Zugriffs weiteren verwenden möchten, müssen Sie sicherstellen, dass Sie nicht die Kommunikation unterbrechen, die die ASE Betrieb erforderlichen.  Obwohl der HTTP-/HTTPS-Zugriff nur über die ILB von der ASE verwendet wird hängt die ASE weiterhin Ressource außerhalb der VNet ein.  Um anzuzeigen, welche Netzwerkzugriff weiterhin erforderlich suchen auf die Informationen in das Dokument auf [Eingehende Datenverkehr zu einer App-Service-Umgebung steuern] ist[ ControlInbound] und das Dokument auf [Netzwerk Konfigurationsdetails für App-Service-Umgebungen mit ExpressRoute][ExpressRoute].  


Zum Konfigurieren Ihrer NSGs müssen Sie die IP-Adresse kennen, die von Azure zum Verwalten Ihrer ASE verwendet wird.  Diese IP-Adresse ist auch die ausgehende IP-Adresse aus Ihrer ASE, wenn dies Internetanfragen ist.  Suche nach dieser IP-Adresse wechseln Sie zu **Einstellungen-Eigenschaften >** und suchen Sie die **Ausgehende IP-Adresse**.  

![][5]


#### <a name="general-ilb-ase-management"></a>Allgemeine ILB ASE management ####

Verwalten einer ILB ASE ist weitgehend entspricht einer ASE normal verwalten.  Sie müssen Ihre Arbeitskollegen Gruppierung Weitere ASP Instanzen gehostet und der Front-End-Server höhere Datenmengen HTTP-/HTTPS-Datenverkehr verarbeitet skalieren skalieren.  Weitere allgemeine Informationen zum Verwalten der Konfigurations von einer ASE, lesen Sie das Dokument auf [einer App-Service-Umgebung konfigurieren][ASEConfig].  


Zusätzliche Management Elemente sind zertifikatverwaltung und DNS-Verwaltung.  Sie müssen erhalten und das Zertifikat für HTTPS nach der Erstellung der ILB ASE hochladen, und Ersetzen Sie es, bevor diese abgelaufen ist.  Da Azure die base Domäne besitzt, können wir für ASEs mit einer externen VIP Zertifikate bereitstellen.  Da die Unterdomäne einer ILB ASE untersuchten beliebig sein kann, müssen Sie Ihr eigenes Zertifikat für HTTPS bereitstellen. 


#### <a name="dns-configuration"></a>DNS-Konfiguration ####

Wenn Sie eine externe VIP verwenden die DNS-Einträge von Azure verwaltet wird.  Eine beliebige app, die in Ihrem ASE erstellt wird automatisch bei Azure DNS-hinzugefügt, also eine öffentliche DNS-Server.  In einer ILB ASE müssen Sie ein eigenes DNS verwalten.  Für einen angegebenen Subdomain beispielsweise contoso.corp.net müssen Sie DNS A Einträge erstellen, die auf Ihre ILB Adresse verweisen:

    * 
    *.SCM ftp veröffentlichen 


## <a name="getting-started"></a>Erste Schritte
Alle Artikel und wie-des für App-Service-Umgebungen in der [Infodatei für Service-Umgebungen Anwendung](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Um mit der App-Service-Umgebungen anzufangen, finden Sie unter [Einführung App-Service-Umgebungen][WhatisASE]

Weitere Informationen über die App-Verwaltungsdienst Azure-Plattform finden Sie unter [Azure-App-Verwaltungsdienst][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
