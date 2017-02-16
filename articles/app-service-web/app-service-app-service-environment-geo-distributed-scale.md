<properties 
    pageTitle="Geo verteilt Skala mit App-Service-Umgebungen" 
    description="Erfahren Sie, wie die horizontale Skalierung von apps Geo-Verteilung mit den Datenverkehr Manager und App-Service-Umgebungen verwendet werden." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="stefsch"/>   

# <a name="geo-distributed-scale-with-app-service-environments"></a>Geo verteilt Skala mit App-Service-Umgebungen

## <a name="overview"></a>(Übersicht) ##
Anwendungsszenarien, die sehr hoch Maßstab erfordern können berechnen Ressource über ausreichend Kapazität für eine einzelne Bereitstellung von einer app überschreiten.  Abstimmung von Applications, sind Sport auch televised Unterhaltung Ereignisse alle Beispiele für Szenarien, die extrem hohe Maßstab erfordern. Hohe skalieren können erfüllt sein durch horizontales verteilen apps, mit mehreren app-Bereitstellungen und über Bereiche in einem einzigen Bereich ausgelastet Anforderungen verarbeitet gemacht wird.

App-Service-Umgebungen sind eine ideale Plattform für die horizontale Skalierung ab.  Einmal können eine App Service-Umgebung Konfiguration ausgewählt wurde, die einen Satz bekannte Anforderung unterstützen können, Entwickler zusätzliche App Dienst Umgebungen "Ausstechform" Weise zum Erreichen einer gewünschte Höchstwert laden Kapazität bereitstellen.

Nehmen Sie beispielsweise an, dass eine app für eine App-Service-Umgebung Konfiguration ausgeführt getestet wurde, um 20K Abfragen pro Sekunde (RPS) zu behandeln.  Ist die gewünschte Höchstwert laden Kapazität 100 KB RPS, können fünf (5)-App-Service-Umgebungen erstellt und konfiguriert, um sicherzustellen, dass die Anwendung die maximale erwartete Last bewältigen kann werden.

Da Kunden normalerweise apps mithilfe einer benutzerdefinierten (oder Eitelkeit) Domäne zugreifen, benötigen Entwickler eine Möglichkeit zum Verteilen von app-Anfragen auf alle Instanzen der App-Service-Umgebung.  Eine großartige Möglichkeit, dies zu erreichen ist die benutzerdefinierte Domäne mit einem [Azure Datenverkehr-Manager-Profil]Problembehebung[AzureTrafficManagerProfile].  Das Profil Datenverkehr Manager kann auf alle der einzelnen App Service-Umgebungen zeigen konfiguriert werden.  Datenverkehr Manager wird automatisch Verteilen von Kunden über alle der App-Dienst-Umgebungen basierend auf den Lastenausgleich Einstellungen in den Datenverkehr-Manager-Profil bearbeiten.  Dieser Ansatz funktioniert unabhängig davon, ob alle der App-Service-Umgebungen in einer einzelnen Azure Region bereitgestellt oder weltweit über mehrere Azure Regionen bereitgestellt werden.

Darüber hinaus, da Kunden Zugriff auf apps über die Domäne Eitelkeit Kunden wissen ist die Anzahl der App-Service-Umgebungen eine app ausgeführt.  Entwickler können daher schnell und problemlos hinzufügen und entfernen, basierend auf beobachtete Auslastung App Service-Umgebungen.

Im nachstehenden konzeptionelle Diagramm Seitenansichtsmodus Horizontal skalierte in drei Umgebungen der App-Dienst in einem einzigen Bereich app.

![Konzeptionelle Architektur][ConceptualArchitecture] 

Im weiteren Verlauf dieses Themas führt durch die einzelnen Schritte beim Einrichten eines verteilten Suchtopologie für die Stichprobe app mit mehreren App-Service-Umgebungen.

## <a name="planning-the-topology"></a>Planen der Suchtopologie ##
Vor Beginn der Erstellung, einer verteilten app Präsenz, ist es hilfreich, dass einige Textstellen Informationen im voraus.

