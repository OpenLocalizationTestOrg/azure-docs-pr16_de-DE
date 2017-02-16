<properties
   pageTitle="Multi NIC virtuellen Computern mithilfe der Azure CLI im Bereitstellungsmodell klassischen bereitstellen | Microsoft Azure"
   description="Weitere Informationen zum Bereitstellen von Multi NIC virtuellen Computern mithilfe der Azure CLI im Bereitstellungsmodell klassischen"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-classic-using-the-azure-cli"></a>Bereitstellen von Multi NIC virtuellen Computern (klassisch) verwenden die CLI Azure

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Sie können Erstellen von virtuellen Computern (virtuellen Computern) in Azure und mehrere Netzwerkschnittstellen (NICs) zu den einzelnen Ihrer virtuellen Computer anfügen. Mehrere NICs aktivieren Trennung der Datenverkehrstypen über NICs an. Beispielsweise möglicherweise eine NIC mit dem Internet, kommunizieren, während eine andere nur in Verbindung mit internen Ressourcen, die nicht mit dem Internet verbunden kommuniziert. Die Möglichkeit zum Trennen von Netzwerkdatenverkehr über mehrere NICs ist für viele virtuelle Netzwerkgeräte, wie z. B. Anwendung Übermittlungs- und WAN-Optimierung Lösungen erforderlich.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [Führen Sie diese Schritte aus, die mithilfe des Modells Ressourcenmanager](virtual-network-deploy-multinic-arm-cli.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Derzeit haben nicht virtueller Computer mit einen einzelnen Netzwerkadapter und virtueller Computer mit mehreren NICs im gleichen Cloud-Dienst. Daher müssen Sie die Back-End-Server in einem anderen Cloud-Dienst als und alle anderen Komponenten, die im Szenario implementieren. Verwenden Sie die folgenden Schritte aus einen Cloud-Dienst mit dem Namen *IaaSStory* für das Hauptfenster Ressourcen und *IaaSStory-Back-End* für die Back-End-Server aus.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie die Back-End-Server bereitstellen können, müssen Sie Hauptfenster Cloud-Dienst für alle Ressourcen für dieses Szenario bereitstellen. Sie benötigen mindestens zum Erstellen eines virtuellen Netzwerks mit einem Subnetz für die Back-End. Finden Sie unter [Erstellen eines virtuellen Netzwerks mithilfe der CLI Azure](virtual-networks-create-vnet-classic-cli.md) erfahren Sie, wie Sie ein virtuelles Netzwerk bereitstellen.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Bereitstellen der Back-End-virtuellen Computern

Die Back-End-virtuellen Computern abhängig von der Erstellung der unten aufgeführten Ressourcen.

- **Speicher-Konto für Datenfestplatten**. Für eine bessere Leistung werden Festplatten mit den Daten auf dem Datenbankserver einfarbige State Drive (SSD)-Technologie, verwenden, die eine Premium Speicher-Konto erforderlich. Vergewissern Sie sich den Azure Speicherort, die Sie zur Unterstützung von Premium Speicher bereitstellen.
- **NICs**. Jeder virtueller Computer haben die beiden Karten eine Datenbank zugreifen, und eine für die Verwaltung.
- **Verfügbarkeit festzulegen**. Alle Datenbankserver werden Verfügbarkeit einer einzelnen festlegen, um sicherzustellen, dass mindestens eines der virtuellen Computern aktiv ist und ausgeführt werden, während die Wartung hinzugefügt werden.

### <a name="step-1---start-your-script"></a>Schritt 1 – starten Sie das Skript

Sie können das vollständige Bash-Skript zum Herunterladen [können](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh). Folgen Sie den Schritten unter So ändern Sie das Skript in Ihrer Umgebung zu arbeiten.

1. Ändern Sie die Werte der Variablen unten basierend auf Ihrer vorhandenen Ressourcengruppe in [Vorkenntnisse](#Prerequisites)über bereitgestellt.

        location="useast2"
        vnetName="WTestVNet"
        backendSubnetName="BackEnd"

2. Ändern Sie die Werte der Variablen unten auf der Grundlage der Werte, die Sie für die Back-End-Bereitstellung verwenden möchten.

        backendCSName="IaaSStory-Backend"
        prmStorageAccountName="iaasstoryprmstorage"
        image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
        avSetName="ASDB"
        vmSize="Standard_DS3"
        diskSize=127
        vmNamePrefix="DB"
        osDiskName="osdiskdb"
        dataDiskPrefix="db"
        dataDiskName="datadisk"
        ipAddressPrefix="192.168.2."
        username='adminuser'
        password='adminP@ssw0rd'
        numberOfVMs=2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Schritt 2: Erstellen der erforderlichen Ressourcen für Ihre virtuellen Computer

1. Erstellen eines neuen Cloud-Diensts für alle Back-End-virtuellen Computern an. Beachten Sie die Verwendung der `$backendCSName` Variable für den Namen der Ressource Gruppe, und `$location` die Azure Region.

        azure service create --serviceName $backendCSName \
            --location $location

2. Erstellen Sie ein Premium-Speicher-Konto für den OS und Daten Laufwerke von ähnliches virtuellen Computern verwendet werden soll.

        azure storage account create $prmStorageAccountName \
            --location $location \
            --type PLRS

### <a name="step-3---create-vms-with-multiple-nics"></a>Schritt 3 – Erstellen von virtuellen Computern mit mehreren NICs

1. Starten eine Schleife zum Erstellen von mehreren virtuellen Computern, auf der Grundlage der `numberOfVMs` Variablen.

        for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
        do

2. Geben Sie für jeden virtuellen Computer den Namen und die IP-Adresse der einzelnen die beiden Karten aus.

            nic1Name=$vmNamePrefix$suffixNumber-DA
            x=$((suffixNumber+3))
            ipAddress1=$ipAddressPrefix$x

            nic2Name=$vmNamePrefix$suffixNumber-RA
            x=$((suffixNumber+53))
            ipAddress2=$ipAddressPrefix$x

4. Erstellen Sie den virtuellen Computer an. Beachten Sie die Verwendung von der `--nic-config` Parameter, die mit einer Liste aller NICs mit Name, Subnetz und IP-Adresse.

            azure vm create $backendCSName $image $username $password \
                --connect $backendCSName \
                --vm-name $vmNamePrefix$suffixNumber \
                --vm-size $vmSize \
                --availability-set $avSetName \
                --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
                --virtual-network-name $vnetName \
                --subnet-names $backendSubnetName \
                --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::

5. Erstellen Sie für jeden virtuellen Computer zwei Daten Datenträger aus.

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
        done

### <a name="step-4---run-the-script"></a>Schritt 4 – Führen Sie das Skript

Jetzt, da Sie heruntergeladen und das Skript entsprechend Ihren Anforderungen geändert haben, führen Sie das Skript zum Back-End Datenbank virtuellen Computern mit mehreren NICs erstellen.

1. Speichern Sie Ihre Skript, und führen Sie es von Ihrem **Bash** Terminal. Die Ausgabe die anfängliche, sehen, wie unten dargestellt.

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. Nach ein paar Minuten die Ausführung beenden und den Rest der Ausgabe wie unten dargestellt werden angezeigt.

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
