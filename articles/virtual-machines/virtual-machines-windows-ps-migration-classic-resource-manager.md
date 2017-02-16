<properties
    pageTitle="Migrieren zu Ressourcenmanager mit PowerShell | Microsoft Azure"
    description="In diesem Artikel durchläuft der Migration Plattform unterstützt IaaS Ressourcen aus klassischen Ressourcenmanager zu Azure mithilfe von Azure PowerShell-Befehlen"
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
    ms.date="10/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Migrieren Sie IaaS Ressourcen von klassischen Ressourcenmanager zu Azure mithilfe von Azure PowerShell

Diese Schritte zeigen, wie mit Azure PowerShell-Befehlen Infrastruktur als eine Service (IaaS) Ressourcen aus dem Bereitstellungsmodell klassischen zum Bereitstellungsmodell Azure Ressourcenmanager migriert. 

Wenn Sie möchten, können Sie auch Ressourcen mithilfe der [Azure Befehlszeilenschnittstelle (Azure)](virtual-machines-linux-cli-migration-classic-resource-manager.md)migrieren.

- Weitere Hintergrundinformationen zur unterstützten Migrationsszenarien finden Sie unter [Plattform unterstützt Migration IaaS Ressourcen aus klassischen zu Azure Ressourcenmanager](virtual-machines-windows-migration-classic-resource-manager.md). 
- Ausführliche Leitfäden und eine exemplarische Vorgehensweise zur Migration finden Sie unter [technischen detaillierte technische Informationen zur Migration von klassischen zu Azure Ressourcenmanager Plattform unterstützt](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md).

## <a name="step-1-plan-for-migration"></a>Schritt 1: Planen Sie für die migration

Hier sind einige bewährte Methoden, die es empfiehlt sich, wie Sie migrieren von IaaS Ressourcen aus klassischen zu Ressourcenmanager ausgewertet werden soll:

- Lesen Sie [unterstützte und nicht unterstützte Features und Konfigurationen](virtual-machines-windows-migration-classic-resource-manager.md). Wenn Sie virtuellen Computern, die nicht unterstützte Konfigurationen oder Features verwenden haben, wird empfohlen, dass Sie warten, bis die Unterstützung von Konfiguration/Features fest. Alternativ, wenn sie Ihren Bedürfnissen entspricht, entfernen Sie dieses Feature, oder Verschieben von dieser Konfiguration Migration zu aktivieren.
-   Wenn Sie Skripts, die Ihre Infrastruktur und Applikationen heute bereitstellen automatisch haben, versuchen Sie es so erstellen Sie ein ähnliche Test-Setup mithilfe dieser Skripts für die Migration. Alternativ können Sie Stichprobe Umgebungen einrichten, mithilfe des Azure-Portals.

>[AZURE.IMPORTANT]Virtuelle Netzwerkgateways (-Standorten, Azure ExpressRoute, Anwendungsgateway, Punkt-zu-Standort) werden für die Migration von Classic zu Ressourcenmanager derzeit nicht unterstützt. Um ein klassischen virtuelles Netzwerk mit einem Gateway zu migrieren, entfernen Sie das Gateway vor dem Ausführen eines Vorgangs Commit ausführen, um das Netzwerk verschieben (Sie können den vorbereiten Schritt ausführen, ohne das Gateway zu löschen). Schließen Sie nach Abschluss die Migration des Gateways in Azure Ressourcenmanager wieder.

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>Schritt 2: Installieren der neuesten Version von Azure PowerShell

Es gibt zwei grundlegende Optionen Azure PowerShell installieren: [PowerShell-Katalog](https://www.powershellgallery.com/profiles/azure-sdk/) oder [Web Platform Installer (WebPI)](http://aka.ms/webpi-azps). WebPI empfängt monatliche Aktualisierungen an. PowerShell-Katalog empfängt permanenten Updates. In diesem Artikel basiert auf Azure PowerShell Version 2.1.0.

Installation Anweisungen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).

<br>
## <a name="step-3-set-your-subscription-and-sign-up-for-migration"></a>Schritt 3: Einrichten Sie Ihres Abonnements, und melden Sie sich bei der migration

Starten Sie eine Aufforderung PowerShell. Für die Migration, müssen Sie zum Einrichten der Umgebung für beide klassischen und Ressourcen-Manager.

Melden Sie sich bei Ihrem Konto für das Modell Ressourcenmanager.

```powershell
    Login-AzureRmAccount
```

Erhalten Sie die verfügbaren Abonnements mit dem folgenden Befehl aus:

```powershell
    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName
```