- **Benutzerdefinierte Domäne für die app:**  Was ist der benutzerdefinierten Domänennamen, mit denen Kunden auf die app zugreifen?  Für die Stichprobe app wird der benutzerdefinierten Domänennamen *www.scalableasedemo.com*
- **Datenverkehr Manager-Domäne:**  Beim Erstellen einer [Azure Datenverkehr-Manager-Profil]ausgewählt werden muss ein Domänennamen[AzureTrafficManagerProfile].  Dieser Name wird mit dem Suffix *trafficmanager.net* einen Domäneneintrag registrieren, der von den Datenverkehr Manager verwaltet wird, kombiniert werden.  Für die Stichprobe-app ist der gewählte Name *skalierbare ase Demo*.  Daher ist der vollständigen Domänennamen, der durch den Datenverkehr-Manager verwaltet wird *skalierbare ase demo.trafficmanager.net*.
- **Strategie für die Skalierung der app Platzbedarf:**  Wird die Anwendung Platzbedarf in mehreren App Service-Umgebungen in einem einzigen Bereich verteilt?  Mehrere Bereiche?  Eine-Sortiment beider Ansätze?  Die Entscheidung sollte basiert auf erwartet, aus denen Kunden Datenverkehr stammen, werden als auch die restlichen einer app unterstützende Back-End-Infrastruktur wie gut skaliert werden kann.  Beispielsweise kann mit eine statusfreie Anwendung (100 %), eine app massively skaliert werden mithilfe einer Kombination von mehreren App-Service-Umgebungen pro Azure Region multipliziert mit App-Service-Umgebungen über mehrere Azure Regionen bereitgestellt.  Mit 15 + öffentlichen Azure Regionen zur Auswahl zur Verfügung können Kunden wirklich eine Präsenz weltweit hyper-Skala Anwendung erstellen.  Drei App Dienst Umgebungen wurden für die Stichprobe-app für diesen Artikel verwendet in einem einzigen Azure Bereich (Süd zentralen USA) erstellt.
- **Benennungskonvention für die App-Service-Umgebungen:**  Jede App-Service-Umgebung erfordert einen eindeutigen Namen.  Hinter einem oder zwei App-Service-Umgebungen empfiehlt es sich, dass eine Benennungskonvention zum Identifizieren der einzelnen App-Service-Umgebung.  Für die Stichprobe app wurde eine einfache Benennungskonvention verwendet.  Die Namen der drei App-Service-Umgebungen sind *fe1ase*, *fe2ase*und *fe3ase*.
- **Benennungskonvention für die apps:**  Da mehrerer Instanzen von der app bereitgestellt werden, ist ein Name für jede Instanz der bereitgestellten app erforderlich.  Ein wenig bekannte aber sehr praktisch-Feature von App-Service-Umgebungen ist, dass die app gleichnamigen in mehreren Umgebungen der App-Dienst verwendet werden kann.  Da jede App-Service-Umgebung eine eigene Domänensuffix aufweist, können Entwickler erneut mit denselben app Namen in jeder Umgebung auswählen.  Ein Entwickler kann beispielsweise wie folgt mit der Bezeichnung apps besitzen: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*.  Für die Stichprobe app hat jedoch jede Instanz der app auch einen eindeutigen Namen.  Die app-Instanznamen verwendet werden *webfrontend1*, *webfrontend2*und *webfrontend3*.


## <a name="setting-up-the-traffic-manager-profile"></a>Einrichten der Datenverkehr-Manager-Profil ##
Sobald auf mehreren App-Service-Umgebungen mehrerer Instanzen von app bereitgestellt werden, können die einzelnen app-Instanzen mit den Datenverkehr Manager registriert werden.  Für die Stichprobe app a Datenverkehr-Manager ist Profil für *skalierbare ase demo.trafficmanager.net* erforderlich, die eine der folgenden bereitgestellten app Instanzen Kunden weiterleiten können:

- **webfrontend1.fe1ase.p.azurewebsites.net:**  Eine Instanz der Stichprobe app bereitgestellt wird, klicken Sie auf die erste App-Service-Umgebung.
- **webfrontend2.fe2ase.p.azurewebsites.net:**  Eine Instanz der Stichprobe app bereitgestellt wird, klicken Sie auf die zweite App-Service-Umgebung.
- **webfrontend3.fe3ase.p.azurewebsites.net:**  Eine Instanz der Stichprobe app bereitgestellt wird, klicken Sie auf die dritte App-Service-Umgebung.

Die einfachste Möglichkeit zum Registrieren mehrere Azure-App-Verwaltungsdienst Endpunkte alle in **derselben** Azure Region ausgeführt wird mit der Powershell [Azure Ressourcenmanager Manager Datenverkehr unterstützen][ARMTrafficManager].  

Der erste Schritt besteht im Erstellen eines Profils Azure Datenverkehr-Manager.  Der folgende Code zeigt, wie das Profil für die Stichprobe app erstellt wurde:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

Beachten Sie, wie der *RelativeDnsName* -Parameter auf *skalierbare ase Demo*festgelegt wurde.  Dies ist wie die Domäne Namen *skalierbare ase demo.trafficmanager.net* erstellt und ein Profil Datenverkehr Manager zugeordnet.

Der Parameter *TrafficRoutingMethod* definiert den Lastenausgleich Richtlinie Datenverkehr Manager zu bestimmen, wie Kunden Laden auf alle verfügbaren Endpunkte ausdehnen verwendet wird.  In diesem Beispiel die *Weighted* wurde Methode ausgewählt.  Dies führt Kundenanfragen wird auf alle registrierten Anwendungsendpunkte basierend auf der relativen Gewichtung jeder Endpunkt zugeordnet verteilt. 

Mit dem Profil erstellt haben wird jede Instanz der app in das Profil als einer systemeigenen Azure Endpunkt hinzugefügt.  Im folgenden Code einen Verweis auf jede front-End-Web-app abgerufen und addiert anschließend jeder app als einen Endpunkt Manager Verkehr über den Parameter *TargetResourceId* .


    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10
    
    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile
    
