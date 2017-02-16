<properties
   pageTitle="Referenz Azure Architektur - IaaS: Erstellen einer Ressource Active Directory-Struktur in Azure | Microsoft Azure"
   description="Informationen zum Erstellen einer vertrauenswürdigen Active Directory-Domäne in Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="creating-a-active-directory-directory-services-adds-resource-forest-in-azure"></a>Erstellen einer Ressourcengesamtstruktur Active Directory Directory Services (ADDS) in Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

In diesem Artikel beschrieben, wie Sie eine Active Directory-Domäne in Azure zu erstellen, die von getrennten, aber durch, vertraut ist Domänen in der lokalen Gesamtstruktur.

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle: [Ressourcenmanager] [ resource-manager-overview] und klassischen. Dieser Bezug Architektur verwendet Ressourcenmanager, in dem Bereitstellung neuer Microsoft empfiehlt.

Eine Organisation, die Active Directory (AD) lokal ausgeführt wird möglicherweise eine Gesamtstruktur mit zahlreichen Domänen. Beispielsweise könnten Sie einzelne Domänen für unterschiedliche Abteilungen oder Suborganizations erstellen, oder neue Domänen möglicherweise als Ergebnis Erwerb oder Zusammenführung von anderen Organisationen hinzugefügt wurde. Verwenden von Domänen Isolationsgrad zwischen Funktionsbereichen bereitstellen, die separate, oftmals aus Sicherheitsgründen aufbewahrt werden müssen, aber Sie können Informationen zwischen Domänen, indem Sie Beziehungen Trust freigeben.

Eine Organisation, die separate Domänen nutzt kann Azure nutzen, indem Sie eine oder mehrere der folgenden Domänen in der Cloud verschieben. Sie können auch möglicherweise eine Organisation möchten alle Cloudressourcen aus diesen belegten lokalen logisch verschiedenen festhalten möchten, und Speichern von Informationen über Cloudressourcen im eigenen Verzeichnis, in einer Domäne frei, die auch in der Cloud.

Sie können implementieren Active Directory in Azure auf verschiedene Weise, wie in den Artikeln [Erweitern von Active Directory in Azure] beschrieben[ extending-ad-to-azure] und [Implementieren Azure Active Directory][implementing-aad]. Dieses Dokument befasst sich mit einem bestimmten Szenario: Erstellen einer Domäne in der Cloud, die unterscheidet sich von frei, die für lokale Domänen, jedoch können eine Vertrauensstellung mit lokalen Domänen aufweisen. 

Typische Nutzung Fällen für diese Architektur umfassen:

- Verwalten von Sicherheit Trennung für Objekte und Identitäten frei, die in der Cloud.

