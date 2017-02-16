<properties
   pageTitle="Erste Schritte beim Erstellen einer gegenüberliegende Lastenausgleich in der klassischen Bereitstellung Modell für Cloud-Diensten mit Internet | Microsoft Azure"
   description="Informationen Sie zum Erstellen eines Internet gegenüberliegende Lastenausgleich im Modell zur klassischen Bereitstellung für Clouddienste"
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
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Erste Schritte beim Erstellen eines Internet gegenüberliegende Lastenausgleich für Clouddienste

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell klassischen. Sie können auch an, [wie ein Lastenausgleich mit Azure Ressourcenmanager gegenüberliegende Internet erstellt werden](load-balancer-get-started-internet-arm-cli.md).

Cloud-Dienste werden automatisch mit einem Lastenausgleich konfiguriert und können über das Service-Modell angepasst werden.

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>Erstellen Sie ein Lastenausgleich mithilfe der Definition Dienstdatei

Sie können die Azure SDK für .NET 2,5 Aktualisieren der Cloud-Dienst nutzen. Endpunkt Einstellungen für Clouddienste, die in der [Dienst Formulardefinitionsdatei](https://msdn.microsoft.com/library/azure/gg557553.aspx).csdef vorgenommen wurden.

Das folgende Beispiel zeigt, wie eine servicedefinition.csdef-Datei für eine Cloudbereitstellung konfiguriert ist:

Aktivieren die Snipet für die .csdef-Datei, die von einer Cloudbereitstellung generiert, können Sie den externen Endpunkt so konfiguriert ist, verwenden Sie die Ports HTTP auf Port 10000, 10001 und 10002 anzeigen.


    <ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
    <Endpoints>
        <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
        <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
        <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

        <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

        <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
           <AllocatePublicPortFrom>
              <FixedPortRange min=“10110” max=“10120“  />
           </AllocatePublicPortFrom>
        </InstanceInputEndpoint>
        <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
    </Endpoints>
    </WorkerRole>
    </ServiceDefinition>




## <a name="check-load-balancer-health-status-for-cloud-services"></a>Aktivieren Sie laden Lastenausgleich Integritätsstatus für Clouddienste


Im folgenden finden ein Beispiel für einen Gesundheit Prüfpunkt:

        <LoadBalancerProbes>
        <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
        </LoadBalancerProbes>

Lastenausgleich kombiniert der Informationen für den Endpunkt und die benötigten Informationen von der Prüfpunkt eine URL in Form von http://{DIP von VM}:80/Probe.aspx zu erstellen, die die Integrität des Diensts Abfragen verwendet werden können.

Der Dienst erkennt periodisch Versuche die IP-Adresse. Hierbei handelt es sich um die Integrität Prüfpunkt Anfrage in Kürze vom Host des Knotens des virtuellen Computers, auf dem ausgeführt.
Der Dienst wurde reagiert mit einem HTTP 200 Statuscode für den Lastenausgleich davon ausgehen, dass der Dienst fehlerfrei ist. Alle anderen HTTP-Statuscode (beispielsweise 503) direkt dauert des virtuellen Computers bei Drehung ab.

Die Definition Prüfpunkt wird auch die Häufigkeit von der Prüfpunkt gesteuert. In diesem Fall ist der Lastenausgleich den Endpunkt jeder 5 Sekunden Überprüfung. Wenn keine positive Antwort für 10 Sekunden (zwei Prüfpunkt Intervalle) eingeht, wird davon ausgegangen, dass des Prüfpunkts nach unten und des virtuellen Computers von Drehung geöffnet ist. Auf ähnliche Weise wird, wenn der Dienst außerhalb des zulässigen Drehpunkt liegt, und eine positive Antwort empfangen wird, der Dienst wieder um Winkel sofort gespeichert. Wenn der Dienst zwischen fehlerfrei und fehlerhaften entsprechend ist, kann Lastenausgleich entscheiden die erneute Einleitung des Diensts zurück zur Drehung verzögern, bis sie für eine Reihe von Prüfpunkte fehlerfrei wurde.

Aktivieren Sie das Schema der Dienst Definition für den [Systemzustand Prüfpunkt](https://msdn.microsoft.com/library/azure/jj151530.aspx) für Weitere Informationen.

## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte zum Konfigurieren einer internen Lastenausgleich](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurieren eines laden Lastenausgleich Verteilung Modus](load-balancer-distribution-mode.md)

[Konfigurieren von Einstellungen zur im Leerlauf TCP Timeout für Ihre Lastenausgleich](load-balancer-tcp-idle-timeout.md)