Beachten Sie, wie es einen Anruf an *Hinzufügen-AzureTrafficManagerEndpointConfig* für jede einzelne app-Instanz ist.  Der *TargetResourceId* -Parameter in jeder Powershell-Befehl verweist auf eine der drei bereitgestellten app Instanzen.  Das Profil Datenverkehr Manager wird beim Laden auf alle drei Endpunkte im Profil registriert verteilt.

Alle drei Endpunkte verwenden Sie den gleichen Wert (10) für die *Stärke* Parameter.  Dadurch Datenverkehr Manager verbreiten Kundenanfragen über alle Instanzen von drei app relativ gleichmäßig. 


## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>Zeigen Sie die App benutzerdefinierte Domäne bei den Datenverkehr Manager Domäne ##
Der letzte Schritt erforderlich ist die benutzerdefinierte Domäne der app in der Domäne an den Datenverkehr Manager verweisen.  Für die Stichprobe app bedeutet dies *www.scalableasedemo.com* bei *skalierbare ase demo.trafficmanager.net*zeigen.  Dieses Schritts muss bei der domänenregistrierungsstelle ausgeführt werden, die die benutzerdefinierte Domäne verwaltet werden.  

Verwenden sich die Registrierungsstelle Domain Management-Tools, Einträge CNAME muss erstellt werden, der die benutzerdefinierte Domäne bei der Domäne den Datenverkehr Manager verweist.  Die nachstehende Abbildung zeigt ein Beispiel für diese CNAME-Konfiguration aussieht:

![CNAME für benutzerdefinierte Domäne][CNAMEforCustomDomain] 

Obwohl in diesem Thema nicht behandelt, denken Sie daran, dass jede einzelne app-Instanz die benutzerdefinierte Domäne, die ebenfalls mit registriert sein muss.  Andernfalls Wenn eine Anforderung für eine app-Instanz gemacht und die Anwendung verfügt nicht über die benutzerdefinierte Domäne registriert mit der app, fehl die Anforderung.  

In diesem Beispiel wird die benutzerdefinierte Domäne *www.scalableasedemo.com*und jede Anwendungsinstanz weist die benutzerdefinierte Domäne zugeordnet.

![Benutzerdefinierten Domäne][CustomDomain] 

Eine Zusammenfassung der Registrierung einer benutzerdefinierten Domänennamens mit Azure-App-Verwaltungsdienst-apps, finden Sie im folgenden Artikel zum [Registrieren von benutzerdefinierter Domänen][RegisterCustomDomain].

## <a name="trying-out-the-distributed-topology"></a>Der Suchtopologie verteilt ausprobieren ##
Das Ergebnis Ende der Datenverkehr-Manager und DNS-Konfiguration ist, dass Anfragen für *www.scalableasedemo.com* , die folgenden Schritte positioniert werden:

1. Ein Browser oder das Gerät wird eine DNS-Suche für *www.scalableasedemo.com* vornehmen.
2. Der CNAME-Eintrag bei der domänenregistrierungsstelle bewirkt, dass die DNS-Suche nach Azure Datenverkehr Manager umgeleitet werden.
3. Eine DNS-Suche für *skalierbare ase demo.trafficmanager.net* anhand einer der Azure Datenverkehr Manager DNS-Server besteht aus.
4. Basierend auf den Lastenausgleich Richtlinie (der Parameter *TrafficRoutingMethod* zuvor verwendet werden, wenn das Profil Datenverkehr Manager erstellen), wird den Datenverkehr-Manager wählen Sie eine der konfigurierten Endpunkte und den vollqualifizierten Domänennamen des diesen Endpunkt an den Browser oder das Gerät zurückzukehren.
5.  Da Sie der vollqualifizierten Domänennamen des Endpunkts die Url einer app-Instanz für eine App-Service-Umgebung ausgeführt wird, wird der Browser oder das Gerät einen Microsoft Azure-DNS-Server den vollqualifizierten Domänennamen in eine IP-Adresse auflösen bitten. 
6. Im Browser oder das Gerät sendet die Anforderung HTTP/S an die IP-Adresse aus.  
7. Die Anforderung wird in eine app Instanzen auf einem der Umgebungen der App-Dienst ausgeführt werden.

Die nachstehende Abbildung Console zeigt eine DNS-Suche für die Beispiel-app benutzerdefinierte Domäne Auflösen von erfolgreich in einer app-Instanz ausgeführt wird, klicken Sie auf eine der drei Stichprobe App-Service-Umgebungen (in diesem Fall die zweite der drei App-Service-Umgebungen):

![DNS-Suche][DNSLookup] 

## <a name="additional-links-and-information"></a>Weitere Links und Informationen ##
Alle Artikel und wie-des für App-Service-Umgebungen in der [Infodatei für Service-Umgebungen Anwendung](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Dokumentation auf der Powershell [Azure Ressourcenmanager Manager Datenverkehr unterstützen][ARMTrafficManager].  

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]:  https://azure.microsoft.com/documentation/articles/traffic-manager-manage-profiles/
[ARMTrafficManager]:  https://azure.microsoft.com/documentation/articles/traffic-manager-powershell-arm/
[RegisterCustomDomain]:  https://azure.microsoft.com/en-us/documentation/articles/web-sites-custom-domain-name/


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
