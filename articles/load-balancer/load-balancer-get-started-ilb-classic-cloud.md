<properties
   pageTitle="Erstellen Sie eine interne Lastenausgleich für Cloud Services im Bereitstellungsmodell klassischen | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer internen Lastenausgleich mithilfe der PowerShell im Bereitstellungsmodell klassischen"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Erste Schritte zum Erstellen einer internen Lastenausgleich Cloud-Dienste (klassische)

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

<BR>

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [Führen Sie diese Schritte aus, die mithilfe des Modells Ressourcenmanager](load-balancer-get-started-ilb-arm-ps.md).


## <a name="configure-internal-load-balancer-for-cloud-services"></a>Konfigurieren von internen Lastenausgleich für Clouddienste

Interner Lastenausgleich wird sowohl virtuellen Computern und Cloud Services unterstützt. Ein internen laden Lastenausgleich Endpunkt erstellt in einen Cloud-Dienst, der außerhalb eines regionalen virtuellen Netzwerks ist werden nur in der Cloud-Dienst zugegriffen.

Die Konfiguration der internen laden Lastenausgleich muss festgelegt werden, während der Erstellung der ersten Bereitstellung in der Cloud-Dienst, wie im folgenden Beispiel dargestellt.

>[AZURE.IMPORTANT] Voraussetzung für die folgenden Schritte ausführen ist ein virtuelles Netzwerk für die Cloudbereitstellung bereits erstellt haben. Sie benötigen die virtuellen Netzwerknamen und Subnetz gehören, den Namen der internen Lastenausgleich erstellen.

### <a name="step-1"></a>Schritt 1

Öffnen Sie die Konfiguration Dienstdatei (.cscfg) für die Cloudbereitstellung in Visual Studio, und fügen Sie den folgenden Abschnitt zum Erstellen der internen Lastenausgleich unter der letzten "`</Role>`" Element für die Netzwerkkonfiguration.




    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="name of the load balancer">
          <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>


Fügen Sie die Werte für die Konfigurationsdatei Netzwerk angezeigt, wie es aussieht. Im Beispiel wird davon ausgegangen Sie, dass Sie ein Subnetz mit dem Namen "Test_vnet" mit einem Subnetz 10.0.0.0/24 namens Test_subnet und eine statische IP-Adresse 10.0.0.4 erstellt haben. Lastenausgleich erhält den Namen TestLB.

    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="testLB">
          <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>

Weitere Informationen über das Laden Lastenausgleich Schema finden Sie unter [Hinzufügen Lastenausgleich zu laden](https://msdn.microsoft.com/library/azure/dn722411.aspx).

### <a name="step-2"></a>Schritt 2


Ändern der Dienstdatei Definition (.csdef), um Endpunkte internen Lastenausgleich hinzuzufügen. Im Moment, die eine Instanz der Rolle erstellt wurde, wird der Definition Dienstdatei die Rolleninstanzen Hinzufügen der internen Lastenausgleich.


    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
      </Endpoints>
    </WorkerRole>

Folgenden dieselben Werte aus der oben genannten Beispiel können Sie die Werte lassen Sie uns zur Definitionsdatei hinzufügen.

    <WorkerRole name=WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
      </Endpoints>
    </WorkerRole>

Der Netzwerkdatenverkehr werden Lastenausgleich TestLB Lastenausgleich mithilfe von Port 80 für eingehenden Anfragen, senden an Worker Rolleninstanzen auch auf Port 80 verwenden.


## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren eines Quelle IP-Zugehörigkeit mit laden Lastenausgleich Verteilung-Modus](load-balancer-distribution-mode.md)

[Konfigurieren von Einstellungen zur im Leerlauf TCP Timeout für Ihre Lastenausgleich](load-balancer-tcp-idle-timeout.md)