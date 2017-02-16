<properties
   pageTitle="Multi NIC virtuellen Computern mithilfe der PowerShell im Bereitstellungsmodell klassischen bereitstellen | Microsoft Azure"
   description="Weitere Informationen zum Bereitstellen von Multi NIC virtuellen Computern mithilfe der PowerShell im Bereitstellungsmodell klassischen"
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

#<a name="deploy-multi-nic-vms-classic-using-powershell"></a>Bereitstellen von Multi NIC virtuellen Computern (klassische) mithilfe der PowerShell

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Sie können Erstellen von virtuellen Computern (virtuellen Computern) in Azure und mehrere Netzwerkschnittstellen (NICs) zu den einzelnen Ihrer virtuellen Computer anfügen. Mehrere NICs aktivieren Trennung der Datenverkehrstypen über NICs an. Beispielsweise möglicherweise eine NIC mit dem Internet, kommunizieren, während eine andere nur in Verbindung mit internen Ressourcen, die nicht mit dem Internet verbunden kommuniziert. Die Möglichkeit zum Trennen von Netzwerkdatenverkehr über mehrere NICs ist für viele virtuelle Netzwerkgeräte, wie z. B. Anwendung Übermittlungs- und WAN-Optimierung Lösungen erforderlich.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [Führen Sie diese Schritte aus, die mithilfe des Modells Ressourcenmanager](virtual-network-deploy-multinic-arm-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Derzeit haben nicht virtueller Computer mit einen einzelnen Netzwerkadapter und virtueller Computer mit mehreren NICs im gleichen Cloud-Dienst. Daher müssen Sie die Back-End-Server in einem anderen Cloud-Dienst als und alle anderen Komponenten, die im Szenario implementieren. Verwenden Sie die folgenden Schritte aus einen Cloud-Dienst mit dem Namen *IaaSStory* für das Hauptfenster Ressourcen und *IaaSStory-Back-End* für die Back-End-Server aus.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie die Back-End-Server bereitstellen können, müssen Sie Hauptfenster Cloud-Dienst für alle Ressourcen für dieses Szenario bereitstellen. Sie benötigen mindestens zum Erstellen eines virtuellen Netzwerks mit einem Subnetz für die Back-End. Finden Sie unter [Erstellen eines virtuellen Netzwerks mithilfe der PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md) erfahren Sie, wie Sie ein virtuelles Netzwerk bereitstellen.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Bereitstellen der Back-End-virtuellen Computern

Die Back-End-virtuellen Computern abhängig von der Erstellung der unten aufgeführten Ressourcen.

- **Back-End-Subnetz gehören**. Die Datenbankserver werden in einem separaten Subnetz, Aufteilen des Datenverkehrs sein. Das folgende Skript erwartet dieses Subnetz in einer Vnet mit dem Namen *WTestVnet*vorhanden sein.
- **Speicher-Konto für Datenfestplatten**. Für eine bessere Leistung werden Festplatten mit den Daten auf dem Datenbankserver einfarbige State Drive (SSD)-Technologie, verwenden, die eine Premium Speicher-Konto erforderlich. Vergewissern Sie sich den Azure Speicherort, die Sie zur Unterstützung von Premium Speicher bereitstellen.
- **Verfügbarkeit festzulegen**. Alle Datenbankserver werden Verfügbarkeit einer einzelnen festlegen, um sicherzustellen, dass mindestens eines der virtuellen Computern aktiv ist und ausgeführt werden, während die Wartung hinzugefügt werden.

### <a name="step-1---start-your-script"></a>Schritt 1 – starten Sie das Skript

Sie können das vollständige PowerShell-Skript zum Herunterladen [können](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Folgen Sie den Schritten unter So ändern Sie das Skript in Ihrer Umgebung zu arbeiten.

1. Ändern Sie die Werte der Variablen unten basierend auf Ihrer vorhandenen Ressourcengruppe in [Vorkenntnisse](#Prerequisites)über bereitgestellt.

        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"

2. Ändern Sie die Werte der Variablen unten auf der Grundlage der Werte, die Sie für die Back-End-Bereitstellung verwenden möchten.

        $backendCSName         = "IaaSStory-Backend"
        $prmStorageAccountName = "iaasstoryprmstorage"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $diskSize              = 127
        $vmNamePrefix          = "DB"
        $dataDiskSuffix        = "datadisk"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Schritt 2: Erstellen der erforderlichen Ressourcen für Ihre virtuellen Computer

Sie müssen einen neuen Clouddienst und ein Speicherkonto für Festplatten mit den Daten für alle virtuellen Computern erstellen. Sie müssen ein Bild und ein lokales Administratorkonto für die virtuellen Computern angeben. Führen Sie die folgenden Schritte aus, um diese Ressourcen zu erstellen.

1. Erstellen eines neuen Cloud-Diensts an.

        New-AzureService -ServiceName $backendCSName -Location $location

2. Erstellen Sie ein neues Premium Speicher-Konto an.

        New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
            -Location $location `
            -Type Premium_LRS

3. Einrichten des Speicherkontos als dem aktuellen Speicherkonto für Ihr Abonnement über erstellt.

        $subscription = Get-AzureSubscription `
            | where {$_.IsCurrent -eq $true}  
        Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
            -CurrentStorageAccountName $prmStorageAccountName

4. Wählen Sie ein Bild aus, für den virtuellen Computer.

        $image = Get-AzureVMImage `
            | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
            | sort PublishedDate -Descending `
            | select -ExpandProperty ImageName -First 1

5. Festlegen des lokalen Administrators Anmeldeinformationen für das Konto an.

        $cred = Get-Credential -Message "Enter username and password for local admin account"

### <a name="step-3---create-vms"></a>Schritt 3 – Erstellen von virtuellen Computern

Sie müssen eine Schleife verwenden, um so viele virtuellen Computern, wie Sie möchten, und erstellen die erforderlichen NICs und virtuellen Computern innerhalb der Schleife zu erstellen. Führen Sie die folgenden Schritte aus, um die NICs und virtuellen Computern zu erstellen.

1. Starten einer `for` Schleife, um die Befehle zum Erstellen eines virtuellen Computers und zwei NICs so oft wie erforderlich, wiederholen basierend auf dem Wert, der die `$numberOfVMs` Variable.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Erstellen einer `VMConfig` Objekt, das angibt, die Bild-, Größe und Verfügbarkeit für den virtuellen Computer festlegen.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureVMConfig -Name $vmName `
                            -ImageName $image `
                            -InstanceSize $vmSize `
                            -AvailabilitySetName $avSetName  

3. Bereitstellen von den virtuellen Computer als Windows virtueller Computer.

            Add-AzureProvisioningConfig -VM $vmConfig -Windows `
                -AdminUsername $cred.UserName `
                -Password $cred.Password

4. Festlegen Sie der NIC, und weisen sie eine statische IP-Adresse.

            Set-AzureSubnet -SubnetNames $backendSubnetName -VM $vmConfig
            Set-AzureStaticVNetIP -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig

5. Fügen Sie einen zweiten Netzwerkadapter für jeden virtuellen Computer hinzu.

            Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
                -SubnetName $backendSubnetName `
                -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
                -VM $vmConfig

6. Erstellen Sie für jeden virtuellen Computer in Daten Datenträger.

            $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk1Name `
                -LUN 0       

            $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk2Name `
                -LUN 1

7. Erstellen Sie jeden virtuellen Computer, und Beenden der Schleife.

            New-AzureVM -VM $vmConfig `
                -ServiceName $backendCSName `
                -Location $location `
                -VNetName $vnetName
        }

### <a name="step-4---run-the-script"></a>Schritt 4 – Führen Sie das Skript

Jetzt, da Sie heruntergeladen und das Skript entsprechend Ihren Anforderungen geändert, Skripts Runt er zum Back-End Datenbank virtuellen Computern mit mehreren NICs erstellen.

1. Speichern Sie das Skript, und führen Sie sie aus der **PowerShell** -Eingabeaufforderung oder **PowerShell ISE**. Die Ausgabe die anfängliche, sehen, wie unten dargestellt.

        OperationDescription    OperationId                          OperationStatus
        --------------------    -----------                          ---------------
        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      

        WARNING: No deployment found in service: 'IaaSStory-Backend'.

2. Füllen Sie die Informationen in der Aufforderung Anmeldeinformationen erforderlich, und klicken Sie auf **OK**. Die nachstehende Ausgabe wird angezeigt.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
