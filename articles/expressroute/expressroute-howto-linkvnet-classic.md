<properties
   pageTitle="Verknüpfen Sie ein virtuelles Netzwerk mit einer ExpressRoute Verbindung mit dem Modell zur klassischen Bereitstellung und PowerShell | Microsoft Azure"
   description="Dieses Dokument bietet einen Überblick über virtuelle Netzwerke (VNets) verknüpfen zu ExpressRoute Schaltkreise, mit dem Modell zur klassischen Bereitstellung und PowerShell."
   services="expressroute"
   documentationCenter="na"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr" />

# <a name="link-a-virtual-network-to-an-expressroute-circuit"></a>Erstellen einer Verknüpfung ein virtuelles Netzwerk mit einer ExpressRoute-Verbindung

> [AZURE.SELECTOR]
- [Azure-Portal - Ressourcenmanager](expressroute-howto-linkvnet-portal-resource-manager.md)
- [PowerShell - Ressourcenmanager](expressroute-howto-linkvnet-arm.md)
- [PowerShell - klassisch](expressroute-howto-linkvnet-classic.md)



In diesem Artikel helfen Ihnen die virtuelle Netzwerke (VNets) mit Azure ExpressRoute Schaltkreise verknüpfen, mit dem Modell zur klassischen Bereitstellung und PowerShell. Virtuelle Netzwerke können entweder im selben Abonnement oder können ein anderes Abonnement enthalten sein.

**Informationen zu Datenmodellen Azure-Bereitstellung**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>Voraussetzungen für die Konfiguration