Legen Sie Ihre Azure-Abonnement für die aktuelle Sitzung ein. In diesem Beispiel wird den Namen des Abonnements zu **Meine Azure-Abonnement**. Ersetzen Sie den Namen des Abonnements Beispiel durch eigene. 

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

>[AZURE.NOTE] Die Registrierung ist eine einmalige Schritt müssen, doch werden es einmal, bevor Sie versuchen, die Migration. Ohne zu registrieren wird die folgende Fehlermeldung angezeigt: 

>   *BadRequest: Abonnement ist für die Migration nicht registriert.* 

Registrieren Sie sich mit der Migrations-Anbieter für Ressourcen mit den folgenden Befehl aus:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Warten Sie fünf Minuten für die Registrierung auf Fertig stellen. Sie können den Status der Genehmigung überprüfen, indem Sie mit dem folgenden Befehl:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Stellen Sie sicher, dass RegistrationState ist `Registered` bevor Sie fortfahren. 

Jetzt melden Sie bei Ihrem Konto für das klassische Modell an.

```powershell
    Add-AzureAccount
```

Erhalten Sie die verfügbaren Abonnements mit dem folgenden Befehl aus:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Legen Sie Ihre Azure-Abonnement für die aktuelle Sitzung ein. In diesem Beispiel wird das Standardabonnement zu **Meine Azure-Abonnement**. Ersetzen Sie den Namen des Abonnements Beispiel durch eigene. 

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-4-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Schritt 4: Stellen Sie sicher, dass Sie der Azure Region Ihrer aktuellen Bereitstellung oder VNET genügend Kerne Azure Ressourcenmanager virtuellen Computern haben

Den folgenden PowerShell-Befehl können die aktuelle Anzahl der Kerne zu überprüfen, die Sie in Azure Ressourcenmanager haben. Weitere Informationen zum Core Kontingenten finden Sie unter [Grenzwerte und Ressourcenmanager Azure](../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager). 

In diesem Beispiel überprüft die Verfügbarkeit in der Region **Westen US** . Ersetzen Sie den Namen des Beispiel Region durch ein eigenes ein. 

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-5-run-commands-to-migrate-your-iaas-resources"></a>Schritt 5: Ausführen von Befehlen zur IaaS Ressourcen migrieren

>[AZURE.NOTE] Alle Vorgänge, die hier beschriebenen sind Idempotent. Wenn Sie ein Problem als ein nicht unterstütztes Feature oder einem Konfigurationsfehler verfügen, es empfiehlt sich, dass Sie zur Vorbereitung, wiederholen Sie Abbrechen oder Vorgang abzuschließen. Die Plattform versucht dann erneut die Aktion aus.

### <a name="migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Migrieren von virtuellen Maschinen in einen Cloud-Dienst (nicht in einem virtuellen Netzwerk)

Abrufen der Liste der Cloud-Diensten mit den folgenden Befehl aus, und wählen Sie dann in des Cloud-Diensts, den Sie migrieren möchten. Wenn Sie die virtuellen Computern in der Cloud-Dienst in einem virtuellen Netzwerk befinden, oder wenn sie Web oder Arbeitskollegen Rollen haben, gibt der Befehl eine Fehlermeldung angezeigt.

```powershell
    Get-AzureService | ft Servicename
```

Abrufen der Bereitstellung Name für den Clouddienst an. In diesem Beispiel ist der Name des Dienstes **Meine-Dienst**an. Ersetzen Sie den Dienst Beispielname mit Ihren eigenen Name Service ein. 

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Bereiten Sie den virtuellen Computern in der Cloud-Dienst für die Migration vor. Sie haben zwei Optionen zur Auswahl.

* **Option 1. Migrieren von den virtuellen Computern zu einem Plattform erstellten virtuellen Netzwerk**

    Überprüfen Sie zuerst, wenn Sie mit den folgenden Befehlen Cloud-Dienst migriert werden können:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    Der vorherige Befehl zeigt alle Warnungen und Fehlern, die Migration blockieren. Wenn die Überprüfung erfolgreich ist, können Sie zu der Seite **Vorbereiten** auf verschieben:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```

* **Option 2. Migrieren Sie zu einem vorhandenen virtuellen Netzwerk im Bereitstellungsmodell Ressourcenmanager**

    In diesem Beispiel wird die Gruppe Ressourcenname **MyResourceGroup**, den Namen des virtuellen Netzwerks zu **MyVirtualNetwork** und den Subnetnamen zu **MySubNet**. Ersetzen Sie die Namen im Beispiel mit den Namen Ihrer eigenen Ressourcen.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Überprüfen Sie zuerst, wenn Sie mit dem folgenden Befehl Cloud-Dienst migriert werden können:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    Der vorherige Befehl zeigt alle Warnungen und Fehlern, die Migration blockieren. Wenn die Überprüfung erfolgreich ist, können Sie mit den folgenden vorbereiten Schritt fortfahren:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```
    

