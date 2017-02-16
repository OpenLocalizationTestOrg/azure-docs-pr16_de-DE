<properties
   pageTitle="Azure-Architektur Referenz - IaaS: Implementierung einer zwischen Azure und im Internet | Microsoft Azure"
   description="Wie Sie eine sichere Hybrid Netzwerkarchitektur mit Zugriff auf das Internet in Azure implementieren."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
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

# <a name="implementing-a-dmz-between-azure-and-the-internet"></a>Implementierung einer zwischen Azure und im Internet

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

In diesem Artikel werden die optimale Methoden zum Implementieren eines sicheren Hybrid-Netzwerk, die Ihrem lokalen Netzwerk erweitert, das Datenverkehr aus dem Internet Netzwerk in Azure akzeptiert. Dieser Bezug Architektur erweitert im Artikel [Implementierung einer zwischen Azure und Datencenters lokalen]beschriebene Architektur[implementing-a-secure-hybrid-network-architecture]. Es wird empfohlen, Lesen dieses Dokument und verstehen die Verweis Architektur, bevor Sie dieses Dokument lesen.

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle: [Ressourcenmanager] [ resource-manager-overview] und klassischen. Dieser Bezug Architektur verwendet Ressourcenmanager, in dem Bereitstellung neuer Microsoft empfiehlt. 

Typische Nutzung Fällen für diese Architektur umfassen:

- Hybrid Applications, wo Auslastung ausführen teilweise lokalen, und teilweise in Azure.

- Azure Infrastruktur, mit dem eingehenden Datenverkehr aus lokalen und im Internet weitergeleitet.

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm hervorgehoben wichtigen Komponenten in dieser Architektur:

> Ein Visio-Dokument, das diese Architekturdiagramm enthält wird unter der [Microsoft download Center]herunterladen[visio-download]. Dieses Diagramm wird auf der Seite "DMZ – öffentlichen".

[! [0]][0]

- **Öffentliche IP-Adresse (PIP).** Dies ist die IP-Adresse des öffentlichen-Endpunkts an. Externe Benutzer, die mit dem Internet verbunden, können das System über diese Adresse zugreifen.

- **Virtuelle Netzwerk-Anwendung (NVA).**  NVA ist eine generische Begriff, der einen virtuellen Computer ausführen von Aufgaben, z. B. zulassen oder Verweigern des Zugriffs als eine Firewall, optimieren WAN-Vorgänge (einschließlich Netzwerk Komprimierung), benutzerdefinierte routing oder sonstige Netzwerkfunktionen beschreibt.

- **Azure Lastenausgleich.** Alle eingehende Anfragen aus dem Internet pass-through dieser Lastenausgleich und an NVAs in der öffentlichen DMZ verteilt werden eingehende Subnetz.

- **Öffentliche DMZ eingehende Subnetz gehören.** Dieses Subnetz akzeptiert Anfragen Azure Lastenausgleich. Eingehende Anfragen werden an eine der NVAs in der DMZ übergeben.

- **Öffentliche DMZ ausgehende Subnetz.** Besprechungsanfragen, die von der NVA genehmigt werden durchlaufen dieses Subnetz an den internen Lastenausgleich für die Web-Leiste.

## <a name="recommendations"></a>Empfehlungen

Azure bietet viele Ressourcentypen, sodass diese Architektur Bezug genommen kann und anderen Ressourcen viele verschiedene Arten bereitgestellt. Wir haben eine Ressourcenmanager Azure-Vorlage, um die Verweis-Architektur zu installieren, die diese Empfehlungen folgt bereitgestellt. Wenn Sie zum Erstellen Ihrer eigenen Bezug Architektur auswählen sollten Sie diese Empfehlungen folgen, es sei denn, Sie einen bestimmten benötigen, die nicht empfohlen passen.

### <a name="nva-recommendations"></a>NVA Empfehlungen

Implementieren Sie eine Reihe von NVAs für den Datenverkehr im Internet und einen anderen für den Datenverkehr auf eine lokale. Es ist ein Sicherheitsrisiko für nur eine Reihe von NVAs für beide verwenden, da dieser Entwurf keine Sicherheit Umfang zwischen den zwei Sätze von Netzwerkverkehr bereitstellt. Es ist ein Vorteil dieses Design zu verwenden, da die Komplexität verringert der Sicherheitsregeln werden überprüft und macht es übersichtlicher jede eingehende Netzwerk Anforderung welche Regeln entsprechen. Beispielsweise implementiert eine Reihe von NVAs Regeln für Datenverkehr im Internet nur während der NVAs implementieren Regeln für lokale Datenverkehr nur einen weiteren Satz.

Einschließen ein Layers 7 NVA Anwendung Verbindungen Ebene der NVA beendet und die Zugehörigkeit zu den Back-End-Ebenen verwalten. Dadurch wird sichergestellt, symmetrischen Connectivity in der Antwort Datenverkehr von den Back-End-Ebenen über der NVA zurückgibt.  

### <a name="public-load-balancer-recommendations"></a>Öffentliche laden Lastenausgleich Empfehlungen ###

