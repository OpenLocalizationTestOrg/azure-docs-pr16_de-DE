<properties
   pageTitle="Zurücksetzen ein Gateways Azure VPN | Microsoft Azure"
   description="In diesem Artikel führt Sie durch das Azure VPN-Gateway zurücksetzen. Der Artikel bezieht sich auf VPN-Gateways in der Standardansicht, und die Ressourcenmanager Bereitstellungsmodelle."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc"/>

# <a name="reset-an-azure-vpn-gateway-using-powershell"></a>Zurücksetzen eines Azure VPN-Gateway mithilfe der PowerShell


In diesem Artikel führt Sie durch das Azure VPN-Gateway mit PowerShell-Cmdlets zurücksetzen. Dabei werden sowohl die klassischen Bereitstellung auch das Bereitstellungsmodell Ressourcenmanager.

Das Zurücksetzen des Gateways Azure VPN ist hilfreich, wenn Sie übergreifend lokale VPN-Konnektivität auf eine oder mehrere S2S VPN-Tunnel gehen verloren. In diesem Fall Ihren lokalen VPN-Geräten werden alle ordnungsgemäß funktioniert, jedoch sind nicht IPSec-Tunnel mit der Gateways Azure VPN herstellen. 

Jede Azure VPN Gateway besteht aus zwei virtuellen Computer Instanzen in eine aktiv-Standby-Konfiguration ausgeführt. Wenn Sie das PowerShell-Cmdlet verwenden, um das Gateway zurücksetzen, Neustart des Gateways, und klicken Sie dann wendet die Cross lokale Konfigurationen darauf erneut. Das Gateway behält die öffentliche IP-Adresse, die sie bereits an. Dies bedeutet, dass Sie nicht die Option VPN Routerkonfiguration mit einer neuen öffentlichen IP-Adresse für Azure VPN-Gateway aktualisieren benötigen.  

Nachdem Sie der Befehl ausgegeben wird, wird die aktuelle aktive Instanz des Gateways Azure VPN sofort neu gestartet. Es wird eine kurze Lücke während des Failovers von der aktiven Instanz (gerade neu gestartet), mit der standby-Instanz vorhanden sein. Die Lücke sollte weniger als einer Minute sein.

Wenn die Verbindung nach dem ersten Neustart nicht wiederhergestellt wurde, Emission denselben Befehl erneut aus, um die zweite Instanz virtueller Computer (dem neuen aktiven Gateway) neu starten. Wenn die zwei nach einem Neustart aufeinander folgend aufgefordert werden, werden etwas längere, in dem beide Instanzen virtueller Computer (aktive und standby) neu gestartet werden müssen. Dies bewirkt eine Lücke mehr auf die VPN-Konnektivität, bis zu 2 bis 4 Minuten bei virtuellen Computern, die nach einem Neustart ausführen.

Nach zwei nach einem Neustart Wenn Sie weiterhin Cross lokale Konnektivitätsprobleme auftreten, öffnen Sie eine Supportanfrage vom Azure-Portal.

## <a name="before-you-begin"></a>Vorbemerkung

Bevor Sie Ihr Gateway zurücksetzen, vergewissern Sie sich die wichtigsten Elemente für jede IPsec-Standorten (S2S) VPN-Tunnel aufgeführten. Jede Abweichung Elemente führt die Trennung der S2S VPN-Tunnel. Überprüfung und korrigieren die Konfigurationen für Ihren lokalen und Azure VPN-Gateways ersparen Sie sich unnötige nach einem Neustart und Ausbluten für andere arbeiten Verbindungen auf die Gateways.

Überprüfen Sie Folgendes, bevor Sie Ihr Gateway zurücksetzen:

- Die IP-Adressen (VIPs) für das Gateway Azure VPN und lokale VPN-Gateway sind in der Azure und in der lokalen VPN-Richtlinien ordnungsgemäß konfiguriert.
- Vorinstallierte Schlüssel muss auf sowohl Azure und lokalen VPN-Gateways identisch sein.
- Wenn Sie bestimmte IPSec-/IKE-Konfiguration anwenden wie Verschlüsselung, hashing Algorithmen und PFS (perfekten weiterleiten Geheimhaltung), sicherstellen Sie, dass die Azure und den lokalen VPN-Gateways dieselben Konfigurationen haben.

## <a name="reset-a-vpn-gateway-using-the-resource-management-deployment-model"></a>Zurücksetzen Sie ein VPN-Gateway mit dem Modell zur Bereitstellung von Ressourcenverwaltung

Ist das Ressourcenmanager PowerShell-Cmdlet für das Gateway zurücksetzen `Reset-AzureRmVirtualNetworkGateway`. Im folgende Beispiel wird das Azure VPN Gateway "VNet1GW", in der Ressourcengruppe "TestRG1" zurückgesetzt.

    $gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
    Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw

## <a name="reset-a-vpn-gateway-using-the-classic-deployment-model"></a>Zurücksetzen Sie ein VPN-Gateway mit dem Bereitstellungsmodell klassischen

Das PowerShell-Cmdlet für das Zurücksetzen Azure VPN-Gateway ist `Reset-AzureVNetGateway`. Im folgende Beispiel werden zurückgesetzt Azure VPN-Gateway für das virtuelle Netzwerk "ContosoVNet" bezeichnet.
 
    Reset-AzureVNetGateway –VnetName “ContosoVNet” 

Ergebnis:

    Error          :
    HttpStatusCode : OK
    Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
    Status         : Successful
    RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
    StatusCode     : OK


## <a name="next-steps"></a>Nächste Schritte
    
Finden Sie unter den [Servicemanagement PowerShell-Cmdlet Bezug](https://msdn.microsoft.com/library/azure/mt617104.aspx) und den [Bezug Ressourcenmanager PowerShell-Cmdlet](http://go.microsoft.com/fwlink/?LinkId=828732) für Weitere Informationen.






