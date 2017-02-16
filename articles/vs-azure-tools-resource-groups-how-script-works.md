<properties
    pageTitle="Übersicht über das Azure Ressourcengruppe Project Bereitstellungsskript | Microsoft Azure"
    description="Beschreibt, wie das PowerShell-Skript im Projekt Azure Ressourcengruppe Bereitstellung funktioniert."
    services="visual-studio-online"
    documentationCenter="na"
    authors="tfitzmac"
    manager="timlt"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/26/2016"
    ms.author="tomfitz" />

# <a name="overview-of-the-azure-resource-group-project-deployment-script"></a>Übersicht über das Azure Ressourcengruppe Project Bereitstellungsskript

Ressourcengruppe Azure-Bereitstellung Projekte helfen, die Ihnen Phaseneigenschaften und Bereitstellen von Dateien und andere Elemente in Azure. Wenn Sie ein Ressourcenmanager Azure Bereitstellungsprojekt in Visual Studio erstellen, wird ein PowerShell-Skript aufgerufen **Bereitstellen-AzureResourceGroup.ps1** zum Projekt hinzugefügt. Dieses Thema enthält Details zur Funktionsweise dieses Skripts und zum Ausführen von innerhalb und außerhalb von Visual Studio.

## <a name="what-does-the-script-do"></a>Wie funktioniert das Skript?

Das Skript bereitstellen-AzureResourceGroup.ps1 enthält zwei Dinge, die den Bereitstellungsworkflow wichtig sind.

- Hochladen von Dateien oder Elemente, die für die Bereitstellung der Vorlage erforderlich
- Bereitstellen der Vorlage

Im erste Teil des Skripts uploads der Dateien und Elemente für die Bereitstellung, und das letzte Cmdlet in das Skript tatsächlich bereitstellen, die Vorlage. Angenommen, wenn Sie ein virtuellen Computer muss mit einem Skript konfiguriert sein, uploads Bereitstellungsskript zuerst sicher das Skript mit einer Firma Azure-Speicher. Dadurch verfügbar zu Azure Ressourcenmanager zum Konfigurieren des virtuellen Computers während der Bereitstellung.

Da nicht alle Vorlage Bereitstellungen zusätzliche Elemente, die besitzen müssen hochgeladen werden müssen, wird ein Wechselparameter mit der Bezeichnung *UploadArtifacts* ausgewertet. Wenn alle Elemente hochgeladen werden müssen, legen Sie die *UploadArtifacts* wechseln, wenn das Skript aufrufen. Beachten Sie, dass das Hauptfenster Vorlagendatei und Parameterdatei nicht hochgeladen werden. Nur andere Dateien, wie etwa das Skript, geschachtelte Bereitstellung Vorlagen und Anwendungsdateien hochgeladen werden müssen.

## <a name="detailed-script-description"></a>Ausführliche Skript Beschreibung

Es folgt eine Beschreibung der welche Abschnitte auswählen die bereitstellen-AzureResourceGroup.ps1 Azure PowerShell Skript führen.

>[AZURE.NOTE] Beschreibt, Version 1.0, der das Skript AzureResourceGroup.ps1 bereitstellen.

