<properties
   pageTitle="Immer zum Availability Group Listener – Microsoft Azure konfigurieren"
   description="Konfigurieren der Verfügbarkeit Gruppe Listener auf Azure Ressourcenmanager Modell und eine interne Lastenausgleich mit eine oder mehrere IP-Adressen verwenden."
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="10/20/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Konfigurieren Sie einen oder mehrere immer auf Verfügbarkeit Gruppe Listener - Ressourcenmanager 

In diesem Thema zeigt, wie zwei Schritte ausführen:

- Erstellen Sie eine interne Lastenausgleich für SQL Server Verfügbarkeit Gruppen mit PowerShell-Cmdlets.

- Hinzufügen von zusätzlichen IP-Adressen zu ein Lastenausgleich zur Unterstützung von mehr als eine Gruppe von SQL Server-Verfügbarkeit. 

In SQL Server ist eine Verfügbarkeit Gruppe Zuhörer einen virtuelle Netzwerknamen der Clients für den Zugriff auf eine Datenbank in der primären oder sekundären Replikation eine Verbindung herstellen. Klicken Sie auf Azure-virtuellen Computern enthält ein Lastenausgleich die IP-Adresse für die Zuhörer an. Lastenausgleich leitet den Datenverkehr auf die Instanz von SQL Server, die den Port Prüfpunkt überwacht. In den meisten Fällen wird eine Gruppe Verfügbarkeit einer internen Lastenausgleich verwendet. Ein Azure internen Lastenausgleich kann eine oder mehrere IP-Adressen hosten. Jede IP-Adresse verwendet einen bestimmten Prüfpunkt-Anschluss. Dieses Dokument wird gezeigt, wie mithilfe der PowerShell zum Erstellen eines neuen Lastenausgleichsmoduls oder IP-Adressen zu einer vorhandenen Lastenausgleich für SQL Server-Verfügbarkeit Gruppen hinzufügen. 

