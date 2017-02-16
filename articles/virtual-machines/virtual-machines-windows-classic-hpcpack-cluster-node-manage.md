<properties
 pageTitle="Verwalten von HPC Pack Cluster berechnen Knoten | Microsoft Azure"
 description="Erfahren Sie mehr über PowerShell-Skript-Tools zum Hinzufügen, entfernen, starten und Beenden von HPC Pack Cluster Datenverarbeitungsknoten in Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Verwalten Sie die Anzahl und die Verfügbarkeit von Datenverarbeitungsknoten in einer HPC Pack Cluster in Azure

Wenn Sie einen HPC Pack Cluster Azure-virtuellen Computern erstellt haben, sollten Sie die Methoden zum problemlos hinzufügen, entfernen, (bereitstellen) beginnen oder beenden (entziehen) eine Reihe von berechnen Knoten virtuellen Computern im Cluster. Führen Sie zum Ausführen dieser Aufgaben Azure PowerShell-Skripts, die auf dem am Knoten virtuellen Computer installiert werden. Diese Skripts können Sie kontrollieren, die Anzahl und die Verfügbarkeit von HPC Pack Clusterressourcen, damit Sie Kosten steuern können.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Erforderliche Komponenten

* **HPC Pack Cluster in Azure-virtuellen Computern** – erstellen einen HPC Pack Cluster im Bereitstellungsmodell klassischen mit mindestens HPC Pack 2012 R2 Update 1. Beispielsweise können Sie die Bereitstellung mithilfe der aktuellen HPC Pack virtueller Computer Bilds in dem Azure Marketplace und einer Azure PowerShell-Skript automatisieren. Informationen und Komponenten finden Sie unter [Erstellen einer HPC Cluster mit der HPC Pack IaaS Bereitstellungsskript vor](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

    Nach der Bereitstellung, suchen Sie den Knoten Management Skripts in den Feldern % CCP\_Start % Bin-Ordner auf dem am Knoten. Sie müssen die beiden Skripts als Administrator ausführen.

* **Azure veröffentlichen Einstellungen Datei oder Management Zertifikat** – Sie müssen eine der folgenden Optionen auf den Knoten am:

    * **Importieren der Azure-Einstellungsdatei zu veröffentlichen**. Führen Sie hierzu die folgenden Azure PowerShell-Cmdlets auf dem am Knoten ein:

    ```
    Get-AzurePublishSettingsFile

    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```

    * **Konfigurieren des Zertifikats Azure Management auf dem am Knoten**. Wenn Sie die CER-Datei haben, importieren Sie es im CurrentUser\My Zertifikat Store, und führen Sie das folgende Azure PowerShell-Cmdlet für Ihre Azure-Umgebung (AzureCloud oder AzureChinaCloud):

    ```
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>Hinzufügen von Computeknoten virtuellen Computern

Knoten mit dem **Hinzufügen-HpcIaaSNode.ps1** Skript hinzufügen zu berechnen.

### <a name="syntax"></a>Syntax
```
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Parameter

* **ServiceName** - Name der Cloud-service diesen Knoten berechnen, die virtuellen Computern hinzugefügt werden soll.

* **ImageName** - Bildnamen Azure-virtuellen Computer, die über die Azure klassischen Portal oder Azure PowerShell-Cmdlet **Get-AzureVMImage**abgerufen werden kann. Das Bild muss erfüllen:

    1. Windows-Betriebssystems muss installiert sein.

    2. HPC Pack muss in die Rolle des berechnen Knoten installiert sein.

    3. Das Bild muss ein privates Bild in der Kategorie Benutzer nicht das Bild einer öffentlichen Azure-virtuellen Computer an.

* **Menge** - Anzahl berechnen Knoten virtuellen Computern hinzugefügt werden soll.

* **InstanceSize** - Größe des virtuellen Computern berechnen Knotens.

* **DomainUserName** - Domäne Benutzername, der verwendet wird, um den neuen virtuellen Computern in der Domäne hinzufügen.

* **DomainUserPassword** - Kennwort des Domänenbenutzers.

* **NodeNameSeries** (optional) – benennen Muster für die Knoten berechnen. Das Format muss &lt; *Root\_Namen*&gt;&lt;*Starten\_Zahl*&gt;%. Beispielsweise den Namen MyCN % 10 % bedeutet, dass eine Reihe von den Berechnungsknoten von MyCN11 ab. Wenn nicht angegeben, wird das Skript den konfigurierten Knoten benennen die Reihe im HPC-Cluster verwendet.

### <a name="example"></a>Beispiel

Im folgende Beispiel fügt 20 Größe großen berechnen Knoten virtuellen Computern in der Cloud-Dienst *hpcservice1*, basierend auf den virtuellen Computer Bild *hpccnimage1*hinzu.

```
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>Entfernen berechnen Knoten virtuellen Computern

Entfernen Knoten mit dem **Entfernen-HpcIaaSNode.ps1** Skript zu berechnen.

### <a name="syntax"></a>Syntax

```
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Parameter

* **Name** - Namen Clusterknoten entfernt werden. Platzhalter werden unterstützt. Der Namen des Parameters ist Name. Sie können keine den **Namen** und den **Knoten** Parameter angeben.

* **Knoten** - HpcNode das Objekt für die Knoten entfernt werden, die über das HPC PowerShell-Cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)abgerufen werden können. Der Namen des Parameters ist Knoten. Sie können keine den **Namen** und den **Knoten** Parameter angeben.

