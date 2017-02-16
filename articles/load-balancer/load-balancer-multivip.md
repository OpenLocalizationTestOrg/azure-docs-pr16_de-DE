<properties
   pageTitle="Mehrerer VIPs für einen Clouddienst"
   description="Übersicht über MultiVIP und wie Sie mehrere VIPs auf einen Clouddienst festlegen"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-multiple-vips-for-a-cloud-service"></a>Konfigurieren Sie mehrere VIPs für einen Clouddienst

Sie können über das Internet öffentlichen Azure Cloud Services zugreifen, über eine IP-Adresse von Azure bereitgestellt. Diese öffentliche IP-Adresse wird als eine VIP (virtual IP) bezeichnet, da mit Azure Lastenausgleich verknüpft ist, und nicht den virtuellen Computern (virtueller Computer) in der Cloud-Dienst Instanzen. Sie können eine Instanz der virtueller Computer in einen Clouddienst mithilfe einer einzelnen VIP zugreifen.

Es gibt jedoch Szenarien, in denen Sie möglicherweise mehrere VIP als Eintrag zeigen Sie auf den gleichen Clouddienst. Beispielsweise kann Ihre Cloud-Dienst mehrere Websites hosten, die mit den Standardport 443, SSL-Konnektivität erforderlich, wie jede Website für einen anderen Kunden gehostet wird, oder den Mandanten. In diesem Szenario müssen Sie eine andere öffentliche zugänglichen IP-Adresse für jede Website haben. Die folgende Abbildung zeigt ein mit mehreren Mandanten Webdienst hosten mit einer Notwendigkeit mehrerer SSL-Zertifikate auf dem gleichen öffentlichen Port.

![Multi VIP SSL-Szenario](./media/load-balancer-multivip/Figure1.png)

Im Beispiel oben alle VIPs verwenden Sie den gleichen öffentlichen Port (443) und den Datenverkehr auf eine umgeleitet, oder weitere Lastenausgleich virtuellen Computern auf einen eindeutigen privaten Port für die interne IP-Adresse der Cloud-Dienst hosten aller Websites.

>[AZURE.NOTE] Eine andere Situation mit Anforderung der Verwendung mehrerer VIPs ist mehrere SQL AlwaysOn Verfügbarkeit Gruppe Listener auf demselben Satz von virtuellen Computern hosten.

VIPs sind dynamisch standardmäßig, was bedeutet, dass die tatsächliche IP-Adresse in die Cloud-Service zugewiesen im Laufe der Zeit ändern kann. Um die zu verhindern, können Sie eine VIP des Diensts reservieren. Finden Sie weitere Informationen zum reservierte VIPs [Reservierte öffentliche IP-Adresse](../virtual-network/virtual-networks-reserved-public-ip.md)ein.

