<properties 
   pageTitle="Informationen zu VPN-Geräten für Standort-zu-Standort VPN-Gateway-Verbindungen für virtuelle Netzwerke Azure | Microsoft Azure"
   description="In diesem Artikel erläutert VPN-Geräte und IPSec-Parameter für S2S VPN-Gateway-Verbindungen und enthält Links zu Konfiguration Anweisungen und Beispiele."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
  tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/13/2016"
   ms.author="yushwang;cherylmc" />

# <a name="about-vpn-devices-for-site-to-site-vpn-gateway-connections"></a>Informationen zu VPN-Geräten für Standort-zu-Standort VPN-Gateway-Verbindungen

Ein VPN-Gerät ist erforderlich, eine Website-zu-Standort (S2S) VPN-Verbindung zu konfigurieren. Verbindungen zwischen Standorten zum Erstellen einer Hybrid-Lösung verwendet werden können, oder wann immer Sie eine sichere Verbindung zwischen Ihrem lokalen Netzwerk und virtuellen Netzwerks möchten. Dieser Artikel beschreibt die kompatiblen VPN-Geräte und Konfigurationsparameter. 

>[AZURE.NOTE] Wenn Sie eine Verbindung zwischen Standorten konfigurieren zu können, ist eine öffentlich zugängliche IPv4-IP-Adresse für Ihr Gerät VPN erforderlich.                                                                                                                                                                               

