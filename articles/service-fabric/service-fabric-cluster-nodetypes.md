<properties
   pageTitle="Typen von Dienst Fabric Knoten und virtueller Computer Maßstab Sätze | Microsoft Azure"
   description="Beschreibt, wie Typen von Dienst Fabric Knoten beziehen sich auf virtuellen Computer Maßstab Datensätze, und wie zu remote Herstellen einer Verbindung mit einer Instanz virtueller Computer Skalierung festlegen oder eines Knotens."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Das Verhältnis zwischen Typen der Dienst Fabric-Knoten und virtuellen Computern skalieren Datensätze

Virtuellen Computern sind skalieren eine Ressource mit Azure zu berechnen, die Sie zum Bereitstellen und Verwalten einer Gruppe von virtuellen Computern als Gruppe verwenden können. Jede Art von Knoten, die in einem Dienst Fabric Cluster definiert ist wird als eine separate virtueller Computer Skalierung festlegen eingerichtet. Einzelnen Knoten können dann nach oben skaliert werden oder nach unten unabhängig voneinander, haben unterschiedliche Optionssätze Ports öffnen und können andere Speicherkapazität Kennzahlen haben.

Die folgende Abbildung zeigt einen Cluster, der zwei Typen von Knoten: Front-End- und Back-End.  Jeder Knoten verfügt über fünf Knoten.

![Screenshot der einen Cluster, der zwei Typen von Knoten][NodeTypes]

## <a name="mapping-vm-scale-set-instances-to-nodes"></a>Zuordnen von Instanzen virtueller Computer Skalierung festlegen zu Knoten

Wie oben zu sehen ist, beginnen die Instanzen virtueller Computer Skalierung festlegen aus Instanz 0, und klicken Sie dann wechselt nach oben. Die Nummerierung wird in die Namen angezeigt. BackEnd_0 beträgt beispielsweise Instanz 0 Back-End-virtuellen Computer Skalierung festlegen. Diese bestimmten virtuellen Computer Skalierung festlegen hat fünf Instanzen, mit dem Namen BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 und BackEnd_4.

Wenn Sie von einer virtuellen Computer Skalierung festlegen Skalieren wird eine neue Instanz erstellt. Der neue virtuellen Computer Skalierung festlegen Instanznamen werden in der Regel den Namen virtueller Computer Skalierung festlegen + die Anzahl der nächsten Instanz. In diesem Beispiel ist es BackEnd_5.


## <a name="mapping-vm-scale-set-load-balancers-to-each-node-typevm-scale-set"></a>Zuordnung virtueller Computer Maßstab festlegen Lastenausgleich zu einzelnen Knoten Typ/virtueller Computer Maßstab festlegen

Wenn Sie Ihre Cluster aus dem Portal bereitgestellt haben, oder die Ressourcenmanager Beispielvorlage, die wir dann bereitgestellt, wenn Sie eine Liste aller Ressourcen unter einer Ressourcengruppe abrufen verwendet haben wird der Lastenausgleich für jeden virtuellen Computer Maßstab festlegen oder Knoten angezeigt.

Der Name sollte ungefähr wie folgt: **LB -&lt;NodeType Namen&gt;**. Beispielsweise Pfd-sfcluster4doc-0, wie in diese Abbildung dargestellt:


![Ressourcen][Resources]


## <a name="remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node"></a>Remote Herstellen einer Verbindung eine Instanz virtueller Computer Skalierung festlegen oder eines Knotens mit
Jeder Knoten in einem Cluster definierten Typ wird als eine separate virtueller Computer Skalierung festlegen eingerichtet.  Das bedeutet, dass die Knotentypen können skaliert werden nach oben oder nach unten unabhängig voneinander und anderen virtuellen Computer SKU vorgenommen werden können. Im Gegensatz zu den einzelnen Instanz virtuellen Computern erhalte die Instanzen virtueller Computer Skalierung festlegen eine virtuelle IP-Adresse der eigene nicht. Daher lässt sich eine kurze Anmerkung mitunter nur bei Ihnen eine IP-Adresse gesuchte und Port, mit denen Sie auf Remote, Verbinden mit einer bestimmten Instanz.

Hier sind die Schritte, die Sie folgen können, um diese zu ermitteln.

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a>Schritt 1: Suchen Sie die virtuelle IP-Adresse für den Knoten ein, und klicken Sie dann NAT eingehende Regeln für RDP

