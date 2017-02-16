<properties 
   pageTitle="So legen Sie einer statischen privaten IP-Adresse im klassischen Modus Ausing CLI | Microsoft Azure"
   description="Grundlegendes zu statischen privaten IP-Adressen (DIPs) und wie sie in der klassischen Ansicht mit der CLI verwaltet"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-azure-cli"></a>Wie eine statische private IP-Adresse (klassische) in Azure CLI festgelegt.

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell klassischen. Sie können auch [eine statische private IP-Adresse in das Modell zur Bereitstellung von Ressourcenmanager verwalten](virtual-networks-static-private-ip-arm-cli.md).

Die folgenden Beispiel Azure CLI Befehle erwarten eine einfache-Umgebung, die bereits erstellt haben. Wenn die Befehle ausführen, wie sie in diesem Dokument angezeigt werden soll, erstellen Sie zuerst der testumgebung beschrieben, die in [einer Vnet erstellen](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>So geben Sie beim Erstellen eines virtuellen Computers eine statische private IP-Adresse
Gehen Sie folgendermaßen vor, um einen neuen virtuellen Computer mit dem Namen *DNS01* in einer neuen Cloud-Dienst mit dem Namen *TestService* basierend auf dem oben genannten Szenario zu erstellen:

1. Wenn Sie noch keine Erfahrung Azure CLI haben, finden Sie unter [Installieren und Konfigurieren der CLI Azure](../xplat-cli-install.md) , und folgen Sie den Anweisungen auf den Punkt, in dem Sie Ihre Azure-Konto und Ihr Abonnement auswählen.
1. Führen Sie den Befehl **Azure Service erstellen** , in der Clouddienst erstellen.

        azure service create TestService --location uscentral

    Erwartetes Ergebnis:

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
    
2. Führen Sie den Befehl **Azure Erstellen virtueller Computer** , um den virtuellen Computer zu erstellen. Beachten Sie den Wert für eine statische private IP-Adresse ein. Der Liste angezeigt, nachdem die Ausgabe wird, die Parameter verwendet erläutert.

        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd

    Erwartetes Ergebnis:

        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK

    - **-l (oder – Speicherort)**. Azure Region, in der virtuellen Computer erstellt werden soll. In diesem Szenario *Centralus*.
    - **-n (oder Name des – virtuellen Computers-)**. Name des den virtuellen Computer erstellt werden.
    - **-w (oder – virtuelle Netzwerknamen)**. Name der VNet, in der virtuellen Computer erstellt werden soll. 
    - **S-(oder – statische Ip-)**. Statische private IP-Adresse für den virtuellen Computer.
    - **TestService**. Name des Cloud-Dienst, in der virtuellen Computer erstellt werden soll.
    - **bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-X64-v14.2**. Bild, mit den virtuellen Computer zu erstellen.
    - **Adminuser**. Lokaler Administrator für den Windows-virtuellen Computer.
    - **AdminP@ssw0rd**. Lokale Administratorkennwort für die Windows-virtuellen Computer.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Informationen zu Vorgehensweisen abrufen statische private IP-Adresse für einen virtuellen Computer
Zum Anzeigen der statischen privaten IP-Adresse-Informationen für den virtuellen Computer mit dem oben genannten Skript erstellt führen Sie den folgenden Befehl aus Azure CLI, und achten Sie den Wert für *Netzwerk StaticIP*:

    azure vm static-ip show DNS01

Erwartetes Ergebnis:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>So entfernen Sie eine statische private IP-Adresse eines virtuellen Computers
So entfernen Sie die statische private IP-Adresse hinzugefügt den virtuellen Computer in das Skript oben, führen Sie den folgenden Befehl aus Azure CLI:
    
    azure vm static-ip remove DNS01

Erwartetes Ergebnis:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>So fügen Sie eine statische private IP-Adresse zu einem vorhandenen virtuellen Computer
Hinzufügen eine statischen privaten IP-Adresse an den virtuellen Computer erstellt haben, verwenden das Skript über Runt er den folgenden Befehl:

    azure vm static-ip set DNS01 192.168.1.101

Erwartetes Ergebnis:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a>Nächste Schritte

- Informationen Sie zu [Reservierte öffentliche IP-](virtual-networks-reserved-public-ip.md) Adressen.
- Informationen Sie zu Adressen [Instanz Ebene öffentlichen IP-(ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Wenden Sie sich an die [reservierte IP-REST-APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).
