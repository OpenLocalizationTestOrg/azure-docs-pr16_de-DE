<properties
   pageTitle="Verschieben von ExpressRoute Schaltkreise von klassischen zu Ressourcenmanager | Microsoft Azure"
   description="Diese Seite enthält einen Überblick darüber, was Sie über der Standardansicht und die Ressourcenmanager Bereitstellungsmodelle bridging wissen müssen."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>

# <a name="moving-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model"></a>Verschieben von ExpressRoute Schaltkreise in der Standardansicht zum Bereitstellungsmodell Ressourcenmanager

In diesem Artikel bietet einen Überblick darüber, was dies bedeutet das Modell zur Bereitstellung von Azure Ressourcenmanager eine Verbindung Azure ExpressRoute von der Standardansicht wechseln.

[AZURE.INCLUDE [vpn-gateway-sm-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Eine einzelne ExpressRoute Verbindung können Verbindung zum virtuelle Netzwerke, die sowohl in der Standardansicht und die Ressourcenmanager Bereitstellungsmodelle bereitgestellt werden. Eine Verbindung ExpressRoute, unabhängig davon, wie es erstellt wurde, kann jetzt über beide Bereitstellungsmodelle mit virtuellen Netzwerken verknüpfen.

![Eine ExpressRoute Verbindung mit einem Link zu virtuelle Netzwerke ist über beide Bereitstellungsmodelle](./media/expressroute-move/expressroute-move-1.png)

## <a name="expressroute-circuits-that-are-created-in-the-classic-deployment-model"></a>Schaltkreise ExpressRoute, die in der klassischen Bereitstellungsmodell erstellt werden

ExpressRoute Schaltkreise, die in der klassischen Bereitstellungsmodell erstellt werden müssen, zum Bereitstellungsmodell Ressourcenmanager zuerst verschoben werden sollen, ermöglichen Sie die Verbindung der Standardansicht und die Ressourcenmanager Bereitstellungsmodelle. Gibt es keine Verlust der Konnektivität oder Unterbrechung, wenn eine Verbindung verschoben wird. Alle Verbindung zu virtuellen Netzwerk Links im Bereitstellungsmodell klassischen (innerhalb der gleichen Abonnement und Cross-Abonnement) werden beibehalten.

Nach das Verschieben erfolgreich abgeschlossen wurde, wird die Verbindung ExpressRoute sucht, führt und kommt Ihnen vor genau wie eine ExpressRoute-Verbindung, die im Bereitstellungsmodell Ressourcenmanager erstellt wurde. Sie können nun Verbindungen mit virtuellen Netzwerken im Bereitstellungsmodell Ressourcenmanager erstellen.

Nach einer ExpressRoute Verbindung verschoben in das Datenmodell Ressourcenmanager Bereitstellung, Sie nur mithilfe der Ressourcenmanager Bereitstellung des Modells zum Ende des Lebenszyklus der Verbindung ExpressRoute verwalten können. Dies bedeutet, dass Sie Operationen wie hinzufügen/aktualisieren/löschen Peerings Eigenschaften der Verbindung (z. B. Bandbreite, SKU und Abrechnung Typ) aktualisieren und Löschen von Schaltkreise nur im Bereitstellungsmodell Ressourcenmanager ausführen können. Finden Sie im Abschnitt unten auf Schaltkreise, die im Bereitstellungsmodell Ressourcenmanager Weitere Informationen zur Verwaltung der Zugriff auf beide Bereitstellungsmodelle erstellt werden.

Sie müssen nicht Ihren Connectivity-Anbieter, um den Umzug (Variablen) haben.

## <a name="expressroute-circuits-that-are-created-in-the-resource-manager-deployment-model"></a>ExpressRoute Schaltkreise, die im Bereitstellungsmodell Ressourcenmanager erstellt wurden

Sie können ExpressRoute Schaltkreise aktivieren, die im Bereitstellungsmodell Ressourcenmanager zugreifen kann beide Bereitstellungsmodelle erstellt werden. Aus beiden Bereitstellungsmodellen zugegriffen werden, kann alle ExpressRoute Verbindung in Ihr Abonnement aktiviert sein.

- ExpressRoute Schaltkreise, die im Bereitstellungsmodell Ressourcenmanager erstellt wurden haben keinen Zugriff auf das Bereitstellungsmodell klassischen standardmäßig.
- ExpressRoute Schaltkreise, die zum Modell Bereitstellung Manager Ressourcen aus dem Bereitstellungsmodell klassischen verschoben wurden werden beide Bereitstellungsmodelle standardmäßig zugegriffen.
- Eine Verbindung ExpressRoute enthält immer Zugriff auf die Ressourcenmanager Bereitstellungsmodell, unabhängig davon, ob sie in der Ressourcenmanager erstellt wurde oder das Modell zur klassischen Bereitstellung. Dies bedeutet, dass Sie Verbindungen zu virtuelle Netzwerke im Bereitstellungsmodell Ressourcenmanager erstellte folgenden Anweisungen [zum Verknüpfen virtuelle Netzwerke](expressroute-howto-linkvnet-arm.md)erstellen können.
- Zugriff auf das Bereitstellungsmodell klassischen wird durch den Parameter **AllowClassicOperations** in der Verbindung ExpressRoute gesteuert.

>[AZURE.IMPORTANT] Wenden Sie alle Kontingente, die auf der Seite [Beschränkungen Service](../azure-subscription-service-limits.md) beschrieben werden. Beispielsweise kann eine standard-Verbindung höchstens 10 virtuelles Netzwerk Links/über sowohl das klassische und die Ressourcenmanager Bereitstellungsmodelle verwenden.


## <a name="controlling-access-to-the-classic-deployment-model"></a>Steuern des Zugriffs auf das Bereitstellungsmodell klassischen

Sie können eine einzelne ExpressRoute Verbindung zum Herstellen einer Verknüpfung mit virtuellen Netzwerken in beiden Bereitstellungsmodellen durch Festlegen des Parameters **AllowClassicOperations** der Verbindung ExpressRoute aktivieren.

**AllowClassicOperations** auf "TRUE" festlegen, können Sie virtuelle Netzwerke aus beiden Bereitstellungsmodellen der ExpressRoute angeschlossen verknüpfen. Sie können virtuelle Netzwerke im Bereitstellungsmodell klassischen von folgen Anleitungen [zum Verknüpfen von virtueller Netzwerken im Bereitstellungsmodell klassischen](expressroute-howto-linkvnet-classic.md)verknüpfen. Sie können virtuelle Netzwerke im Bereitstellungsmodell Ressourcenmanager von folgen Anleitungen [zum Verknüpfen von virtueller Netzwerken im Bereitstellungsmodell Ressourcenmanager](expressroute-howto-linkvnet-arm.md)verknüpfen.

**AllowClassicOperations** festlegen auf falsch blockiert den Zugriff auf die Verbindung aus dem Bereitstellungsmodell klassischen. Allerdings werden alle virtuelles Netzwerk Links in der klassischen Bereitstellungsmodell beibehalten. In diesem Fall ist die ExpressRoute Verbindung nicht im Bereitstellungsmodell klassischen sichtbar.

## <a name="supported-operations-in-the-classic-deployment-model"></a>Unterstützte Vorgänge im Bereitstellungsmodell klassischen

Die folgenden klassischen Vorgänge werden auf eine Verbindung ExpressRoute unterstützt, wenn **AllowClassicOperations** auf TRUE gesetzt ist:

 - Erhalten von Informationen für ExpressRoute-Verbindung
 - Erstellen, aktualisieren und Get/löschen virtuelles Netzwerklinks zu klassischen virtuelle Netzwerke
 - Erstellen, aktualisieren und Get/löschen virtuelles Netzwerk Link Autorisierungs für Cross-Abonnement-Konnektivität

Die folgenden klassischen Vorgänge nicht möglich, wenn **AllowClassicOperations** true eingestellt ist:

 - Erstellen, aktualisieren und Get/löschen Rahmen Gateway Protocol (BGP) Peerings für Azure private, Azure öffentlichen und Microsoft peerings
 - Schaltkreise ExpressRoute löschen

## <a name="communication-between-the-classic-and-the-resource-manager-deployment-models"></a>Kommunikation zwischen der Standardansicht und die Ressourcenmanager Bereitstellungsmodelle

Die Verbindung ExpressRoute verhält sich wie eine Verbindung zwischen der Standardansicht und die Ressourcenmanager Bereitstellungsmodelle. Datenverkehr zwischen virtuellen Computern in virtuelle Netzwerke im Bereitstellungsmodell klassischen und denen in virtueller Netzwerke in der Ressourcenmanager Bereitstellung Modell Zahlungen über ExpressRoute, wenn beide virtuelle Netzwerke mit der gleichen ExpressRoute Verbindung verknüpft sind.

Aggregierte Durchsatz wird durch die Durchsatzkapazität des Gateways virtuelles Netzwerk begrenzt. In diesem Fall geben den Datenverkehr nicht den Connectivity-Anbieter Netzwerke oder Ihre Netzwerke. Datenfluss zwischen den virtuellen Netzwerken ist vollständig innerhalb des Microsoft-Netzwerks enthalten.

## <a name="access-to-azure-public-and-microsoft-peering-resources"></a>Zugriff auf Azure öffentlichen und Peeringliste Microsoft-Ressourcen

Sie können weiterhin auf Ressourcen zugreifen, die in der Regel durch Azure öffentlichen peering und Microsoft peering ohne jegliche Unterbrechung zugegriffen werden kann.  

## <a name="whats-supported"></a>Was wird unterstützt

In diesem Abschnitt beschrieben, was für ExpressRoute Schaltkreise unterstützt wird:

 - Eine einzelne ExpressRoute Verbindung können Sie virtuelle Netzwerke zugreifen, die in der Standardansicht und die Ressourcenmanager Bereitstellungsmodelle bereitgestellt werden.
 - Sie können eine Verbindung ExpressRoute in der Standardansicht zum Bereitstellungsmodell Ressourcenmanager verschieben. Nach dem verschieben, wird die Verbindung ExpressRoute sucht, ist mit der und verhält sich wie alle anderen ExpressRoute-Verbindung, die im Bereitstellungsmodell Ressourcenmanager erstellt wird.
 - Sie können nur die ExpressRoute Verbindung verschieben. Durch diesen Vorgang können Verbindung Links, virtuelle Netzwerke und VPN-Gateways verschoben werden.
 - Nach einer ExpressRoute Verbindung verschoben in das Datenmodell Ressourcenmanager Bereitstellung, Sie nur mithilfe der Ressourcenmanager Bereitstellung des Modells zum Ende des Lebenszyklus der Verbindung ExpressRoute verwalten können. Dies bedeutet, dass Sie Operationen wie hinzufügen/aktualisieren/löschen Peerings Eigenschaften der Verbindung (z. B. Bandbreite, SKU und Abrechnung Typ) aktualisieren und Löschen von Schaltkreise nur im Bereitstellungsmodell Ressourcenmanager ausführen können.
 - Die Verbindung ExpressRoute verhält sich wie eine Verbindung zwischen der Standardansicht und die Ressourcenmanager Bereitstellungsmodelle. Datenverkehr zwischen virtuellen Computern in virtuelle Netzwerke im Bereitstellungsmodell klassischen und denen in virtueller Netzwerke in der Ressourcenmanager Bereitstellung Modell Zahlungen über ExpressRoute, wenn beide virtuelle Netzwerke mit der gleichen ExpressRoute Verbindung verknüpft sind.
 - Cross-Abonnement-Konnektivität wird in der Standardansicht und in der Ressourcenmanager Bereitstellungsmodelle unterstützt.

## <a name="whats-not-supported"></a>Was wird nicht unterstützt.

In diesem Abschnitt beschrieben, welche ExpressRoute Schaltkreise nicht unterstützt wird:

 - Verschieben Sie in der Standardansicht Verbindung Links, Gateways und virtuelle Netzwerke zum Bereitstellungsmodell Ressourcenmanager.
 - Verwalten des Lebenszyklus von einer ExpressRoute Verbindung aus dem Bereitstellungsmodell klassischen.
 - Rollenbasierte Access-Steuerelement (RBAC) Unterstützung für das Bereitstellungsmodell klassischen. RBAC-Steuerelemente, um eine Verbindung nicht im Bereitstellungsmodell klassischen möglich. Alle Administrator/Coadministrator des Abonnements können verknüpfen oder Aufheben der Verknüpfung mit virtuelle Netzwerke, um die Verbindung.

## <a name="configuration"></a>Konfiguration

Folgen Sie den Anweisungen, die in [einer ExpressRoute Verbindung in der Standardansicht zum Bereitstellungsmodell Ressourcenmanager verschieben](expressroute-howto-move-arm.md)beschrieben werden.

## <a name="next-steps"></a>Nächste Schritte

- Workflow-Informationen finden Sie unter [ExpressRoute Verbindung provisioning Workflows und Status Verbindung](expressroute-workflows.md).
- So konfigurieren Sie die Verbindung ExpressRoute

    - [Erstellen einer Verbindung ExpressRoute](expressroute-howto-circuit-arm.md)
    - [Konfigurieren der Weiterleitung](expressroute-howto-routing-arm.md)
    - [Erstellen einer Verknüpfung ein virtuelles Netzwerk mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)
