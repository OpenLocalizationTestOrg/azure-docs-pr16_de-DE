<properties 
   pageTitle="Hybrid-Verbindung mit 2-Ebenen-Anwendung | Microsoft Azure"
   description="Weitere Informationen zum Bereitstellen von virtuellen Einheiten und UDR zum Erstellen einer Anwendung mit mehreren Ebenen-Umgebung in Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/05/2016"
   ms.author="jdial" />

# <a name="virtual-appliance-scenario"></a>Virtuelle Einheit Szenario

Häufig zwischen größere Azure Kunden wird die Notwendigkeit, bieten eine Anwendung zwei Ebenen im Internet, beim Zulassen des Zugriffs auf die Ebene wieder aus einem lokalen Rechenzentrum offen gelegt. Dieses Dokument wird erläutert, wie Sie ein Szenario mit Benutzer definiert ist (UDR), ein VPN-Gateway und virtuelle Netzwerkgeräte eine zweistufigen Umgebung bereitstellen, die die folgenden Anforderungen erfüllt:

- Webanwendung muss vom öffentlichen Internet zugänglich sein.
- Webserver mit der Anwendung muss einen Back-End-Anwendungsserver zugreifen können.
- Gesamten Verkehr aus dem Internet mit der Webanwendung muss über eine Firewall virtuelle Einheit wechseln. Diese virtuelle Einheit wird für nur Datenverkehr im Internet verwendet werden.
- Alle Datenverkehr an den Anwendungsserver muss über eine Firewall virtuelle Einheit wechseln. Diese virtuelle Einheit wird für den Zugriff auf die Back-End-End-Server und in Kürze über ein VPN-Gateway aus dem lokalen Netzwerk Access verwendet werden.
- Administratoren müssen die Firewall virtuelle Einheiten auf ihren lokalen Computern verwalten können, mithilfe von firewall eine dritte virtuelle Einheit, die ausschließlich für die Verwaltung verwendet.

Dies ist ein standard DMZ-Szenario mit einer DMZ und einem geschützten Netzwerk. Solche Szenario kann mithilfe von NSGs, Firewall virtuelle Einheiten oder eine Kombination aus beiden in Azure erstellt werden. In der nachfolgenden Tabelle zeigt einige der vor- und Nachteile zwischen NSGs und Firewall virtuelle Einheiten.

||Experten|Aufzulisten|
|---|---|---|
|NSG|Keine Kosten. <br/>In Azure RBAC integriert. <br/>Regeln können in der Cloud-Vorlagen erstellt werden.|Komplexität konnte in größere Umgebungen unterschiedlich sein. |
|Firewall|Vollzugriff auf Ebene der Daten. <br/>Zentrale Verwaltung über Firewall-Verwaltungskonsole.|Kosten der Firewall Einheit. <br/>Nicht integriert mit Azure RBAC.|

Die folgenden Lösung wird mit Firewall virtuelle Einheiten ein DMZ/geschützte Netzwerkszenario implementiert.

## <a name="considerations"></a>Aspekte

Sie können die Umgebung in Azure mithilfe der verschiedenen verfügbaren Features heute, wie folgt oben erläuterten bereitstellen.

- **Virtuellen Netzwerk (VNet)**. Ein Azure VNet verhält sich in ähnlicher Weise mit einem lokalen Netzwerk, und in einem oder mehreren Subnetzen bereitstellen Datenverkehr Isolation und Trennung von Bereichen aufgeteilt werden können.
- **Virtuellen Einheit**. Mehrere Partner bieten virtuelle Einheiten in der Azure Marketplace, die für die oben beschriebenen drei Firewalls verwendet werden können. 
- **Benutzerdefinierte leitet (UDR)**. Routing-Tabellen können von Azure Netzwerke verwendet, um die Steuerung des Pakete innerhalb einer VNet UDRs enthalten. In diesen Tabellen Routing können auf Subnetze angewendet werden. Eines der neuesten Features in Azure ist die Möglichkeit, eine Routingtabelle auf die GatewaySubnet, bietet die Möglichkeit, den Datenverkehr in die Azure VNet vom eine Verbindung Hybrid an eine virtuelle Einheit weiterleiten anzuwenden.
- **IP-Forwarding**. Standardmäßig wird die Azure Netzwerke-Engine Pakete an virtuelle Netzwerk (Schnittstellenkarten) weitergeben nur, wenn die Zieladresse IP-Pakets die NIC IP-Adresse entspricht. Daher würde eine UDR definiert, ein Paket an eine bestimmte virtuelle Einheit gesendet werden muss, die Netzwerke Azure-Engine des Pakets ablegen. Um sicherzustellen, dass das Paket an einen virtuellen Computer (in diesem Fall eine virtuelle Einheit) übermittelt wird, die nicht den tatsächlichen Ziel für das Paket ist, müssen Sie IP-Weiterleitung für die virtuelle Anwendung zu aktivieren.
- **Netzwerk-Sicherheitsgruppen (NSGs)**. Im folgenden Beispiel wird nicht der NSGs verwenden, aber Sie konnte in dieser Lösung die Subnetze und/oder NICs angewendete NSGs verwenden, um den Datenverkehr ein-und diese Subnetze und NICs weiter zu filtern.