- Migrieren von einzelnen Domänen aus lokalen in der Cloud.

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm hervorgehoben wichtigen Komponenten in dieser Architektur (*Klicken Sie zum Vergrößern auf*). Weitere Informationen zu den Elementen abgeblendet angezeigt werden, finden Sie unter [Implementieren einer sicheren Hybrid Netzwerkarchitektur in Azure] [ implementing-a-secure-hybrid-network-architecture] und [Implementieren einer sicheren Hybrid Netzwerkarchitektur mit Internetzugang in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **Lokale Netzwerk.** Das lokale Netzwerk enthält einen eigenen Active Directory-Struktur und Domänen.

- **AD-Servern.** Hierbei handelt es sich um Domänencontroller implementieren Directory Services (AD DS) als virtuellen Computern in der Cloud ausgeführt. Diese Server hosten, eine Struktur, in eine oder mehrere Domänen, unabhängig von dieser Position am lokalen.

- **Unidirektionale Vertrauensstellung.** Das Beispiel im Diagramm zeigt eine unidirektionale Vertrauensstellung aus der Domäne, in der Cloud zu der lokalen Domäne. Diese Beziehung ermöglicht lokalen Benutzern Zugriff auf Ressourcen in der Domäne in der Cloud, jedoch nicht umgekehrt. Dies ist eine allgemeine Fall. Sie können jedoch eine bidirektionale Vertrauensstellung erstellen, wenn Cloud Benutzer auch Zugriff auf lokale Ressourcen benötigen.

- **Active Directory-Subnetz.** AD DS-Servern gehostet werden in einem separaten Subnetz. NSG Regeln Schützen der AD DS-Servers und eine Firewall gegen den Datenverkehr von unerwarteten Quellen bereitstellen.

- **Web Ebene Subnetz**, **Business Ebene Subnetz**und **Datenebene Subnetz gehören**. Diese Subnets hosten die Server und Komponenten, die in der Cloud Applikationen ausgeführt werden. Weitere Informationen finden Sie unter [Ausführen von virtuellen Computern, für eine mehrstufige Architektur auf Azure][running-VMs-for-an-N-tier-architecture-on-Azure]. Die Ressourcen und virtuellen Computern in diesem Subnetz sind in der Cloud-Domäne enthalten.

- **Azure Gateway**. Das Gateway Azure bietet eine Verbindung zwischen dem lokalen Netzwerk und die VNet Azure. Dies kann eine [VPN-Verbindung] sein[ azure-vpn-gateway] oder [Azure ExpressRoute][azure-expressroute]. Weitere Informationen finden Sie unter [Implementieren einer sicheren Hybrid Netzwerkarchitektur in Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Empfehlungen

Dieser Abschnitt enthält eine Liste der Empfehlungen basierend auf die wesentlichen Komponenten erforderlich, um die grundlegende Architektur implementieren. Diese Empfehlungen behandeln:

- ADDIERT Settings, und

- Konfiguration von Beziehung zu vertrauen.

Sie müssen möglicherweise zusätzliche oder unterschiedliche Anforderungen von den hier beschriebenen. Sie können die Elemente in diesem Abschnitt als Ausgangspunkt verwenden, zur Einstufung zum Anpassen der Architektur für Ihre eigenen System.

### <a name="adds-recommendations"></a>ADDIERT Empfehlungen

Für bestimmte Empfehlungen zur Implementierung von Active Directory in der Cloud, finden Sie im Dokument [Active Directory erweitern, um Azure][extending-ad-to-azure]. [Richtlinien zum Bereitstellen von Windows Server Active Directory auf Azure virtuellen Computern] Artikel[ ad-azure-guidelines] zusätzliche detaillierte Informationen enthält.

### <a name="trust-recommendations"></a>Empfehlungen vertrauen

Die lokale Domänen aus den Domänen in der Cloud innerhalb einer anderen enthalten ist. Zum Aktivieren der lokale Benutzer in der Cloud-Authentifizierung müssen die Domänen in der Cloud Die Anmeldedomäne in der lokalen Gesamtstruktur vertrauen. Auf ähnliche Weise, wenn die Cloud eine Anmeldedomäne für externe Benutzer bereitstellt, kann es für die lokale Gesamtstruktur die Cloud-Domäne vertrauen erforderlich sein.

Sie können auf Gesamtstrukturebene Vertrauensstellungen einrichten, indem Sie [Erstellen von Gesamtstrukturvertrauensstellungen][creating-forest-trusts], oder auf der Domänenebene, indem Sie [externe erstellen][creating-external-trusts]. Eine Ebene Vertrauensstellung wird eine Beziehung zwischen allen Domänen in zwei Gesamtstrukturen erstellt. Eine Ebene Vertrauensstellung von externen Domänen wird nur eine Beziehung zwischen zwei angegebenen Domänen erstellt. Sie sollten nur externe Domäne Ebene vertraut zwischen Domänen in verschiedenen Gesamtstrukturen erstellen.

Vertrauensstellungen können unidirektionale sein (unidirektionale) oder bidirektionale (bidirektionale):

- Eine unidirektionale Vertrauensstellung ermöglicht Benutzern in einer Domäne oder Gesamtstruktur (der *eingehenden* Domäne oder Gesamtstruktur genannt) Zugriff auf die Ressourcen frei, die in einer anderen ( *ausgehenden* Domäne oder Gesamtstruktur). 

- Eine bidirektionale Vertrauensstellung kann Benutzer in der Domäne oder der Gesamtstruktur Ressourcen frei, die in einer anderen zugreifen.

In der folgenden Tabelle sind die Trust Konfigurationen für einige einfachen Szenarien zusammengefasst:

| Szenario | Lokale vertrauen | Cloud-trust |
|----------|-------------------|-------------|
| Lokale Benutzer erfordern den Zugriff auf Ressourcen in der Cloud, aber nicht umgekehrt | Unidirektionale, eingehende | Unidirektionale, ausgehende |
| Benutzer in der Cloud erfordern den Zugriff Ressourcen ansässig lokalen, jedoch nicht umgekehrt | Unidirektionale, ausgehende | Unidirektionale, eingehende |
| Benutzer in der Cloud und lokale erfordert Zugriff auf Ressourcen in der Cloud und lokalen frei | Bidirektionale, eingehenden und ausgehenden | Bidirektionale, eingehenden und ausgehenden |

## <a name="security-considerations"></a>Zur Sicherheit

Ebene Gesamtstrukturvertrauensstellungen sind transitiven. Wenn Sie eine Ebene Vertrauensstellung zwischen einer lokalen und einer Gesamtstruktur in der Cloud eingerichtet haben, wird diese Vertrauensstellung zu anderen neuen Domänen in beiden Gesamtstrukturen erstellt erweitert. Wenn Sie Domänen verwenden, um die Trennung aus Sicherheitsgründen bereitstellen, sollten Sie die Ebene der Domäne nur erstellen. Domäne Ebene vertraut sind intransitive.

AD-spezifische aus Sicherheitsgründen finden Sie im Abschnitt *zur Sicherheit* in [Active Directory erweitern, um Azure][extending-ad-to-azure].

## <a name="availability-considerations"></a>Verfügbarkeit Aspekte

Mindestens zwei Domänencontroller für jede Domäne zu implementieren. Dadurch wird die automatische Replikation zwischen Servern. Erstellen einer Verfügbarkeit für die virtuellen Computern, die als AD-Server behandeln jede Domäne einrichten. Stellen Sie sicher, dass mindestens zwei Server festlegen. 

Erwägen Sie auch, definieren einen oder mehrere Server in jeder Domäne als [standby Vorgänge Master] [ standby-operations-masters] für den Fall, dass Sie Verbindung zu einem Server fungiert als eine FSMO-Rolle schlägt fehl.

## <a name="scalability-considerations"></a>Aspekte Skalierbarkeit

AD ist automatisch skalierbar für Domänencontroller, die Teil der gleichen Domäne sind. Anfragen werden auf alle Controller innerhalb einer Domäne verteilt. Sie können eine andere Domänencontroller hinzufügen, und wird automatisch mit der Domäne. Konfigurieren Sie ein separates Lastenausgleich, um den Datenverkehr in Controller innerhalb der Domäne direkte nicht. Stellen Sie sicher, dass alle Controller ausreichende Arbeitsspeicher und Speicher Ressourcen, um die Domänendatenbank verarbeitet haben. Nehmen Sie alle Domänencontroller virtuellen Computern die gleiche Größe.

## <a name="management-and-monitoring-considerations"></a>Management und Überwachung Aspekte

Informationen zur Verwaltung und Überwachung Aspekte, die entsprechende Abschnitte in [Active Directory erweitern, um Azure]finden Sie unter[extending-ad-to-azure]. 

Weitere Informationen finden Sie unter [Überwachen von Active Directory][monitoring_ad]. Sie können Tools wie [Microsoft Systems Center] installieren[ microsoft_systems_center] auf einem Server Überwachung, in dem Subnetz Projektmanagement helfen, die folgenden Aufgaben ausführen. 

## <a name="solution-components"></a>Komponenten einer Lösung

Ein Beispiel Lösung Skript [Bereitstellen-ReferenceArchitecture.ps1][solution-script], steht zur Verfügung, die Sie verwenden können, die Architektur implementiert wird, die die in diesem Artikel beschriebenen Empfehlungen folgt. Dieses Skript nutzt Ressourcenmanager Azure-Vorlagen. Die Vorlagen stehen als eine Reihe von grundlegender Bausteine, von die jede eine bestimmte Aktion wie eine VNet erstellen oder Konfigurieren einer NSG ausführt. Der Zweck des Skripts ist Vorlage Bereitstellung koordinieren.

Die Vorlagen werden mit den Parametern, die als separate JSON-Dateien parametrisierte. Sie können die Parameter in diesen Dateien so konfigurieren Sie die Bereitstellung Ihrer eigenen Bedürfnisse zugeschnitten ändern. Sie müssen nicht die Vorlagen selbst zu ändern. Sie müssen die Schemas der Objekte in der Parameterdateien nicht ändern.

Beim Bearbeiten von Vorlagen erstellen Objekte, die die Benennungskonventionen [Benennungskonventionen für Ressourcen Azure empfohlen]beschrieben folgen[naming-conventions].

Die Lösung für die Stichprobe erstellt und konfiguriert eine Umgebung in der Cloud, die eine Domäne namens *treyresearch.com*implementiert. Diese Umgebung umfasst die ADDS Subnetz und Server, DMZ, Webebene, Business Ebene und Access Komponenten der Datenebene, VPN-Gateway und Management Ebene. Die Lösung Beispiel enthält darüber hinaus eine optionale Konfiguration zum Erstellen einer simulierten lokalen Umgebung mit einem eigenen Domäne *"contoso.com"*. Die Lösung umfasst Skripts, die eine Vertrauensstellung für diese Domänen, die lokal Benutzern Zugriff auf Objekte in der Domäne *treyresearch.com* in der Cloud ermöglicht einführen.

In den folgenden Abschnitten werden die Elemente, die lokal beschrieben und Konfigurationen cloud.

### <a name="on-premises-components"></a>Lokale Komponenten

>[AZURE.NOTE] Diese Komponenten sind nicht im Mittelpunkt dieses in diesem Dokument beschriebenen Architektur und einfach um Sie Gelegenheit Cloud-Umgebung testen sicher, anstatt mit einer realen Herstellung Umgebung bereitgestellt werden. Aus diesem Grund werden in diesem Abschnitt nur die wichtigsten Parameterdateien zusammengefasst. Sie können die Einstellungen für die IP-Adressen oder die Größe der virtueller Computer ändern, aber es ist ratsam, die viele der anderen Parameter unverändert lassen.

Diese Umgebung umfasst eine Active Directory-Struktur der Domäne *"contoso.com"* . Die Domäne enthält zwei ADDS-Servern mit IP-Adressen 192.168.0.4 und 192.168.0.5. Diese zwei Servern wird außerdem den DNS-Dienst ausgeführt. Das lokale Administratorkonto auf beide virtuellen Computern heißt `testuser` mit Kennwort `AweS0me@PW`. Darüber hinaus richtet die Konfiguration ein VPN-Gateway für das Herstellen einer Verbindung mit der VNet in der Cloud. Ändern Sie die Konfiguration, indem Sie die folgenden JSON-Dateien in den [**Parameter/lokale Edition**] [ on-premises-folder] Ordner:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Diese Datei definiert den Netzwerk Adresse Speicherplatz für den lokalen Umgebung.

- ** [VirtualMachines-adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Diese Datei enthält die Konfiguration für die lokale virtuellen Computern Hostingdienste addiert. Standardmäßig werden zwei *Standard-DS3-Version 2* -virtuellen Computern erstellt.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** und ** [connection.parameters.json][on-premises-connection-parameters]**. Diese Dateien enthalten die Einstellungen für die VPN-Verbindung mit dem Gateway Azure VPN in der Cloud, einschließlich der freigegebenen-Taste, um zum Schutz des Datenverkehrs Durchlaufen des Gateways verwendet werden.

Die verbleibenden Dateien im Ordner enthalten die Konfigurationsinformationen zum Erstellen der lokalen Domäne (*contoso.com*) verwenden diese Infrastruktur verwendet. Sie verwenden können, addiert, Setup DNS, installieren und die lokalen Gesamtstruktur erstellen.

Die Lösung verwendet auch das folgende Skript, mit dem Namen [eingehende-trust.ps1][incoming-trust], das auf einem Computer in der lokalen Domäne ausgeführt wird:

```Powershell
# Run the following powershell script in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)

$TrustedDomainName = "contoso.com"
#$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

$TrustingDomainName = "treyresearch.com"
$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustingDomainDnsIpAddresses
$ForwardDomainName = $TrustingDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Dieses Skript fügt die IP-Adressen der AD DS-Server in der Cloud (finden Sie im nächste Abschnitt) an den lokalen DNS-Dienst, und klicken Sie dann mithilfe der [Netdom] [ netdom] Befehl zum Erstellen einer eingehenden unidirektionalen Vertrauensstellung aus der Domäne in der Cloud (*treyresearch.com*).

### <a name="cloud-components"></a>Cloud-Komponenten

Diese Komponenten bilden das Herzstück dieser Architektur. Einrichten die Infrastruktur für die Domäne *treyresearch.com* , und die Trust Beziehungen mit der lokalen *contoso.com* -Domäne erstellen. Der [**Parameter/Azure**] [ azure-folder] Ordner enthält die folgenden Parameterdateien für diese Komponenten konfigurieren:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Diese Datei definiert die Struktur der VNet für den virtuellen Computern und andere unsichere Komponenten in der Cloud. Er enthält Einstellungen wie der Name, Adresse Leerzeichen, Subnetze und die Adressen aller DNS-Server erforderlich. Die DNS-Adressen, die in diesem Beispiel Verweis die IP-Adressen der DNS-Server lokal und auch dem standardmäßigen Azure DNS-Server angezeigt. Ändern Sie diese Adressen Ihrer eigenen DNS-Konfiguration verweisen, wenn Sie nicht die Stichprobe lokalen Umgebung verwenden:
    
    ```json
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg",
        "addressPrefixes": [
          "10.0.0.0/16"
        ],
        "subnets": [
          {
            "name": "dmz-private-in",
            "addressPrefix": "10.0.0.0/27"
          },
          {
            "name": "dmz-private-out",
            "addressPrefix": "10.0.0.32/27"
          },
          {
            "name": "dmz-public-in",
            "addressPrefix": "10.0.0.64/27"
          },
          {
            "name": "dmz-public-out",
            "addressPrefix": "10.0.0.96/27"
          },
          {
            "name": "mgmt",
            "addressPrefix": "10.0.0.128/25"
          },
          {
            "name": "GatewaySubnet",
            "addressPrefix": "10.0.255.224/27"
          },
          {
            "name": "web",
            "addressPrefix": "10.0.1.0/24"
          },
          {
            "name": "biz",
            "addressPrefix": "10.0.2.0/24"
          },
          {
            "name": "data",
            "addressPrefix": "10.0.3.0/24"
          },
          {
            "name": "adds",
            "addressPrefix": "10.0.4.0/27"
          }
        ],
        "dnsServers": [
          "10.0.4.4",
          "10.0.4.5",
          "168.63.129.16"
        ]
      }
    }
    ```

- ** [VirtualMachines-adds.parameters.json ] [ virtualmachines-adds-parameters] **. Diese Datei konfiguriert die virtuellen Computern addiert in der Cloud ausgeführt wird. Die Konfiguration besteht aus zwei virtuellen Computern. Ändern Sie den Administrator-Benutzernamen und das Kennwort in das `virtualMachineSettings` Abschnitt, und Sie können optional die virtueller Speicher entsprechend die Anforderungen der Domäne ändern:

    Weitere Informationen finden Sie unter [Active Directory erweitern, um Azure][extending-ad-to-azure].
    
    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-ad",
        "computerNamePrefix": "aad",
        "size": "Standard_DS3_v2",
        "osType": "Windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "adds",
            "privateIPAllocationMethod": "Static",
            "startingIPAddress": "10.0.4.4",
            "enableIPForwarding": false,
            "dnsServers": [
            ],
            "isPrimary": "true"
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 127,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [
        ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-as"
        }
      }
    },
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg"
      }
    },
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ```

- ** [hinzufügen-Fügt-Domäne-controller.parameters.json][add-adds-domain-controller-parameters]**. Diese Datei enthält die Einstellungen für das Erstellen der *treyresearch.com* -Domäne, die die Servern ADDS dauern. Es wird verwendet, benutzerdefinierte Erweiterungen, die die Domäne eingerichtet und fügen Sie die ADDS-Server hinzu. Es sei denn, Sie zusätzliche ADDS Server erstellen (in diesem Fall Sie diese hinzufügen sollte der `vms` Array), deren Namen von Standard ändern oder eine Domäne mit einem anderen Namen zu erstellen, Sie nicht zum Ändern dieser Datei müssen, möchten.

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Diese Datei enthält die Einstellungen zum Erstellen des Gateways Azure VPN in der Cloud verwendet, um eine Verbindung mit dem lokalen Netzwerk herzustellen. Sie sollten ändern die `sharedKey` Wert in der `connectionsSettings` Abschnitt mit dem lokalen VPN-Gerät übereinstimmt. Weitere Informationen finden Sie unter [Implementieren eines Hybrid Netzwerkarchitektur mit Azure und lokalen VPN][hybrid-azure-on-prem-vpn].

- ** [dmz-private.parameters.json] [ dmz-private-parameters] ** und ** [dmz-public.parameters.json ] [ dmz-public-parameters] **. Diese Dateien konfigurieren die eingehende (öffentlich) und ausgehenden (privaten) Rückseite der virtuellen Computern, die die DMZ, Schützen von den Servern in der Cloud umfassen. Weitere Informationen zu diesen Elementen und deren Konfiguration, finden Sie unter [Implementierung einer zwischen Azure und im Internet][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [Systems zum Lastenausgleich-web.parameters.json][loadBalancer-web-parameters]**, ** [Systems zum Lastenausgleich-biz.parameters.json][loadBalancer-biz-parameters]**, und ** [Systems zum Lastenausgleich-data.parameters.json][loadBalancer-data-parameters]**. Diese Dateien Parameter enthält die virtuellen Computer Spezifikationen für die Ebenen der Access Web, Business und Daten und Lastenausgleich für jede Ebene konfigurieren. Hierbei handelt es sich um den virtuellen Computern, die der Web apps und Datenbanken hosten, und führen Sie die Business Auslastung für die Organisation. Die virtuellen Computern in der Webebene der Domäne in der Cloud mit den Einstellungen im angegebenen hinzugefügt werden die ** [Web-virtueller Computer-Domäne-join.parameters.json] [ web-vm-domain-join-parameters] ** Datei.

    Jede Datei enthält zwei Sätze von Konfigurationsparametern. Die `virtualMachineSettings` Abschnitt definiert die virtuellen Computern, die den ADFS-Dienst in der Cloud zu hosten. Standardmäßig erstellt zwei dieser das Skript virtuellen Computern in der gleichen Verfügbarkeit festlegen. Die folgenden Fragmente zeigen, wie die relevanten Teile des *Systems zum Lastenausgleich web.parameters.json* Datei:

    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-web",
        "computerNamePrefix": "web",
        "size": "Standard_DS1_v2",
        "osType": "windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "web",
            "privateIPAllocationMethod": "Dynamic",
            "isPrimary": "true",
            "enableIPForwarding": false,
            "dnsServers": [ ]
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 128,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [ ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-web-vm-as"
        }
      }
    },
    ...
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ````
    Sie können die Größe und Anzahl von virtuellen Computern in jede Ebene gemäß Ihren Anforderungen ändern.

    Die `loadBalancerSettings` Abschnitt enthält die Beschreibung des Lastenausgleich für diese virtuelle Computer. Lastenausgleich übergibt Datenverkehr, das eingeblendet wird auf Port 80 (HTTP) und 443 (HTTPS) eine oder andere von den virtuellen Computern. 

    >[AZURE.NOTE] Die Regel für Port 80 verwendet eine TCP-Verbindung anstelle von HTTP. Dies ist, da die Windows-Authentifizierung unterstützt nur die Installation von IIS auf der Webebene konfiguriert ist. Anonyme Authentifizierung ist deaktiviert. *Ping* versucht, schlägt fehl Port 80 über eine HTTP-Verbindung mit einer 401 (nicht autorisiert) zurück, während eine TCP-Verbindung nur mit erkennt, ob der Anschluss aktiv ist:

    ```json
    "loadBalancerSettings": {
      "value": {
        "name": "ra-adtrust-web-lb",
        "frontendIPConfigurations": [
          {
            "name": "ra-adtrust-web-lb-fe",
            "loadBalancerType": "internal",
            "internalLoadBalancerSettings": {
              "privateIPAddress": "10.0.1.254",
              "subnetName": "web"
            }
          }
        ],
        "backendPools": [
          {
            "name": "ra-adtrust-web-lb-bep",
            "nicIndex": 0
          }
        ],
        "loadBalancingRules": [
          {
            "name": "http-rule",
            "frontendPort": 80,
            "backendPort": 80,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "http-probe",
            "enableFloatingIP": false
          },
          {
            "name": "https-rule",
            "frontendPort": 443,
            "backendPort": 443,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "https-probe",
            "enableFloatingIP": false
          }
        ],
        "probes": [
          {
            "name": "http-probe",
            "port": 80,
            "protocol": "Tcp",
            "requestPath": null
          },
          {
            "name": "https-probe",
            "port": 443,
            "protocol": "Tcp",
            "requestPath": null
          }
        ],
        "inboundNatRules": [ ]
      }
    }
    ```

