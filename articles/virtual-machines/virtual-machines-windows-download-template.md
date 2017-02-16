<properties
    pageTitle="Erstellen Sie ein Bild virtueller Computer aus einer Azure-virtuellen Computer | Microsoft Azure"
    description="Informationen Sie zum Erstellen eines GRG virtueller Computer Bilds aus einer vorhandenen Azure virtueller Computer im Bereitstellungsmodell Ressourcenmanager erstellt"
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
    ms.date="10/10/2016"
    ms.author="cynthn"/>


# <a name="download-the-template-for-a-vm"></a>Laden Sie die Vorlage eines virtuellen Computers

Bei der Erstellung eines virtuellen Computers in Azure mit dem Portal oder PowerShell ist eine Vorlage Ressourcenmanager automatisch für Sie erstellt. Sie können mithilfe dieser Vorlage können Sie schnell eine Bereitstellung duplizieren. Die Vorlage enthält Informationen zu allen Ressourcen in einer Ressourcengruppe. Für einen virtuellen Computer bedeutet dies die Vorlagencontainer alles, die zur Unterstützung der virtuellen Computer in dieser Ressourcengruppe, einschließlich der Netzwerken Ressourcen erstellt wird.

## <a name="download-the-template-using-the-portal"></a>Laden Sie die Vorlage mithilfe des Portals

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie eine im Menü Hub **virtuellen Computern**.
3. Wählen Sie den virtuellen Computer aus der Liste aus.
5. Wählen Sie **Automatisierungsskript**.
6. Wählen Sie **herunterladen** , und speichern Sie die ZIP-Datei mit Ihrem lokalen Computer.
7. Öffnen Sie die ZIP-Datei, und extrahieren Sie die Dateien in einen Ordner zu. Enthält die ZIP-Datei:
    
    - Deploy.ps1
    - Deploy.sh 
    - deployer.RB
    - DeploymentHelper.cs
    - Parameters.JSON
    - Template.JSON

Die Datei .json ist die Vorlage.
    
## <a name="download-the-template-using-powershell"></a>Herunterladen der Vorlage mithilfe der PowerShell

Sie können auch die .json Vorlagendatei mithilfe des Cmdlets [Exportieren-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) herunterladen. Sie können die `-path` Parameter, um den Dateinamen und den Pfad für die Datei .json bereitzustellen. In diesem Beispiel wird gezeigt, wie die Vorlage für die Ressourcengruppe mit dem Namen **MyResourceGroup** **C:\users\public\downloads** Ordner auf Ihrem lokalen Computer herunterladen.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Bereitstellen von Ressourcen mithilfe von Vorlagen finden Sie unter [Exemplarische Vorgehensweise Ressourcenmanager-Vorlage](../resource-manager-template-walkthrough.md).