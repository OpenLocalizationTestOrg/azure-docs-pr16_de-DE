<properties
   pageTitle="Automatisierung der Bereitstellung der Anwendung mit virtuellen Computern Erweiterungen | Microsoft Azure"
   description="Azure-virtuellen Computern DotNet Core Lernprogramm"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Bereitstellung der Anwendung mit Azure Ressourcenmanager Vorlagen

Nachdem alle Azure infrastrukturelle Anforderungen identifiziert und eine Vorlage für die Bereitstellung übersetzt wurden, muss die Bereitstellung der tatsächlichen Anwendung berücksichtigt werden. Hier die Bereitstellung der Anwendung wird bei der tatsächlichen Anwendungsbinärdateien auf Azure Ressourcen verweisen. Das Beispiel Musik Store .net Core und IIS installiert und auf jedem virtuellen Computer konfiguriert werden müssen. Die Musik Store Binärdateien auf den virtuellen Computern installiert werden müssen, und die Datenbank Musik vorab erstellt.

Dieses Dokument beschreibt, wie virtuellen Computern Erweiterungen Bereitstellung der Anwendung und Konfiguration bis zur Azure-virtuellen Computern automatisieren können. Alle Abhängigkeiten und eindeutige Konfigurationen werden hervorgehoben. Für optimale Ergebnisse, vorab eine Instanz der Lösung Azure-Abonnement und Arbeit zusammen mit der Vorlage Ressourcenmanager Azure bereitgestellt werden. Die vollständige Vorlage – [Musik Store Bereitstellung unter Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-Windows)finden Sie hier.

## <a name="configuration-script"></a>Konfigurationsskript

Erweiterungen virtuellen Computern sind spezielle Programme, die für virtueller Computer für die Konfiguration Automatisierung ausgeführt. Extensions stehen viele bestimmten Zwecken wie Anti-Virus, für die Protokollierung und Docker Konfiguration. Die Erweiterung benutzerdefinierte Skripts kann zum Ausführen eines Skripts anhand eines virtuellen Computers verwendet werden. Mit der Stichprobe Musik Store ist es auf die Erweiterung benutzerdefiniertes Skript zum Konfigurieren der Windows-virtuellen Computern und installieren die Anwendung Musik Store.

Überprüfen Sie bevor Sie mit detaillierten Informationen zu wie virtuellen Computern Erweiterungen in einer Vorlage Azure Ressourcenmanager deklariert sind das Skript, das ausgeführt wird. Dieses Skript konfiguriert das Windows virtuellen Computern, um die Musik Store-Anwendung zu hosten. Beim Ausführen, installiert das Skript alle erforderlichen Software, installieren die Anwendung von Musik Store aus Datenquellen-Steuerelement, und Vorbereiten der Datenbank. 

> In diesem Beispiel wird für die Vorführung auf.

```none
Param (
    [string]$user,
    [string]$password,
    [string]$sqlserver
)

# firewall
netsh advfirewall firewall add rule name="http" dir=in action=allow protocol=TCP localport=80

# folders
New-Item -ItemType Directory c:\temp
New-Item -ItemType Directory c:\music

# install iis
Install-WindowsFeature web-server -IncludeManagementTools

# install dot.net core sdk
Invoke-WebRequest http://go.microsoft.com/fwlink/?LinkID=615460 -outfile c:\temp\vc_redistx64.exe
Start-Process c:\temp\vc_redistx64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkID=809122 -outfile c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe
Start-Process c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkId=817246 -outfile c:\temp\DotNetCore.WindowsHosting.exe
Start-Process c:\temp\DotNetCore.WindowsHosting.exe -ArgumentList '/quiet' -Wait

# download / config music app
Invoke-WebRequest  https://github.com/Microsoft/dotnet-core-sample-templates/raw/master/dotnet-core-music-windows/music-app/music-store-azure-demo-pub.zip -OutFile c:\temp\musicstore.zip
Expand-Archive C:\temp\musicstore.zip c:\music
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceserver>", $sqlserver } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceuser>", $user } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replacepass>", $password } | Set-Content C:\music\config.json

# workaround for db creation bug
Start-Process 'C:\Program Files\dotnet\dotnet.exe' -ArgumentList 'c:\music\MusicStore.dll'

# configure iis
Remove-WebSite -Name "Default Web Site"
Set-ItemProperty IIS:\AppPools\DefaultAppPool\ managedRuntimeVersion ""
New-Website -Name "MusicStore" -Port 80 -PhysicalPath C:\music\ -ApplicationPool DefaultAppPool
& iisreset
```

## <a name="vm-script-extension"></a>Virtueller Computer Skript-Erweiterung

Virtueller Computer Erweiterungen können durch die Erweiterung Ressource in der Vorlage Azure Ressourcenmanager einschließlich anhand eines virtuellen Computers Erstellungszeit ausgeführt werden. Die Erweiterung kann mit dem Assistenten für Visual Studio-Ressource zum Hinzufügen oder durch Einfügen von gültigen JSON in der Vorlage hinzugefügt werden. Skript-Erweiterung Ressource, die innerhalb der Ressource virtuellen Computern geschachtelt ist. Dies kann im folgenden Beispiel angezeigt werden.

Führen Sie diesen Link, um das JSON-Beispiel innerhalb der Ressourcenmanager Vorlage – [Virtueller Computer Skript Erweiterung](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339)finden Sie unter. 

Beachten Sie in der unter JSON, dass das Skript in GitHub gespeichert ist. Dieses Skript kann auch in Azure Blob Storage gespeichert werden. Darüber hinaus zulassen Azure Ressourcenmanager Vorlagen die Skript Ausführungszeichenfolge erstellt werden, sodass Vorlage Parameterwerte als Parameter für die Ausführung von Skripts verwendet werden können. In diesem Fall Daten beim Bereitstellen von Vorlagen bereitgestellt werden, und diese Werte dann beim Ausführen der Skripts verwendet werden können.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
  }
}
```

Weitere Informationen zur Verwendung der Erweiterung benutzerdefinierter Skripts finden Sie unter [benutzerdefinierte Skripts Erweiterungen mit Ressourcenmanager Vorlagen](./virtual-machines-windows-extensions-customscript.md).

## <a name="next-step"></a>Als Nächstes

<hr>

[Weitere Azure Ressourcenmanager Vorlagen durchsuchen](https://github.com/Azure/azure-quickstart-templates)
