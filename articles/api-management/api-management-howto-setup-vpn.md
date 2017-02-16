<properties
    pageTitle="Zum Einrichten von VPN-Verbindungen in Azure-API Management"
    description="Informationen Sie zum Einrichten einer VPN-Verbindungs in Azure-API Management und Access-Webdienste über diese."
    services="api-management"
    documentationCenter=""
    authors="antonba"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="antonba"/>

# <a name="how-to-setup-vpn-connections-in-azure-api-management"></a>Zum Einrichten von VPN-Verbindungen in Azure-API Management

Die VPN-Unterstützung-API-Management können Sie Ihr Management-API Gateway zu einem Azure-virtuellen Netzwerk (klassisch) verbinden. Dadurch wird die Verwaltung API Kunden eine sichere Verbindung mit ihren Back-End-Webdiensten herstellen, die lokal sind oder andernfalls mit dem öffentlichen Internet kann nicht zugegriffen werden.

>[AZURE.NOTE] Azure-API Management funktioniert mit klassischen VNETs. Informationen zum Erstellen einer klassischen VNET finden Sie unter [Erstellen eines virtuellen Netzwerks mithilfe der Azure-Portal (klassische)](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Informationen zum Verbinden von klassischen VNETs mit Cloud VNETS finden Sie unter [Herstellen einer Verbindung klassischen VNets auf neue VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="enable-vpn"> </a>Aktivieren VPN-Verbindungen

>VPN-Konnektivität ist nur in den **Premium** und **Entwicklertools** Ebenen verfügbar. Wechseln sie in, Ihrer API Verwaltungsdienst im [Klassischen Azure-Portal][] zu öffnen, und öffnen Sie dann auf die Registerkarte **Zeichnungsmaßstab** . Wählen Sie unter Abschnitt **Allgemein** die Premium-Leiste, und klicken Sie auf Speichern.

Um VPN-Konnektivität aktivieren, öffnen Sie Ihre API Verwaltungsdienst im [Klassischen Azure-Portal][] zu, und wechseln Sie zur Registerkarte **Konfigurieren** . 

Wechseln Sie unter dem Abschnitt VPN **VPN-Verbindung** zu **auf**ein.

![Registerkarte API Management Instanz konfigurieren][api-management-setup-vpn-configure]

Sie sehen nun, eine Liste aller Bereiche, wo Ihre API Verwaltungsdienst bereitgestellt wird.

Wählen Sie ein VPN und Subnetz für jede Region. Die Liste der VPN wird basierend auf den virtuelle Netzwerke in Ihrem Abonnement Azure verfügbar, die in der Region Setup sind, die Sie konfigurieren ausgefüllt.

![Wählen Sie VPN][api-management-setup-vpn-select]

Klicken Sie auf **Speichern** , am unteren Rand des Bildschirms. Sie können werden nicht während der aktualisiert wird, andere Aktionen auf der API Verwaltungsdienst vom klassischen Azure-Portal ausführen. Das Gateway Dienst steht weiterhin zur Verfügung und Laufzeit Anrufe nicht betroffen sein sollen.

Beachten Sie, dass die VIP-Adresse des Gateways wird jedes Mal ändern, VPN aktiviert oder deaktiviert ist.

## <a name="connect-vpn"> </a>Mit einem Webdienst hinter VPN verbinden

Nachdem Ihre API-Verwaltungsdienst mit dem VPN verbunden ist, unterscheidet den Zugriff auf Webdienste innerhalb des virtuellen Netzwerks Welt sich den Zugriff auf Öffentliche Dienste. Geben Sie einfach die lokale Adresse oder der Hostname des Web Service (Wenn Sie ein DNS-Server für das virtuelle Azure-Netzwerk konfiguriert wurde) in das Feld **Webdienst-URL** beim Erstellen einer neuen API oder bearbeiten eine vorhandene Schablone.

![Hinzufügen von API von VPN][api-management-setup-vpn-add-api]

## <a name="required-ports-for-api-management-vpn-support"></a>Erforderliche Ports für API Management VPN-Unterstützung

Wenn eine Instanz der API Management-Dienst in einer VNET gehostet wird, werden die Ports in der folgenden Tabelle verwendet. Wenn diese Ports blockiert werden, kann der Dienst möglicherweise nicht ordnungsgemäß. Gibt es eine oder mehrere der folgenden Ports gesperrt ist die am häufigsten verwendeten falsche Konfiguration Problem bei Verwendung von Management-API mit einem VNET.

| Ports                      | Richtung        | Transportregel-Protokoll | Zweck                                                          | Quelle / Ziel              |
|------------------------------|------------------|--------------------|------------------------------------------------------------------|-----------------------------------|
| 80, 443                      | Eingehende          | TCP                | Client-Kommunikation API erfahrener                           | INTERNET / VIRTUAL_NETWORK        |
| 80,443                       | Ausgehende         | TCP                | API Management Abhängigkeit Azure-Speicher und Azure Dienstbus | VIRTUAL_NETWORK / INTERNET        |
| 1433                         | Ausgehende         | TCP                | Projektmanagement-API Abhängigkeiten von SQL                               | VIRTUAL_NETWORK / INTERNET        |
| 9350, 9351, 9352, 9353, 9354 | Ausgehende         | TCP                | Projektmanagement-API Abhängigkeiten von Dienstbus                       | VIRTUAL_NETWORK / INTERNET        |
| 5671                         | Ausgehende         | AMQP               | Projektmanagement-API Abhängigkeit für Protokoll auf Ereignis Hub Richtlinie            | VIRTUAL_NETWORK / INTERNET        |
| 6381, 6382, 6383             | Ein-/ausgehende | UDP                | Projektmanagement-API Abhängigkeiten von Redis Cache                       | VIRTUAL_NETWORK / VIRTUAL_NETWORK |
| 445                          | Ausgehende         | TCP                | API Management Abhängigkeit Azure Dateifreigabe für GIT            | VIRTUAL_NETWORK / INTERNET        |

## <a name="custom-dns"> </a>Benutzerdefinierte DNS-Server-Setup

Projektmanagement-API hängt eine Reihe von Diensten Azure aus. Wenn eine Instanz der API Management-Dienst in einer VNET gehostet wird, in ein benutzerdefinierter DNS-Server verwendet wird, muss es von Azure Dienste Hostnamen auflösen können. Folgen Sie [diesem](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) Handbuch auf benutzerdefinierte DNS-Setup.  

## <a name="related-content"> </a>Verbundener Inhalt


* [Erstellen eines virtuellen Netzwerks mit einem Standort-zu-Standort VPN-Verbindung über das klassische Azure-Portal][]
* [So verwenden Sie den Inspektor API verfolgen Anrufe bei Azure-API Verwaltung][]

[api-management-setup-vpn-configure]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-configure.png
[api-management-setup-vpn-select]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-add-api.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[Azure klassischen-Portal]: https://manage.windowsazure.com/

[Erstellen eines virtuellen Netzwerks mit einem Standort-zu-Standort VPN-Verbindung über das klassische Azure-Portal]: ../vpn-gateway/vpn-gateway-site-to-site-create.md
[So verwenden Sie den Inspektor API verfolgen Anrufe bei Azure-API Verwaltung]: api-management-howto-api-inspector.md