![IPv6-Konnektivität](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

In diesem Beispiel ist ein Abonnement, die folgenden enthält:

- 2 Ressourcengruppen, nicht im Diagramm angezeigt. 
    - **ONPREMRG**. Enthält alle Ressourcen, die erforderlich sind, um eine lokale Netzwerk simulieren.
    - **AZURERG**. Enthält alle Ressourcen, die für die Azure virtuelle Netzwerk-Umgebung erforderlich sind. 
- Eine VNet mit dem Namen **Onpremvnet** verwendet, um eine lokale Datacenter unterteilt wie nachstehend nachbilden.
    - **onpremsn1**. Subnetz, die mit einer virtuellen Computern (virtueller Computer) zu einen lokalen Server Nachbilden Ubuntu ausgeführt werden.
    - **onpremsn2**. Subnetz mit eines virtuellen Computers unter Ubuntu zum Nachbilden eines lokalen Computers von einem Administrator verwendet.
- Es gibt eine virtuelle Firewall-Anwendung mit dem Namen **OPFW** auf **Onpremvnet** verwendet, um einen Tunnel zu **Azurevnet**verwalten.
- Eine VNet mit dem Namen **Azurevnet** aufgeteilt wie unten aufgelistet.
    - **azsn1**. Externe Firewall Subnetz ausschließlich für die externe Firewall verwendet. Alle Internet-Datenverkehr wird durch dieses Subnetz nützlich sein. Dieses Subnetz enthält nur einen Netzwerkadapter mit der externen Firewall verknüpft sind.
    - **azsn2**. Front-End-Subnetz Hostinganbieter eines virtuellen Computers als Webserver, der aus dem Internet zugegriffen wird ausgeführt.
    - **azsn3**. Back-End-Subnetz Hostinganbieter eines virtuellen Computers einen Back-End-Anwendungsserver, der von der front-End-Webserver zugegriffen wird ausgeführt.
    - **azsn4**. Management Subnetz ausschließlich zur Verwaltung des Zugriffs auf alle Firewall virtuelle Geräte bieten. Dieses Subnetz enthält nur einen Netzwerkadapter für jedes virtuelle Firewall-Anwendung, die in der Lösung.
    - **GatewaySubnet**. Azure Hybrid Verbindung Subnetz für ExpressRoute und VPN-Gateway erforderlich ist, um die Verbindungen zwischen Azure VNets und anderen Netzwerken bereitzustellen. 
- Es gibt 3 Firewall virtuelle Einheiten im Netzwerk **Azurevnet** aus. 
    - **AZF1**. Externe Firewall mithilfe einer öffentlichen IP-Adressenressource in Azure im öffentlichen Internet offen gelegt. Müssen Sie sicherstellen, dass Sie eine Vorlage aus dem Marketplace oder direkt vom Hersteller Ihres Einheit, diese Vorschriften eine 3-NIC virtuelle Einheit haben.
    - **AZF2**. Interne Firewall verwendet, um den Datenverkehr zwischen **azsn2** und **azsn3**steuern. Dies ist auch eine 3-NIC virtuelle Einheit.
    - **AZF3**. Projektmanagement-Firewall Administratoren aus lokalen Datencenter zugreifen, und eine Verbindung mit einem Management Subnetz verwendet, um alle Firewall Einheiten verwalten. Sie können 2-NIC virtuelle Einheit-Vorlagen auf der Marketplace finden, oder Sie können eine direkt vom Hersteller Einheit anfordern.

## <a name="user-defined-routing-udr"></a>Benutzerdefinierte Routing (UDR)

Zu einer UDR Tabelle verwendet, um zu definieren, wie Datenverkehr initiiert, dieses Subnetz weitergeleitet wird, kann jedes Subnetz in Azure verknüpft werden. Wenn keine UDRs definiert werden, verwendet Azure Standard weitergeleitet, um Datenverkehr von einem Subnetz in ein anderes zulassen. Zum besseren Verständnis UDRs finden Sie auf [Was sind Benutzer definiert ist und die IP-Weiterleitung](./virtual-networks-udr-overview.md#ip-forwarding).

Um sicherzustellen, dass die Kommunikation erfolgt über die richtigen Firewall Einheit, basierend auf der letzten Anforderung oben, müssen Sie die folgenden Routing-Tabelle, die mit UDRs in **Azurevnet**zu erstellen.

### <a name="azgwudr"></a>azgwudr

In diesem Szenario der einzige Datenverkehr aus lokalen in Azure entdeckt die Firewalls verwalten durch Herstellen einer Verbindung mit **AZF3**verwendet werden, und der Datenverkehr muss die interne Firewall und **AZF2**aufzurufen. Daher ist nur eine Routing erforderlich in **GatewaySubnet** wie unten dargestellt.

|Ziel|Nächste Abschnitt|Erläuterung|
|---|---|---|
|10.0.4.0/24|10.0.3.11|Kann lokalen Datenverkehr Management Firewall **AZF3** erreichen|

### <a name="azsn2udr"></a>azsn2udr

|Ziel|Nächste Abschnitt|Erläuterung|
|---|---|---|
|10.0.3.0/24|10.0.2.11|Diese Berechtigung ermöglicht den Datenverkehr in die Back-End-Subnetz Hostinganbieter Anwendungsserver bis **AZF2**|
|0.0.0.0/0|10.0.2.10|Alle anderen Verkehr über **AZF1** geleitet werden können|

### <a name="azsn3udr"></a>azsn3udr

|Ziel|Nächste Abschnitt|Erläuterung|
|---|---|---|
|10.0.2.0/24|10.0.3.10|Ermöglicht Datenverkehr **azsn2** Nachrichtenfluss von app-Server zu dem Webserver bis **AZF2**|

Sie müssen auch in **Onpremvnet** in der lokalen Datacenter Nachbilden Routing Tabellen für die Subnetze zu erstellen.

### <a name="onpremsn1udr"></a>onpremsn1udr

|Ziel|Nächste Abschnitt|Erläuterung|
|---|---|---|
|192.168.2.0/24|192.168.1.4|Ermöglicht Datenverkehr **onpremsn2** bis **OPFW**|

### <a name="onpremsn2udr"></a>onpremsn2udr

|Ziel|Nächste Abschnitt|Erläuterung|
|---|---|---|
|10.0.3.0/24|192.168.2.4|Diese Berechtigung ermöglicht Datenverkehr mit dem gesicherten Subnetz in Azure bis **OPFW**|
|192.168.1.0/24|192.168.2.4|Ermöglicht Datenverkehr **onpremsn1** bis **OPFW**|

## <a name="ip-forwarding"></a>IP-Weiterleitung 

UDR und IP-Weiterleitung sind Features, mit denen Sie, kombinierter virtuelle Einheiten Datenfluss in einer Azure VNet steuern verwendet werden können.  Eine virtuelle Einheit ist nicht mehr als ein virtueller Computer, die eine Anwendung zur Behandlung von Netzwerkverkehr in irgendeiner Weise, wie eine Firewall oder ein NAT-Gerät verwendet ausgeführt wird.

Diese virtuelle Einheit virtueller Computer muss eingehenden Datenverkehr empfangen, der nicht an sich selbst adressiert ist. Damit ein virtuellen Computers Datenverkehr an andere Ziele berücksichtigt empfangen können, müssen Sie die IP-Weiterleitung für den virtuellen Computer aktivieren. Hierbei handelt es sich um eine Azure, keine Einstellung im Gastbetriebssystem. Ihre virtuelle Einheit muss immer noch einige Typ der Anwendung, um den eingehenden Datenverkehr verarbeitet, und es entsprechend weiterleiten ausführen.

Weitere Informationen zum IP-Weiterleitung finden Sie auf [Was sind Benutzer definiert ist und die IP-Weiterleitung](./virtual-networks-udr-overview.md#ip-forwarding).

Beispiel: Stellen Sie sich vor, dass Sie die folgenden Einrichten einer Azure Vnet haben:

- Subnetz **onpremsn1** enthält einen virtuellen Computer mit dem Namen **onpremvm1**.
- Subnetz **onpremsn2** enthält einen virtuellen Computer mit dem Namen **onpremvm2**.
- **Onpremsn1** und **onpremsn2**ist eine virtuelle Einheit mit dem Namen **OPFW** verbunden.
- Eine benutzerdefinierte Routing **onpremsn1** verknüpfte gibt an, dass alle den Datenverkehr in **onpremsn2** **OPFW**gesendet werden muss.

Zu diesem Zeitpunkt Wenn **onpremvm1** zum Herstellen einer Verbindung mit **onpremvm2**versucht, die UDR verwendet werden, und den Datenverkehr an **OPFW** als den nächsten Abschnitt gesendet. Beibehalten, beachten Sie, dass das Ziel des ist-Paket nicht geändert wird, besagt es immer noch, dass **onpremvm2** das Ziel ist. 

Ohne IP-Weiterleitung für **OPFW**aktiviert wird die Azure virtuelle Netzwerke Logik Pakete, verwirft, da dies zulässt nur Pakete des virtuellen Computers IP-Adresse ist das Ziel für das Paket eines virtuellen Computers gesendet werden.

Mit IP-weiterleiten wird die Logik Azure virtuelles Netzwerk Pakete an OPFW, weiterleiten, ohne seine ursprüngliche Zieladresse zu ändern. **OPFW** müssen die Pakete verarbeitet und ermitteln, was zu tun ist mit ihnen.

Für die oben beschriebenen Szenario damit arbeiten können, müssen Sie IP-Weiterleitung auf den NICs für aktivieren **OPFW**, **AZF1**, **AZF2**und **AZF3** , die für das routing verwendet werden (alle NICs mit Ausnahme derjenigen, die mit dem Subnetz Management verknüpft). 

## <a name="firewall-rules"></a>Firewall-Regeln

Wie zuvor beschrieben, sicher IP-Weiterleitung nur Pakete auf die virtuelle Geräte gesendet werden. Ihr Gerät muss immer noch entscheiden, was mit diesen Paketen Verfahren. Im oben beschriebenen Szenario müssen Sie die folgenden Regeln in Ihrer Einheiten zu erstellen:

### <a name="opfw"></a>OPFW

OPFW steht für eine lokale Gerät, enthält die folgenden Regeln:

- **Routing**: alle den Datenverkehr in 10.0.0.0/16 (**Azurevnet**) durch Tunnel **ONPREMAZURE**gesendet werden muss.
- **Richtlinie**: alle bidirektionaler Verkehr zwischen **Anschluss2** und **ONPREMAZURE**zulassen.
 
### <a name="azf1"></a>AZF1

AZF1 stellt eine Azure virtuelle Einheit, enthält die folgenden Regeln:

- **Richtlinie**: alle bidirektionaler Verkehr zwischen **Anschluss1** und **Anschluss2**zulassen.

### <a name="azf2"></a>AZF2

AZF2 stellt eine Azure virtuelle Einheit, enthält die folgenden Regeln:

- **Routing**: alle Datenverkehr an 10.0.0.0/16 (**Onpremvnet**) muss der Azure Gateway IP-Adresse (d. h. 10.0.0.1) bis **Anschluss1**gesendet werden.
- **Richtlinie**: alle bidirektionaler Verkehr zwischen **Anschluss1** und **Anschluss2**zulassen.

## <a name="network-security-groups-nsgs"></a>Netzwerk-Sicherheitsgruppen (NSGs)

In diesem Szenario werden NSGs nicht verwendet wird. Sie können jedoch NSGs für jedes Subnetz zum Einschränken des eingehenden und ausgehenden Datenverkehr anwenden. Beispielsweise könnten Sie mit dem externen FW Subnetz gelten die folgenden NSG Regeln zu.

**Eingehende**

- Aktivieren Sie die Option alle TCP-Datenverkehr aus dem Internet Port 80 auf einem beliebigen virtuellen Computer im Subnetz.
- Verweigern Sie alle anderen Datenverkehr aus dem Internet.

**Ausgehende**
- Den gesamten Verkehr mit dem Internet ablehnen.

## <a name="high-level-steps"></a>High-Level-Schritte

Um dieses Szenario bereitstellen möchten, gehen Sie auf hoher Ebene.

1.  Melden Sie sich für Ihr Abonnement Azure.
2.  Wenn Sie eine VNet, um das lokale Netzwerk Nachbilden bereitstellen möchten, stellen Sie die Ressourcen, die Bestandteil von **ONPREMRG**bereit.
3.  Bereitstellungsressourcen Sie **AZURERG**gehören.
4.  Bereitstellen der Tunnel aus **Onpremvnet** zum **Azurevnet**.
5.  Nachdem alle Ressourcen bereitgestellt werden, melden Sie sich bei **onpremvm2** und pingen 10.0.3.101 zum Testen der Konnektivität zwischen **onpremsn2** und **azsn3**.
