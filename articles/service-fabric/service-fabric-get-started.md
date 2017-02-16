<properties
   pageTitle="Einrichten Ihrer Entwicklungsumgebung | Microsoft Azure"
   description="Installieren Sie die Laufzeit, SDK und Tools, und erstellen Sie einen lokale Entwicklung Cluster. Nach Abschluss der diese Installation werden Sie zum Erstellen von Applications bereit."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/26/2016"
   ms.author="ryanwi"/>

# <a name="prepare-your-development-environment"></a>Vorbereiten der Entwicklungsumgebung

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 So erstellen und Ausführen von [Applications Azure Service Fabric] [ 1] auf Ihrem Entwicklungscomputer, installieren Sie die Laufzeit, SDK und Tools. Sie benötigen ferner Ausführung von Windows PowerShell-Skripts, die im Lieferumfang des SDKS aktivieren.

## <a name="prerequisites"></a>Erforderliche Komponenten
### <a name="supported-operating-system-versions"></a>Unterstützte Betriebssysteme
Die folgenden Betriebssystem-Versionen werden für die Entwicklung unterstützt:

- Windows 7
- Windows 8/Windows 8.1
- Windows Server 2012 R2
- Windows-10

>[AZURE.NOTE] Windows 7, Windows PowerShell 2.0 nur enthält standardmäßig. Dienst Fabric PowerShell-Cmdlets ist PowerShell 3.0 oder höher erforderlich. Sie können [das Herunterladen von Windows PowerShell 5.0] [ powershell5-download] vom Microsoft Download Center.

## <a name="install-the-runtime-sdk-and-tools"></a>Installieren Sie die Laufzeit, SDK und tools

Die Web Platform Installer bietet zwei Konfigurationen für die Entwicklung von Fabric Service:

- [Installieren Sie den Dienst Fabric Laufzeit, SDK und Tools für Visual Studio 2015 (erfordert Visual Studio 2015 Update 2 oder höher)][full-bundle-vs2015]
- [Installieren Sie die Laufzeit Dienst Fabric und nur SDK (ohne Visual Studio-Tools)][core-sdk]

## <a name="enable-powershell-script-execution"></a>Aktivieren Sie Ausführung der PowerShell-Skript

Dienst Fabric verwendet Windows PowerShell-Skripts zum Erstellen eines lokalen Entwicklung Clusters sowie für die Bereitstellung von Applications aus Visual Studio. Standardmäßig blockiert Windows diese Skripts automatisch ausgeführt. Um diese zu aktivieren, müssen Sie die PowerShell Ausführungsrichtlinie ändern. Öffnen Sie PowerShell als Administrator, und geben Sie den folgenden Befehl aus:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Nächste Schritte
Jetzt, da Sie die Einrichtung Ihrer Entwicklungsumgebung abgeschlossen haben, starten Sie erstellen und Ausführen von apps.

- [Erstellen Sie Ihrer erste Fabric Service-Anwendung in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
- [Informationen Sie zum Bereitstellen und Verwalten von Applications auf Ihrem lokalen cluster](service-fabric-get-started-with-a-local-cluster.md)
- [Erfahren Sie mehr über die Programmierung Modelle: zuverlässigen Diensten und zuverlässigen Akteuren](service-fabric-choose-framework.md)
- [Der Dienst Fabric Codebeispielen auf GitHub Auschecken](https://aka.ms/servicefabricsamples)
- [Visualisieren Sie Ihren Cluster mithilfe von Dienst Fabric-Explorer](service-fabric-visualizing-your-cluster.md)
- [Folgen Sie den Pfad Dienst Fabric Schulung, um eine umfassende Einführung in die Plattform zu erhalten](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Dienst Fabric Campaign Seite"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "IM VERGLEICH MIT EINER RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Im Vergleich mit einer 2015 WebPI link"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI link"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI link"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
