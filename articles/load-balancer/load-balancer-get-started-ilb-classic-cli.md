<properties
   pageTitle="Erstellen Sie eine interne Lastenausgleich mithilfe der Azure CLI im Bereitstellungsmodell klassischen | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer internen Lastenausgleich mithilfe der Azure CLI im Bereitstellungsmodell klassischen"
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

# <a name="get-started-creating-an-internal-load-balancer-classic-using-the-azure-cli"></a>Erste Schritte beim Erstellen einer Azure CLI mit internen Lastenausgleich (klassisch)

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [Führen Sie diese Schritte aus, die mithilfe des Modells Ressourcenmanager](load-balancer-get-started-ilb-arm-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


## <a name="to-create-an-internal-load-balancer-set-for-virtual-machines"></a>So erstellen eine interne Lastenausgleich einrichten für virtuellen Computern

Zum Erstellen einer internen laden Lastenausgleich festlegen und den Servern, die den Datenverkehr zu senden, müssen Sie Folgendes ausführen:

1. Erstellen Sie eine Instanz der internen Lastenausgleich, die für den Endpunkt von eingehendem Lastenausgleich auf den Servern einer Gruppe mit Lastenausgleich sein.

1. Hinzufügen von Endpunkten entspricht den virtuellen Computern, die den eingehenden Datenverkehr empfangen soll.

1. Konfigurieren Sie die Server, die den Datenverkehr, um den Datenverkehr an die virtuelle IP-Adresse (VIP) Adresse der internen Lastenausgleich Instanz senden Lastenausgleich werden senden soll.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>Schrittweise durch Erstellen einer internen Lastenausgleich mit CLI

In diesem Handbuch wird gezeigt, wie eine interne Lastenausgleich basierend auf dem oben genannten Szenario zu erstellen.

1. Wenn Sie noch keine Erfahrung Azure CLI haben, finden Sie unter [Installieren und Konfigurieren der CLI Azure](../../articles/xplat-cli-install.md) , und folgen Sie den Anweisungen auf den Punkt, in dem Sie Ihre Azure-Konto und Ihr Abonnement auswählen.

2. Führen Sie den Befehl **Azure Config Modus** , um zur klassischen Ansicht wechseln aus, wie unten dargestellt.

        azure config mode asm

    Erwartetes Ergebnis:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Erstellen von Endpunkt und Laden Sie Lastenausgleich festlegen

Das Szenario setzt voraus, die virtuellen Computern "DB1" und "DB2" in einen Cloud-Dienst "Mytestcloud" bezeichnet. Beide virtuellen Computern verwenden ein virtuelles Netzwerk mit Subnetz "Subnetz-1" Meine "Testvnet" bezeichnet.

In diesem Handbuch erstellt eine interne laden Lastenausgleich Sammlung mit Port 1433 als privat Anschluss und 1433 als lokalen Anschluss.

Dies ist ein gängiges Szenario, in dem Sie SQL-virtuellen Computern haben, Back-End mithilfe eines internen Lastenausgleich sichergestellt ist, dass die Datenbankserver wird nicht direkt mithilfe einer öffentlichen IP-Adresse bereitgestellt werden.


### <a name="step-1"></a>Schritt 1

Erstellen Sie eine interne Lastenausgleich set mithilfe von `azure network service internal-load-balancer add`.

     azure service internal-load-balancer add -r mytestcloud -n ilbset -t subnet-1 -a 192.168.2.7

Parameter verwendet:

**-r** - Cloud-Dienstnamen<BR>
**-n** - internen laden Lastenausgleich Namen<BR>
**-t** - Subnetnamen (im selben Subnetz von den virtuellen Computern, die Sie dem internen Lastenausgleich hinzufügen werden)<BR>
**-ein** - (optional) fügen Sie eine statische private IP-Adresse<BR>

Schauen Sie sich `azure service internal-load-balancer --help` Weitere Informationen.

Sie können die internen laden Lastenausgleich Eigenschaften mit dem Befehl überprüfen `azure service internal-load-balancer list` *Cloud-Dienstnamen*.

Es folgt ein Beispiel für die Ausgabe:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


## <a name="step-2"></a>Schritt 2

Sie konfigurieren internen laden Lastenausgleich festlegen, wenn Sie den ersten Endpunkt hinzufügen. Sie können den Endpunkt, virtuellen Computern und Prüfpunkt Port zum internen laden Lastenausgleich Satz in diesem Schritt zugeordnet werden.

    azure vm endpoint create db1 1433 -k 1433 tcp -t 1433 -r tcp -e 300 -f 600 -i ilbset

Parameter verwendet:

**-k** - Anschluss lokalen virtuellen Computern<BR>
**-t** - Prüfpunkt port<BR>
**-r** - Protokoll Prüfpunkt<BR>
**-e** - Prüfpunkt Intervall in Sekunden<BR>
**f-** – Timeoutintervall in Sekunden <BR>
**i-** - internen laden Lastenausgleich Namen <BR>


## <a name="step-3"></a>Schritt 3

Überprüfen, ob die Konfiguration mit laden Lastenausgleich `azure vm show` *Name des virtuellen Computers*

    azure vm show DB1

Die Ausgabe werden:

        azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK


## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Erstellen Sie einen remote desktop-Endpunkt für eine virtuellen Computern

Sie können einen remote desktop-Endpunkt zum Weiterleiten von Netzwerkdatenverkehr von einem öffentlichen Port an einen lokalen Anschluss zur Verwendung von einer bestimmten virtuellen Computern erstellen `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Entfernen von virtuellen Computern von Lastenausgleich

Sie können einen virtuellen Computer aus einer internen Lastenausgleich festlegen, indem Sie den zugehörigen Endpunkt entfernen. Sobald der Endpunkt entfernt wird, wird nicht mehr festlegen Lastenausgleich des virtuellen Computers angehören.

 Sie können mithilfe das oben genannten Beispiel, den Endpunkt für virtuellen Computern "DB1" von internen Lastenausgleich "Ilbset" erstellt werden, mithilfe des Befehls entfernen `azure vm endpoint delete`.

    azure vm endpoint delete DB1 tcp-1433-1433


Schauen Sie sich `azure vm endpoint --help` Weitere Informationen.


## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren eines Quelle IP-Zugehörigkeit mit laden Lastenausgleich Verteilung-Modus](load-balancer-distribution-mode.md)

[Konfigurieren von Einstellungen zur im Leerlauf TCP Timeout für Ihre Lastenausgleich](load-balancer-tcp-idle-timeout.md)