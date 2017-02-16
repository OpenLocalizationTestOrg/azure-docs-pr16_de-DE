<properties
   pageTitle="Erstellen von Zuhörer für AlwaysOn Availabilty Gruppe für SQL Server in Azure virtuellen Computern"
   description="Eine schrittweise Anleitung zum Erstellen einer Zuhörer bei Gruppe Availabilty AlwaysOn für SQL Server in Azure virtuellen Computern"
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
   ms.date="07/12/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-an-internal-load-balancer-for-an-alwayson-availability-group-in-azure"></a>Konfigurieren eines internen Lastenausgleich für eine AlwaysOn Availability Group in Azure

In diesem Thema wird erläutert, wie eine interne Lastenausgleich für eine SQL Server AlwaysOn Verfügbarkeit Gruppe in Azure-virtuellen Computern ausführen in Ressource-Manager-Modell zu erstellen. Eine AlwaysOn Availability Group erfordert ein Lastenausgleich, wenn SQL Server-Instanzen auf Azure-virtuellen Computern sind. Die IP-Adresse für die Verfügbarkeit Gruppe Zuhörer zum Lastenausgleich gespeichert. Wenn eine Gruppe Verfügbarkeit mehrere Bereiche umfasst, benötigt jeder Region ein Lastenausgleich aus.

Um diese Aufgabe ausführen zu können, müssen Sie eine SQL Server AlwaysOn Verfügbarkeit Gruppe auf Azure-virtuellen Computern in Ressource-Manager-Modell bereitgestellt haben. Beide SQL Server-virtuellen Computern muss auf dieselben Verfügbarkeit gehören. Die [Microsoft-Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) können Sie die AlwaysOn Availability Group in Azure Ressourcenmanager automatisch erstellt. Diese Vorlage erstellt internen Lastenausgleich automatisch für Sie. 