- ** [VirtualMachines-mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Diese Datei enthält die Konfiguration für die Sprung Feld/Verwaltung virtueller Computer. Es ist jedoch nur möglich, Anmelde- und Administratorzugriff auf den virtuellen Computern in der Web, Geschäfts- und Datenebenen aus dem Feld Sprung zu erhalten. Standardmäßig das Skript erstellt eine einzelne *Standard_DS1_v2* virtueller Computer, aber Sie können diese Datei zum Vergrößern oder zusätzliche virtuellen Computern erstellen, ist die Arbeitsbelastung Management wahrscheinlich wesentlichen ändern.

Die Konfiguration verwendet auch die [ausgehende trust.ps1] [ outgoing-trust] Skript Siehe unten, um eine unidirektionale ausgehende Vertrauensstellung mit der Domäne *"contoso.com"* zu erstellen:

```powershell
# prerequiste: 
# You need to first run incoming-trust.ps1 in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)
# Then,
# Run the following powershell script in ra-adtrust-ad-vm1 (ip 10.0.4.4)

$TrustedDomainName = "contoso.com"
$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

#$TrustingDomainName = "treyresearch.com"
#$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustedDomainDnsIpAddresses
$ForwardDomainName = $TrustedDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

#netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Dieses Skript ist ähnlich wie die zuvor beschriebenen *eingehende-trust.ps1* Skript. Fügt die IP-Adressen der lokalen AD DS-Servern an den lokalen DNS-Dienst, und klicken Sie dann mithilfe der [Netdom] [ netdom] Befehl aus, um die ausgehende Vertrauensstellung zu erstellen.

## <a name="solution-deployment"></a>Lösung-Bereitstellung

Die Lösung setzt die folgenden Komponenten:

- Sie haben ein vorhandenes Azure Abonnement, in dem Sie eine Ressourcengruppen erstellen können.

- Sie haben die heruntergeladen und installiert die neueste Erstellen von Azure Powershell. Finden Sie [hier] [ azure-powershell-download] Anweisungen.

So führen Sie das Skript aus, das die Lösung bereitgestellt wird.

1. Wechseln zu einem geeigneten Ordner auf Ihrem lokalen Computer, und erstellen Sie die folgenden Unterordner:

    - Skripts

    - Skripts/Parameter

    - Lokale Skripts/Parameter/Edition

    - Skripts/Parameter/Azure

2. Herunterladen der [Bereitstellen-ReferenceArchitecture.ps1] [ solution-script] Datei zu dem Ordner Skripts

3. Herunterladen des Inhalts der [Parameter/lokale Edition] [ on-premises-folder] Ordner in den Ordner lokale Skripts/Parameter/Edition:

4. Herunterladen des Inhalts der [Parameter/Azure] [ azure-folder] Ordner in den Ordner Skripts/Parameter/Azure.

5. Bearbeiten Sie die Datei bereitstellen-ReferenceArchitecture.ps1 in den Ordner Skripts, und ändern Sie die folgenden Linien, um die Ressourcengruppen angeben, die verwendet, um die Ressourcen erstellt, das Skript aufzunehmen oder erstellt werden:

    ```powershell
    # Azure Onpremise Deployments
    $onpremiseNetworkResourceGroupName = "ra-adtrust-onpremise-rg"

    # Azure ADDS Deployments
    $azureNetworkResourceGroupName = "ra-adtrust-network-rg"
    $workloadResourceGroupName = "ra-adtrust-workload-rg"
    $securityResourceGroupName = "ra-adtrust-security-rg"
    $addsResourceGroupName = "ra-adtrust-adds-rg"
    ```

6. Bearbeiten Sie die Parameterdateien in den Skripts/Parameter/Azure-Ordnern und Skripts/Parameter/lokale Edition. Aktualisieren Sie die Ressource Gruppe Bezüge in diese Dateien auf den Namen der Ressourcengruppen zugewiesen an die Variablen in der Datei bereitstellen-ReferenceArchitecture.ps1 übereinstimmen. Die folgende Tabelle zeigt, welche Parameterdateien der Ressourcengruppe verwiesen werden. Die *RAS-Adfs-Arbeitsbelastung-Rg*, *RAS-Adfs-Sicherheit-Rg*, *RAS Adfs addiert Rg*, *RAS-Adfs-Adfs-Rg*und *RAS-Adfs-Proxy-Rg* Ressourcengruppen sind nur in der PowerShell-Skript verwendet und treten nicht in den Parameterdateien.

  	|Ressourcengruppe|Parameterdateien|
  	|--------------|--------------|
  	|RAS-Adtrust-lokale Edition-rg|parameters\onpremise\connection.Parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.Parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.Parameters.JSON<br />parameters\onpremise\virtualNetwork.Parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON
  	|RAS-Adtrust-Netzwerk-rg|parameters\onpremise\connection.Parameters.JSON<br />parameters\azure\dmz-private.Parameters.JSON<br />parameters\azure\dmz-public.Parameters.JSON<br />parameters\azure\loadBalancer-biz.Parameters.JSON<br />parameters\azure\loadBalancer-Data.Parameters.JSON<br />parameters\azure\loadBalancer-Web.Parameters.JSON<br />parameters\azure\virtualMachines-ADDS.Parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.Parameters.JSON<br />parameters\azure\virtualNetwork-ADDS-DNS.Parameters.JSON<br />parameters\azure\virtualNetwork.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON (*zwei Vorkommen*)

    Darüber hinaus Festlegen der Konfiguration für die lokale und cloud-Komponenten, in die [Komponenten einer Lösung] beschriebenen[ solution-components] Abschnitt.

7. Öffnen Sie ein Azure PowerShell-Fenster zu, wechseln Sie zu dem Ordner Skripts und führen Sie den folgenden Befehl aus:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Ersetzen Sie `<subscription id>` mit Ihrer Azure-Abonnement-ID an.

    Für `<location>`, geben Sie eine Azure Region, wie `eastus` oder `westus`.

    Die `<mode>` Parameter kann einen der folgenden Werte aufweisen:

    - `Onpremise`, um die simulierten lokalen Umgebung zu erstellen.

    - `Infrastructure`, erstellen die VNet Infrastruktur und Feld in der Cloud springen.

    - `CreateVpn`, Azure-virtuellen Netzwerk-Gateway erstellen und verbinden Sie es mit dem lokalen Netzwerk.

    - `AzureADDS`, um den virtuellen Computern, die als ADDS Server erstellen, Bereitstellen von Active Directory für diesen virtuellen Computern und die Domäne in der Cloud zu erstellen.

    - `WebTier`, wodurch im Web erstellt gestuft virtuellen Computern und Lastenausgleich.

    - `Prepare`, das alle vorhergehenden Aufgaben ausführt. **Dies ist die empfohlene Option, wenn Sie eine vollständig neue Bereitstellung erstellen, und Sie nicht über eine vorhandene lokale Infrastruktur verfügen.**

    - `Workload`zum Erstellen der Ebene Geschäfts- und virtuellen Computern und Lastenausgleich. Diese virtuellen Computern sind nicht Bestandteil der `Prepare` Option.

    >[AZURE.NOTE] Wenn Sie verwenden die `Prepare` Option das Skript Ausführung dauert mehrere Stunden.

8.  Wenn Sie die Konfiguration der Stichprobe lokal verwenden:

    1. Verbinden Sie mit (*RAS-Adtrust-Management-vm1* in der Ressourcengruppe *RAS-Adtrust-Sicherheit-Rg* ) im Feld Gehe zu. Melden Sie sich als *Testbenutzer* mit Kennwort *AweS0me@PW*.

    2.  Öffnen Sie im Dialogfeld Sprung eine RDP-Sitzung des ersten virtuellen Computers in der Domäne " *contoso.com* " (die lokale Domäne). Diesem virtuellen Computer weist die IP-Adresse 192.168.0.4. Der Benutzername ist *Contoso\testuser* mit Kennwort *AweS0me@PW*.

    3. Herunterladen der [eingehende-trust.ps1] [ incoming-trust] Skripts, und führen Sie es aus der Domäne *treyresearch.com* eingehende Vertrauensstellung zu erstellen.

9. Wenn Sie Ihre eigenen lokalen Infrastruktur verwenden:

    1. Herunterladen der [eingehende-trust.ps1] [ incoming-trust] Skript.

    2. Bearbeiten Sie das Skript, und Ersetzen Sie den Wert, der die `$TrustedDomainName` Variable durch den Namen Ihrer eigenen Domäne.

    3. Führen Sie das Skript.

10. Klicken Sie im Feld Sprung verbinden Sie mit dem ersten virtuellen Computer in der Domäne *treyresearch.com* (die Domäne in der Cloud). Diesem virtuellen Computer weist die IP-Adresse 10.0.4.4. Der Benutzername ist *Treyresearch\testuser* mit Kennwort *AweS0me@PW*.

11. Herunterladen der [ausgehenden trust.ps1] [ outgoing-trust] Skripts, und führen Sie es aus der Domäne *treyresearch.com* eingehende Vertrauensstellung zu erstellen. Wenn Sie Ihre eigenen lokalen Computer verwenden, bearbeiten Sie das Skript zuerst. Festlegen der `$TrustedDomainName` auf den Namen Ihrer Domäne lokale Variable, und geben Sie die IP-Adressen der AD DS-Server für diese Domäne in die `$TrustedDomainDnsIpAddresses` Variable.

12. Führen Sie die Schritte im Artikel [Prüfen Sie eine Vertrauensstellung] auf einem lokalen Computer mit[ verify-a-trust] feststellen, ob die Vertrauensstellung zwischen den Domänen *contoso.com* und *treyresearch.com* ordnungsgemäß konfiguriert wurde. Sie müssen möglicherweise warten Sie einige Minuten, nachdem Sie die vorherigen Schritte durchführen, bevor die Vertrauensstellung überprüft werden kann.

Die verbleibenden optionalen Schritten anzeigen feststellen, ob das Domäne vertrauen wie erwartet funktioniert. Diese Schritte erfordern, dass Sie Zugriff auf einen Entwicklungscomputer mit Visual Studio installiert haben.

1.  Aus dem Fenster Azure PowerShell, führen Sie den folgenden Befehl die Webebene zu erstellen, wenn Sie nicht es zuvor eingerichtet wurde (mithilfe der `Prepare` Option):

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> WebTier
    ```

    Dieser Befehl erstellt die Webebene und die Domäne *treyresearch.com* die virtuellen Computern hinzugefügt.

2. Erstellen Sie mit Visual Studio auf dem Entwicklungscomputer einer ASP.NET Web-Anwendungs, die mit dem Namen *TreyResearchWebApp*. Verwenden von .NET Framework 4.5.2.

3. Wählen Sie die *MVC* -Vorlage aus, und ändern Sie die Authentifizierung auf *Windows-Authentifizierung*. Erstellen Sie eine App-Verwaltungsdienst nicht in der Cloud.

3. Erstellen Sie und führen Sie die Anwendung, um die Authentifizierung zu testen. Stellen Sie sicher, dass Ihre aktuelle Windows-Benutzername in der Menüleiste am oberen Rand der Seite in Richtung der rechten Seite angezeigt wird.

4. Schließen Sie InternetExplorer.

5. Die *Lösung Explorer* -Fenster mit der rechten Maustaste in des Projekts TreyResearchWebApp, klicken Sie auf *Veröffentlichen*.

6. Klicken Sie im *Web veröffentlichen* klicken Sie auf *Benutzerdefiniert*. Erstellen Sie ein benutzerdefiniertes Profil mit dem Namen *TreyResearchWebApp*.

7. Klicken Sie auf der Seite *Verbindung* legen Sie die *Methode veröffentlichen* *File System* , und geben Sie einen Ordner mit dem Namen *TreyResearchWebApp*, sich an einem geeigneten Ort auf Ihrem Computer befindet.

8. Legen Sie auf der Seite *Einstellungen* der *Konfiguration* auf *Release*.

9. Klicken Sie auf der Seite *Vorschau* auf *Veröffentlichen*.

10. Verbinden mit jeder virtuelle Computer in der Webebene wiederum (über dem Feld Sprung), und führen Sie die folgenden Aufgaben. Die IP-Adressen im Web Ebene virtuellen Computern sind 10.0.1.4 und 10.0.1.5. Der Benutzername für beide virtuelle Computer ist *Treyresearch\testuser* mit Kennwort *AweS0me@PW*:

    1. Kopieren Sie die *TreyResearchWebApp* Ordner und seinen Inhalt in den Ordner *C:\inetpub* vom Entwicklungscomputer ein.

    2. Navigieren Sie mithilfe der Verwaltungskonsole für Internet Information Services (IIS), zur *Website Sites\Default* auf dem Computer.

    3. Klicken Sie im Bereich *Aktionen* klicken Sie auf *Grundlegende Einstellungen*, und ändern Sie den physischen Pfad der Website zu *%SystemDrive%\inetpub\TreyResearchWebApp*.

    4. Doppelklicken Sie im Bereich *Ansicht Features* auf *Authentifizierung*. Stellen Sie sicher, dass die *Windows-Authentifizierung* aktiviert ist, und *Anonyme Authentifizierung* ist deaktiviert.

11. Öffnen Sie aus einer lokalen Computer Internet Explorer, und navigieren Sie zu der Website unter http://10.0.1.254 (Dies ist die Adresse des Web Ebene Lastenausgleich).

12. Geben Sie im Dialogfeld *Windows-Sicherheit* die Anmeldeinformationen eines Benutzers in der lokalen Domäne aus. Geben Sie einen Benutzernamen, der nicht in der *Treyresearch* Domäne vorhanden ist. Wenn Sie die simulierten lokalen Umgebung verwenden erstellen Sie zuerst einen Benutzer in der *Contoso* -Domäne, und geben Sie die Anmeldeinformationen des Benutzers.

13. Wenn die Startseite angezeigt wird, stellen Sie sicher, dass der richtige Domänen- und Benutzernamen in der Menüleiste am oberen Rand der Seite in Richtung der rechten Seite angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, die bewährte Methoden für die [Erweiterung Ihrer lokalen ADDS-Domäne zu Azure][adds-extend-domain]

- Erfahren Sie, die bewährte Methoden zum [Erstellen einer ADFS-Infrastruktur] [ adfs] in Azure.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[implementing-aad]: ./guidance-identity-aad.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[running-VMs-for-an-N-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[creating-forest-trusts]: https://technet.microsoft.com/library/cc816810(v=ws.10).aspx
[creating-external-trusts]: https://technet.microsoft.com/library/cc816837(v=ws.10).aspx
[naming-conventions]: ./guidance-naming-conventions.md
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[verify-a-trust]: https://technet.microsoft.com/library/cc753821.aspx
[netdom]: https://technet.microsoft.com/library/cc835085.aspx
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://github.com/mspnp/reference-architectures/blob/master/guidance-identity-adds-trust/parameters/onpremise/connection.parameters.json
[incoming-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/incoming-trust.ps1
[outgoing-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/outgoing-trust.ps1
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-adds.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/add-adds-domain-controller.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-public.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-mgmt.parameters.json
[web-vm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/web-vm-domain-join.parameters.json
[solution-components]: [#solution_components]
[0]: ./media/guidance-identity-aad-resource-forest/figure1.png "Secure Hybrid Netzwerkarchitektur mit separaten AD-Domänen"