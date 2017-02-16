<properties
   pageTitle="Konfigurieren der Lastenausgleich für SQL immer auf | Microsoft Azure"
   description="Konfigurieren von Lastenausgleich Arbeitens mit SQL immer auf und wie Sie zum Erstellen von Lastenausgleich für die SQL-Implementierung Powershell nutzen können"
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

# <a name="configure-load-balancer-for-sql-always-on"></a>Konfigurieren der Lastenausgleich für SQL immer auf

SQL Server AlwaysOn Verfügbarkeit Gruppen kann nun mit ILB ausgeführt werden. Availability Group ist eine SQL Server führenden Lösung für hohe Verfügbarkeit und Disaster Wiederherstellung. Die Verfügbarkeit Gruppe Zuhörer ermöglicht Clientanwendungen nahtlos mit primäres Replikat, ohne Rücksicht auf die Anzahl der in der Konfiguration Replikate verbinden.

Die Zuhörer (DNS) zugeordnet ist, einer Lastenausgleich IP-Adresse und des Azure Lastenausgleich leitet den eingehenden Datenverkehr zum primären Server bestimmen.

Sie können ILB Support für SQL Server AlwaysOn (Zuhörer) Endpunkte verwenden. Klicken Sie jetzt können steuern, den Zugriff auf die Zuhörer und können die Lastenausgleich IP-Adresse aus einem bestimmten Subnetz in Ihrem virtuellen Netzwerk (VNet) auswählen.

Mithilfe von ILB auf die Zuhörer, den SQL Server-Endpunkt (z. B. Server = Tcp:ListenerName, 1433; Datenbank = Datenbankname) zugegriffen werden nur durch:

- Services und virtuellen Computern im gleichen virtuellen Netzwerk
- Services und virtuellen Computern aus verbundenen lokalen Netzwerk
- Services und virtuellen Computern aus verbundener VNets

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Abbildung 1: SQL AlwaysOn mit Internet zugänglichen Lastenausgleich konfiguriert

## <a name="add-internal-load-balancer-to-the-service"></a>Interner Lastenausgleich zum Dienst hinzufügen

1. Im folgenden Beispiel wird eine virtuelle Netzwerk konfiguriert, die ein Subnetz mit dem Namen "Subnetz-1" enthält:

        Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc

2. Hinzufügen von Lastenausgleich Endpunkten für jedes virtuellen Computers ILB

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –
        DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Im Beispiel oben, stehen Ihnen 2 virtuellen Computers sogenannte "sqlsvc1" und "sqlsvc2" Ausführung in der Cloud-service "Sqlsvc". Nach dem Erstellen der ILB mit "DirectServerReturn" wechseln, fügen Sie Lastenausgleich Endpunkte der ILB dürfen SQL so konfigurieren Sie die Listener für die Verfügbarkeit von Gruppen hinzu.

Weitere Informationen zu SQL AlwaysOn finden Sie unter [Konfigurieren eines internen Lastenausgleich für eine AlwaysOn Availability Group in Azure](../virtual-machines/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Siehe auch

[Erste Schritte zum Konfigurieren einer gegenüberliegende Lastenausgleich Internet](load-balancer-get-started-internet-arm-ps.md)

[Erste Schritte zum Konfigurieren einer internen Lastenausgleich](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurieren eines laden Lastenausgleich Verteilung Modus](load-balancer-distribution-mode.md)

[Konfigurieren von Einstellungen zur im Leerlauf TCP Timeout für Ihre Lastenausgleich](load-balancer-tcp-idle-timeout.md)