Wenn Sie es vorziehen, können Sie [manuell konfigurieren eine AlwaysOn Verfügbarkeit Gruppe](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Dieses Thema setzt voraus, dass Ihre Verfügbarkeit Gruppen bereits konfiguriert sind.  

Verwandte Themen:

 - [Konfigurieren von AlwaysOn Verfügbarkeit Gruppen Azure virtuellen Computer (zusätzlich)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
 
 - [Konfigurieren einer VNet-VNet-Verbindungs mithilfe von Azure Ressourcenmanager und PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="steps"></a>Schritte

Indem Sie dieses Dokument durchlaufen Sie erstellen und konfigurieren Sie ein Lastenausgleich Azure-Portal. Konfigurieren Sie nach Abschluss, die den Cluster, um die IP-Adresse aus dem Lastenausgleich für die AlwaysOn Verfügbarkeit Gruppe Zuhörer verwenden.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Erstellen und Konfigurieren von Lastenausgleich Azure-Portal

Dieses Teils des Vorgangs führen Sie die folgenden Schritte aus, in dem Azure-Portal:

1. Erstellen Sie Lastenausgleich und konfigurieren Sie die IP-Adresse

1. Konfigurieren Sie die Back-End-Ressourcenpool

1. Erstellen des Prüfpunkts 

1. Legen Sie den Lastenausgleich Regeln

>[AZURE.NOTE] Wenn die SQL Server in anderen Ressourcengruppen und Regionen sind, werden Sie alle Schritte zweimal, einmal in jede Ressourcengruppe ausführen.

## <a name="1-create-the-load-balancer-and-configure-the-ip-address"></a>1. erstellen Sie Lastenausgleich und konfigurieren Sie die IP-Adresse

Der erste Schritt besteht im Erstellen Lastenausgleich. Öffnen Sie im Portal Azure der Ressourcengruppe, die SQL Server-virtuellen Computern enthält. Klicken Sie auf **Hinzufügen**, in der Ressourcengruppe.

- Suchen Sie nach den **Lastenausgleich**. Wählen Sie in den Suchergebnissen **Lastenausgleich**, die von **Microsoft**veröffentlicht wird.

- Klicken Sie auf das Blade **Lastenausgleich** klicken Sie auf **Erstellen**.

- Konfigurieren von **Erstellen Lastenausgleich**, klicken Sie auf den Lastenausgleich wie folgt:

| Einstellung | Wert |
| ----- | ----- |
| **Namen** | Ein Textfeld-Name, den Lastenausgleich darstellt. Beispielsweise **SqlLB**. |
| **Schema** | **Interner** |
| **Virtuelles Netzwerk** | Wählen Sie das virtuelle Netzwerk, dem die SQL Server haben.   |
| **Subnetz**  | Wählen Sie aus dem Subnetz, dem die SQL Server haben. |
| **Abonnement** | Wenn Sie mehrere Abonnements haben, kann dieses Feld angezeigt. Wählen Sie das Abonnement, das mit dieser Ressource verknüpft werden soll. Es ist normalerweise den gleichen Abonnement als aller Ressourcen für die Verfügbarkeit Gruppe.  |
| **Ressourcengruppe** | Wählen Sie aus der Ressourcengruppe, der die SQL Server haben. | 
| **Speicherort** | Wählen Sie den Azure Speicherort, dem die SQL Server haben. |

- Klicken Sie auf **Erstellen**. 

Azure erstellt den Lastenausgleich, dem Sie über konfiguriert. Lastenausgleich gehört zu einem bestimmten Netzwerk, Subnetz, Ressourcengruppe und Speicherort. Überprüfen Sie nach Abschluss der Azure laden Lastenausgleich Einstellungen in Azure. 

Konfigurieren Sie nun die Auslastung Lastenausgleich IP-Adresse ein.  

- Klicken Sie auf das Laden Lastenausgleich **Einstellungen** Blade **IP-Adresse**ein. Das **IP-Adresse** Blade zeigt, dass dies ein privates Lastenausgleich im gleichen virtuellen Netzwerk, wie Ihre SQL Server ist. 

- Legen Sie die folgenden Einstellungen: 

| Einstellung | Wert |
| ----- | ----- |
| **Subnetz** | Wählen Sie aus dem Subnetz, dem die SQL Server haben. |
| **Zuordnung** | **Statische** |
| **IP-Adresse** | Geben Sie eine nicht verwendete virtuelle IP-Adresse aus dem Subnetz ein.  |

- Speichern Sie die Einstellungen.

Lastenausgleich verfügt jetzt über eine IP-Adresse. Zeichnen Sie diese IP-Adresse ein. Sie verwenden diese IP-Adresse bei der Erstellung einer Zuhörer auf dem Cluster. In einem Powershellskript weiter unten in diesem Artikel werden soll, verwenden Sie diese Adresse für die `$ILBIP` Variable.



## <a name="2-configure-the-backend-pool"></a>2. konfigurieren Sie die Back-End-Ressourcenpool

Im nächste Schritt besteht im Erstellen einer Back-End-Adresse Ressourcenpool. Azure ruft der Back-End-Ressourcenpool *Back-End-Pool*. In diesem Fall ist der Back-End-Pool die Adressen der beiden SQL Server in der Gruppe Verfügbarkeit. 

- Klicken Sie auf den Lastenausgleich, die, den Sie erstellt haben, in der Ressourcengruppe. 

- Klicken Sie auf **Einstellungen**klicken Sie auf **Back-End-Pools**.

- Klicken Sie auf **Back-End-Adresspools**klicken Sie auf **Hinzufügen** , um einem Back-End-Adresse Ressourcenpool zu erstellen. 

- Geben Sie auf **die Back-End-Ressourcenpool hinzufügen** unter **Name**einen Namen für die Back-End-Pool aus.

- Klicken Sie unter **virtuellen Computern** auf **+ Hinzufügen eines virtuellen Computers**. 

- Klicken Sie unter **Auswählen von virtuellen Computern** klicken Sie auf **eine Sammlung Verfügbarkeit auswählen** und geben Sie an, dass die Verfügbarkeit festlegen, dass die SQL Server-virtuellen Computern gehören soll.

- Nachdem Sie die Menge der Verfügbarkeit ausgewählt haben, klicken Sie auf **den virtuellen Computern auswählen**. Klicken Sie auf den beiden virtuellen Computern, die SQL Server-Instanzen in der Gruppe Verfügbarkeit hosten. Klicken Sie auf **auswählen**. 

- Klicken Sie auf **OK** , um die Blades für **auswählen virtuellen Computern**und **die Back-End-Ressourcenpool hinzufügen**zu schließen. 

Azure aktualisiert die Einstellungen für die Back-End-Adresse Ressourcenpool. Ihre Verfügbarkeit festlegen verfügt jetzt über einen Pool von zwei SQL Server.

## <a name="3-create-a-probe"></a>3 Erstellen Sie 3 einen Prüfpunkt

Im nächste Schritt besteht im Erstellen einer Abfrage. Der Prüfpunkt definiert wie Azure prüfen, welcher der SQL Server derzeit die Verfügbarkeit Gruppe Zuhörer gehört. Azure wird den Dienst basierend auf IP-Adresse ein, die Sie beim Erstellen des Prüfpunkts definieren Port Prüfpunkt.

- Klicken Sie auf das Laden Lastenausgleich **Einstellungen** Blade klicken Sie auf **Überprüfen**. 

- Klicken Sie auf das Blade **untersucht** auf **Hinzufügen**.

- Konfigurieren des Prüfpunkts auf das Blade **Prüfpunkt hinzufügen** . Verwenden Sie die folgenden Werte zum Konfigurieren des Prüfpunkts:

| Einstellung | Wert |
| ----- | ----- |
| **Namen** | Ein Textname, den Prüfpunkt darstellt. Beispielsweise **SQLAlwaysOnEndPointProbe**. |
| **Protokoll** | **TCP** |
| **Port** | Sie können einen beliebigen verfügbaren Anschluss verwenden. Beispielsweise *59999*.    |
| **Intervall**  | *5* | 
| **Fehlerhafte Schwellenwert**  | *2* | 

- Klicken Sie auf **OK**. 

>[AZURE.NOTE] Stellen Sie sicher, dass der Anschluss, die, den Sie angeben, der Firewall beider SQL Server geöffnet ist. Beide Server erfordern eine eingehende Regel für die TCP-, die Sie verwenden. Weitere Informationen finden Sie unter [Hinzufügen oder Bearbeiten von Firewall-Regel](http://technet.microsoft.com/library/cc753558.aspx) . 

Azure wird den Prüfpunkt erstellt. Azure wird den Prüfpunkt verwendet testen, mit welchem SQL Server die Zuhörer für die Gruppe Verfügbarkeit hat.

## <a name="4-set-the-load-balancing-rules"></a>4 Legen Sie 4 den Lastenausgleich Regeln

Legen Sie den Lastenausgleich Regeln. Die Auslastung Lastenausgleich Regeln konfigurieren, wie Lastenausgleich den Datenverkehr an die SQL Server weiterleiten. Für diese Lastenausgleich aktivieren Sie direkte Server zurückgegeben, da nur von einem der beiden SQL Server die Verfügbarkeit Gruppe Zuhörer Ressource jemals nacheinander besitzen wird.

- Klicken Sie auf das Laden Lastenausgleich **Einstellungen** Blade klicken Sie auf **Regeln Lastenausgleich laden**. 

- Klicken Sie auf **Hinzufügen**, klicken Sie auf das Blade **Lastenausgleich Regeln** .

- Verwenden Sie das Blade **Hinzufügen laden Lastenausgleich Regeln** , um den Lastenausgleich Regel konfigurieren. Verwenden Sie die folgenden Einstellungen: 

| Einstellung | Wert |
| ----- | ----- |
| **Namen** | Ein Textfeld-Name, den Lastenausgleich Regeln darstellt. Beispielsweise **SQLAlwaysOnEndPointListener**. |
| **Protokoll** | **TCP** |
| **Port** | *1433*   |
| **Back-End-Anschluss** | *1433*. Beachten Sie, dass dies deaktiviert ist, da diese Regel **Beweglich IP (Absenderadresse direkte Server)**verwendet.   |
| **Prüfpunkt** | Verwenden Sie für diese Lastenausgleich den Namen der Prüfpunkt, den Sie erstellt haben. |
| **Sitzung Persistenz**  | **Keine** | 
| **Im Leerlauf Timeout (Minuten)**  | *4* | 
| **Beweglich IP (Absenderadresse direkte Server)**  | **Aktiviert** | 

 >[AZURE.NOTE] Möglicherweise müssen Sie einen Bildlauf in der Blade alle Einstellungen anzeigen.

- Klicken Sie auf **OK**. 

- Azure konfiguriert den Lastenausgleich Regel. Lastenausgleich ist nun für die Weiterleitung Verkehr auf dem SQL Server konfiguriert, das dem Zuhörer für die Gruppe Verfügbarkeit hostet. 

An diesem Punkt hat die Ressourcengruppe ein Lastenausgleich, Herstellen einer Verbindung mit beide SQL Server-Computer. Lastenausgleich enthält auch eine IP-Adresse für den SQL Server AlwaysOn Verfügbarkeit Gruppe Zuhörer, dass entweder Computer die Anfrage für die Verfügbarkeit von Gruppen bearbeiten kann.

>[AZURE.NOTE] Wenn Ihre SQL Server in zwei separaten Regionen sind, wiederholen Sie die Schritte in der anderen Region ein. Jeder Region erfordert ein Lastenausgleich. 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Konfigurieren Sie den Cluster, um die Auslastung Lastenausgleich IP-Adresse verwenden 

Im nächsten Schritt wird die Zuhörer auf Cluster konfigurieren, und die Zuhörer online schalten. Um dies zu erreichen, führen Sie folgende Schritte aus: 

1. Erstellen Sie die Verfügbarkeit Gruppe Zuhörer auf Failovercluster 

1. Zeigen Sie die Zuhörer online

## <a name="1-create-the-availablity-group-listener-on-the-failover-cluster"></a>1. Erstellen der Verfügbarkeit Gruppe Zuhörer auf Failovercluster

In diesem Schritt erstellen Sie manuell die Verfügbarkeit Gruppe Zuhörer Failovercluster-Manager und SQL Server Management Studio (SSMS).

- Verwenden Sie RDP Verbindung zu den Azure-virtuellen Computern, die das primäre Replikat befindet. 

- Failovercluster-Manager zu öffnen.

- Wählen Sie den Knoten **Netzwerke** aus, und notieren Sie den Namen der Cluster-Netzwerk. Dieser Name wird verwendet werden, der `$ClusterNetworkName` Variable in der PowerShell-Skript.

- Erweitern Sie den Clusternamen, und klicken Sie dann auf **Rollen**.

- Klicken Sie im Bereich **Rollen** mit der rechten Maustaste in des Verfügbarkeit Gruppennamen ein, und wählen Sie dann auf **Ressource hinzufügen** > **Client Access Point**.

- Erstellen Sie im Feld **Name** einen Namen für diese neue Zuhörer, und dann klicken Sie zweimal auf **Weiter** , und klicken Sie dann auf **Fertig stellen**. Schalten Sie die Zuhörer oder Ressource online an diesem Punkt.

 >[AZURE.NOTE] Der Namen für die neue Zuhörer ist der Netzwerkname, der Anwendungen in Verbindung mit Datenbanken in der Gruppe der SQL Server-Verfügbarkeit verwendet werden.

- Klicken Sie auf der Registerkarte **Ressourcen** , und klicken Sie dann erweitern Sie den soeben erstellten Client Access-Point. Mit der rechten Maustaste in der IP-Ressource, und klicken Sie auf Eigenschaften. Notieren Sie den Namen der IP-Adresse ein. Verwenden Sie diesen Namen in der `$IPResourceName` Variable in der PowerShell-Skript.

- Klicken Sie unter **IP-Adresse** klicken Sie auf **Statische IP-Adresse** aus, und legen Sie die statische IP-Adresse auf derselben Adresse, die Sie verwendet werden, wenn Sie die Auslastung Lastenausgleich IP-Adresse der Azure-Portal festlegen. Aktivieren Sie NetBIOS für diese Adresse ein, und klicken Sie auf **OK**. Wiederholen Sie diesen Schritt für jede IP-Ressource, wenn Ihre Lösung mehrere Azure VNets umfasst. 

- Klicken Sie auf Cluster-Knoten, der aktuell primäre Replikat hostet, öffnen Sie einer erhöhten PowerShell ISE, und fügen Sie die folgenden Befehle in ein neues Skript.

        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    
        Import-Module FailoverClusters
    
        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

- Aktualisieren der Variablen, und führen Sie das Skript PowerShell, um die IP-Adresse und den Port für den neuen Zuhörer konfigurieren.

 >[AZURE.NOTE] Wenn Ihre SQL Server in separaten Regionen sind, müssen Sie das PowerShell-Skript zweimal ausführen. Verwenden Sie Erstmaliges der Clusternetzwerkname Cluster IP-Ressourcenname, und Laden Sie Lastenausgleich IP-Adresse aus der ersten Ressourcengruppe. Verwenden Sie den Clusternetzwerknamen Cluster IP-Ressourcenname, der zweiten Zeitangabe und Laden Sie Lastenausgleich IP-Adresse aus der zweiten Ressourcengruppe zu.

Der Cluster verfügt jetzt über eine Verfügbarkeit Gruppe Zuhörer Ressource.

## <a name="2-bring-the-listener-online"></a>2 nehmen Sie 2 die Zuhörer online

Mit der Verfügbarkeit Gruppe Zuhörer Ressource so konfiguriert ist können Sie die Zuhörer online schalten, damit Clientanwendungen auf Datenbanken in der Gruppe Verfügbarkeit mit dem Zuhörer eine Verbindung herstellen können.

- Navigieren Sie wieder zum Failovercluster-Manager. Erweitern Sie **Rollen** , und markieren Sie die Gruppe Verfügbarkeit. Klicken Sie auf der Registerkarte **Ressourcen** mit der rechten Maustaste in des Zuhörer Namens, und klicken Sie auf **Eigenschaften**.

- Klicken Sie auf der Registerkarte **Abhängigkeiten** . Falls mehrere Ressourcen aufgeführt sind, stellen Sie sicher, dass die IP-Adressen oder nicht verfügen und, Abhängigkeiten. Klicken Sie auf **OK**.

- Mit der rechten Maustaste in des Zuhörer Namens, und klicken Sie auf **Online schalten**.


- Sobald die Zuhörer online und über die Registerkarte **Ressourcen ist** , mit der rechten Maustaste in der Gruppe Verfügbarkeit, und klicken Sie auf **Eigenschaften**.

- Erstellen Sie eine Abhängigkeit für die Ressource Zuhörer Namen (nicht die IP-Adresse Ressourcenname) ein. Klicken Sie auf **OK**.


- SQL Server Management Studio starten und an die primäre verbinden.


- Navigieren Sie zu **AlwaysOn hohen Verfügbarkeit** | **Verfügbarkeit Gruppen** | **Verfügbarkeit Gruppe Listener**. 


- Der Name der Zuhörer sollte jetzt angezeigt werden, die Sie im Failovercluster-Manager erstellt. Mit der rechten Maustaste in des Zuhörer Namens, und klicken Sie auf **Eigenschaften**.


- Geben Sie im Feld **Port** die Port-Nummer für die Verfügbarkeit Gruppe Zuhörer mithilfe der $EndpointPort früheren verwendet (1433 wurde die Standardeinstellung), klicken Sie dann auf **OK**.

Sie besitzen nun eine SQL Server AlwaysOn Verfügbarkeit Gruppe in Azure-virtuellen Computern in Ressource-Manager-Modus ausgeführt. 

## <a name="test-the-connection-to-the-listener"></a>Testen Sie die Verbindung zu der Zuhörer

Die Verbindung zu testen:

1. RDP zu einem SQL Server, die im gleichen virtuellen Netzwerk ist, aber nicht das Replikat besitzt. Dies kann die anderen SQL Server im Cluster sein.

1. Verwenden Sie **Sqlcmd** -Programm, um die Verbindung zu testen. Beispielsweise stellt das folgende Skript eine **Sqlcmd** -Verbindung an die primäre durch die Zuhörer mit Windows-Authentifizierung her:

        sqlcmd -S <listenerName> -E

Die Verbindung SQLCMD automatisch Herstellen einer Verbindung mit dem SQL Server-Instanz primäre Replikat hostet. 

## <a name="guidelines-and-limitations"></a>Richtlinien und Einschränkungen

Beachten Sie, dass die folgenden Richtlinien auf Verfügbarkeit Gruppe Zuhörer in Azure über einen internen Lastenausgleich laden:

- Pro Cloud-Dienst wird nur eine interne Verfügbarkeit Gruppe Zuhörer unterstützt, da die Zuhörer als Lastenausgleich konfiguriert ist, und es nur eine interne Lastenausgleich ist. Es ist jedoch möglich, Suchverweis externe Listener zu erstellen. 

- Mit einem internen Lastenausgleich können Sie nur die Zuhörer aus innerhalb der gleichen virtuellen Netzwerk zugreifen.
 
