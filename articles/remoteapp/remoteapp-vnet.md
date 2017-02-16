
<properties
    pageTitle="Überprüfen der Azure-VNET zur Verwendung mit Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie, wie Sie sicherstellen, dass Ihre Azure VNET mit Azure RemoteApp verwendet werden kann"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="validate-the-azure-vnet-to-use-with-azure-remoteapp"></a>Überprüfen der Azure-VNET zur Verwendung mit Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Bevor Sie eine Azure VNET mit Azure RemoteApp verwenden, sollten Sie die VNET überprüfen. Auf diese Weise können Probleme mit der Konnektivität zu verhindern.

Gehen Sie wie folgt vor, um Ihre Azure VNET zu überprüfen:

1. Erstellen Sie eine Azure-virtuellen Computern innerhalb der Subnetz von der Azure-VNET mit Azure RemoteApp verwenden möchten.

2. Mit diesen virtuellen Computer mithilfe der Option **Verbinden** im Verwaltungsportal verbinden.
3. Fügen Sie des virtuellen Computers zur gleichen Domäne, die Sie mit Azure RemoteApp verwenden möchten. Wenn Sie eine Auflistung Hybrid, die mit Ihrem lokalen Netzwerk verbindet erstellen, teilnehmen des virtuellen Computers der lokalen Domäne an.

Wenn dies erfolgreich ist, ist der Azure-VNET zur Verwendung mit RemoteApp bereit.

Weitere Informationen zu den End-to-End-Hybrid Websitesammlung Workflow finden Sie unter den folgenden Artikeln:

- [So virtuellen Netzwerks für Azure RemoteApp planen](remoteapp-planvnet.md)
- [Erstellen einer Websitesammlung hybrid](remoteapp-create-hybrid-deployment.md)
- [Bereitstellen von Azure RemoteApp Websitesammlung mit Ihrem Azure-virtuellen Netzwerk (mit Unterstützung für ExpressRoute)](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)
