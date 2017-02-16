<properties
   pageTitle="Bundesland Konfiguration Azure Übersicht gewünscht | Microsoft Azure"
   description="Übersicht über das mit dem Microsoft Azure Erweiterung Ereignishandler für PowerShell gewünscht Zustand Konfiguration. Einschließlich erforderliche Komponenten, Architektur, Cmdlets..."
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

# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Einführung in die Konfiguration des Azure gewünscht Erweiterung Ereignishandler #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Der Azure-virtuellen Computer-Agents und die zugeordneten Erweiterungen gehören die Microsoft Azure-Infrastrukturdienste. Virtueller Computer Erweiterungen sind Softwarekomponenten, die erweitern die Funktionalität virtueller Computer und verschiedene virtueller Computer Management Vorgänge zu vereinfachen. Angenommen, die Erweiterung VMAccess zum Zurücksetzen des Kennworts eines Administrators verwendet werden kann, oder die Erweiterung benutzerdefinierte Skripts zur Ausführung eines Skripts des virtuellen Computers verwendet werden kann.

In diesem Artikel werden die Erweiterung PowerShell gewünscht Zustand Konfiguration (DSC) für Azure-virtuellen Computern als Teil des SDK PowerShell Azure vorgestellt. Neue Cmdlets können Sie hochladen und Anwenden einer PowerShell DSC Konfigurations einer Azure virtuellen Computers mit der Erweiterung PowerShell DSC aktiviert. Der PowerShell DSC Erweiterung Anrufe in PowerShell DSC darin, die empfangene DSC Konfiguration des virtuellen Computers. Diese Funktion steht auch über das Azure-Portal.

## <a name="prerequisites"></a>Erforderliche Komponenten ##
**Lokaler Computer** Um mit der Erweiterung Azure-virtuellen Computer zu interagieren, müssen Sie entweder im Azure-Portal oder im Azure PowerShell SDK verwenden. 