Um, die angezeigt wird, müssen Sie die eingehende Regeln NAT Werte zu erhalten, die als Teil der Ressourcendefinition für **Microsoft.Network/loadBalancers**definiert wurden.

Navigieren Sie im Portal auf das Laden Lastenausgleich Blade und dann auf **Einstellungen**.

![LBBlade][LBBlade]


Klicken Sie unter **Einstellungen**auf **NAT eingehende Regeln**. Diese jetzt bietet Ihnen die IP-Adresse, und verbinden Sie Port, den Sie zum remote verwenden können, auf die erste Instanz von virtuellen Computer Skalierung festlegen. In den folgenden Screenshot ist es **104.42.106.156** und **3389**

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-vm-scale-set-instancenode"></a>Schritt 2: Finden Sie heraus, die den Port, mit denen Sie auf Remote, zu verbinden, auf den bestimmten virtuellen Computer Skalierung festlegen Instanz/Knoten

In diesem Dokument aus einer früheren Version gesprochen ich über die Instanzen virtueller Computer Skalierung festlegen wie Knoten entsprechen. Wir verwenden, die, um den genauen Port ermitteln.

Die Ports werden in aufsteigender Reihenfolge der virtuellen Computer Skalierung festlegen Instanz zugewiesen. in meinem Beispiel für den Typ der Front-End-Knoten, daher sind die Ports für jede der fünf Instanzen vor. Sie müssen jetzt die gleiche Zuordnung für Ihre Instanz virtueller Computer Skalierung festlegen.

|**Instanz der virtueller Computer Maßstab festlegen**|**Port**|
|-----------------------|--------------------------|
|FrontEnd_0|3389|
|FrontEnd_1|3390|
|FrontEnd_2|3391|
|FrontEnd_3|3392|
|FrontEnd_4|3393|
|FrontEnd_5|3394|


### <a name="step-3-remote-connect-to-the-specific-vm-scale-set-instance"></a>Schritt 3: Remote Herstellen einer Verbindung mit der jeweiligen Instanz von virtuellen Computer Skalierung festlegen.

In den folgenden Screenshot verwende ich Remotedesktop-Verbindung für die Verbindung zu den FrontEnd_1 aus:

![RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a>So ändern Sie die RDP Port Bereichswerte

### <a name="before-cluster-deployment"></a>Vor der Clusterbereitstellung

Wenn Sie den Cluster mithilfe einer Vorlage Ressourcenmanager einrichten, können Sie den Bereich in der **InboundNatPools**angeben.

Wechseln Sie zu der Ressourcendefinition für **Microsoft.Network/loadBalancers**. Finden Sie unter, die Sie die Beschreibung für die **InboundNatPools**aus.  Ersetzen Sie die Werte *FrontendPortRangeStart* und *FrontendPortRangeEnd* .

![InboundNatPools][InboundNatPools]


### <a name="after-cluster-deployment"></a>Nach der Clusterbereitstellung
Dies ist etwas komplizierter und dazu führen, dass die virtuellen Computern erste wiederverwendet. Sie verfügen jetzt neue Werte mithilfe der PowerShell Azure festlegen. Stellen Sie sicher, dass Azure PowerShell 1.0 oder höher auf Ihrem Computer installiert ist. Wenn dies noch nicht geschehen ist, vorschlagen ich stimme, Sie die Schritte befolgen in [zum Installieren und Konfigurieren von Azure PowerShell.](../powershell-install-configure.md)

Melden Sie sich bei Ihrem Konto Azure. Wenn diese PowerShell-Befehl aus irgendeinem Grund fehlschlägt, sollten Sie überprüfen, ob Sie Azure PowerShell ordnungsgemäß installiert haben.

```
Login-AzureRmAccount
```

Führen Sie vor, um die Details zu Ihrer Lastenausgleich zu erhalten, und Sie die Werte für die Beschreibung für die **InboundNatPools**finden Sie unter:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Legen Sie jetzt *FrontendPortRangeEnd* und *FrontendPortRangeStart* , um die gewünschten Werte.

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über das Feature "Überall bereitstellen" und einen Vergleich mit Azure verwaltet Cluster](service-fabric-deploy-anywhere.md)
- [Cluster-Sicherheit](service-fabric-cluster-security.md)
- [Service Fabric SDK und erste Schritte](service-fabric-get-started.md)


<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
