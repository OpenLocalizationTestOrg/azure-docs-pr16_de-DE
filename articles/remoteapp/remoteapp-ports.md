
<properties
    pageTitle="Liste der Ports und URLs zur weißen Liste für Azure RemoteApp bereitgestellt in Kunden virtuelles Netzwerk | Microsoft Azure"
    description="Erfahren Sie, welche Ports und URLs Sie für die Kommunikation über Azure RemoteApp konfigurieren müssen."
    services="remoteapp"
    documentationCenter=""
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="elizapo" />



# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Liste mit Ports und URLs Zugriff für Azure RemoteApp bereitgestellt in Kunden virtuelles Netzwerk zulassen 

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Folgendes gilt für Azure RemoteApp eine Auflistung Cloud oder Hybrid Wenn Sie es in ein virtuelles Netzwerk (VNET) bereitstellen. Weitere Informationen zum virtuelle Netzwerke, lesen Sie bitte [Virtuelle Network (Übersicht)](../virtual-network/virtual-networks-overview.md). Wenn Sie eine Netzwerk-Sicherheitsgruppe (NSG) Einschränkung des Datenverkehrs an dem ausgewählten für Azure RemoteApp virtuelles Netzwerkressourcen erstellt haben, stellen Sie sicher, dass die folgenden zugänglich und durch die Sicherheitsrichtlinien für die auf das virtuelle Netzwerk zulässige aufweisen. Weitere Informationen zum Netzwerk Sicherheitsgruppen lesen Sie bitte [was eine Sicherheitsgruppe Netzwerk? (NSG)](../virtual-network/virtual-networks-nsg.md).

##  <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a>Azure RemoteApp Subnetz benötigt Zugriff auf diese Endpunkte und URLs: 
*   *. servicebus.windows.net
*    *. servicebus.net
*    https://*.RemoteApp.windwsazure.com  
*    https://www.RemoteApp.windowsazure.com 
*    https://*RemoteApp.windowsazure.com  
*    https://*.Core.Windows.NET  
*    Ausgehende: TCP: 443, TCP: 10101-10175 
*    Optional – UDP: 10201-10275  
 
## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a>Azure RemoteApp-Clients benötigen Zugriff auf diese Endpunkte und URLs: 

Von Clients bedeutet, dass ich die Desktops, Geräte usw., die es Benutzern mit der apps bereitgestellt, die in der Auflistung Azure RemoteApp herstellen.

-  https://telemetry.RemoteApp.windowsazure.com  
-  https://*.RemoteApp.windowsazure.com (die optional UDP-Ports sind für diese Adresse) 
-  https://Login.Windows.NET  
-  https://Login.microsoftonline.com  
-  https://www.RemoteApp.windowsazure.com 
-  https://*.Core.Windows.NET  
-  Ausgehende: TCP: 443  
-  Optional – UDP: 3391 