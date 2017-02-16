<properties
    pageTitle="Ändern der Größe eines klassischen Windows virtuellen Computers | Microsoft Azure"
    description="Ändern der Größe eines Windows-Computers im Bereitstellungsmodell klassischen mithilfe der Powershell Azure erstellt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>


# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a>Ändern der Größe eines Windows virtuellen Computers im Bereitstellungsmodell klassischen erstellt

In diesem Artikel wird gezeigt, wie zum Ändern der Größe eines Windows virtuellen Computers im Bereitstellungsmodell klassischen mithilfe der Powershell Azure erstellt werden können.

Wenn die Möglichkeit zum Ändern der Größe eines virtuellen Computers in Betracht ziehen, gibt es zwei Konzepte, die Steuern der Auswahl an Papiergrößen, die Größe des virtuellen Computers zur Verfügung. Das erste Konzept ist der Bereich, in dem Ihre virtuellen Computer bereitgestellt wird. Die Liste der virtuellen Computer in der Region verfügbaren Größen ist unter der Registerkarte Dienstleistungen der Azure Regionen Webseite angezeigt. Das zweite Konzept ist die physische Hardware aktuell Hostinganbieter Ihrer virtuellen Computer an. Die physische Server hosten virtuellen Computern sind in Cluster von allgemeinen physischen Hardware zusammengefasst. Die Methode zum Ändern einer virtueller Speicher unterscheidet sich je nach, wenn die gewünschte Größe der neue virtuellen Computer vom Hostinganbieter aktuell auf des virtuellen Computer Hardware Cluster unterstützt wird.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Sie können auch das [Ändern der Größe eines virtuellen Computers im Bereitstellungsmodell Ressourcenmanager erstellt](virtual-machines-windows-resize-vm.md).


## <a name="add-your-account"></a>Fügen Sie Ihres Kontos hinzu

Sie müssen Azure PowerShell für die Arbeit mit klassischen Azure Ressourcen konfigurieren. Führen Sie die folgenden Schritte zur Azure-PowerShell zum Verwalten von klassischer Ressourcen konfigurieren.

1. Geben Sie bei der entsprechenden Aufforderung PowerShell `Add-AzureAccount` , und klicken Sie auf die **EINGABETASTE**. 
2. Geben Sie die e-Mail-Adresse, die mit Ihrem Azure-Abonnement verknüpft ist, und klicken Sie auf **Weiter**. 
3. Geben Sie das Kennwort für Ihr Konto ein. 
4. Klicken Sie auf **Anmelden**. 


## <a name="resize-in-the-same-hardware-cluster"></a>Ändern der Größe in demselben Hardware cluster

Gehen Sie zum Ändern der Größe eines virtuellen Computers auf eine Größe im Cluster Hardware Hostinganbieter den virtuellen Computer zur Verfügung.

1. Führen Sie den folgenden PowerShell-Befehl zur Liste der virtuellen Computer Größen in der Cloud-Dienst, der enthält den virtuellen Computer Hostinganbieter Hardware Cluster verfügbar.

    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```

2. Führen Sie die folgenden Befehle zum Ändern der Größe des virtuellen Computer an.

    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Ändern der Größe in einem neuen Hardware cluster

Ändern der Größe ein virtuellen Computers auf eine Größe nicht verfügbar in den virtuellen Computer, der Cloud Hostinganbieter Hardware Cluster müssen Dienst und alle virtuellen Computern in der Cloud-Dienst neu erstellt werden. Jede Cloud-Dienst wird auf einem einzelnen Hardware Cluster gehostet, daher müssen alle virtuellen Computern in der Cloud-Dienst eine Größe sein, die in einem Cluster Hardware unterstützt wird. Die folgenden Schritte werden das Ändern der Größe eines virtuellen Computers durch Löschen und Neuerstellen klicken Sie dann auf den Cloud-Dienst beschreiben.

1. Führen Sie den folgenden PowerShell-Befehl in die Liste der in der Region verfügbaren virtuellen Computer-Größen. 

    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```

2. Notieren Sie sich alle Konfigurationen für die einzelnen virtuellen Computer in der Cloud-Dienst, der enthält den virtuellen Computer geändert werden kann. 
3. Löschen Sie alle virtuellen Computern in der Cloud-Dienst auswählen die Option aus, um die Datenträger für jeden virtuellen Computer beibehalten.
4. Erstellen Sie den virtuellen Computer geändert werden kann, verwenden die gewünschte Größe des virtuellen Computer neu.
5. Erstellen Sie alle anderen virtuellen Computern, in der Cloud-Dienst, der mit einer virtueller Speicher im Cluster Hardware jetzt Hostinganbieter Cloud-Dienst verfügbar waren.

Ein Beispiel-Skript zum Löschen und Neuerstellen eines Cloud-Diensts, der mit einer neuen virtueller Speicher finden Sie [hier](https://github.com/Azure/azure-vm-scripts). 


## <a name="next-steps"></a>Nächste Schritte

- [Ändern der Größe eines virtuellen Computers im Bereitstellungsmodell Ressourcenmanager erstellt](virtual-machines-windows-resize-vm.md).