**Gast-Agent** Ein OS werden, die entweder Windows Management Framework (WMF) 4.0 oder 5.0 unterstützt muss den Azure-virtuellen Computer, der durch die Konfiguration DSC konfiguriert werden. Die vollständige Liste der unterstützten OS-Versionen finden Sie unter den [DSC Erweiterung Versionsverlauf](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Konzepte und Begriffe ##
Mit diesem Leitfaden wird davon ausgegangen Vertrautheit mit den folgenden Konzepten:

Konfigurations - DSC ein Dokument. 

Knoten - Ziel für eine DSC-Konfiguration. In diesem Dokument bezieht sich "Knoten" immer eine Azure-virtuellen Computer.

Konfigurations - eine .psd1 der Datendatei enthaltenden Produktdaten für eine Konfiguration

## <a name="architectural-overview"></a>Übersicht über die Architektur ##

Die Erweiterung Azure DSC verwendet Framework Azure-virtuellen Computer-Agent zum Vorführen widersprechenden und Erstellen von Berichten zu DSC Konfigurationen auf Azure-virtuellen Computern ausgeführt. Die Erweiterung DSC erwartet ZIP-Datei enthält mindestens ein Dokument, und eine Reihe von Parametern mit dem Azure PowerShell SDK oder über das Azure-Portal zur Verfügung.

Wenn die Erweiterung zum ersten Mal aufgerufen wird, wird er eine Installationsprozess ausgeführt. Dieser Vorgang installiert eine Version von Windows Management Framework (WMF) unter Verwendung der folgenden Logik:

1. Wenn das Azure-virtuellen Computer Betriebssystem Windows Server 2016 ist, wird keine Aktion ausgeführt. Windows Server 2016 haben bereits die neueste Version von PowerShell installiert.
2. Wenn die `wmfVersion` angegeben wird, werden diese Version von WMF ist installiert, es sei denn, es mit Betriebssystem des virtuellen Computers inkompatibel ist.
3. Wenn keine `wmfVersion` angegeben wird, werden die neueste anwendbare Version der WMF installiert ist.

Installation von WMF erfordert einen Neustart. Nach dem Neustart die Erweiterung downloads im angegebenen ZIP-Datei der `modulesUrl` Eigenschaft. Ist dieser Speicherort in Azure Blob-Speicher, kann ein SAS Token angegeben werden, der `sasToken` Eigenschaft Zugriff auf die Datei. Nachdem die ZIP heruntergeladen und entpackt wurde, die Konfigurationsfunktion definiert `configurationFunction` wird ausgeführt, um die MOF-Datei zu generieren. Klicken Sie dann die Erweiterung ausgeführt wird, `Start-DscConfiguration -Force` auf die generierten MOF-Datei. Die Erweiterung erfasst Ausgabe und schreibt an den Azure-Kanal Status zurück. Von diesem Punkt an übernimmt die DSC KGV Überwachung und Korrektur wie gewohnt. 

## <a name="powershell-cmdlets"></a>PowerShell-cmdlets ##

PowerShell-Cmdlets kann Verpacken, veröffentlichen und überwachen DSC Erweiterung Bereitstellungen mit Cloud oder ASM verwendet werden. Die folgenden Cmdlets aufgelistet sind die ASM Module, aber mit "AzureRm" mit der Cloud-Modell "Azure" ersetzt werden. Beispielsweise `Publish-AzureVMDscConfiguration` ASM, verwendet, in dem `Publish-AzureRmVMDscConfiguration` Cloud verwendet. 

`Publish-AzureVMDscConfiguration`in einer Konfigurationsdatei akzeptiert, überprüft es abhängige DSC Ressourcen und erstellt eine ZIP-Datei mit der Konfiguration und DSC Ressourcen darin, die Konfiguration erforderlich. Sie können auch das Paket lokal mit Erstellen der `-ConfigurationArchivePath` Parameter. Andernfalls wird die ZIP-Datei in Azure Blob-Speicher veröffentlicht und Sichern Sie es mit einem SAS Token.

Die ZIP-Datei, die mit diesem Cmdlet erstellte weist das Skript ps1 im Stammverzeichnis der Archivordner. Ressourcen müssen im Modul-Ordner in den Archivordner platziert. 

`Set-AzureVMDscExtension`die Einstellungen von der PowerShell DSC Erweiterung in ein virtueller Computer-Konfigurations-Objekt, das Klicken Sie dann auf eine Azure-virtuellen Computer mit angewendet werden kann erforderlich Barcode `Update-AzureVM`.

`Get-AzureVMDscExtension`Ruft den Status DSC Erweiterung eines bestimmten virtuellen Computers ab. 

`Get-AzureVMDscExtensionStatus`Ruft den Status der vom Ereignishandler Erweiterung DSC Stellvertreter DSC Konfiguration ab. Diese Aktion kann auf einem einzelnen virtuellen Computer oder eine Gruppe von virtuellen Computern ausgeführt werden.

`Remove-AzureVMDscExtension`Entfernt den Erweiterung Ereignishandler aus einem angegebenen virtuellen Computern. Dieses Cmdlet unterstützt wird **nicht** Entfernen der Konfiguration WMF deinstallieren oder Ändern der angewendeten Einstellungen des virtuellen Computers. Es werden nur den Erweiterung Ereignishandler entfernt. 

**Wichtigsten Unterschiede in ASM und Cloud-cmdlets**

- Cloud-Cmdlets sind synchron. ASM Cmdlets sind asynchrone.
- ResourceGroupName, VMName, ArchiveStorageAccountName, Version und Speicherort sind alle neuen erforderlichen Parameter.
- ArchiveResourceGroupName handelt es sich um eine neue optionalen Parameter für Cloud. Wenn Ihr Speicherkonto zu einer anderen Ressourcengruppe als den gehört, wo des virtuellen Computers erstellt haben, können Sie diesen Parameter angeben.
- ConfigurationArchive heißt ArchiveBlobName in der Cloud
- ContainerName heißt ArchiveContainerName in der Cloud
- StorageEndpointSuffix heißt ArchiveStorageEndpointSuffix in der Cloud
- Die Option AutoUpdate wurde hinzugefügt, um die Cloud aktivieren Sie automatische Aktualisierung von der Erweiterung Ereignishandler auf die neueste Version als, und wenn es verfügbar ist. Inhaltsindexserver für diesen Parameter möglicherweise verursachen sind Neustart des virtuellen Computers, wenn eine neue Version der WMF veröffentlicht wird. 


## <a name="azure-portal-functionality"></a>Azure Portals Funktionalität ##
Navigieren Sie zu klassischen eines virtuellen Computers. Klicken Sie unter Einstellungen -> Allgemein auf "Extensions". Ein neuer Bereich wird erstellt. Klicken Sie auf "Hinzufügen", und wählen Sie PowerShell DSC.

Im Portal benötigt Eingabe.
**Konfiguration Module und kein Skript**: Dieses Feld ist erforderlich. Erfordert eine. ps1-Datei mit einem Konfigurationsskript oder eine ZIP-Datei mit einem ps1 Konfigurationsskript am Stamm und alle abhängigen Ressourcen im Modul-Ordner in die ZIP. Es kann erstellt werden, mit der `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` Cmdlet im Azure PowerShell SDK enthalten. Die ZIP-Datei hochgeladen wird in Ihrer Benutzer Blob-Speicher gesichert durch ein SAS Token. 

**Konfigurationsdatei der Daten PSD1**: Dieses Feld ist optional. Wenn Ihre Konfiguration eine Datendatei Konfiguration in .psd1 erforderlich ist, verwenden Sie dieses Feld zu markieren, und klicken Sie auf Ihre Benutzer Blob-Speicher hochladen, wo sie durch ein Token SAS gesichert. 
 
**Name der Konfiguration Module-Qualified**: ps1-Dateien können mehrere Konfigurationsfunktionen haben. Geben Sie den Namen des Konfiguration ps1 Skripts gefolgt von einem '\' sowie den Namen der Konfigurationsfunktion. Angenommen, wenn Ihr ps1-Skript hat den Namen "configuration.ps1", und die Konfiguration "IisInstall", geben Sie:`configuration.ps1\IisInstall`

**Konfiguration Argumente**: Wenn die Konfigurationsfunktion Argumente annimmt, geben Sie diese hier im Format `argumentName1=value1,argumentName2=value2`. Beachten Sie, dass dieses Format ist ein anderes Format als wie Konfiguration Argumente über PowerShell-Cmdlets oder Ressourcenmanager Vorlagen akzeptiert werden. 

## <a name="getting-started"></a>Erste Schritte ##

Die Erweiterung Azure DSC erhält DSC Konfigurationsdokumente und setzt diese auf Azure-virtuellen Computern um. Ein einfaches Beispiel für eine Konfiguration folgt. Speichern Sie es lokal als "IisInstall.ps1":

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

Die folgenden Schritte aus platzieren Sie das Skript IisInstall.ps1 des angegebenen virtuellen Computers, führen Sie die Konfiguration, und melden sich wieder auf Status.
 
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "demo.ps1.zip" -StorageContext $storageContext -ConfigurationName "runScript" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```

## <a name="logging"></a>Protokollierung ##

Protokolle befinden:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[Versionsnummer]

## <a name="next-steps"></a>Nächste Schritte ##

Weitere Informationen zu PowerShell DSC, [finden Sie auf der Mitte der PowerShell-Dokumentation](https://msdn.microsoft.com/powershell/dsc/overview). 

Untersuchen der [Ressourcenmanager Azure-Vorlage für die Erweiterung DSC](virtual-machines-windows-extensions-dsc-template.md
)an. 

Um zusätzliche Funktionen finden können Sie mit PowerShell DSC, [Navigieren Sie im Katalog PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) für weitere DSC Ressourcen verwalten.

Details zum vertrauliche Parameter in Konfigurationen übergeben finden Sie unter [Verwalten von Anmeldeinformationen sichere Weise mit dem DSC Erweiterung Ereignishandler](virtual-machines-windows-extensions-dsc-credentials.md).