>[AZURE.NOTE] Finden Sie unter [IP-Adresse Preise](https://azure.microsoft.com/pricing/details/ip-addresses/) , Informationen auf VIPs Preise und reservierte IP-Adressen.

Sie können PowerShell verwenden, um zu überprüfen, die von der Clouddiensten verwendeten VIPs als auch hinzufügen und Entfernen von VIPs, zuordnen eine VIP an den Endpunkt und Lastenausgleich, die auf einer bestimmten VIP konfigurieren.

## <a name="limitations"></a>Einschränkungen

Zu diesem Zeitpunkt beträgt VIP Multi-Funktionen für die folgenden Szenarien:

- **Nur IaaS**. Sie können nur Multi VIP für Clouddienste aktivieren, die virtuellen Computern enthalten. Sie können keine Multi VIP in PaaS Szenarien mit Rolleninstanzen verwenden.
- **Nur PowerShell**. Sie können nur mithilfe der PowerShell Multi VIP verwalten.

Diese Einschränkungen temporär sind, und können zu einem beliebigen Zeitpunkt ändern. Vergewissern Sie sich auf dieser Seite zum Überprüfen der Änderungen zukünftiger besuchen.


## <a name="how-to-add-a-vip-to-a-cloud-service"></a>So fügen Sie eine VIP in einen Cloud-service

Führen Sie den folgenden PowerShell-Befehl zum Hinzufügen einer VIP zu dem Dienst:

    Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

Dieser Befehl zeigt ein Ergebnis ähnlich wie im folgenden Beispiel:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>So entfernen Sie eine VIP aus einem Clouddienst

Führen Sie zum Entfernen der VIP hinzugefügt, die mit Ihrem Dienst im obigen Beispiel den folgenden PowerShell-Befehl aus:

    Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

>[AZURE.IMPORTANT] Wenn es keine Endpunkte zugeordnet wurde, können Sie nur eine VIP entfernen.

## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Zum Abrufen von VIP Informationen aus der Cloud-Dienst

Um einen Cloud-Service zugeordnete VIPs abzurufen, führen Sie das folgende PowerShell-Skript ein:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Das Skript zeigt ein Ergebnis ähnlich wie im folgenden Beispiel:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

In diesem Beispiel ist der Cloud-Dienst 3 VIPs:

- **Vip1** ist die standardmäßige VIP, die Sie kennen, da der Wert für IsDnsProgrammedName eingestellt ist true.
- **Vip2** und **Vip3** werden nicht verwendet, wenn sie keine IP-Adressen verfügen. Sie werden nur verwendet werden, wenn Sie einen Endpunkt an die VIP zuordnen.

>[AZURE.NOTE] Ihr Abonnement werden nur für zusätzliche VIPs Rechnung gestellt, sobald sie einen Endpunkt zugeordnet sind. Weitere Informationen zu Preisen finden Sie unter [IP-Adresse Preise](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Zum Zuordnen einer VIP auf einen Endpunkt

Um eine VIP auf einen Clouddienst, um einen Endpunkt zugeordnet werden soll, führen Sie den folgenden PowerShell-Befehl aus:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 `
  	| Update-AzureVM

Der Befehl erstellt einen Endpunkt mit der VIP *Vip2* für Port *80*und Links, die sie den virtuellen Computer mit der Bezeichnung *myVM1* in einen Cloud-Dienst mit dem Namen *MyService* mithilfe von *TCP* auf Port *8080*aufgerufen verknüpft sind.

Führen Sie den folgenden PowerShell-Befehl aus, um die Konfiguration überprüfen:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Die Ausgabe sieht ähnlich wie im folgenden Beispiel:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>So Lastenausgleich, die auf einer bestimmten VIP aktivieren

Sie können eine einzelne VIP mit mehreren virtuellen Computern für den Lastenausgleich Zwecke zuordnen. Angenommen, müssen Sie einen Cloud-Dienst mit dem Namen *MyService*und zwei virtuellen Computer *myVM1* und *myVM2*. Und Ihre Cloud-Dienst verfügt über mehrere VIPs, einer von ihnen mit dem Namen *Vip2*. Wenn Sie, um sicherzustellen, dass möchten wird der gesamte Datenverkehr an den Port *81* auf *Vip2* zwischen *myVM1* und *myVM2* auf Port *8181*, führen Sie das folgende PowerShell-Skript verteilt:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

    Get-AzureVM -ServiceName myService -Name myVM2 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

Sie können auch Ihre Lastenausgleich zum Verwenden einer anderen VIP aktualisieren. Wenn Sie den folgenden PowerShell-Befehl ausführen, werden Sie den Lastenausgleich festlegen, um einer VIP mit dem Namen Vip1 ändern:

    Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1

## <a name="next-steps"></a>Nächste Schritte

[Log Analytics für Azure Lastenausgleich](load-balancer-monitor-log.md)

[Internet zugänglichen laden Lastenausgleich (Übersicht)](load-balancer-internet-overview.md)

[Erste Schritte zum Lastenausgleich gegenüberliegende Internet](load-balancer-get-started-internet-arm-ps.md)

[Virtuelle Network (Übersicht)](../virtual-network/virtual-networks-overview.md)

[Reservierte IP-REST-APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx)