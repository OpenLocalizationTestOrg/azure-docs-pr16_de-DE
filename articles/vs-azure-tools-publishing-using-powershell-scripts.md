<properties
   pageTitle="Verwendung von Windows PowerShell-Skripts zum Veröffentlichen von Entwicklung und Test-Umgebungen | Microsoft Azure"
   description="Erfahren Sie, wie die Verwendung von Windows PowerShell Skripts von Visual Studio auf Entwicklung veröffentlichen und Testen der Umgebungen."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-windows-powershell-scripts-to-publish-to-dev-and-test-environments"></a>Verwenden von Windows PowerShell-Skripts auf Entwickler veröffentlichen und Testen der Umgebungen

Wenn Sie eine Webanwendung in Visual Studio erstellen, können Sie ein Windows PowerShell-Skript generieren, die Sie später verwenden können, um die Veröffentlichung Ihrer Website zu Azure als Web App im App-Verwaltungsdienst Azure oder eines virtuellen Computers zu automatisieren. Sie können bearbeiten und erweitern in der Visual Studio-Editor, um Ihren Anforderungen entsprechend das Windows PowerShell-Skript oder das Skript mit vorhandenen erstellen, testen und Veröffentlichen von Skripts zu integrieren.

Verwenden diese Skripts, können Sie angepasste Versionen (auch bekannt als Paar und Test-Umgebungen) Ihrer Website für die temporäre Verwendung bereitstellen. Beispielsweise könnten Sie eine bestimmte Version Ihrer Website auf eine Azure-virtuellen Computern oder in den staging Slot auf einer Website Ausführen einer Test-Suite, reproduzieren eines Fehlers, Testen einer Fehlerkorrektur, Testversion einer vorgeschlagenen Änderung oder Einrichten einer benutzerdefinierten Umgebung für eine Demo oder die Präsentation einrichten. Nachdem Sie ein Skript, die Ihr Projekt veröffentlicht erstellt haben, können Sie identische Umgebungen erstellen, indem Sie das Skript erneut ausführen, je nach Bedarf, oder führen Sie das Skript mit Ihrer eigenen Erstellen Ihrer Web-Anwendung, um eine benutzerdefinierte Umgebung zum Testen zu erstellen.

## <a name="what-you-need"></a>Was Sie benötigen

- Azure SDK 2.3 oder höher. Weitere Informationen finden Sie unter [Visual Studio-Downloads](http://go.microsoft.com/fwlink/?LinkID=624384) .

Die Azure SDK zum Generieren der Skripts für Webprojekte, die benötigen nicht. Dieses Feature ist für Webprojekte,, nicht Webrollen in der Cloud-Dienste.

- Azure PowerShell 0.7.4 oder höher. Weitere Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](powershell-install-configure.md) .

- [Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) oder höher.

## <a name="additional-tools"></a>Zusätzliche tools

Zusätzliche Tools und Ressourcen für die Arbeit mit PowerShell in Visual Studio für Azure-Entwicklung sind verfügbar. [PowerShell-Tools für Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012)finden Sie unter.

## <a name="generating-the-publish-scripts"></a>Das Veröffentlichen Skripts werden generiert.

Sie können die Skripts veröffentlichen für einen virtuellen Computer generieren, die Ihre Website beim Erstellen eines neuen Projekts durch folgenden [diese Anweisungen hostet](./virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md). Sie können auch [generieren veröffentlichen Skripts von Web apps in Azure-App-Verwaltungsdienst](./app-service-web/web-sites-dotnet-get-started.md).

## <a name="scripts-that-visual-studio-generates"></a>Skripts, die Visual Studio generiert

Visual Studio generiert einen Lösung Ebene Ordner mit dem Namen **PublishScripts** , die zwei Windows PowerShell-Dateien, die ein Skript veröffentlichen für Ihre virtuellen Computern oder Website enthält, und ein Modul, das Funktionen enthält, die Sie in den Skripts verwenden können. Visual Studio generiert eine Datei auch in das JSON-Format, das die Details des Projekts angibt, die Sie bereitstellen möchten.

### <a name="windows-powershell-publish-script"></a>Skript zum Veröffentlichen von Windows PowerShell

Das Skript veröffentlichen enthält bestimmte veröffentlichen Schritte zum Bereitstellen auf einer Website oder virtuellen Computern. Visual Studio bietet Syntax für die Entwicklung von Windows PowerShell färben. Hilfe für die Funktionen zur Verfügung steht, und Sie können die Funktionen im Skript auf entsprechend Ihren Anforderungen geändert uneingeschränkt bearbeiten.

### <a name="windows-powershell-module"></a>Windows PowerShell-Modul

Das Windows PowerShell-Modul, das Visual Studio generiert enthält Funktionen, die das Skript Veröffentlichen verwendet. Diese sind Azure PowerShell-Funktionen und werden nicht geändert werden sollen. Weitere Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](powershell-install-configure.md) .

### <a name="json-configuration-file"></a>JSON-Konfigurationsdatei

Die JSON-Datei wird in den Ordner **Konfigurationen** erstellt und von Konfigurationsdaten, die angibt, genau welche Ressourcen für die Bereitstellung auf Azure enthält. Der Name der Datei, die Visual Studio generiert wird Project-Namen-WAWS-dev.json Wenn Sie eine Website oder Project Name-virtueller Computer-dev.json erstellt, wenn Sie einen virtuellen Computer erstellt haben. Hier ist ein Beispiel für eine JSON-Konfigurationsdatei, die generiert wird, wenn Sie eine Website erstellen. Die meisten Werte sind sofort verständlich. Der Name der Website wird von Azure generiert, sodass es möglicherweise nicht den Projektnamen übereinstimmt.

```
{
"environmentSettings": {
"webSite": {
"name": "WebApplication26632",
"location": "West US"
},
"databases": [
{
"connectionStringName": "DefaultConnection",
"databaseName": "WebApplication26632_db",
"serverName": "YourDatabaseServerName",
"user": "sqluser2",
"password": "",
"edition": "",
"size": "",
"collation": "",
"location": "West US"
}
]
}
}
```
Beim Erstellen eines virtuellen Computers sieht die JSON-Konfigurationsdatei ähnlich wie der folgende aus. Beachten Sie, dass ein Cloud-Dienst als Container des virtuellen Computers erstellt wird. Des virtuellen Computers enthält die üblichen Endpunkte für Web Access über HTTP und HTTPS als auch die Endpunkte für Web bereitstellen, die Sie von Ihrem lokalen Computer, Remotedesktop und Windows PowerShell auf der Website veröffentlichen können.

```
{
"environmentSettings": {
"cloudService": {
"name": "myusernamevm1",
"affinityGroup": "",
"location": "West US",
"virtualNetwork": "",
"subnet": "",
"availabilitySet": "",
"virtualMachine": {
"name": "myusernamevm1",
"vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
"size": "Small",
"user": "vmuser1",
"password": "",
"enableWebDeployExtension": true,
"endpoints": [
{
"name": "Http",
"protocol": "TCP",
"publicPort": "80",
"privatePort": "80"
},
{
"name": "Https",
"protocol": "TCP",
"publicPort": "443",
"privatePort": "443"
},
{
"name": "WebDeploy",
"protocol": "TCP",
"publicPort": "8172",
"privatePort": "8172"
},
{
"name": "Remote Desktop",
"protocol": "TCP",
"publicPort": "3389",
"privatePort": "3389"
},
{
"name": "Powershell",
"protocol": "TCP",
"publicPort": "5986",
"privatePort": "5986"
}
]
}
},
"databases": [
{
"connectionStringName": "",
"databaseName": "",
"serverName": "",
"user": "",
"password": ""
}
],
"webDeployParameters": {
"iisWebApplicationName": "Default Web Site"
}
}
}
```

Sie können die JSON-Konfiguration, damit ändern, was passiert, wenn Sie die Skripts veröffentlichen ausführen bearbeiten. Die `cloudService` und `virtualMachine` Abschnitte sind erforderlich, aber Sie können Löschen der `databases` Abschnitt, wenn Sie es nicht benötigen. Die Eigenschaften, die leere in die standardmäßige Konfigurationsdatei befinden, die Visual Studio generiert sind optional; Personen, die Werte in der standardmäßigen Konfigurationsdatei enthalten sind erforderlich.

Wenn Sie über eine Website, die mehrere Bereitstellung Umgebungen (als Steckplätze bezeichnet) anstelle einer einzelnen Herstellung Website in Azure verfügen, können Sie die Slot Name den Namen der Website in der JSON-Konfigurationsdatei einbeziehen. Beispielsweise, wenn Sie über eine Website verfügen, die hat **MeineWebsite** ernannt und ein Slot dafür benannte **Testen** klicken Sie dann auf den URI ist MeineWebsite-test.cloudapp.net, aber der richtige Namen zur Verwendung in der Konfigurationsdatei ist mysite(test). Sie können nur führen Sie dieses aus, wenn die Website und Steckplätze bereits in Ihrem Abonnement vorhanden sind. Wenn diese nicht vorhanden ist, erstellen Sie die Website, indem Sie das Skript ohne Angabe den Slot ausgeführt und dann erstellen Sie den Slot im [Azure klassischen Portal](http://go.microsoft.com/fwlink/?LinkID=213885)und danach führen Sie das Skript mit dem Websitenamen geänderte. Weitere Informationen zur Bereitstellung Steckplätze für Web apps finden Sie unter [Einrichten von staging-Umgebungen von Web apps in Azure-App-Dienst](./app-service-web/web-sites-staged-publishing.md).

## <a name="how-to-run-the-publish-scripts"></a>So führen Sie die Skripts veröffentlichen

Wenn Sie ein Windows PowerShell-Skript vor nie ausgeführt haben, müssen Sie zuerst die Ausführungsrichtlinie zum Ausführen von Skripts aktivieren festlegen. Dies ist ein Sicherheitsfeature, um zu verhindern, dass Benutzer Windows PowerShell-Skripts ausgeführt, wenn sie gefährdet Schadsoftware oder Viren, die betreffen Ausführen von Skripts befinden.

### <a name="run-the-script"></a>Führen Sie das Skript

1. Erstellen Sie das Paket Web bereitstellen für ein Projekt. Bereitstellen von Web-Paket ist einem komprimierten Archiv (ZIP-Datei), die Dateien enthalten, die Sie mit Ihrer Website oder virtuellen Computern kopieren möchten. Sie können Pakete Web bereitstellen für jede Web-Anwendung in Visual Studio erstellen.

![Erstellen von Web Bereitstellen des Pakets](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

Weitere Informationen finden Sie unter [wie: Erstellen einer Web-Bereitstellungspaket in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx). Darüber hinaus können Sie die Erstellung von Ihrem Paket Web bereitstellen automatisieren, wie im Abschnitt **Anpassen und erweitern die Skripts veröffentlichen** später in diesem Artikel beschrieben.

1. Öffnen Sie im **Explorer Lösung**das Kontextmenü für das Skript, und wählen Sie dann auf **Öffnen mit PowerShell ISE**.

1. Ist dies das erste Mal, wenn Sie Windows PowerShell-Skripts auf diesem Computer ausgeführt haben, öffnen Sie ein Eingabeaufforderungsfenster mit Administratorrechten, und geben Sie den folgenden Befehl ein:

`Set-ExecutionPolicy RemoteSigned`

1. Melden Sie sich bei Azure mit den folgenden Befehl aus.

`Add-AzureAccount`

Wenn Sie dazu aufgefordert werden, geben Sie Ihren Benutzernamen und Ihr Kennwort ein.

Beachten Sie, dass, wenn Sie das Skript automatisieren, diese Methode zum Bereitstellen von Azure Anmeldeinformationen funktioniert. Verwenden Sie stattdessen die PUBLISHSETTINGS-Datei für die Anmeldeinformationen. Einmaliges nur Sie mit dem Befehl **Get-AzurePublishSettingsFile** können Sie die Datei aus Azure herunterladen und danach **Importieren-AzurePublishSettingsFile** verwenden, um die Datei zu importieren. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](powershell-install-configure.md).

1. (Optional) Wenn Sie Azure Ressourcen wie die virtuellen Computern, die Datenbank und die Website zu erstellen, ohne Sie zu Ihrer Webanwendung veröffentlichen möchten, verwenden Sie den Befehl **Veröffentlichen-WebApplication.ps1** mit der **-Konfiguration** Argument auf den JSON-Konfigurationsdatei festgelegt. Diese Befehlszeile verwendet die JSON-Konfigurationsdatei um zu bestimmen, welche Ressourcen zu erstellen. Da die Standardeinstellungen für andere Befehlszeilenargumente verwendet, die Ressourcen erstellt, aber nicht veröffentlichen Sie die Webanwendung. Das – ausführlich Option bietet Ihnen weitere Informationen dazu, was passiert ist.

`Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json`

1. Verwenden Sie den Befehl **Veröffentlichen-WebApplication.ps1** rufen Sie das Skript, und veröffentlichen Sie die Webanwendung wie in einem der folgenden Beispiele dargestellt. Wenn Sie die Standardeinstellungen für alle anderen Argumente, wie dem Namen des Abonnements, Name des Pakets, virtuellen Computeranmeldeinformationen oder Datenbankbenutzers-Server veröffentlichen müssen, können Sie diesen Parameter angeben. Verwenden der **– ausführlich** Option aus, um weitere Informationen zu den Fortschritt des Veröffentlichungsprozesses anzuzeigen.

```
Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
–SubscriptionName Contoso `
-WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
-DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
-Verbose
```

Wenn Sie einen virtuellen Computer erstellen, sieht der Befehl folgendermaßen aus. Außerdem wird veranschaulicht, wie die Anmeldeinformationen für mehrere Datenbanken angegeben. Für den virtuellen Computern, die diese Skripts zu erstellen, ist das SSL-Zertifikat nicht von einer vertrauenswürdigen Stammzertifizierungsstelle. Daher müssen Sie die Option **– AllowUntrusted** verwenden.

```
Publish-WebApplication.ps1 `
-Configuration C:\Path\ADVM-VM-test.json `
-SubscriptionName Contoso `
-WebDeployPackage C:\Path\ADVM.zip `
-AllowUntrusted `
-VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
-DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
-Verbose
```

Das Skript kann Datenbanken erstellen, aber keine Datenbankserver erstellt. Wenn Sie einen Datenbankserver erstellen möchten, können Sie die Funktion **Neu-AzureSqlDatabaseServer** im Modul Azure verwenden.

## <a name="customizing-and-extending-the-publish-scripts"></a>Anpassen und erweitern die Skripts veröffentlichen

Sie können das Skript veröffentlichen und JSON-Konfigurationsdatei anpassen. Die Funktionen in der Windows PowerShell-Modul **AzureWebAppPublishModule.psm1** werden nicht geändert werden sollen. Wenn Sie nur eine andere Datenbank angeben oder einige der Eigenschaften des virtuellen Computers ändern möchten, bearbeiten Sie die JSON-Konfigurationsdatei. Wenn Sie die Funktionalität des Skripts zu automatisieren, erstellen und Testen des Projekts erweitern möchten, können Sie die Funktion Stubs in **Veröffentlichen-WebApplication.ps1**implementieren.

Zum Erstellen des Projekts zu automatisieren, Hinzufügen von Code, die MSBuild ruft `New-WebDeployPackage` wie in diesem Beispiel dargestellt. Der Pfad zum Befehl MSBuild unterscheidet sich je nach der Version von Visual Studio, die Sie installiert haben. Um den richtigen Pfad zu gelangen, können Sie die Funktion **Get-MSBuildCmd**, wie im folgenden Beispiel gezeigt.

### <a name="to-automate-building-your-project"></a>Zum Erstellen des Projekts automatisieren

1. Hinzufügen der `$ProjectFile` Parameter im Abschnitt globale Parameter.

```
[Parameter(Mandatory = $false)]
  [ValidateScript({Test-Path $_ -PathType Leaf})]
  [String]
  $ProjectFile,
```

1. Kopieren Sie die Funktion `Get-MSBuildCmd` in Ihrer Skriptdatei.

```
function Get-MSBuildCmd
{
        process
{

             $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                   Sort-Object {[double]$_.PSChildName} -Descending |
                                   Select-Object -First 1 |
                                   Get-ItemProperty -Name MSBuildToolsPath |
                                   Select -ExpandProperty MSBuildToolsPath
       
            $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

        return Get-Item $path
    }
}
```

1. Ersetzen Sie `New-WebDeployPackage` mit die folgenden code, und Ersetzen Sie die Platzhalter in der Zeile bauen `$msbuildCmd`. Dieser Code ist für Visual Studio 2015. Wenn Sie Visual Studio 2013 verwenden, ändern Sie die Eigenschaft **VisualStudioVersion** unter zu `12.0`.

```
function New-WebDeployPackage
{
    #Write a function to build and package your web application
      
#To build your web application, use MsBuild.exe. For help, see MSBuild Command-Line Reference at: http://go.microsoft.com/fwlink/?LinkId=391339
      
Write-VerboseWithTime 'Build-WebDeployPackage: Start'
      
$msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory
      
Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
      
#Start execution of the build command
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru
      
if ($job.ExitCode -ne 0)
{
throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain the project name
$projectName = (Get-Item $ProjectFile).BaseName
      
#Construct the path to web deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName
      
      
#Get the full path for the web deploy zip package. This is required for MSDeploy to work
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir
      
Write-VerboseWithTime 'Build-WebDeployPackage: End'
      
return $WebDeployPackage
}
```

1. Rufen Sie die `New-WebDeployPackage` (Funktion), bevor Sie diese Zeile: `$Config = Read-ConfigFile $Configuration` von Web apps oder `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` für virtuelle Computer.

```
if($ProjectFile)
{
$WebDeployPackage = New-WebDeployPackage
}
```

1. Rufen Sie das angepasste Skript über die Befehlszeile mit Übergabe der `$Project` Argument, wie im folgenden Beispielbefehlszeile.

```
.\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
-ProjectFile ..\WebApplication5\WebApplication5.csproj `
-VMPassword @{Name="VMUser";Password="Test.123"} `
-AllowUntrusted `
-Verbose
```

Fügen Sie zum Testen der Anwendung zu automatisieren, Code zu `Test-WebApplication`. Achten Sie darauf, dass die Zeilen im **Veröffentlichen-WebApplication.ps1** kommentieren, wo diese Funktionen aufgerufen werden. Wenn eine Implementierung nicht bereitgestellt werden, können Sie manuell Ihres Projekts mit Visual Studio erstellen, und führen Sie das Skript Veröffentlichen in Azure veröffentlichen.

## <a name="publishing-function-summary"></a>Zusammenfassung für die Veröffentlichung (Funktion)

Erhalten von Hilfe zu Funktionen an der Windows PowerShell-Eingabeaufforderung können, verwenden Sie den Befehl `Get-Help function-name`. Die Hilfe enthält Parameter Hilfe- und Beispielen. Denselben Hilfetext wird auch in den Skript-Quelldateien **AzureWebAppPublishModule.psm1** und **Veröffentlichen-WebApplication.ps1**. Das Skript und die Hilfe werden in der Visual Studio-Sprache lokalisiert.

**AzureWebAppPublishModule**

|Funktionsname|Beschreibung|
|---|---|
|Hinzufügen von AzureSQLDatabase|Erstellt eine neue SQL Azure-Datenbank.|
|Hinzufügen von AzureSQLDatabases|SQL Azure-Datenbanken erstellt aus den Werten in der Konfiguration JSON-Datei, die Visual Studio generiert.|
|Hinzufügen von AzureVM|Erstellt eine Azure-virtuellen Computern und gibt die URL des den bereitgestellten virtuellen Computer. Die Funktion richtet die erforderlichen Komponenten und ruft dann die **Neu AzureVM** Funktion (Azure Modul) zum Erstellen eines neuen virtuellen Computers.|
|Hinzufügen von AzureVMEndpoints|Fügt neue Eingabewerte Endpunkte eines virtuellen Computers und des virtuellen Computers mit dem neuen Endpunkt gibt.|
|Hinzufügen von AzureVMStorage|Erstellt ein neues Konto mit Azure-Speicher im aktuellen-Abonnement. Der Name des Kontos beginnt mit "Devtest" gefolgt von einer eindeutigen alphanumerische Zeichenfolge. Die Funktion gibt den Namen des neuen Speicher-Kontos. Sie müssen entweder einen Speicherort oder eine Gruppe Zugehörigkeit für das neue Speicherkonto angeben.|
|Hinzufügen von AzureWebsite|Erstellt eine Website mit dem angegebenen Namen und Speicherort. Diese Funktion ruft die **Neu AzureWebsite** Funktion im Modul "Azure". Wenn das Abonnement bereits eine Website mit dem angegebenen Namen enthält, wird diese Funktion die Website erstellt und eine Website zurück. Andernfalls gibt es `$null`.|
|Sicherung-Abonnements|Speichert das aktuelle Azure-Abonnement in der `$Script:originalSubscription` Variable Skript Umfang. Diese Funktion speichert das aktuelle Azure-Abonnement (durch abgerufenen `Get-AzureSubscription -Current`) und deren Speicher-Konto und das Abonnement, die von diesem Skript geändert wird (in der Variablen gespeicherten `$UserSpecifiedSubscription`) und deren Speicherkonto Skript Umfang. Speichern Sie die Werte ein, Sie können eine Funktion, wie `Restore-Subscription`, um das ursprüngliche aktuellen Abonnement und Speicher Konto zum aktuellen Status wiederherstellen, wenn der aktuelle Status geändert hat.|
|Suchen-AzureVM|Ruft die angegebene Azure-virtuellen Computern ab.|
|Format-DevTestMessageWithTime|Voran Datum und Uhrzeit zu einer Nachricht an. Diese Funktion wurde für Nachrichten, die in die Fehler und ausführlich Streams geschrieben.|
|Get-AzureSQLDatabaseConnectionString|Stellt eine Verbindungszeichenfolge für eine Verbindung mit einer SQL Azure-Datenbank zusammen.|
|Get-AzureVMStorage|Gibt den Namen des ersten Speicherkonto mit dem Namensmuster "Devtest*" (ohne Berücksichtigung von Groß-und Kleinschreibung) in der angegebenen Position oder Zugehörigkeit. Wenn die "Devtest*" Speicher-Konto entsprechen nicht den Speicherort oder die Zugehörigkeit Gruppe, die Funktion wird ignoriert. Sie müssen entweder einen Speicherort oder eine Gruppe für die Zugehörigkeit angeben.|
|Get-MSDeployCmd|Gibt einen Befehl zum Ausführen des Tools MsDeploy.exe.|
|Neue AzureVMEnvironment|Sucht oder erstellt eine virtuellen Computern im Abonnement, das die Werte in der Konfigurationsdatei JSON entspricht.|
|Veröffentlichen WebPackage|Veröffentlichen Paket MsDeploy.exe verwendet und ein Web. ZIP-Datei in einer Website Ressourcen bereitstellen. Diese Funktion nicht Ausgabe generiert. Wenn Sie der Anruf an MSDeploy.exe fehlschlägt, löst die Funktion eine Ausnahme aus. Eine detailliertere Ausgabe verwenden, um die **-ausführliche** Option.|
|Veröffentlichen WebPackageToVM|Die Parameterwerte überprüft und dann Ruft die **Veröffentlichen WebPackage** Funktion.|
|Lesen-ConfigFile|Überprüft die JSON-Konfigurationsdatei und gibt eine Hashtabelle der ausgewählten Werte an.|
|Wiederherstellen-Abonnements|Setzt das aktuelle Abonnement in das ursprüngliche Abonnement zurück.|
|Test-AzureModule|Gibt `$true` ist die installierten Azure-Modulversion 0.7.4 oder höher. Gibt `$false` Wenn das Modul ist nicht installiert oder eine frühere Version ist. Diese Funktion hat keine Parameter.|
|Test-AzureModuleVersion|Gibt `$true` ist die Version des Moduls den Azure 0.7.4 oder höher. Gibt `$false` Wenn das Modul ist nicht installiert oder eine frühere Version ist. Diese Funktion hat keine Parameter.|
|Test-HttpsUrl|Konvertiert die eingegebene URL zu einem Objekt System.Uri. Gibt `$True` , wenn die URL absolut und Schema Https ist. Gibt `$false` ist die URL relativ, deren Schema nicht HTTPS ist oder die eingegebene Zeichenfolge nicht in einer URL konvertiert werden.|
|Test-Element|Gibt `$true` , wenn eine Eigenschaft oder Methode ein Mitglied des Objekts ist. Andernfalls gibt `$false`.|
|Schreiben-ErrorWithTime|Schreibt eine Fehlermeldung mit dem Präfix der aktuellen Uhrzeit. Diese Funktion ruft die **Format-DevTestMessageWithTime** -Funktion, um die Zeit vor dem Schreiben der Nachricht in den Fehlerstream vorangestellt werden.|
|Schreiben-HostWithTime|Schreibt eine Meldung in das Host-Programm (**Schreiben-Host**) mit dem Präfix der aktuellen Uhrzeit. Das Verhalten beim Schreiben in das Host-Programm wird aktualisiert. Die meisten Programme schreiben, hosten Windows PowerShell diese Nachrichten in standard Ausgabe.|
|Schreiben-VerboseWithTime|Schreibt eine ausführliche Meldung mit dem Präfix der aktuellen Uhrzeit. Da es **Schreiben-ausführlich**Ruft, wird die Meldung nur, wenn das Skript mit dem Parameter **ausführlich** ausgeführt wird oder die **VerbosePreference** -Einstellung, klicken Sie auf **Weiter festgelegt wurde**.|

**Veröffentlichen WebApplication**

|Funktionsname|Beschreibung|
|---|---|
|Neue AzureWebApplicationEnvironment|Azure Ressourcen erstellt, z. B. einer Website oder virtuellen Computern.|
|Neue WebDeployPackage|Diese Funktion ist nicht implementiert. Sie können diese Funktion zum Erstellen des Projekts Befehle hinzufügen.|
|Veröffentlichen AzureWebApplication|Veröffentlicht eine Webanwendung in Azure.|
|Veröffentlichen WebApplication|Erstellt und Web Apps, virtuellen Computern, SQL-Datenbanken und Speicherkonten für Visual Studio Web-Projekt bereitstellt.|
|Test-WebApplication|Diese Funktion ist nicht implementiert. Sie können diese Funktion zum Testen der Anwendung Befehle hinzufügen.|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu PowerShell-Skripting lesen [Scripting mit Windows PowerShell](https://technet.microsoft.com/library/bb978526.aspx) und andere Azure PowerShell-Skripts in der [Script Center](https://azure.microsoft.com/documentation/scripts/)finden Sie unter.
