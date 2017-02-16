<properties
    pageTitle="Erfassen Sie ein Bild virtuellen Computer aus GRG Azure-virtuellen Computer | Microsoft Azure"
    description="Erfahren Sie, wie ein Bild virtuellen Computer aus in das Modell zur Bereitstellung von Ressourcenmanager erstellt einen GRG Azure-virtuellen erfassen"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>

# <a name="how-to-capture-a-vm-image-from-a-generalized-azure-vm"></a>Wie Sie ein Bild virtueller Computer eines GRG Azure-virtuellen Computers zu erfassen


In diesem Artikel wird veranschaulicht, wie Azure PowerShell verwenden, um ein Bild eines GRG Azure-virtuellen Computers zu erstellen. Das Bild können dann einen anderen virtuellen Computer erstellen. Das Bild enthält der Datenträger OS und Festplatten mit den Daten, die den virtuellen Computern zugeordnet sind. Das Bild umfasst nicht die Netzwerkressourcen virtuelle, sodass Sie diese Ressourcen einrichten, wenn Sie den neuen virtuellen Computer erstellen müssen. 


## <a name="prerequisites"></a>Erforderliche Komponenten

- Sie müssen bereits [GRG-den virtuellen Computer](virtual-machines-windows-generalize-vhd.md)haben. Verallgemeinern eines virtuellen Computers entfernt alle Ihre persönlichen Kontoinformationen, unter anderem und vorbereitet des Computers als ein Bild verwendet werden soll.

- Benötigen Sie Azure PowerShell Version 1.0.x oder höher installiert sein. Wenn Sie PowerShell bereits installiert haben, lesen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Installationsschritte weiter.


## <a name="log-in-to-azure-powershell"></a>Melden Sie sich bei Azure PowerShell

1. Öffnen Sie Azure PowerShell zu, und melden Sie sich bei Ihrem Konto Azure.

    ```powershell
    Login-AzureRmAccount
    ```

    Ein Popupfenster wird geöffnet, dienen soll, geben Sie Ihre Kontoanmeldeinformationen ein Azure.

2. Holen Sie sich das Abonnement IDs für Ihre Abonnements zur Verfügung.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Legen Sie das richtige Abonnement mit der Abonnement-ID.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>Freigeben Sie den virtuellen Computer, und legen Sie den Status zu GRG       

1. Freigeben der Ressourcen virtueller Computer an.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```

    Der *Status* für den virtuellen Computer im Azure-Portal von **weiterspielen** auf **beendet (freigegeben)**geändert wird.

2. Legen Sie den Status des virtuellen Computers auf **Generalized**ein. 

    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```

3. Überprüfen des Status der virtuellen Computer an. Im Abschnitt **OSState/GRG** für den virtuellen Computer sollte der **DisplayStatus** auf **virtuellen Computer GRG**festgelegt haben.  

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>Erstellen Sie das Bild 

1. Kopieren Sie das Bild des virtuellen Computers zum Ziel Speichercontainer mit dem folgenden Befehl ein. Das Bild wird in demselben Speicherkonto wie dem ursprünglichen virtuellen Computer erstellt. Die `-Path` Parameter speichert eine Kopie der Vorlage JSON lokal. Die `-DestinationContainerName` Parameter ist der Name des Containers, die Sie Ihre Bilder halten möchten. Wenn der Container nicht vorhanden ist, ist es für Sie erstellt.

    ```powershell
    Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
        -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
        -Path <C:\local\Filepath\Filename.json>
    ```

    Sie können die URL des Bilds aus der JSON-Dateivorlage erhalten. Wechseln Sie zu den **Ressourcen** > **StorageProfile** > **OsDisk** > **Bild** > **Uri** -Abschnitt für den vollständigen Pfad des Bilds. Die URL des Bilds sieht wie folgt aus: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
    
    Sie können auch den URI im Portal überprüfen. Das Bild wird zu einem Container mit dem Namen **System** in Ihr Speicherkonto kopiert. 


## <a name="next-steps"></a>Nächste Schritte

- Jetzt können Sie [einen virtuellen Computer aus dem Bild zu erstellen](virtual-machines-windows-create-vm-generalized.md).