Die Möglichkeit, eine interne Lastenausgleich mehrere IP-Adressen zuweisen ist neu in Azure und steht nur zur Verfügung, in dem Modell Ressourcenmanager. Um diese Aufgabe ausführen zu können, müssen Sie eine SQL Server-Verfügbarkeit Gruppe auf Azure-virtuellen Computern Ressourcenmanager Modell bereitgestellt haben. Beide SQL Server-virtuellen Computern muss auf dieselben Verfügbarkeit gehören. Die [Microsoft-Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) können Sie automatisch die Gruppe Verfügbarkeit in Azure Ressourcenmanager erstellen. Diese Vorlage erstellt automatisch die Verfügbarkeit Gruppe, einschließlich den internen Lastenausgleich für Sie. Wenn Sie es vorziehen, können Sie [manuell konfigurieren eine AlwaysOn Verfügbarkeit Gruppe](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Dieses Thema setzt voraus, dass Ihre Verfügbarkeit Gruppen bereits konfiguriert sind.  

Verwandte Themen:

- [Konfigurieren von AlwaysOn Verfügbarkeit Gruppen Azure virtuellen Computer (zusätzlich)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   

- [Konfigurieren einer VNet-VNet-Verbindungs mithilfe von Azure Ressourcenmanager und PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>Konfigurieren Sie die Windows-Firewall

Konfigurieren Sie die Windows-Firewall, um den SQL Server-Zugriff zulassen. Sie müssen zum Konfigurieren der Firewall TCP-Verbindungen auf die Ports Nutzung von SQL Server-Instanz sowie den von der Zuhörer Prüfpunkt verwendeten Anschluss zulässt. Weitere Informationen finden Sie unter [Konfigurieren einer Windows-Firewall für den Zugriff auf die Datenbank-Engine](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Erstellen Sie eine eingehende Regel für die SQL Server-Ports und für den Port Probe an.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Beispielskript: Erstellen einer internen Lastenausgleich mit PowerShell

> [AZURE.NOTE] Wenn Sie die Verfügbarkeit von Gruppe mit der [Microsoft-Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) die laden erstellt brauchen Sie diesen Schritt abgeschlossen haben. 

Das folgende PowerShell-Skript erstellt eine interne Lastenausgleich, den Lastenausgleich Regeln konfiguriert und legt eine IP-Adresse für den Lastenausgleich. Führen Sie das Skript, Windows PowerShell ISE öffnen, und fügen Sie das Skript im Bereich Skript. Verwenden Sie `Login-AzureRMAccount` zum Anmelden bei PowerShell. Wenn Sie mehrere Azure-Abonnements verfügen, verwenden Sie `Select-AzureRmSubscription ` das Abonnement festlegen. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the Front End configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the Back End configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="example-script-add-an-ip-address-to-an-existing-load-balancer-with-powershell"></a>Beispiel für Skript: eine IP-Adresse an eine vorhandene Lastenausgleich mit PowerShell hinzufügen

Wenn Sie mehr als eine Gruppe von Verfügbarkeit verwenden zu können, verwenden Sie PowerShell zusätzliche IP-Adresse an eine vorhandene Lastenausgleich hinzuzufügen. Jede IP-Adresse erfordert eine eigene Lastenausgleich Regel, Prüfpunkt Anschluss und front-Anschluss.

Der front-End-Port wird, die mit Anwendungen SQL Server-Instanz herstellen. IP-Adressen für die Verfügbarkeit von anderen Gruppen können den gleichen front-End-Anschluss.

>[AZURE.NOTE] Für SQL Server-Verfügbarkeit Gruppen erfordert jede IP-Adresse einen bestimmten Prüfpunkt Anschluss. Wenn eine IP-Adresse auf einer Lastenausgleich Prüfpunkt Port 59999 verwendet, können keine anderen IP-Adressen auf die Lastenausgleich beispielsweise Prüfpunkt-Anschluss 59999 verwenden. 

- Informationen zum Lastenausgleich finden Sie unter Grenzwerte unter [Grenzwerte Networking - Ressourcenmanager Azure](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits) **Private Vorderseite IP pro Lastenausgleich zu beenden** .

- Informationen zur Verfügbarkeit finden Sie unter Gruppe Grenzwerte [Einschränkungen (Verfügbarkeit Gruppen)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Das folgende Skript fügt eine neue IP-Adresse an eine vorhandene Lastenausgleich. Aktualisieren Sie die Variablen für Ihre Umgebung an. Die ILB verwendet den Zuhörer Port für den Lastenausgleich laden-front-End-Port an. Dieser Anschluss kann den Port sein, dem SQL Server überwacht. Dies ist für Standardinstanzen von SQL Server Port 1433. Der Lastenausgleich Regel für eine Gruppe Verfügbarkeit erfordert eine unverankerte IP-Adresse (direkte Server zurückgegeben), damit der Back-End-Anschluss der front-End-Anschluss identisch ist.

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```



## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Konfigurieren Sie den Cluster, um die Auslastung Lastenausgleich IP-Adresse verwenden 

Im nächsten Schritt wird die Zuhörer auf Cluster konfigurieren, und die Zuhörer online schalten. Um dies zu erreichen, führen Sie folgende Schritte aus: 

1. Erstellen Sie die Verfügbarkeit Gruppe Zuhörer auf Failovercluster  

2. Zeigen Sie die Zuhörer online

## <a name="1-create-the-availability-group-listener-on-the-failover-cluster"></a>1. Erstellen der Verfügbarkeit Gruppe Zuhörer auf Failovercluster

In diesem Schritt fügen Sie einen Client Access Point zum Failovercluster mit Failovercluster-Manager, und klicken Sie dann verwenden PowerShell so konfigurieren Sie die Clusterressource zum Abhören der Prüfpunkt Ports. 

1. Verwenden Sie RDP Verbindung zu der Azure-virtuellen Computern, die das primäre Replikat hostet. 

2. Failovercluster-Manager zu öffnen.

3. Wählen Sie den Knoten **Netzwerke** aus, und notieren Sie den Namen der Cluster-Netzwerk. Verwenden Sie diesen Namen in der `$ClusterNetworkName` Variable in der PowerShell-Skript.

4. Erweitern Sie den Clusternamen, und klicken Sie dann auf **Rollen**.

5. Klicken Sie im Bereich **Rollen** mit der rechten Maustaste in des Verfügbarkeit Gruppennamen ein, und wählen Sie dann auf **Ressource hinzufügen** > **Client Access Point**.

6. Erstellen Sie im Feld **Name** einen Namen für diese neue Zuhörer, und dann klicken Sie zweimal auf **Weiter** , und klicken Sie dann auf **Fertig stellen**. Schalten Sie die Zuhörer oder Ressource online an diesem Punkt.
 
    Der Namen für die neue Zuhörer ist der Netzwerkname, der Anwendungen in Verbindung mit Datenbanken in der Gruppe der SQL Server-Verfügbarkeit verwendet werden.

7. Klicken Sie auf der Registerkarte **Ressourcen** , und klicken Sie dann erweitern Sie den soeben erstellten Client Access-Point. Mit der rechten Maustaste in der IP-Ressource, und klicken Sie auf Eigenschaften. Notieren Sie den Namen der IP-Adresse ein. Verwenden Sie diesen Namen in der `$IPResourceName` Variable in der PowerShell-Skript.

8. Klicken Sie unter **IP-Adresse** klicken Sie auf **Statische IP-Adresse** aus, und legen Sie die statische IP-Adresse auf derselben Adresse, die Sie verwendet werden, wenn Sie die Auslastung Lastenausgleich IP-Adresse der Azure-Portal festlegen. 

9. Deaktivieren von NetBIOS für diese Adresse ein, und klicken Sie auf **OK**. Wiederholen Sie diesen Schritt für jede IP-Ressource, wenn Ihre Lösung mehrere Azure VNets umfasst. 

10. Machen Sie die SQL Server-Verfügbarkeit Gruppe Ressource die IP-Adresse abhängig. Klicken Sie rechts in der Ressource im Cluster-Manager, ist dies auf der Registerkarte **Ressourcen** unter **Andere Ressourcen**. 

11. Klicken Sie auf Cluster-Knoten, der aktuell primäre Replikat hostet, öffnen Sie einer erhöhten PowerShell ISE, und fügen Sie die folgenden Befehle in ein neues Skript. Klicken Sie auf der Registerkarte **Abhängigkeiten** auf den Namen der Zuhörer.
 
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>

    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```
 
6. Aktualisieren der Variablen, und führen Sie das Skript PowerShell, um die IP-Adresse und den Port für den neuen Zuhörer konfigurieren.

 >[AZURE.NOTE] Wenn Ihre SQL Server in separaten Regionen sind, müssen Sie das PowerShell-Skript zweimal ausführen. Verwenden Sie Erstmaliges der Clusternetzwerkname Cluster IP-Ressourcenname, und Laden Sie Lastenausgleich IP-Adresse aus der ersten Ressourcengruppe. Verwenden Sie den Clusternetzwerknamen Cluster IP-Ressourcenname, der zweiten Zeitangabe und Laden Sie Lastenausgleich IP-Adresse aus der zweiten Ressourcengruppe zu.

Der Cluster verfügt jetzt über eine Verfügbarkeit Gruppe Zuhörer Ressource.

## <a name="2-bring-the-listener-online"></a>2 nehmen Sie 2 die Zuhörer online

Mit der Verfügbarkeit Gruppe Zuhörer Ressource so konfiguriert ist können Sie die Zuhörer online schalten, damit Clientanwendungen auf Datenbanken in der Gruppe Verfügbarkeit mit dem Zuhörer eine Verbindung herstellen können.

1. Navigieren Sie wieder zum Failovercluster-Manager. Erweitern Sie **Rollen** , und markieren Sie die Gruppe Verfügbarkeit. Klicken Sie auf der Registerkarte **Ressourcen** mit der rechten Maustaste in des Zuhörer Namens, und klicken Sie auf **Eigenschaften**.

1. Klicken Sie auf der Registerkarte **Abhängigkeiten** . Falls mehrere Ressourcen aufgeführt sind, stellen Sie sicher, dass die IP-Adressen oder nicht verfügen und, Abhängigkeiten. Klicken Sie auf **OK**.

1. Mit der rechten Maustaste in des Zuhörer Namens, und klicken Sie auf **Online schalten**.

1. Sobald die Zuhörer online und über die Registerkarte **Ressourcen ist** , mit der rechten Maustaste in der Gruppe Verfügbarkeit, und klicken Sie auf **Eigenschaften**.

1. Erstellen Sie eine Abhängigkeit für die Ressource Zuhörer Namen (nicht die IP-Adresse Ressourcenname) ein. Klicken Sie auf **OK**.

1. SQL Server Management Studio starten und an die primäre verbinden.

1. Navigieren Sie zu **AlwaysOn hohen Verfügbarkeit** | **Verfügbarkeit Gruppen** | **Verfügbarkeit Gruppe Listener**. 

1. Der Name der Zuhörer sollte jetzt angezeigt werden, die Sie im Failovercluster-Manager erstellt. Mit der rechten Maustaste in des Zuhörer Namens, und klicken Sie auf **Eigenschaften**.

1. Geben Sie im Feld **Port** die Port-Nummer für die Verfügbarkeit Gruppe Zuhörer mithilfe der $EndpointPort früheren verwendet (1433 wurde die Standardeinstellung), klicken Sie dann auf **OK**.

Sie besitzen nun eine SQL Server-Gruppe Verfügbarkeit in Azure-virtuellen Computern im Ressourcenmanager Modus ausgeführt. 

## <a name="test-the-connection-to-the-listener"></a>Testen Sie die Verbindung zu der Zuhörer

Die Verbindung zu testen:

1. RDP zu einem SQL Server, die im gleichen virtuellen Netzwerk ist, aber nicht das Replikat besitzt. Dies kann die anderen SQL Server im Cluster sein.

2. Verwenden Sie **Sqlcmd** -Programm, um die Verbindung zu testen. Beispielsweise stellt das folgende Skript eine **Sqlcmd** -Verbindung an die primäre durch die Zuhörer mit Windows-Authentifizierung her:

    ```
    sqlmd -S <listenerName> -E
    ```

    Wenn die Zuhörer einen anderen Anschluss als Standard verwendet port (1433) verwenden, geben Sie den Port in der Verbindungszeichenfolge. Verbindet beispielsweise der folgenden Befehl aus Sqlcmd zu einem Zuhörer am Port 1435 an: 
    
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

Die Verbindung SQLCMD verbindet automatisch auf dem SQL Server-Instanz primäre Replikat hostet. 

>[AZURE.NOTE] Stellen Sie sicher, dass der Anschluss, die, den Sie angeben, der Firewall beider SQL Server geöffnet ist. Beide Server erfordern eine eingehende Regel für die TCP-, die Sie verwenden. Weitere Informationen finden Sie unter [Hinzufügen oder Bearbeiten von Firewall-Regel](http://technet.microsoft.com/library/cc753558.aspx) . 

## <a name="guidelines-and-limitations"></a>Richtlinien und Einschränkungen

Beachten Sie, dass die folgenden Richtlinien auf Verfügbarkeit Gruppe Zuhörer in Azure über einen internen Lastenausgleich laden:

- Mit einem internen Lastenausgleich können Sie nur die Zuhörer aus innerhalb der gleichen virtuellen Netzwerk zugreifen.

## <a name="for-more-information"></a>Weitere Informationen

Weitere Informationen finden Sie unter [Konfigurieren von immer auf Verfügbarkeit gruppieren Azure-virtuellen Computer manuell](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

### <a name="powershell-cmdlets"></a>PowerShell-cmdlets

Verwenden Sie die folgenden PowerShell-Cmdlets zum Erstellen einer internen Lastenausgleich für Azure-virtuellen Computern an.

- `New-AzureRmLoadBalancer`erstellt ein Lastenausgleich an. Weitere Informationen finden Sie unter [New-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) . 

- `New-AzureRMLoadBalancerFrontendIpConfig`erstellt eine Front-End-IP-Konfiguration für ein Lastenausgleich. Weitere Informationen finden Sie unter [New-AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) .

- `New-AzureRmLoadBalancerRuleConfig`erstellt eine Regelkonfiguration für ein Lastenausgleich. Weitere Informationen finden Sie unter [New-AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) . 

- `New-AzureRMLoadBalancerBackendAddressPoolConfig`erstellt eine Back-End-Adresse Ressourcenpool Konfiguration für ein Lastenausgleich. Weitere Informationen finden Sie unter [New-AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) . 

- `New-AzureRmLoadBalancerProbeConfig`erstellt eine Prüfpunkt Konfiguration für ein Lastenausgleich. Weitere Informationen finden Sie unter [New-AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) .

Wenn Sie ein Lastenausgleich aus einer Azure Ressourcengruppe entfernen müssen, verwenden Sie `Remove-AzureRmLoadBalancer`. Weitere Informationen finden Sie unter [Entfernen-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx).