1.  Deklarieren Sie Parameter von Azure Ressourcenmanager Bereitstellungsprojekt benötigt. Einige Parameter müssen Standardwerte, die festgelegt wurden, wenn das Projekt erstellt wurde. Sie können diese Standardwerte im Skript ändern oder verschiedene Parameterwerte hinzufügen, bevor Sie das Skript ausführen.

    ```
    Param(
      [string] [Parameter(Mandatory=$true)] $ResourceGroupLocation,
      [string] $ResourceGroupName = 'AzureResourceGroup1',
      [switch] $UploadArtifacts,
      [string] $StorageAccountName,
      [string] $StorageAccountResourceGroupName,
      [string] $StorageContainerName = $ResourceGroupName.ToLowerInvariant() + '-stageartifacts',
      [string] $TemplateFile = '..\Templates\azuredeploy.json',
      [string] $TemplateParametersFile = '..\Templates\azuredeploy.parameters.json',
      [string] $ArtifactStagingDirectory = '..\bin\Debug\staging',
      [string] $AzCopyPath = '..\Tools\AzCopy.exe',
      [string] $DSCSourceFolder = '..\DSC'
    )
    ```

  	|Parameter|Beschreibung|
  	|---|---|
  	|$ResourceGroupLocation|Die Region oder Daten zentrieren einen Speicherort für die Ressourcengruppe, z. B. **Westen US** oder **Ostasien**aus.|
  	|$ResourceGroupName|Der Name der Ressourcengruppe Azure.|
  	|$UploadArtifacts|Binäre Wert, der angibt, ob die Elemente aus Ihrem System in Azure hochgeladen werden müssen.|
  	|$StorageAccountName|Der Name Ihres Azure-Speicher-Kontos, wo Ihre Elemente hochgeladen werden.|
  	|$StorageAccountResourceGroupName|Der Name der Azure Ressourcengruppe, die Speicherkonto enthält.|
  	|$StorageContainerName|Der Name des Containers Speicher für das Hochladen Elemente verwendet werden soll.|
  	|$TemplateFile|Der Pfad zu der Bereitstellungsdatei (`<app name>.json`) im Projekt Azure Ressourcengruppe.|
  	|$TemplateParametersFile|Der Pfad zu der Parameterdatei (`<app name>.parameters.json`) im Projekt Azure Ressourcengruppe.|
  	|$ArtifactStagingDirectory|Der Pfad auf Ihrem System, in dem Elemente lokal, einschließlich der PowerShell Skript Stammordner hochgeladen werden. Dieser Pfad kann absolut oder relativ zum Speicherort Skript sein.|
  	|$AzCopyPath|Der Pfad, in dem das Tool AzCopy.exe enthaltenen ZIP-Dateien, einschließlich den PowerShell Skript Stammordner kopiert. Dieser Pfad kann absolut oder relativ zum Speicherort Skript sein.|
  	|$DSCSourceFolder|Der Pfad zum Ordner Quelle DSC (gewünscht Zustand Konfiguration), einschließlich den PowerShell-Skript-Stammordner. Dieser Pfad kann absolut oder relativ zum Speicherort Skript sein. Finden Sie unter [Einführung in die Erweiterung Azure PowerShell DSC (gewünscht Zustand Konfiguration)](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx), sofern zutreffend, Weitere Informationen.|

1.  Prüfen Sie, ob die Elemente in Azure hochgeladen werden müssen. Wenn dies nicht der Fall ist, fahren Sie mit Schritt 11 fort. Führen Sie andernfalls die folgenden Schritte aus.

1.  Konvertieren Sie alle Variablen mit relativen Pfade in absolute Pfade. Ändern Sie einen Pfad beispielsweise wie `..\Tools\AzCopy.exe` auf `C:\YourFolder\Tools\AzCopy.exe`. Darüber hinaus Initialisierung der Variablen *ArtifactsLocationName* und *ArtifactsLocationSasTokenName* auf null ein. *ArtifactsLocation* und *SaSToken* möglicherweise Parameter der Vorlage. Wenn ihre Werte nach dem Einlesen der Parameterdatei null sind, generiert das Skript Werte für diese an.

    Azure-Tools werden die Werte für die Parameter *_artifactsLocation* und *_artifactsLocationSasToken* in der Vorlage verwenden, um Elemente zu verwalten. Wenn das PowerShell-Skript Parameter mit lediglich die Namen findet, aber die Parameterwerte sind nicht angegeben, wird das Skript uploads Elemente und gibt die entsprechenden Werte für diesen Parameter. Es übergibt diese an das Cmdlet über `@OptionsParameters`.

  	|Variable|Beschreibung|
  	|---|---|
  	|ArtifactsLocationName|Der Pfad zu dem die Azure-Elemente gespeichert sind.|
  	|ArtifactsLocationSasTokenName|Der SAS (Access-Signatur freigegeben) Name token durch das Skript zum Authentifizieren Dienstbus verwendet wird. Weitere Informationen finden Sie unter [Freigegebene Access Signatur Authentifizierung mit Dienstbus](service-bus-shared-access-signature-authentication.md) .|

    ```
    if ($UploadArtifacts) {
    # Convert relative paths to absolute paths if needed
    $AzCopyPath = [System.IO.Path]::Combine($PSScriptRoot, $AzCopyPath)
    $ArtifactStagingDirectory = [System.IO.Path]::Combine($PSScriptRoot, $ArtifactStagingDirectory)
    $DSCSourceFolder = [System.IO.Path]::Combine($PSScriptRoot, $DSCSourceFolder)

    Set-Variable ArtifactsLocationName '_artifactsLocation' -Option ReadOnly
    Set-Variable ArtifactsLocationSasTokenName '_artifactsLocationSasToken' -Option ReadOnly

    $OptionalParameters.Add($ArtifactsLocationName, $null)
    $OptionalParameters.Add($ArtifactsLocationSasTokenName, $null)
    ```