Nachdem der Vorgang vorbereiten mit einer der vorangehenden Optionen hergestellt wurde, wird der Migrationsstatus der virtuellen Computern abgefragt. Stellen Sie sicher, dass sie in sind die `Prepared` Zustand.

In diesem Beispiel wird der Name des virtuellen Computers zu **"MyVM"**. Ersetzen Sie den Beispielnamen mit Ihrer eigenen Name des virtuellen Computers.

    ```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
    ```

Überprüfen Sie die Konfiguration für die Ressourcen vorbereiteten mit PowerShell oder der Azure-Portal an. Wenn Sie nicht für die Migration noch – und Sie in der alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl aus:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Wenn die vorbereitete Konfiguration zusagt, können Sie voranzutreiben und die Ressourcen mithilfe des folgenden Befehls abzuschließen:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

### <a name="migrate-virtual-machines-in-a-virtual-network"></a>Migrieren von virtuellen Computern in einem Netzwerk virtuelle

Zum Migrieren von virtuellen Computern in einem Netzwerk virtuelle migrieren Sie das Netzwerk aus. Die virtuellen Computer werden automatisch mit dem Netzwerk migrieren. Wählen Sie das virtuelle Netzwerk aus, das Sie migrieren möchten. 

In diesem Beispiel wird den virtuellen Netzwerknamen zu **MyVnet**an. Ersetzen Sie den virtuelle Netzwerknamen Beispiel mit Ihrer eigenen. 

```powershell
    $vnetName = "myVnet"
```

>[AZURE.NOTE] Wenn das virtuelle Netzwerk Web Worker-Rollen oder virtuellen Computern mit nicht unterstützten Konfigurationen enthält, erhalten Sie eine Fehlermeldung bei Überprüfung an.

Überprüfen Sie zuerst, wenn Sie das virtuelle Netzwerk migrieren, indem Sie mit dem folgenden Befehl:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

Der vorherige Befehl zeigt alle Warnungen und Fehlern, die Migration blockieren. Wenn die Überprüfung erfolgreich ist, können Sie mit den folgenden vorbereiten Schritt fortfahren:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Überprüfen Sie die Konfiguration für den vorbereiteter virtuellen Computern mit Azure PowerShell oder der Azure-Portal an. Wenn Sie nicht für die Migration noch – und Sie in der alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl aus:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Wenn die vorbereitete Konfiguration zusagt, können Sie voranzutreiben und die Ressourcen mithilfe des folgenden Befehls abzuschließen:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

### <a name="migrate-a-storage-account"></a>Migrieren einer Speicher-Konto

Sobald Sie fertig sind migrieren den virtuellen Computern, wir empfehlen Sie die Speicherkonten migrieren.

Bereiten Sie jedes Storage-Konto für die Migration mit dem folgenden Befehl ein. In diesem Beispiel wird der Kontonamen Speicher **MyStorageAccount**. Ersetzen Sie den Beispielnamen, mit dem Namen Ihres eigenen Speicher-Kontos. 

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Überprüfen Sie die Konfiguration für das Speicherkonto kann begonnen mit Azure PowerShell oder der Azure-Portal an. Wenn Sie nicht für die Migration noch – und Sie in der alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl aus:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Wenn die vorbereitete Konfiguration zusagt, können Sie voranzutreiben und die Ressourcen mithilfe des folgenden Befehls abzuschließen:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Migrieren finden Sie unter [Migration IaaS Ressourcen aus zu Azure Ressourcenmanager klassischen Plattform unterstützt](virtual-machines-windows-migration-classic-resource-manager.md).
- Verwenden Sie zum Migrieren von zusätzliche Netzwerk-Ressourcen zu Ressourcenmanager mithilfe der PowerShell Azure ähnliche Schritte mit [Verschieben-AzureNetworkSecurityGroup](https://msdn.microsoft.com/library/mt786729.aspx), [Verschieben-AzureReservedIP](https://msdn.microsoft.com/library/mt786752.aspx)und [Verschieben-AzureRouteTable](https://msdn.microsoft.com/library/mt786718.aspx)ein.
- Open Source-Skripts, die Sie verwenden können, um Azure Ressourcen klassischen zu Ressourcenmanager migrieren, finden Sie unter [Community-Tools für die Migration zu Azure Ressourcenmanager migration](virtual-machines-windows-migration-scripts.md)
