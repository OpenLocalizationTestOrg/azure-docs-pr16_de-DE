<properties 
    pageTitle="Mehrere Sicherheitsebenen Architektur mit App-Service-Umgebungen" 
    description="Implementieren einer mehrere Sicherheitsebenen Architektur mit App-Service-Umgebungen." 
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
    ms.date="08/30/2016" 
    ms.author="stefsch"/>   

# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Implementierung einer mehrere Sicherheitsebenen Architektur mit App-Service-Umgebungen

## <a name="overview"></a>(Übersicht) ##
 
Da App-Service-Umgebungen in einem virtuellen Netzwerk bereitgestellte, eine isoliert Runtime-Umgebung bereitstellen, können Entwickler eine mehrere Sicherheitsebenen Architektur bietet verschiedene Detailebenen Netzwerkzugriff für jede Anwendungsebene physisch erstellen.

Allgemeine Wunsch besteht darin, API Back-enden aus der allgemeine Zugriff auf das Internet auszublenden und nur ermöglichen APIs vom übergeordneten Web apps aufgerufen wird.  [Netzwerk-Sicherheitsgruppen (NSGs)] [ NetworkSecurityGroups] in Subnetzen mit App-Service-Umgebungen zum Einschränken des Zugriffs für öffentliche API Clientanwendungen verwendet werden können.

Im nachstehenden Diagramm zeigt eine Beispiel-Architektur mit einer WebAPI Grundlage app auf einer App-Service-Umgebung bereitgestellt.  Drei separate Web app-Instanzen, bereitgestellt auf drei separate App Dienst Umgebungen, stellen die Back-End-Anrufe mit der gleichen WebAPI.

![Konzeptionelle Architektur][ConceptualArchitecture] 

Die grüne Pluszeichen darauf hinzuweisen, dass die Netzwerk-Sicherheitsgruppe auf das Subnetz mit "Apiase" eingehende Anrufe aus den übergeordneten Web apps als gut Anrufe von sich selbst zulässt.  Die gleiche Netzwerk-Sicherheitsgruppe verweigert jedoch explizit den Zugriff auf allgemeine eingehenden Datenverkehr aus dem Internet. 

Im weiteren Verlauf dieses Themas führt durch die erforderlichen Schritte zum Konfigurieren der Netzwerk-Sicherheitsgruppe im Subnetz, "Apiase" enthält.

## <a name="determining-the-network-behavior"></a>Bestimmen des Netzwerk-Verhaltens ##
Damit Sie wissen, welche Netzwerk Sicherheitsregeln erforderlich sind, müssen Sie bestimmen, welche Netzwerkclients darf erreichen, die App-Service-Umgebung, die die API app enthält, und welche Clients blockiert werden.

Seit [Netzwerk Sicherheitsgruppen (NSGs)] [ NetworkSecurityGroups] gelten für Subnetze, und App-Service-Umgebungen in Subnetze bereitgestellt werden, die Regeln, die in einer NSG enthaltenen gelten für **Alle** apps für eine App-Service-Umgebung ausgeführt.  Verwenden die Stichprobe Architektur in diesem Artikel, sobald eine Netzwerksicherheitsgruppe an das Subnetz mit "Apiase" angewendet wird, werden alle apps für die App-Service-Umgebung "Apiase" ausgeführt von denselben Regeln Sicherheit geschützt werden. 

- **Bestimmen, die ausgehende IP-Adresse des übergeordneten Anrufer:**  Was ist die IP-Adressen von der übergeordneten Anrufer?  Diese Adressen in den NSG explizit Zugriff zugelassen werden müssen.  Da Anrufe zwischen App Dienst Umgebungen "Internet" Anrufe gelten, bedeutet dies, dass die ausgehende IP-Adresse jeder der drei übergeordneten App Dienst Umgebungen muss Zugriff gewährt werden, in der NSG für das Subnetz "Apiase" zugewiesen.   Weitere Informationen zum Bestimmen der ausgehenden IP-Adresse, finden Sie unter in einer App-Service-Umgebung apps die [Netzwerkarchitektur] [ NetworkArchitecture] Artikel Übersicht.
- **Muss die Back-End-API app selbst aufrufen?**  Ein Punkt manchmal übersehen und raffinierten ist das Szenario, in denen die Back-End-Anwendung selbst aufrufen muss.  Wenn die Anwendung einer Back-End-API in einer App-Service-Umgebung selbst aufrufen muss, wird diese auch behandelt als einen Videoanruf "Internet".  In der Stichprobe Architektur erfordert dies zulassen des Zugriffs von der "Apiase" App-Service-Umgebung als auch die ausgehende IP-Adresse.

## <a name="setting-up-the-network-security-group"></a>Einrichten der Netzwerk-Sicherheitsgruppe ##
Nachdem Sie der Satz von ausgehende IP-Adressen bekannt sind, besteht der nächste Schritt um eine Netzwerk-Sicherheitsgruppe zu erstellen.  Netzwerk-Sicherheitsgruppen können erstellt werden, für beide Ressourcenmanager virtuelle Netzwerke sowie klassischen virtuelle Netzwerke basiert.  In den folgenden Beispielen erstellen und Konfigurieren einer NSG in einem klassischen virtuellen Netzwerk mithilfe der Powershell.

Für die Stichprobe Architektur die Umgebungen Süd zentralen USA, befinden sich im, damit eine leere NSG in diesem Bereich erstellt wurde:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Zunächst eine explizite zulassen, wird die Regel für die Azure Management-Infrastruktur wie im Artikel bei [eingehendem Datenverkehr] erwähnt hinzugefügt[ InboundTraffic] für App-Service-Umgebungen.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    
Als Nächstes werden zwei Regeln dürfen HTTP und HTTPS Anrufe aus der ersten übergeordneten App Service-Umgebung ("fe1ase") hinzugefügt.

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Gespült, und wiederholen Sie für die zweite und dritte übergeordneten App Service-Umgebungen ("fe2ase" und "fe3ase").

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Schließlich erteilen Sie Zugriff auf die ausgehende IP-Adresse des App-Service-Umgebung der Back-End-APIs, damit es in sich selbst wieder aufrufen kann.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Keine anderen Netzwerk Sicherheitsregeln müssen konfiguriert werden, da jeder NSG einen Satz von Regeln für das standardmäßige aufweist, die aus dem Internet eingehenden Zugriff blockieren standardmäßig.

Die vollständige Liste der Regeln in der Netzwerk-Sicherheitsgruppe sind, wie unten gezeigt.  Beachten Sie, wie die letzte Regel, die hervorgehoben ist, eingehenden Zugriff aus anderen als den alle Anrufer blockiert die explizit der Zugriff gewährt wurde.

![NSG Konfiguration][NSGConfiguration] 

Der letzte Schritt besteht im Subnetz, die die App-Service-Umgebung "Apiase" enthält die NSG zuweisen.  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

Mit der NSG angewendet, die mit dem Subnetz dürfen nur die drei übergeordneten App Service-Umgebungen, und die App-Service-Umgebung, enthält die API Back-End in der Umgebung "Apiase" aufrufen.


## <a name="additional-links-and-information"></a>Weitere Links und Informationen ##
Alle Artikel und wie-des für App-Service-Umgebungen in der [Infodatei für Service-Umgebungen Anwendung](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Informationen zu [Netzwerk-Sicherheitsgruppen](../virtual-network/virtual-networks-nsg.md). 

Grundlegendes zu [ausgehende IP-Adressen] [ NetworkArchitecture] und App-Service-Umgebungen.

[Netzwerk-Ports] [ InboundTraffic] von App-Service-Umgebungen verwendet.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