1.  Dieser Abschnitt Sie überprüft, ob die <app name>. parameters.json-Datei (wie die Datei"Parameter" bezeichnet) weist einen übergeordneten Knoten benannte **Parameter** (in der `else` blockieren). Andernfalls muss es keinen übergeordneten Knoten. Entweder Format ist zulässig.
    
    ```
    if ($JsonParameters -eq $null) {
            $JsonParameters = $JsonContent
        }
        else {
            $JsonParameters = $JsonContent.parameters
        }
    ```

1.  Durchlaufen Sie die Auflistung der JSON-Parameter aus. Wenn ein Parameterwert *_artifactsLocation* oder *_artifactsLocationSasToken*zugewiesen wurde, legen Sie die Variable *$OptionalParameters* mit diesen Werten. Dadurch wird verhindert, dass das Skript versehentlich überschreiben beliebige Parameterwerte, die Sie bereitstellen.

    ```
    $JsonParameters | Get-Member -Type NoteProperty | ForEach-Object {
        $ParameterValue = $JsonParameters | Select-Object -ExpandProperty $_.Name

        if ($_.Name -eq $ArtifactsLocationName -or $_.Name -eq $ArtifactsLocationSasTokenName) {
            $OptionalParameters[$_.Name] = $ParameterValue.value
        }
    }
    ```

1.  Erhalten der kontoschlüssel Speicherplatz und den Kontext für den Speicher Konto Ressource verwendet, um die Elemente für die Bereitstellung aufzunehmen.

    ```
    $StorageAccountKey = (Get-AzureRMStorageAccountKey -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Key1

    $StorageAccountContext = (Get-AzureRmStorageAccount -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Context
    ```

1.  Wenn Sie PowerShell DSC konfigurieren ein virtuellen Computers verwenden, erfordert die Erweiterung DSC die Elemente in einer einzelnen Zip-Datei sein. Ja, Erstellen einer ZIP-Archivdatei für die DSC-Konfiguration aus. Überprüfen Sie hierzu finden Sie unter Wenn $DSCSourceFolder vorhanden ist. Wenn eine DSC Konfiguration vorhanden ist, entfernen Sie es, und erstellen Sie eine neue komprimierte Datei namens dsc.zip.

    ```
    # Create DSC configuration archive
    if (Test-Path $DSCSourceFolder) {
    Add-Type -Assembly System.IO.Compression.FileSystem
        $ArchiveFile = Join-Path $ArtifactStagingDirectory "dsc.zip"
        Remove-Item -Path $ArchiveFile -ErrorAction SilentlyContinue
        [System.IO.Compression.ZipFile]::CreateFromDirectory($DSCSourceFolder, $ArchiveFile)
    }
    ```

1.  Wenn kein Pfad für Azure Elemente in der Datei Parameter bereitgestellt wird, legen Sie einen Pfad für die PowerShell-Skript zu verwenden, wenn Sie Elemente hochladen aus. Erstellen Sie hierzu eines Pfads mit einer Kombination aus Speicher-Konto Endpunkt Pfad sowie den Namen der Speicher-Container aus. Klicken Sie dann aktualisieren Sie die Parameter-Datei mit dieser neuen Pfad.

    ```
    # Generate the value for artifacts location if it is not provided in the parameter file
    $ArtifactsLocation = $OptionalParameters[$ArtifactsLocationName]
    if ($ArtifactsLocation -eq $null) {
        $ArtifactsLocation = $StorageAccountContext.BlobEndPoint + $StorageContainerName
        $OptionalParameters[$ArtifactsLocationName] = $ArtifactsLocation
    }
    ```

