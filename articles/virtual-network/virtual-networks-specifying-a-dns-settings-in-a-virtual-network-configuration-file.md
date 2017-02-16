<properties 
   pageTitle="Angeben von DNS-Einstellungen in einer Konfigurationsdatei virtuelles Netzwerk | Microsoft Azure"
   description="So ändern Sie die DNS-Server-Einstellungen in einem virtuellen Netzwerk mit einer Konfigurationsdatei virtuelles Netzwerk im Bereitstellungsmodell klassischen"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" 
   tags="azure-service-management" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="jdial" /> 


# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>Angeben von DNS-Einstellungen in einer Konfigurationsdatei virtuelles Netzwerk

Eine Konfigurationsdatei Netzwerk besteht aus zwei Elementen, die Sie verwenden können, um Einstellungen (DNS = Domain Name System) angeben: **DnsServers** und **DnsServerRef**. Sie können eine Liste der DNS-Server hinzufügen, indem Sie ihre IP-Adressen und die Namen der Bezug auf das Element **DnsServers** angeben. Klicken Sie dann können ein Element **DnsServerRef** angeben, welche DNS-Servereinträge aus dem Element DnsServers für anderen Netzwerk Websites innerhalb des virtuellen Netzwerks verwendet werden.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell klassischen.

Konfigurationsdatei Netzwerk möglicherweise die folgenden Elemente enthalten. Zu einer Seite, die zusätzliche Informationen zu den Einstellungen für das Element Wert bereitstellt, wird der Titel jedes Elements verknüpft.

>[AZURE.IMPORTANT] Informationen zum Konfigurieren der Konfigurationsdatei Netzwerk finden Sie unter [einem virtuellen Netzwerk mithilfe einer Netzwerk-Konfigurationsdatei konfigurieren](virtual-networks-using-network-configuration-file.md). Informationen über die einzelnen Elemente in der Konfigurationsdatei Netzwerk enthalten sind finden Sie unter [Azure virtuelle Netzwerk Konfigurationsschema](https://msdn.microsoft.com/library/azure/jj157100.aspx).

[DNS-Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

>[AZURE.WARNING] Das Attribut **Name** im Element **DnsServer** wird nur als Referenz für das Element **DnsServerRef** verwendet. Es stellt den Hostnamen für die DNS-Server keine dar. Jeder **DnsServer** Attributwert muss für das gesamte Microsoft Azure-Abonnement eindeutig sein.

[Virtuelle Netzwerk Websites Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

>[AZURE.NOTE] Damit diese Einstellung für das Element virtuelle Netzwerk Websites angeben möchten, müssen sie zuvor im DNS-Element definiert werden. Den DnsServerRef *Namen* in das virtuelle Netzwerk Websites Element muss auf einen Wert für Name im DNS-Element für den *Namen*des angegebenen verweisen.

## <a name="next-steps"></a>Nächste Schritte

- Grundlegendes zu [Azure virtuelles Netzwerk Konfigurationsschema](http://go.microsoft.com/fwlink/?LinkId=248093)aus.
- Grundlegendes zu [Azure Service Konfigurationsschema](https://msdn.microsoft.com/library/windowsazure/ee758710)aus.
- [Konfigurieren eines virtuellen Netzwerks mithilfe von Netzwerk-Konfigurationsdateien](virtual-networks-using-network-configuration-file.md).
