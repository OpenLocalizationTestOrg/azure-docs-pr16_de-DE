<properties
   pageTitle="Workflows zum Konfigurieren einer Verbindung ExpressRoute | Microsoft Azure"
   description="Diese Seite führt Sie durch die Workflows für die Verbindung ExpressRoute und Peerings konfigurieren"
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a>ExpressRoute Workflows für die Bereitstellung von Verbindung und Verbindung Staaten

Diese Seite führt Sie durch den Dienst bereitgestellt und routing Konfiguration Workflows auf hoher Ebene.

![](./media/expressroute-workflows/expressroute-circuit-workflow.png)

Die folgende Abbildung und den entsprechenden Schritten anzeigen die Aufgaben, die Sie ausführen müssen, um eine ExpressRoute Verbindung nach der Bereitstellung haben End-to-End. 

1. Verwenden von PowerShell so konfigurieren Sie eine Verbindung ExpressRoute. Folgen Sie den Anweisungen im Artikel [Erstellen ExpressRoute Schaltkreise](expressroute-howto-circuit-classic.md) Weitere Details an.

2. Die Reihenfolge der Konnektivität vom Dienstanbieter. Dieser Vorgang variiert. Weitere Informationen dazu, wie Sie die Reihenfolge der Konnektivität wenden Sie sich an Ihren Anbieter Connectivity.

3. Stellen Sie sicher, dass die Verbindung erfolgreich bereitgestellt wurde, indem Sie die Bereitstellung Zustand über PowerShell ExpressRoute Verbindung zu bestätigen. 

4. Konfigurieren von Domänen routing. Wenn Layer 3 für Sie von Ihrem Anbieter Connectivity verwaltet werden, werden diese routing für Ihre Verbindung konfigurieren. Wenn Ihr Connectivity-Anbieter nur Layer 2 Services anbietet, müssen Sie konfigurieren routing pro Richtlinien, die in den Seiten [Anforderungen routing](expressroute-routing.md) und [routing-Konfiguration](expressroute-howto-routing-classic.md) beschrieben.

    -  Azure private peering - Sie müssen Aktivieren dieser peering zum Verbinden mit virtuellen Computern / cloud Services innerhalb virtuelle Netzwerke bereitgestellt.
    -  Azure öffentlichen peering - Sie müssen aktivieren Azure öffentlichen peering Wenn Sie mit Azure auf öffentliche IP-Adressen gehosteten Webdiensten herstellen möchten. Dies ist eine Vorbedingung auf Azure Ressourcen zugreifen, wenn Sie ausgewählt haben, aktivieren Sie standardmäßig für Azure private peering routing.
    -  Aktivieren von Microsoft peering – Sie müssen diese in Access Office 365 und CRM online Services aktivieren. 
    
    >[AZURE.IMPORTANT] Sie müssen sicherstellen, dass Sie einen separaten Proxy verwenden / Kante an Microsoft als die Verbindung hergestellt werden, für das Internet. Mit der gleichen Kante für ExpressRoute und im Internet wird dazu führen, dass asymmetrische routing und Verbindungsausfälle für Ihr Netzwerk verursachen.

    ![](./media/expressroute-workflows/routing-workflow.png)


5. Verknüpfen von virtuellen Netzwerken zu ExpressRoute Schaltkreise - können Sie Ihre Verbindung ExpressRoute virtuelle Netzwerke verknüpfen. Führen Sie die Anweisungen [zum Verknüpfen von VNets](expressroute-howto-linkvnet-arm.md) zu Ihrer Verbindung. Diese VNets kann entweder im selben Azure-Abonnement als die Verbindung ExpressRoute oder in ein anderes Abonnement werden können.


## <a name="expressroute-circuit-provisioning-states"></a>ExpressRoute Verbindung Staaten bereitgestellt

Jede ExpressRoute-Verbindung besteht aus zwei Zustände:

- Provisioning-Anbieter-Dienststatus
- Status

Status stellt Microsoft Bereitstellung Zustand. Diese Eigenschaft wird auf aktiviert festgelegt, wenn Sie eine Verbindung Expressroute erstellen

Der Konnektivität Anbieter provisioning Zustand stellt den Zustand auf der Konnektivität Anbieterseite dar. Sie können entweder *NotProvisioned*, *Provisioning*oder *bereitgestellt*werden. Die Verbindung ExpressRoute muss bereitgestellt Zustand für Sie es verwenden können.