1.  Verwenden Sie dem **AzCopy** -Programm (in den Ordner " **Tools** " des Projekts Bereitstellung Azure Ressourcengruppe enthalten), um alle Dateien aus Ihrem lokalen Speicher ablegen Pfad in Ihr online-Speicher Azure-Konto zu kopieren. Wenn dieser Schritt fehlschlägt, beenden Sie das Skript, da die Bereitstellung nicht wahrscheinlich ohne die erforderlichen Elemente erfolgreich ist.

    ```
    # Use AzCopy to copy files from the local storage drop path to the storage account container
    & $AzCopyPath """$ArtifactStagingDirectory""", $ArtifactsLocation, "/DestKey:$StorageAccountKey", "/S", "/Y", "/Z:$env:LocalAppData\Microsoft\Azure\AzCopy\$ResourceGroupName"
    if ($LASTEXITCODE -ne 0) { return }
    ```

1.  Wenn ein Token SAS für den Speicherort der Elemente in der Parameter-Datei nicht enthalten ist, erstellen Sie eine temporäre schreibgeschützten Zugriff auf den online-Speicher-Container aus. Klicken Sie dann übergeben Sie die SAS Token an die Befehlszeile, als "OptionalParameter". Beachten Sie, dass alle Parameter auf die Befehlszeile übergeben Werte liegen, die in der Parameterdatei Vorrang werden.

    ```
    # Generate the value for artifacts location SAS token if it is not provided in the parameter file
    $ArtifactsLocationSasToken = $OptionalParameters[$ArtifactsLocationSasTokenName]
    if ($ArtifactsLocationSasToken -eq $null) {
       # Create a SAS token for the storage container - this gives temporary read-only access to the container
       $ArtifactsLocationSasToken = New-AzureStorageContainerSASToken -Container $StorageContainerName -Context $StorageAccountContext -Permission r -ExpiryTime (Get-Date).AddHours(4)
       $ArtifactsLocationSasToken = ConvertTo-SecureString $ArtifactsLocationSasToken -AsPlainText -Force
       $OptionalParameters[$ArtifactsLocationSasTokenName] = $ArtifactsLocationSasToken
    }
    ```

1.  Erstellen der Ressourcengruppe an, wenn sie noch nicht vorhanden ist und überprüfen Sie die Datei Vorlage und Parameter für alle Überprüfungsfehler, die die Bereitstellung von nachfolgende verhindern.

    ```
    # Create or update the resource group using the specified template file and template parameters file
    New-AzureRMResourceGroup -Name $ResourceGroupName -Location $ResourceGroupLocation -Verbose -Force -ErrorAction Stop

    Test-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterFile $TemplateParametersFile @OptionalParameters -ErrorAction Stop
    ```