Um Skalierbarkeit und Verfügbarkeit beizubehalten, bereitstellen die öffentliche DMZ eingehende NVAs in eine [Verfügbarkeit festlegen] [ availability-set] und Verwenden eines [Internet gegenüberliegende Lastenausgleich] [ load-balancer] zum Verteilen von Internet-Anfragen auf den NVAs Verfügbarkeit festlegen.  

Konfigurieren Sie den Lastenausgleich, um Besprechungsanfragen nur auf der Port für den Datenverkehr im Internet zu akzeptieren. Beschränken Sie beispielsweise eingehende HTTP-Anfragen an Port 80 und eingehende HTTPS-Anfragen an Port 443 ein.

## <a name="scalability-considerations"></a>Aspekte Skalierbarkeit

Entwerfen Sie Ihre Infrastruktur mit einer Internet gegenüberliegende Lastenausgleich vor dem eingehenden öffentlichen DMZ Subnetz von Anfang an. Auch wenn Ihre Architektur Anfangs eines einzelnen NVA erforderlich ist, wird es einfacher sein auf mehrere NVAs in der Zukunft skalieren, wenn bereits im Internet gegenüberliegende Lastenausgleich bereitgestellt wird.

## <a name="availability-considerations"></a>Verfügbarkeit Aspekte

Im Internet gegenüberliegende Lastenausgleich erfordert jedes NVA in der öffentlichen DMZ eingehenden Subnetz zum Implementieren eines [Gesundheit Prüfpunkt][lb-probe]. Ein Gesundheit Prüfpunkt, der nicht reagiert auf diesen Endpunkt gilt nicht mehr verfügbar sein, und Lastenausgleich leitet Anfragen zu anderen NVAs derselben Verfügbarkeit festlegen. Beachten Sie, dass wenn alle NVAs nicht reagiert, die Anwendung nicht funktioniert, damit es wichtig ist, dass die Überwachung zu benachrichtigen DevOps konfiguriert werden, wenn die Anzahl der Instanzen von fehlerfrei NVA unter den Schwellenwert fällt.

## <a name="manageability-considerations"></a>Verwaltbarkeit Aspekte

Beschränken Sie die Überwachung und Verwaltung Funktionalität für den eingehenden öffentlichen DMZ NVA des aus dem Feld Sprung im Subnetz Management nur auf Anfragen zu reagieren. Wie in der [Implementierung einer zwischen Azure und Datencenters lokalen] erläutert[ implementing-a-secure-hybrid-network-architecture] Dokument, eine einzelne Netzwerk-Routing aus dem lokalen Netzwerk über das Gateway in das Feld Sprung im Management Subnetz zum Einschränken des Zugriffs definieren.

Ist Gateway-Verbindung zwischen Ihrem lokalen Netzwerk und Azure ab, können Sie weiterhin das im Feld Gehe zu erreichen, durch die Bereitstellung von einer PIP, im Feld Gehe zu und Remotezugriff in aus dem Internet hinzufügen.

## <a name="security-considerations"></a>Zur Sicherheit

Dieser Bezug Architektur implementiert mehrere Sicherheitsebenen:

- Im Internet gegenüberliegende Lastenausgleich weist Anfragen an die NVAs im eingehenden öffentlichen DMZ Subnetz nur und nur auf die Ports für die Anwendung erforderlich.

- Die NSG Regeln für das eingehende und ausgehende öffentliche DMZ Subnetz verhindern, dass die NVAs durch das Blockieren Besprechungsanfragen, die außerhalb der Regeln NSG fallen gefährdet wird.

- Die Weiterleitung NAT-Konfiguration für die NVAs weist eingehende Anfragen auf Port 80 und Port 443 an den Web Ebene Lastenausgleich, aber Anfragen auf alle anderen Ports ignoriert.

Beachten Sie, dass Sie alle eingehende Anfragen an allen Ports protokollieren soll. Überwachen Sie regelmäßig die Protokolle, und achten Sie dabei auf Anfragen, die außerhalb der erwarteten Parameter liegen, wie diese Eindringversuche hindeuten.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Verwenden von NSGs zum Blockieren/Datenverkehr zwischen Ebenen der Anwendung pass

Alle Stufen Web, Business und Daten beschränken Sie den Datenverkehr zwischen den beiden NSGs verwenden. D. h., die Ebene Business verwendet eine NSG um gesamten Verkehr zu blockieren, die in der Webebene stammen nicht, und die Datenebene verwendet eine NSG um gesamten Verkehr zu blockieren, die in der Business Ebene stammen nicht. Wenn Sie eine unbedingt die Regeln NSG breitere Zugriff auf diese Ebenen erweitern müssen, abzuschätzen Sie diese Anforderungen gegen die Sicherheit Risiken. Jede neue eingehende Betonplatten stellt eine Verkaufschance versehentlich oder absichtlich Daten Offenlegung oder einer Anwendung Schäden.

## <a name="solution-deployment"></a>Lösung-Bereitstellung

Eine Bereitstellung für eine Verweis-Architektur, die diese Empfehlungen implementiert ist auf Github verfügbar. Dieser Bezug Architektur enthält ein virtuelles Netzwerk (VNet), Netzwerk-Sicherheitsgruppe (NSG), Lastenausgleich und zwei virtuellen Computern (virtuelle Computer).

Die Verweis-Architektur kann entweder mit Windows oder Linux virtuellen Computern unter den folgenden Anweisungen folgen bereitgestellt werden: 

1. Klicken Sie mit der rechten Maustaste auf die Schaltfläche unten, und wählen Sie entweder "Link öffnen in neuer Registerkarte" oder "Link in neuem Fenster öffnen":  
[![Bereitstellen für Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2FvirtualNetwork.azuredeploy.json)

2. Nachdem Sie der Link im Portal Azure geöffnet wurde, müssen Sie die Werte für einige Einstellungen eingeben: 
    - Der Name der **Ressourcengruppe** bereits in der Parameterdatei definiert ist, also wählen Sie **Neu erstellen** und geben Sie `ra-public-dmz-network-rg` in das Textfeld ein.
    - Wählen Sie die Region im Dropdownfeld **Speicherort** aus.
    - Bearbeiten Sie die **Vorlage Stamm-Uri** oder die **Parameter Stamm-Uri** Textfelder nicht.
    - Wählen Sie den **Betriebssystem-Typ** aus der Dropdown-Feld, **Windows** oder **Linux**.
    - Überprüfen Sie die allgemeinen Geschäftsbedingungen, und klicken Sie auf das Kontrollkästchen **ich die allgemeinen Geschäftsbedingungen oben erwähnten zustimmen** .
    - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Warten Sie für die Bereitstellung ausführen.

4. Klicken Sie mit der rechten Maustaste auf die Schaltfläche unten, und wählen Sie entweder "Link öffnen in neuer Registerkarte" oder "Link in neuem Fenster öffnen":  
[![Bereitstellen für Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fworkload.azuredeploy.json)

5. Nachdem Sie der Link im Portal Azure geöffnet wurde, müssen Sie die Werte für einige Einstellungen eingeben: 
    - Der Name der **Ressourcengruppe** bereits in der Parameterdatei definiert ist, also wählen Sie **Neu erstellen** und geben Sie `ra-public-dmz-wl-rg` in das Textfeld ein.
    - Wählen Sie die Region im Dropdownfeld **Speicherort** aus.
    - Bearbeiten Sie die **Vorlage Stamm-Uri** oder die **Parameter Stamm-Uri** Textfelder nicht.
    - Überprüfen Sie die allgemeinen Geschäftsbedingungen, und klicken Sie auf das Kontrollkästchen **ich die allgemeinen Geschäftsbedingungen oben erwähnten zustimmen** .
    - Klicken Sie auf die Schaltfläche **Einkauf** .

6. Warten Sie für die Bereitstellung ausführen.

7. Klicken Sie mit der rechten Maustaste auf die Schaltfläche unten, und wählen Sie entweder "Link öffnen in neuer Registerkarte" oder "Link in neuem Fenster öffnen":  
[![Bereitstellen für Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fsecurity.azuredeploy.json)

8. Nachdem Sie der Link im Portal Azure geöffnet wurde, müssen Sie die Werte für einige Einstellungen eingeben: 
    - Der Name der **Ressourcengruppe** bereits in der Parameterdatei definiert ist, also wählen Sie **Vorhandene verwenden** und geben Sie `ra-public-dmz-network-rg` in das Textfeld ein.
    - Wählen Sie die Region im Dropdownfeld **Speicherort** aus.
    - Bearbeiten Sie die **Vorlage Stamm-Uri** oder die **Parameter Stamm-Uri** Textfelder nicht.
    - Überprüfen Sie die allgemeinen Geschäftsbedingungen, und klicken Sie auf das Kontrollkästchen **ich die allgemeinen Geschäftsbedingungen oben erwähnten zustimmen** .
    - Klicken Sie auf die Schaltfläche **Einkauf** .

9. Warten Sie für die Bereitstellung ausführen.

10. Die Parameterdateien einschließen hartcodierte Administrator-Benutzernamen und das Kennwort für alle virtuellen Computern und es wird dringend empfohlen, dass Sie beide sofort ändern. Wählen Sie für jeden virtuellen Computer in der Bereitstellung es im Azure-Portal aus, und klicken Sie dann auf **Kennwort zurücksetzen** in das Blade **Support + zur Problembehandlung** . Wählen Sie im Dropdownmenü **Modus** **Kennwort zurücksetzen** aus, und wählen Sie dann einen neuen **Benutzernamen** und **ein Kennwort**. Klicken Sie auf die Schaltfläche **Aktualisieren** , um die beibehalten werden.


<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[iptables]: https://help.ubuntu.com/community/IptablesHowTo
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md
[load-balancer]: ../load-balancer/load-balancer-internet-overview.md
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[0]: ./media/blueprints/hybrid-network-secure-vnet-dmz.png "Secure Netzwerkarchitektur hybrid"