* **DeleteVHD** (optional) - Einstellung die zugehörigen Datenträger für die virtuellen Computern löschen, die entfernt werden.

* **Erzwingen, dass** (optional) - Einstellung HPC Knoten offline erzwingen, bevor Sie diese entfernen.

* **Bestätigen** (optional) – vor dem Ausführen des Befehls zur Bestätigung auffordern.

* **WhatIf** - Einstellung beschreiben, was passiert, wenn der Befehl ausgeführt wird, ohne den Befehl tatsächlich ausführen.

### <a name="example"></a>Beispiel

Im folgende Beispiel erzwingt offline Knoten mit Namen Anfang *HPCNode-CN -* und können die Knoten und deren zugeordneten Datenträger entfernt.

```
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Starten Sie berechnen Knoten virtuellen Computern

Start Knoten mit dem **Start-HpcIaaSNode.ps1** Skript zu berechnen.

### <a name="syntax"></a>Syntax

```
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Parameter

* **Name** - Namen der Clusterknoten gestartet werden. Platzhalter werden unterstützt. Der Namen des Parameters ist Name. Sie können keine den **Namen** und den **Knoten** Parameter angeben.

* **Knoten**- HpcNode das Objekt für die Knoten gestartet werden, die über das HPC PowerShell-Cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)abgerufen werden können. Der Namen des Parameters ist Knoten. Sie können keine den **Namen** und den **Knoten** Parameter angeben.

### <a name="example"></a>Beispiel

Im folgende Beispiel beginnt Knoten mit Namen Anfang *HPCNode-CN -*.

```
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>Beenden der berechnen Knoten virtuellen Computern

Beenden berechnen Knoten mit dem Skript **HpcIaaSNode.ps1 beenden** .

### <a name="syntax"></a>Syntax

```
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Parameter


* **Name**- Namen der Clusterknoten beendet werden soll. Platzhalter werden unterstützt. Der Namen des Parameters ist Name. Sie können keine den **Namen** und den **Knoten** Parameter angeben.

* **Knoten** - HpcNode das Objekt für die Knoten beendet werden, die über das HPC PowerShell-Cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)abgerufen werden können. Der Namen des Parameters ist Knoten. Sie können keine den **Namen** und den **Knoten** Parameter angeben.

* **Erzwingen, dass** (optional) - Einstellung HPC Knoten offline erzwingen, bevor Sie sie zu beenden.

### <a name="example"></a>Beispiel

Im folgende Beispiel erzwingt offline Knoten mit Namen Anfang *HPCNode-CN -* und hält dann an die Knoten.

Beenden-HPCIaaSNode.ps1 – Namen HPCNodeCN-*-Force

## <a name="next-steps"></a>Nächste Schritte

* Wenn eine Möglichkeit, die automatisch vergrößert oder verkleinert wird die Cluster-Knoten entsprechend der aktuellen Arbeitsbelastung von Aufträgen und Aufgaben im Cluster werden soll, finden Sie unter [automatisch vergrößern und verkleinern Sie die HPC Pack Clusterressourcen in Azure gemäß der Arbeitsbelastung Cluster](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).