1. Stellen Sie abschließend die Vorlage ein. Dieser Code erstellt einen eindeutigen Namen für die Bereitstellung mit einem Zeitstempel.

    ```
    New-AzureRMResourceGroupDeployment -Name ((Get-ChildItem $TemplateFile).BaseName + '-' + ((Get-Date).ToUniversalTime()).ToString('MMdd-HHmm')) `
        -ResourceGroupName $ResourceGroupName `
        -TemplateFile $TemplateFile `
        -TemplateParameterFile $TemplateParametersFile `
        @OptionalParameters `
        -Force -Verbose
    ```

## <a name="deploy-the-resource-group"></a>Bereitstellen der Ressourcengruppe

### <a name="to-deploy-the-resource-group-in-visual-studio"></a>Die Ressourcengruppe in Visual Studio bereitstellen.

1. Wählen Sie im Kontextmenü des Projekts Ressourcengruppe Azure **Bereitstellen** > **Neue Bereitstellung**.

    ![][0]

1. Klicken Sie im Dialogfeld **Bereitstellen Ressourcengruppe** entweder wählen Sie eine vorhandene Ressourcengruppe in der Dropdown-Listenfeld zum Bereitstellen, oder wählen Sie ** &lt;neu erstellen... &gt;** So erstellen eine neue Ressourcengruppe.

    ![][1]

1. Wenn Sie dazu aufgefordert werden, geben Sie im Dialogfeld **Ressourcengruppe erstellen** einen Ressourcengruppennamen und einen Speicherort aus, und wählen Sie dann auf die Schaltfläche **Erstellen** .

    ![][2]

1. Wählen Sie die Schaltfläche **Bearbeiten Parameter** zum Anzeigen des Dialogfelds **Parameter bearbeiten** , und geben dann alle fehlende Parameterwerte ein.

    ![][3]

    >[AZURE.NOTE] Wenn Sie alle erforderlichen Parameter Werte benötigen, wird in diesem Dialogfeld automatisch beim bereitstellen.

    ![][4]

1. Wenn Sie damit fertig sind Parameterwerte eingeben, wählen Sie die Schaltfläche **Speichern** , und wählen Sie dann auf die Schaltfläche **Bereitstellen** .

    Das Bereitstellungsskript (Bereitstellen-AzureResourceGroup.ps1) ausgeführt wird und die Vorlage, sowie alle Elemente, um Azure bereitstellt.

### <a name="to-deploy-the-resource-group-by-using-powershell"></a>Die Ressourcengruppe bereitstellen mithilfe der PowerShell

Wenn Sie das Skript ohne den Befehl Visual Studio bereitstellen und die Benutzeroberfläche, die im Kontextmenü für das Skript ausführen möchten, wählen Sie **mit PowerShell ISE öffnen**.

![][5]


## <a name="command-deployment-examples"></a>Beispiele für die Bereitstellung von Befehl

### <a name="deploy-using-default-values"></a>Die Bereitstellung mit Standardwerten

In diesem Beispiel wird gezeigt, wie das Skript unter Verwendung der Standardwerte für Parameter ausgeführt. (Da der Speicherort Parameter nicht über einen Standardwert verfügt, müssen Sie eine bereitstellen.)

`.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus`

### <a name="deploy-overriding-the-default-values"></a>Bereitstellen Sie Überschreiben der Standardwerte

Dieses Beispiel zeigt, wie führen Sie das Skript zum Bereitstellen von Vorlage und Parameter-Dateien, die von den Standardwerten abweichen.

```
.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus –TemplateFile ..\templates\AnotherTemplate.json –TemplateParametersFile ..\templates\AnotherTemplate.parameters.json
```

### <a name="deploy-using-uploadartifacts-for-staging"></a>Verwenden für das Staging UploadArtifacts bereitstellen

Dieses Beispiel zeigt das Skript, um Elemente aus dem Releaseordner hochladen und Bereitstellen von Vorlagen für nicht standardmäßige ausführen.

```
.\Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory ..\bin\release\staging
```

Dieses Beispiel zeigt, wie Sie das Skript bei einem Azure PowerShell-Vorgang in Visual Studio Online ausführen.

```
$(Build.StagingDirectory)/AzureResourceGroup1/Scripts/Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory $(Build.StagingDirectory)
```

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über Azure Ressourcenmanager [Azure Ressourcenmanager Überblick](azure-resource-manager/resource-group-overview.md)lesen.

Weitere Beispiele zum Arbeiten mit Projekten Azure Ressourcengruppe, finden Sie unter [Bereitstellen und Verwalten von Azure Ressourcen](https://github.com/Microsoft/HealthClinic.biz/wiki/Deploy-and-manage-Azure-resources) aus [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 verbinden [Demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Weitere Schnellstart aus HealthClinic.biz anschauen finden Sie unter [Azure Developer Tools Schnellstart](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

[0]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy1c.png
[1]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy2bc.png
[2]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy3bc.png
[3]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy4bc.png
[4]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy5c.png
[5]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy6c.png
