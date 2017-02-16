<properties
   pageTitle="Erste Schritte beim Erstellen eines Internet gegenüberliegende Lastenausgleich im Modell zur klassischen Bereitstellung mithilfe der CLI Azure | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer gegenüberliegende Lastenausgleich im Modell zur klassischen Bereitstellung mithilfe der CLI Azure Internet"
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

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-cli"></a>Erste Schritte beim Erstellen einer gegenüberliegende Lastenausgleich (klassisch) in der CLI Azure Internet

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell klassischen. Sie können auch an, [wie ein Lastenausgleich mit Azure Ressourcenmanager gegenüberliegende Internet erstellt werden](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a>Schrittweise durch Erstellen einer gegenüberliegende Lastenausgleich mit CLI Internet

In diesem Handbuch wird gezeigt, wie eine Internet Lastenausgleich basierend auf dem oben genannten Szenario zu erstellen.

1. Wenn Sie noch keine Erfahrung Azure CLI haben, finden Sie unter [Installieren und Konfigurieren der CLI Azure](../../articles/xplat-cli-install.md) , und folgen Sie den Anweisungen auf den Punkt, in dem Sie Ihre Azure-Konto und Ihr Abonnement auswählen.

2. Führen Sie den Befehl **Azure Config Modus** , um zur klassischen Ansicht wechseln aus, wie unten dargestellt.

        azure config mode asm

    Erwartetes Ergebnis:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Erstellen von Endpunkt und Laden Sie Lastenausgleich festlegen

Das Szenario setzt voraus, die virtuellen Computern "web1" und "web2" erstellt wurden.
Mit diesem Leitfaden wird einen laden Lastenausgleich mithilfe von Port 80 als öffentliche Port und Port 80 als lokale Port erstellen können. Ein Prüfpunkt Port ist auch auf Port 80 konfiguriert und mit dem Namen der laden Lastenausgleich festlegen "Lbset".


### <a name="step-1"></a>Schritt 1

Erstellen Sie den ersten Endpunkt und Lastenausgleich set mithilfe von `azure network vm endpoint create` virtuellen Computers "web1".

    azure vm endpoint create web1 80 -k 80 -o tcp -t 80 -b lbset

Parameter verwendet:

**-k** - Anschluss lokalen virtuellen Computern<br>
**-o** - Protokoll<BR>
**-t** - Prüfpunkt port<BR>
**-b** – laden Lastenausgleich Namen<BR>

## <a name="step-2"></a>Schritt 2

Hinzufügen einer zweiten virtuellen Computern "web2" zum Laden Lastenausgleich Satz.

    azure vm endpoint create web2 80 -k 80 -o tcp -t 80 -b lbset

## <a name="step-3"></a>Schritt 3

Überprüfen, ob die Konfiguration mit laden Lastenausgleich `azure vm show` .

    azure vm show web1

Die Ausgabe werden:

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Erstellen Sie einen remote desktop-Endpunkt für eine virtuellen Computern

Sie können einen remote desktop-Endpunkt zum Weiterleiten von Netzwerkdatenverkehr von einem öffentlichen Port an einen lokalen Anschluss zur Verwendung von einer bestimmten virtuellen Computern erstellen `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Entfernen von virtuellen Computern von Lastenausgleich

Sie müssen den Endpunkt zugeordnet ist, an den Lastenausgleich Festlegen von des virtuellen Computers zu löschen. Nachdem Sie der Endpunkt entfernt wird, gehört nicht des virtuellen Computers zu Lastenausgleich nicht mehr festlegen.

 Verwenden das oben genannten Beispiel, können Sie den Endpunkt für virtuellen Computern "web1" erstellt aus Lastenausgleich "Lbset" mit dem Befehl Entfernen `azure vm endpoint delete`.

    azure vm endpoint delete web1 tcp-80-80


>[AZURE.NOTE] Sie können weitere Optionen zum Verwalten von Endpunkten mithilfe des Befehls durchsuchen.`azure vm endpoint --help`


## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte zum Konfigurieren einer internen Lastenausgleich](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurieren eines laden Lastenausgleich Verteilung Modus](load-balancer-distribution-mode.md)

[Konfigurieren von Einstellungen zur im Leerlauf TCP Timeout für Ihre Lastenausgleich](load-balancer-tcp-idle-timeout.md)