Wenn Ihr Gerät in der Tabelle [überprüft VPN-Geräten](#devicetable) nicht angezeigt wird, finden Sie im Abschnitt [nicht geprüfte VPN-Geräte](#additionaldevices) in diesem Artikel aus. Es ist möglich, dass Ihr Gerät mit Azure möglicherweise noch funktioniert. Unterstützung für VPN-Geräte wenden Sie sich an Ihrem Gerätehersteller.

**Beachten Sie beim Anzeigen von Tabellen Elemente:**

- Für das routing von statischen und dynamischen Terminologie geändert wurde. Sie können beide Begriffe wahrscheinlich auftreten. Es ist keine Änderung der Funktionalität, werden nur die Namen ändern.
    - Statisches Routing = Richtlinien
    - Dynamisches Routing = RouteBased
- Spezifikationen für hohe Leistung VPN-Gateway und RouteBased VPN Gateway sind gleich, sofern nicht anders angegeben. Beispielsweise sind überprüften VPN-Geräte, die mit RouteBased VPN Gateways kompatibel sind auch mit dem Gateway Azure hohe Leistung VPN kompatibel. 


## <a name="a-namedevicetableavalidated-vpn-devices"></a><a name="devicetable"></a>Überprüften VPN-Geräte 

Wir haben eine Reihe von standard VPN-Geräten in Zusammenarbeit mit Gerätehersteller überprüft. Azure VPN-Gateways sollte alle Geräte in das Gerät Familien in der folgenden Liste enthaltenen konzipiert. Finden Sie unter [Informationen zu VPN-Gateway](vpn-gateway-about-vpngateways.md) zu den Typ des Gateways überprüfen, die Sie benötigen für die Lösung erstellen, die Sie konfigurieren möchten. 

Zum Konfigurieren von Ihrem Geräts VPN unterstützen, finden Sie unter den Links, die entsprechenden Gerätefamilie entsprechen. Unterstützung für VPN-Geräte wenden Sie sich an Ihrem Gerätehersteller.



| **Anbieter**                      | **Gerätefamilie**                                        | **Mindestversion OS**                             | **Richtlinien**                                                                                                                                                                                                             | **RouteBased**                                                                                                                                                                    |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Allied Telesis                  | AR Reihe VPN-Router                                    | 2.9.2                                              | Demnächst                                                                                                                                                                                                                                          | Nicht kompatibel                                                                                                                                                                                               |
| Barracuda Netzwerke, Inc.        | Barracuda NextGen Firewall F-Serie             | Richtlinien: 5.4.3, RouteBased: 6.2.0  | [Hinweise zur Konfiguration](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) | [Hinweise zur Konfiguration](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW)                                                                                                                                                                                              |
| Barracuda Netzwerke, Inc.        |  Barracuda NextGen Firewall X-Serie                 | Barracuda Firewall 6.5 | [Barracuda Firewall](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | Nicht kompatibel                                                                                                                                                                                               |
| Rosafarbener blumenbrokat                         | Vyatta 5400 vRouter                                      | Virtueller Router 6.6R3 GA                            | [Hinweise zur Konfiguration](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html)                                       | Nicht kompatibel                                                                                                                                                                                               |
| Check Point                     | Sicherheitsgateway                                         | R75.40, R75.40VS                                     | [Hinweise zur Konfiguration](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275)                                         | [Hinweise zur Konfiguration](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco                           | ASA                                                      | 8.3                                                | [Beispiele für Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA)                                                                                                                                                                        | Nicht kompatibel                                                                                                                                                                                               |
| Cisco                           | ASR                                                      | IOS 15.1 (Richtlinien), IOS 15.2 (RouteBased)                | [Beispiele für Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                                                        | [Beispiele für Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                 |
| Cisco                           | ISR                                                      | IOS 15.0 (Richtlinien), IOS 15.1 (RouteBased *)               | [Beispiele für Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                                                        | [Cisco Beispiele *](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                |
| Citrix                          | NetScaler MPX, SDX, VPX      |10.1 und höher                                           | [Integration Anweisungen](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html)                                                                                                                                                                            | Nicht kompatibel                                                                                                                                                                                               |
| Dell SonicWALL                  | TZ Reihe NSA Reihe SuperMassive Reihe E-Klasse NSA Reihe | SonicOS 5.8.x, [SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850), [SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646 )          | [Anweisungen - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Anweisungen - SonicOS 5,9 auf](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                   | [Anweisungen - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Anweisungen - SonicOS 5,9 auf](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                                                                      |
| F5                              | BIG-IP-Reihe                                 |           12.0                                            | [Hinweise zur Konfiguration](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip)                                                                                                                                                                          | [Hinweise zur Konfiguration](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling)                                                                                                                                                                                         |
| Fortinet                        | Antivirenfirewall                                                | FortiOS 5.2.7                                      | [Hinweise zur Konfiguration](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                                                          | [Hinweise zur Konfiguration](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                  |
| Internet-Initiative Japan (IIJ) | SEIL Reihe                                              | SEIL / X 4.60, SEIL B1/4.60, 3,20 SEIL/X86            | [Hinweise zur Konfiguration](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf)                                                                                                                                                                   | Nicht kompatibel                                                                                                                                                                                               |
| Wacholder                         | SRX                                                      | JunOS 10.2 (Richtlinien), JunOS 11.4 (RouteBased)            | [Beispiele für Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                                                                      | [Beispiele für Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                              |
| Wacholder                         | J-Serie                                                 | JunOS 10.4r9 (Richtlinien), JunOS 11.4 (RouteBased)          | [Beispiele für Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                                                                 | [Beispiele für Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                         |
| Wacholder                         | ISG                                                      | ScreenOS 6.3 (Richtlinien und RouteBased)                  | [Beispiele für Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                                                                      | [Beispiele für Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                              |
| Wacholder                         | SSG                                                      | ScreenOS 6.2 (Richtlinien und RouteBased)                  | [Beispiele für Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                                                                      | [Beispiele für Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                              |
| Microsoft                       | Routing und Remote Access Service                        | WindowsServer 2012                                | Nicht kompatibel                                                                                                                                                                                                                                       | [Microsoft-Beispiele](http://go.microsoft.com/fwlink/p/?LinkId=717761)                                                                                           |
| Offene Systeme AG | Erfolgsentscheidenden Steuerelement Security Gateway | N/V | [Leitfaden für die Installation](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) | [Leitfaden für die Installation](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan                        | Openswan                                                 | 2.6.32                                             | (In Kürze verfügbar)                                                                                                                                                                                                                                        | Nicht kompatibel                                                                                                                                                                                               |
| Palo Alto Netzwerken              | Alle Geräte, die verschieben-OS     | SCHWENKEN-OS 6.1.5 oder höher (Richtlinien), SCHWENKEN-OS 7.0.5 oder höher (RouteBased)       | [Hinweise zur Konfiguration]( https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065)                                                                                                                                                                                          | [Hinweise zur Konfiguration](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340)                                                                                                                                                                                    |
| WatchGuard                      | Alle                                                      | Fireware XTM v11.x                                 | [Hinweise zur Konfiguration](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/)                                                                                                                                                                          | Nicht kompatibel                                                                                                                                                                                               |

(*) ISR 7200 Reihe Routern unterstützt nur Richtlinien VPN.

## <a name="a-nameadditionaldevicesanon-validated-vpn-devices"></a><a name="additionaldevices"></a>Nicht geprüfte VPN-Geräte

Wenn Ihr Gerät aufgeführt, die in der Tabelle der überprüft VPN-Geräte nicht angezeigt wird, kann es trotzdem unter einer Verbindung zwischen Standorten arbeiten. Stellen Sie sicher, dass Ihr Gerät VPN die im Abschnitt dieses Artikels [Zu VPN-Gateways](vpn-gateway-about-vpngateways.md#gateway-requirements) Gateway Anforderungen beschriebenen Mindestanforderungen erfüllt. Die Mindestanforderungen Besprechung Geräte sollten auch gut mit VPN-Gateways arbeiten. Wenden Sie sich an Ihrem Gerätehersteller Weitere Anweisungen für Support und Konfiguration.


## <a name="editing-device-configuration-samples"></a>Beispiele für Gerät Konfiguration bearbeiten

Nachdem Sie die bereitgestellte VPN-Gerät Konfiguration Stichprobe heruntergeladen haben, müssen Sie einige der Werte entsprechend die Einstellungen für Ihre Umgebung zu ersetzen. 

**So bearbeiten Sie ein Beispiel:**

1. Öffnen Sie das Beispiel, die mit dem Editor ein. 
1. Suchen Sie und Ersetzen Sie alle <*Text*> Zeichenfolgen mit den Werten, die auf Ihre Umgebung beziehen. Vergessen Sie < und >. Wenn ein Name angegeben wird, muss der Name, den Sie auswählen eindeutig sein. Wenn Sie ein Befehl nicht funktioniert, finden Sie in der Dokumentation Ihres Geräts Hersteller.

| **Beispiel für text**                | **Zum Ändern**                                                                                                        |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP_OnPremisesNetwork&gt;           | Der ausgewählte Name für dieses Objekt. Beispiel: MyOnPremisesNetwork                                                       |
| &lt;RP_AzureNetwork&gt;                | Der ausgewählte Name für dieses Objekt. Beispiel: MyAzureNetwork                                                            |
| &lt;RP_AccessList&gt;                  | Der ausgewählte Name für dieses Objekt. Beispiel: MyAzureAccessList                                                         |
| &lt;RP_IPSecTransformSet&gt;           | Der ausgewählte Name für dieses Objekt. Beispiel: MyIPSecTransformSet                                                       |
| &lt;RP_IPSecCryptoMap&gt;              | Der ausgewählte Name für dieses Objekt. Beispiel: MyIPSecCryptoMap                                                          |
| &lt;SP_AzureNetworkIpRange&gt;         | Geben Sie Bereich an. Beispiel: 192.168.0.0                                                                                  |
| &lt;SP_AzureNetworkSubnetMask&gt;      | Geben Sie die Subnetz-Maske an. Beispiel: 255.255.0.0                                                                            |
| &lt;SP_OnPremisesNetworkIpRange&gt;    | Geben Sie an lokalen Bereich. Beispiel: 10.2.1.0                                                                         |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; | Geben Sie die lokale Subnetzmaske an. Beispiel: 255.255.255.0                                                              |
| &lt;SP_AzureGatewayIpAddress&gt;       | Diese Informationen, die speziell für Ihre virtuelle Netzwerk und befindet sich im Verwaltungsportal als **Gateway-IP-Adresse**. |
| &lt;SP_PresharedKey&gt;                | Diese Informationen sind bestimmte mit Ihrem Netzwerk virtuelle und befindet sich im Verwaltungsportal als Schlüssel verwalten.          |



## <a name="ipsec-parameters"></a>IPSec-Parameter

>[AZURE.NOTE] Obwohl die Werte in der folgenden Tabelle aufgeführten von Azure VPN-Gateway unterstützt werden, derzeit gibt es Möglichkeit keine für Sie angeben, oder wählen Sie eine bestimmte Kombination aus dem Azure VPN-Gateway. Sie müssen alle Einschränkungen aus dem lokalen VPN-Gerät angeben. Darüber hinaus müssen Sie bei 1350 MSS Unterlage.

### <a name="ike-phase-1-setup"></a>Einrichten von IKE Phase 1

| **Eigenschaft**                                       | **Richtlinien** | **RouteBased "und" Standard "oder" hohe Leistung VPN gateway** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| IKE-Version                                        | IKEv1                          | IKEv2                                                            |
| Diffie-Hellman-Gruppe                               | Gruppe 2 (1024 Bit)             | Gruppe 2 (1024 Bit)                                               |
| Authentifizierungsmethode                              | Vorinstallierten Schlüssel                 | Vorinstallierten Schlüssel                                                   |
| Algorithmen für die Verschlüsselung                              | AES256 AES128 3DES             | AES256 3DES                                                      |
| Hashing-Algorithmus                                  | SHA1(SHA128)                   | SHA1(SHA128), SHA2(SHA256)                                                     |
| Phase 1 Security Association (SA) Lebensdauer (Zeit) | 28,800 Sekunden                 | 10,800 Sekunden                                                   |


### <a name="ike-phase-2-setup"></a>Einrichten von IKE Phase 2

| **Eigenschaft**                                                             | **Richtlinien**                 | **RouteBased "und" Standard "oder" hohe Leistung VPN gateway**   |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| IKE-Version                                                              | IKEv1                                          | IKEv2                                                              |
| Hashing-Algorithmus                                                        | SHA1(SHA128)                                   | SHA1(SHA128)                                                       |
| Phase 2 Security Association (SA) Lebensdauer (Zeit)                        | 3.600 Sekunden                                  | 3.600 Sekunden                                                                  |
| Phase 2 Security Association (SA) Lebensdauer (Durchsatz)                  | 102,400,000 KB                                 | -                                                                  |
| IPSec-SA Verschlüsselung und Authentifizierung bietet (Rangfolge) | 1. 2 ESP-AES256. ESP-AES128 3. ESP-3DES 4. N/V | Angezeigt (siehe unten) *bietet RouteBased Gateway IPsec Security Association (SA)* |
| Perfekten weiterleiten Geheimhaltung (PFS)                                            | Nein                                             | Keine (*)|
| Leer Peer-Erkennung                                                      | Nicht unterstützt                                  | Unterstützt                                                          |

(*) Azure Gateway als IKE-Antwort kann PFS DH Gruppe 1, 2, 5, 14, 24 übernehmen.

### <a name="routebased-gateway-ipsec-security-association-sa-offers"></a>Bietet RouteBased Gateway IPsec Security Association (SA)

Die folgende Tabelle listet IPSec-SA Verschlüsselung und Authentifizierung bietet. Angebote werden aufgeführt, die Reihenfolge der Einstellung, dass das Angebot angezeigt oder akzeptiert wird.

| **IPSec-SA Verschlüsselung und Authentifizierung Angebote** | **Azure Gateway als initiator**                               | **Azure Gateway als-Antwort**                                   |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1                                                 | ESP AES_256 SHA                                              | ESP AES_128 SHA                                              |
| 2                                                 | ESP AES_128 SHA                                              | ESP 3_DES MD5                                                |
| 3                                                 | ESP 3_DES MD5                                                | ESP 3_DES SHA                                                |
| 4                                                 | ESP 3_DES SHA                                                | AH SHA1 mit ESP AES_128 mit null HMAC                      |
| 5                                                 | AH SHA1 mit ESP AES_256 mit null HMAC                      | AH SHA1 mit ESP 3_DES mit null HMAC                        |
| 6                                                 | AH SHA1 mit ESP AES_128 mit null HMAC                      | AH MD5 mit ESP 3_DES mit null HMAC, keine Gültigkeitsdauer vorgeschlagen |
| 7                                                 | AH SHA1 mit ESP 3_DES mit null HMAC                        | AH SHA1 mit ESP 3_DES SHA1, keine Gültigkeitsdauer                    |
| 8                                                 | AH MD5 mit ESP 3_DES mit null HMAC, keine Gültigkeitsdauer vorgeschlagen | AH MD5 mit ESP 3_DES MD5, keine Gültigkeitsdauer                     |
| 9                                                 | AH SHA1 mit ESP 3_DES SHA1, keine Gültigkeitsdauer                    | ESP DES MD5                                                  |
| 10                                                | AH MD5 mit ESP 3_DES MD5, keine Gültigkeitsdauer                     | ESP DES SHA1, keine Gültigkeitsdauer                                   |
| 11                                                | ESP DES MD5                                                  | AH vorgeschlagen SHA1 mit ESP DES null HMAC, keine Gültigkeitsdauer        |
| 12                                                | ESP DES SHA1, keine Gültigkeitsdauer                                   | AH vorgeschlagen MD5 mit ESP DES null HMAC, keine Gültigkeitsdauer        |
| 13                                                | AH vorgeschlagen SHA1 mit ESP DES null HMAC, keine Gültigkeitsdauer        | AH SHA1 mit ESP DES SHA1, keine Gültigkeitsdauer                      |
| 14                                                | AH vorgeschlagen MD5 mit ESP DES null HMAC, keine Gültigkeitsdauer        | AH MD5 mit ESP DES MD5, keine Gültigkeitsdauer                       |
| 15                                                | AH SHA1 mit ESP DES SHA1, keine Gültigkeitsdauer                      | ESP SHA, keine Gültigkeitsdauer                                        |
| 16                                                | AH MD5 mit ESP DES MD5, keine Gültigkeitsdauer                       | ESP MD5, keine Gültigkeitsdauer                                        |
| 17                                                | -                                                            | AH SHA, keine Gültigkeitsdauer                                         |
| 18                                                | -                                                            | AH MD5, keine Gültigkeitsdauer                                        |


- Sie können IPsec ESP NULL-Verschlüsselung mit RouteBased und hohe Leistung VPN Gateways angeben. NULL serverbasierte Verschlüsselung bietet Schutz auf Daten während der Übertragung keine und sollte nur verwendet werden, wenn die maximale Durchsatz und Minimum Wartezeit ist erforderlich.  Clients können Hiermit in VNet-VNet-Kommunikationsszenarien, oder wenn die Verschlüsselung an anderer Stelle in der Lösung angewendet wird.

- Verwenden Sie für Cross lokale Verbindung über das Internet die Standardeinstellungen für Azure VPN Gateway mit Verschlüsselung und hashing Algorithmen, die in den Tabellen über aufgelistet sind, um Ihre kritischen Kommunikation Sicherheit zu gewährleisten.





