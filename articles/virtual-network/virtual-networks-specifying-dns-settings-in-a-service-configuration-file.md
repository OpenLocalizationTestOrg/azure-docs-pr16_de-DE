<properties 
   pageTitle="Angeben von DNS-Einstellungen in einer Dienstkonfigurationsdatei | Microsoft Azure"
   description="Angeben von benutzerdefinierten DNS-Einstellungen Dienstdatei-Konfiguration für virtuelle Netzwerk verwenden"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/24/2016"
   ms.author="jdial" />

# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>Angeben von DNS-Einstellungen in einer Konfigurationsdatei Service

## <a name="dns-elements"></a>DNS-Elemente

Dienstkonfigurationsdatei möglicherweise ein DnsServers Element mit einer Liste von IPv4-Adressen für die Server (DNS = Domain Name System) enthält, der der Dienst verwendet werden. Einstellungen in der Konfiguration Dienstdatei Vorrang vor Einstellungen in der Konfigurationsdatei Netzwerk. Weitere Informationen finden Sie unter [Azure Service Konfigurationsschema (.cscfg Datei)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration element**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

>[AZURE.WARNING] Das Attribut **Name** im Element **DnsServer** wird nur als Referenz Namen verwendet. Es stellt den Hostnamen für die DNS-Server keine dar. Jeder **DnsServer** Attributwert muss über das gesamte Microsoft Azure-Abonnement eindeutig sein.

## <a name="see-also"></a>Siehe auch

[Azure Service Konfigurationsschema (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Die Konfigurationsschema Azure virtuelles Netzwerk](http://go.microsoft.com/fwlink/?LinkId=248093)

[Konfigurieren eines virtuellen Netzwerks mit Netzwerk-Konfigurations-Dateien](http://go.microsoft.com/fwlink/?LinkId=248094)

[Informationen zu virtuellen Netzwerk-Einstellungen im Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkId=248092)

