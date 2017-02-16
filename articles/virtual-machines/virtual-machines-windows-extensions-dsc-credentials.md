<properties
   pageTitle="Übergabe von Anmeldeinformationen in Azure mithilfe von DSC | Microsoft Azure"
   description="Übersicht sichere Übergabe von Anmeldeinformationen an Azure-virtuellen Computern mithilfe der PowerShell gewünscht Zustand Konfiguration"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a>Übergabe von Anmeldeinformationen an den Azure DSC Erweiterung Ereignishandler #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

In diesem Artikel werden die Konfiguration des gewünschten Erweiterung für Azure behandelt. Übersicht über den DSC Erweiterung Ereignishandler finden Sie unter [Einführung in die Konfiguration des Azure gewünscht Erweiterung Ereignishandler](virtual-machines-windows-extensions-dsc-overview.md). 


## <a name="passing-in-credentials"></a>Übergabe von Anmeldeinformationen
Als Teil des Konfigurationsprozesses benötigten zum Einrichten von Benutzerkonten, Zugriff auf Dienste, oder ein Programm in einem Benutzerkontext installieren. Wenn Sie diese Schritte durchgeführt haben, müssen Sie für die Anmeldeinformationen. 

DSC ermöglicht parametrisierte Konfigurationen in denen Anmeldeinformationen in der Konfiguration übergeben und sicher in MOF-Dateien gespeichert werden. Ereignishandler der Azure-Erweiterung vereinfacht die Verwaltung von Anmeldeinformationen können, indem Sie die automatische Verwaltung von Zertifikaten. 

Erwägen Sie das folgende DSC Skript-Konfiguration, das ein lokales Benutzerkonto mit dem angegebenen Kennwort erstellt:

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

Es ist wichtig, *Knoten Localhost* als Teil der Konfiguration aufnehmen möchten. Wenn diese Anweisung fehlt, funktionieren die folgenden Schritte nicht wie der Erweiterung Ereignishandler speziell für die Knoten Localhost-Anweisung dargestellt wird. Unbedingt auch die Typumwandlung *[PsCredential]*, aufnehmen möchten wie dieses bestimmten Typs die Erweiterung zum Verschlüsseln der Anmeldeinformations auslöst. 

Veröffentlichen Sie diese Skripts auf Blob-Speicher ein:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Richten Sie die Erweiterung Azure DSC, und geben Sie die Anmeldeinformationen:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"
 
$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments
 
$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Wie werden Anmeldeinformationen geschützt
Ausführen dieses Codes für einen Satz von Anmeldeinformationen aufgefordert werden. Sobald sie bereitgestellt wird, wird es kurz im Speicher gespeichert. Wenn mit veröffentlicht `Set-AzureVmDscExtension` Cmdlet, es übertragenen HTTPS an den virtuellen Computer, wobei Azure gespeichert wird auf dem Datenträger, mit dem lokalen virtuellen Computer Zertifikat verschlüsselt. Es ist dann kurz im Speicher entschlüsselt und erneut verschlüsselt werden, um ihn an DSC übergeben.

Dieses Verhalten unterscheidet sich als [sichere Konfigurationen ohne die Erweiterung Ereignishandler verwenden](https://msdn.microsoft.com/powershell/dsc/securemof). Die Azure-Umgebung bietet eine Möglichkeit, um die von Konfigurationsdaten über Zertifikate sicher übertragen. Bei Verwendung den DSC Erweiterung Ereignishandler vorhanden ist nicht erforderlich, $CertificatePath oder eine $CertificateID bereitstellen / $Thumbprint Eintrag in ConfigurationData.


## <a name="next-steps"></a>Nächste Schritte ##

Weitere Informationen zu den Azure DSC Erweiterung Ereignishandler finden Sie unter [Einführung in die Konfiguration des Azure gewünscht Erweiterung Ereignishandler](virtual-machines-windows-extensions-dsc-overview.md). 

Untersuchen der [Ressourcenmanager Azure-Vorlage für die Erweiterung DSC](virtual-machines-windows-extensions-dsc-template.md)an.

Weitere Informationen zu PowerShell DSC, [finden Sie auf der Mitte der PowerShell-Dokumentation](https://msdn.microsoft.com/powershell/dsc/overview). 

Um zusätzliche Funktionen finden können Sie mit PowerShell DSC, [Navigieren Sie im Katalog PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) für weitere DSC Ressourcen verwalten.