1. Sie benötigen die neueste Version der Azure-PowerShell-Module. Sie können die neuesten PowerShell-Module aus dem Abschnitt PowerShell der [Azure-Downloadseite](https://azure.microsoft.com/downloads/)herunterladen. Folgen Sie den Anweisungen in [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) schrittweise Anleitung zum Konfigurieren Ihres Computers die Azure PowerShell-Module verwenden.
2. Sie müssen zum Überprüfen der [Voraussetzungen](expressroute-prerequisites.md), [Weiterleitung Anforderungen](expressroute-routing.md)und [Workflows](expressroute-workflows.md) Vorbemerkung Konfiguration.
3. Sie müssen eine aktive ExpressRoute Verbindung.
    - Führen Sie die Schritte zum [Erstellen einer Verbindung ExpressRoute](expressroute-howto-circuit-classic.md) und haben Sie Ihre Connectivity-Anbieter die Verbindung zu aktivieren.
    - Stellen Sie sicher, dass Sie Azure private peering für Ihre Verbindung konfiguriert haben. Finden Sie im Artikel [Konfigurieren routing](expressroute-howto-routing-classic.md) , routing Anweisungen aus.
    - Sicherstellen Sie, dass Azure private peering konfiguriert ist und die BGP peering zwischen Ihrem Netzwerk und Microsoft von besteht, damit Sie End-to-End-Konnektivität aktivieren können.
    - Sie müssen ein virtuelles Netzwerk und ein Gateway virtuelles Netzwerk erstellt und vollständig nach der Bereitstellung. Folgen Sie den Anweisungen zum [Konfigurieren eines virtuellen Netzwerks für ExpressRoute](expressroute-howto-vnet-portal-classic.md).

Sie können bis zu 10 virtuelle Netzwerken eine Verbindung ExpressRoute verknüpfen. Alle virtuellen Netzwerke muss in der gleichen geopolitische Region. Sie können eine größere Anzahl von virtuellen Netzwerken zu Ihrem ExpressRoute Verbindung oder Link virtuelle Netzwerke, die in anderen geopolitischen Regionen sind, wenn Sie das ExpressRoute Premium Add-on aktiviert verknüpfen. Überprüfen Sie die [häufig gestellte Fragen zu](expressroute-faqs.md) Weitere Details auf das Add-on Premium.

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Herstellen einer Verbindung eine Verbindung mit einem virtuellen Netzwerk im selben-Abonnement

Sie können ein virtuelles Netzwerk zu einer ExpressRoute Verbindung mit dem folgenden Cmdlet verknüpfen. Stellen Sie sicher, dass das Gateway virtuelles Netzwerk erstellt wird und Lust verknüpfen, bevor Sie das Cmdlet ausgeführt wird.

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
    Provisioned

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Herstellen einer Verbindung eine Verbindung mit einem virtuellen Netzwerk in ein anderes Abonnement

Sie können eine Verbindung ExpressRoute über mehrere Abonnements freigeben. Die folgende Abbildung zeigt eine einfache Schema der wie Freigabe funktioniert nicht bei ExpressRoute Schaltkreise über mehrere Abonnements.

Jeder kleinere Wolken innerhalb der großen Cloud wird verwendet, um Abonnements darzustellen, die zu anderen Abteilungen innerhalb einer Organisation gehören. Jede Abteilung innerhalb der Organisation können eigene Abonnements für ihre Dienste – aber die Abteilungen bereitstellen eine einzelne ExpressRoute Verbindung wieder mit Ihrem lokalen Netzwerk Verbindung gemeinsam nutzen kann. Eine einzelne Abteilung (in diesem Beispiel: IT) können die Verbindung ExpressRoute besitzen. Andere Abonnements innerhalb der Organisation können die Verbindung ExpressRoute.

>[AZURE.NOTE] Konnektivität und Bandbreite Gebühren für die dedizierte Verbindung werden an den Besitzer der ExpressRoute Verbindung angewendet. Alle virtuellen Netzwerke freigeben die gleiche Bandbreite an.

![Konnektivität Cross-Abonnements](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Verwaltung

Der *Besitzer der Verbindung* ist der Administrator/Coadministrator des Abonnements, in denen die ExpressRoute Verbindung erstellt worden ist. Der Besitzer der Verbindung kann Administratoren/Coadministrators von anderen Abonnements, genannt *Verbindung Benutzer*, um die dedizierte Verbindung zu verwenden, die sie besitzen autorisieren. Verbindung-Benutzer, die berechtigt sind, verwenden Sie die Organisation ExpressRoute Verbindung können die Verbindung ExpressRoute das virtuelle Netzwerk in ihrem Abonnement verknüpfen, nachdem sie autorisiert sind.

Der Besitzer der Verbindung wurde widerrufen Autorisierungs zu einem beliebigen Zeitpunkt und auswerten und ändern. Widerrufen einer Genehmigung führt zu Fehlern im alle Links aus dem Abonnement, deren Zugriff widerrufen wurde, gelöscht wird.

### <a name="circuit-owner-operations"></a>Verbindung Besitzer Vorgänge

#### <a name="creating-an-authorization"></a>Erstellen einer Autorisierung

Der Besitzer der Verbindung autorisiert die Administratoren von anderen Abonnements mit der angegebenen Verbindung. Im folgenden Beispiel kann der Administrator der Verbindung (Contoso IT) den Administrator einer anderen Abonnementtyp (Entwickler-Test), um bis zu zwei virtuelle Netzwerke mit der Verbindung zu verknüpfen. Contoso-IT-Administratoren ermöglicht dies durch Angabe der Entwicklung-Test Microsoft-ID an. Das Cmdlet senden nicht auf die angegebene Microsoft-ID-e-Mail. Der Besitzer der Verbindung muss explizit zu Ihrem Besitzer benachrichtigen, dass die Autorisierung abgeschlossen ist.

    New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : **********************************
    MicrosoftIds        : devtest@contoso.com
    Used                : 0

#### <a name="reviewing-authorizations"></a>Überprüfen von Autorisierungs

Der Besitzer der Verbindung kann alle Autorisierungs überprüfen, die auf einer bestimmten Verbindung ausgestellt werden, indem Sie das folgende Cmdlet aus:

    Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: "**************************"

    Description         : EngineeringTeam
    Limit               : 3
    LinkAuthorizationId : ####################################
    MicrosoftIds        : engadmin@contoso.com
    Used                : 1

    Description         : MarketingTeam
    Limit               : 1
    LinkAuthorizationId : @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    MicrosoftIds        : marketingadmin@contoso.com
    Used                : 0

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : salesadmin@contoso.com
    Used                : 2


#### <a name="updating-authorizations"></a>Aktualisieren von Autorisierungs

Der Besitzer der Verbindung kann Autorisierungs ändern, indem Sie das folgende Cmdlet mit:

    Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5

    Description         : Dev-Test Links
    Limit               : 5
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : devtest@contoso.com
    Used                : 0


#### <a name="deleting-authorizations"></a>Löschen von Autorisierungs

Der Besitzer der Verbindung kann widerrufen/Autorisierungs für den Benutzer löschen, indem Sie das folgende Cmdlet ausgeführt:

    Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"


### <a name="circuit-user-operations"></a>Benutzeraktionen Verbindung

#### <a name="reviewing-authorizations"></a>Überprüfen von Autorisierungs

Der Benutzer Verbindung kann Autorisierungs überprüfen, mithilfe von das folgende Cmdlet aus:

    Get-AzureAuthorizedDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : ContosoIT
    Location                         : Washington DC
    MaximumAllowedLinks              : 2
    ServiceKey                       : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled
    UsedLinks                        : 0

#### <a name="redeeming-link-authorizations"></a>Link Autorisierungs einlösen

Das folgende Cmdlet aus, um einen Link Autorisierung einzulösen kann der Benutzer Verbindung ausgeführt werden:

    New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'

    State VnetName
    ----- --------
    Provisioned SalesVNET1

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu ExpressRoute finden Sie im [ExpressRoute häufig gestellte Fragen](expressroute-faqs.md).
