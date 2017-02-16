<properties
    pageTitle="Bereitstellen von Vorlagen mit PowerShell in Azure Stapel | Microsoft Azure"
    description="Erfahren Sie, wie Sie einen virtuellen Computer mit einer Vorlage Ressourcenmanager und PowerShell bereitstellen."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Bereitstellen von Vorlagen in Azure Stapel mithilfe der PowerShell

Mithilfe von PowerShell Ressourcenmanager Azure-Vorlagen für die Azure Stapel Prüfung des Konzepts ist bereitstellen.  Ressourcenmanager Vorlagen bereitstellen und alle Ressourcen für eine Anwendung in einem einzigen, koordinierte Vorgang bereitstellen.

## <a name="run-azurerm-powershell-cmdlets"></a>Ausführen von AzureRM-PowerShell-cmdlets

In diesem Beispiel führen Sie ein Skript zum Bereitstellen eines virtuellen Computers Azure Stapel Prüfung des Konzepts ist mithilfe einer Vorlage Ressourcenmanager.  Bevor Sie fortfahren, stellen Sie sicher, dass Sie [installiert und PowerShell konfiguriert](azure-stack-connect-powershell.md) haben.  

Die virtuelle Festplatte verwendet, die in dieser Beispielvorlage ist ein Standardbild Marketplace (WindowsServer-2012-R2-Datacenter).

1.  Wechseln Sie zu <http://aka.ms/AzureStackGitHub>, suchen Sie die Vorlage **101 einfach Windows virtuellen Computer** , und speichern es an folgendem Speicherort: c:\\Vorlagen\\Azuredeploy-101-einfach-Windows-vm.json.

2.  Führen Sie das folgenden Bereitstellungsskript in PowerShell. Ersetzen Sie *Benutzername* und *Kennwort* mit Ihren Benutzernamen und Ihr Kennwort ein. Klicken Sie auf weitere Verwendungen erhöhen Sie den Wert für den Parameter *$myNum* nicht überschrieben wird die Bereitstellung aus.

    ```PowerShell
        # Set Deployment Variables
        $myNum = "001" #Modify this per deployment
        $RGName = "myRG$myNum"
        $myLocation = "local"

        # Create Resource Group for Template Deployment
        New-AzureRmResourceGroup -Name $RGName -Location $myLocation

        # Deploy Simple IaaS Template
        New-AzureRmResourceGroupDeployment `
            -Name myDeployment$myNum `
            -ResourceGroupName $RGName `
            -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
            -NewStorageAccountName mystorage$myNum `
            -DnsNameForPublicIP mydns$myNum `
            -AdminUsername <username> `
            -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
            -VmName myVM$myNum `
            -WindowsOSVersion 2012-R2-Datacenter
    ```

3.  Öffnen Sie das Portal Azure Stapel, klicken Sie auf **Durchsuchen**, klicken Sie auf **virtuellen Computern**und suchen Sie nach dem neuen virtuellen Rechner (*myDeployment001*).

## <a name="video-example-hybrid-virtual-machine-deployment"></a>Video-Beispiel: hybridbereitstellung-virtuellen Computern

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-hybrid-vm-deployment]

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Vorlagen mit Visual Studio](azure-stack-deploy-template-visual-studio.md)