### <a name="possible-states-of-an-expressroute-circuit"></a>Mögliche Statusoptionen eine Verbindung ExpressRoute

Dieser Abschnitt enthält, die möglichen Zustände für eine Verbindung ExpressRoute.

#### <a name="at-creation-time"></a>Zum Zeitpunkt der Erstellung

Die Verbindung ExpressRoute in den folgenden Zustand werden angezeigt, sobald Sie die PowerShell-Cmdlets zum Erstellen der Verbindung ExpressRoute ausführen.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


#### <a name="when-connectivity-provider-is-in-the-process-of-provisioning-the-circuit"></a>Wenn gerade die Verbindung bereitgestellt ist Connectivity-Anbieter

Die Verbindung ExpressRoute in den folgenden Zustand werden angezeigt, sobald Sie dem Anbieter Connectivity Service-Taste übergeben, und sie haben den Bereitstellung Prozess gestartet.

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


#### <a name="when-connectivity-provider-has-completed-the-provisioning-process"></a>Wenn Connectivity-Anbieter das provisioning abgeschlossen wurde

Die Verbindung ExpressRoute in den folgenden Status wird angezeigt, sobald die Bereitstellung des Connectivity-Anbieters abgeschlossen.

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled

Nach der Bereitstellung und aktiviert ist, dass der einzige Zustand der Verbindung befinden kann, damit Sie es verwenden können. Wenn Sie einen Layer 2-Anbieter verwenden, können Sie konfigurieren, routing für Ihre Verbindung nur, wenn es in diesem Zustand befindet.

#### <a name="when-connectivity-provider-is-deprovisioning-the-circuit"></a>Wenn Connectivity-Anbieter die Verbindung aufheben

Wenn Sie den Dienstanbieter, die Verbindung ExpressRoute entziehen angefordert, sehen Sie die Verbindung in den folgenden Status festlegen, nachdem der Dienstanbieter den deprovisioning Vorgang abgeschlossen ist.


    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Sie können auswählen, um ihn wieder aktivieren, falls erforderlich, oder führen Sie die PowerShell-Cmdlets, um die Verbindung zu löschen.  

>[AZURE.IMPORTANT] Wenn Sie des PowerShell-Cmdlets zum Löschen der Verbindung ausführen, wenn die ServiceProviderProvisioningState bereitgestellt wird oder die fehlschlagen bereitgestellt. Arbeiten Sie an Ihren Connectivity-Anbieter, um zuerst die Verbindung ExpressRoute entziehen, und löschen Sie die Verbindung. Microsoft weiterhin die Verbindung Stücklisten, bis die Ausführung des PowerShell-Cmdlets, um die Verbindung zu löschen.


## <a name="routing-session-configuration-state"></a>Weiterleitung Sitzung Konfigurationszustand

Der BGP Zustand bereitgestellt können Sie feststellen, ob die Sitzung BGP am Rand Microsoft aktiviert wurde. Der Zustand muss zur Eingabe der peering verwenden können aktiviert sein.

Es ist wichtig, prüfen Sie den Status BGP Sitzung, vor allem für Microsoft peering. Die BGP Zustand bereitgestellt es ist ein anderes Zustand mit dem Namen *Öffentliche Präfixe Zustand angekündigt*. Im angekündigt öffentlichen Präfixe Zustand muss *konfiguriert* Zustand, für die Sitzung BGP von werden und wie Sie für Ihre weiterleiten End-to-End funktioniert. 

Wenn im Zustand angekündigt öffentlichen Präfix zu einem *Validierung erforderlich* Status festgelegt ist, ist die Sitzung BGP nicht aktiviert, wie die AS-Zahl in eine der Weiterleitung Registrierungen, die angekündigt Präfixe nicht übereinstimmen. 

>[AZURE.IMPORTANT] Ist im Zustand angekündigt öffentlichen Präfixe in *Manuelle* Überprüfungsstatus, öffnen Sie ein Ticket Support mit [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) und nachweisen, den Sie die IP-Adressen zusammen mit der zugeordneten eigenständigen System Zufallszahl angekündigt besitzen.


## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren Sie die Verbindung ExpressRoute.

    - [Erstellen einer Verbindung ExpressRoute](expressroute-howto-circuit-arm.md)
    - [Konfigurieren der Weiterleitung](expressroute-howto-routing-arm.md)
    - [Verknüpfen eines VNet zu einer ExpressRoute Verbindung](expressroute-howto-linkvnet-arm.md)
