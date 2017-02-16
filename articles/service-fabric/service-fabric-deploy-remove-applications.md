<properties
   pageTitle="Bereitstellung von Fabric Service-Anwendung | Microsoft Azure"
   description="Gewusst wie: Bereitstellen und Entfernen von Applications Dienst Struktur"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="deploy-and-remove-applications-using-powershell"></a>Bereitstellen und Entfernen von Applications mithilfe der PowerShell

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Nachdem ein [Anwendungstyp verpackt][10], es ist für die Bereitstellung in einem Cluster Azure Service Fabric bereit. Bereitstellung umfasst die folgenden drei Schritte:

1. Hochladen der Anwendungspakets
2. Registrieren Sie sich den Anwendungstyp
3. Erstellen Sie die Instanz der Anwendung

>[AZURE.NOTE] Wenn Sie Visual Studio für die Bereitstellung und Debuggen von auf Ihren Cluster lokale Entwicklung verwenden, werden die folgenden Schritte aus automatisch durch ein PowerShell-Skript im Ordner "Skripts" des Anwendungsprojekts behandelt. In diesem Artikel erklärt, was diese Skripts tun, damit Sie die gleichen Operationen außerhalb von Visual Studio ausführen können.

## <a name="upload-the-application-package"></a>Hochladen der Anwendungspakets

Hochladen der Anwendungspakets verschoben an einem Speicherort, der von internen Service Fabric-Komponenten zugegriffen. PowerShell können Sie den Upload ausführen. Beginnen Sie bevor Sie eine beliebige PowerShell-Befehlen in diesem Artikel ausführen immer [Verbinden-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) für die Verbindung zum Dienst Fabric Cluster mit.

Angenommen, Sie haben einen Ordner namens *MyApplicationType* enthält, die die notwendigen Anwendungsmanifest, Dienst Manifeste und Code/Config/Data Pakete. Der Befehl [Kopieren-ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125905.aspx) lädt das Paket mit dem Cluster Image Store hoch. Das Cmdlet **Get-ImageStoreConnectionStringFromClusterManifest** , das also Teil der Dienst Fabric SDK PowerShell-Modul, wird das Bild Store Verbindungszeichenfolge verwendet.  Um das Modul SDK zu importieren, führen Sie Folgendes aus:

```
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Sie können ein Anwendungspaket von *C:\users\ryanwi\Documents\Visual Studio 2015\Projects\MyApplication\myapplication\pkg\debug* in *c:\temp\MyApplicationType* (Umbenennen Verzeichnis "Debuggen", "MyApplicationType") kopieren. Im folgende Beispiel uploads das Paket an:

~~~
PS C:\temp> dir

    Directory: c:\temp


Mode                LastWriteTime         Length Name                                                                                   
----                -------------         ------ ----                                                                                   
d-----        8/12/2016  10:23 AM                MyApplicationType                                                                          

PS C:\temp> tree /f .\Stateless1Pkg

C:\TEMP\MyApplicationType
│   ApplicationManifest.xml
|
└───Stateless1Pkg
  	|   ServiceManifest.xml
    │
    └───Code
    │   │  Microsoft.ServiceFabric.Data.dll
    │   │  Microsoft.ServiceFabric.Data.Interfaces.dll
    │   │  Microsoft.ServiceFabric.Internal.dll
    │   │  Microsoft.ServiceFabric.Internal.Strings.dll
    │   │  Microsoft.ServiceFabric.Services.dll
    │   │  ServiceFabricServiceModel.dll
    │   │  MyService.exe
    │   │  MyService.exe.config
    │   │  MyService.pdb
    │   │  System.Fabric.dll
    │   │  System.Fabric.Strings.dll
    │   │
    │   └───en-us
  	|         Microsoft.ServiceFabric.Internal.Strings.resources.dll
  	|         System.Fabric.Strings.resources.dll
  	|
    ├───Config
    │     Settings.xml
    │
    └───Data
          init.dat

PS C:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath MyApplicationType -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
Copy application package succeeded

PS D:\temp>
~~~

## <a name="register-the-application-package"></a>Registrieren Sie sich das Anwendungspaket

Registrieren des Anwendungspakets macht den Anwendungstyp und die Version deklariert im Anwendungsmanifest verfügbar für die Verwendung. Das System liest das Paket, das im vorherigen Schritt hochgeladen, vergewissern Sie sich das Paket (entspricht dem [Test-ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125950.aspx) lokal ausgeführt), der Inhalt des Pakets verarbeiten und verarbeitete Paket an einem Systemspeicherort internen kopieren.

~~~
PS D:\temp> Register-ServiceFabricApplicationType MyApplicationType
Register application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp>
~~~

Der Befehl [Register-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125958.aspx) gibt erst das System das Anwendungspaket erfolgreich kopiert wurde. Wie lange dauert, hängt von der Inhalt des Anwendungspakets ab. Falls erforderlich, kann der **TimeoutSec -** Parameter zu einen längeren Timeout angeben verwendet werden. (Der Standard-Timeoutwert ist 60 Sekunden.)

Der [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125871.aspx) -Befehl listet alle Versionen der Anwendung erfolgreich registrierten Typ.

## <a name="create-the-application"></a>Erstellen Sie die Anwendung

Sie können eine Anwendung instanziieren, mit einer beliebigen Anwendung Typversion, die über den Befehl [Neu-ServiceFabricApplication](https://msdn.microsoft.com/library/mt125913.aspx) erfolgreich registriert wurde. Der Name der jede Anwendung muss anfangen, mit der *Fabric:* -Schemanamen und für jede Anwendungsinstanz eindeutig sein. Zu diesem Zeitpunkt werden alle im Anwendungsmanifest von den Zielanwendungstyp definierten Standarddienste erstellt.

~~~
PS D:\temp> New-ServiceFabricApplication fabric:/MyApp MyApplicationType AppManifestVersion1

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationStatus      : Ready
HealthState            : OK
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/MyService
ServiceKind            : Stateless
ServiceTypeName        : MyServiceType
IsServiceGroup         : False
ServiceManifestVersion : SvcManifestVersion1
ServiceStatus          : Active
HealthState            : Ok

PS D:\temp>
~~~

Der [Get-ServiceFabricApplication](https://msdn.microsoft.com/library/mt163515.aspx) -Befehl listet alle Anwendungsinstanzen, die erfolgreich, zusammen mit den allgemeinen Status erstellt wurden.

Der [Get-ServiceFabricService](https://msdn.microsoft.com/library/mt125889.aspx) -Befehl listet alle Dienstinstanzen, die innerhalb einer bestimmten Anwendungsinstanz erfolgreich erstellt wurden. Standarddienste (falls vorhanden) werden hier aufgeführt.

Für alle angegebenen Version eines registrierten Anwendungstyps können mehrere Anwendungsinstanzen erstellt werden. Jede Anwendungsinstanz isoliert, mit dem eigenen Arbeit Verzeichnis und Prozess ausgeführt wird.

## <a name="remove-an-application"></a>Entfernen einer Anwendungs

Wenn eine Instanz der Anwendung nicht mehr benötigt wird, können Sie es dauerhaft mithilfe des Befehls [Entfernen-ServiceFabricApplication](https://msdn.microsoft.com/library/mt125914.aspx) entfernen. Dieser Befehl entfernt automatisch alle Dienste, die mit der Anwendung ebenfalls, gehören alle Dienststatus dauerhaft entfernt. Dieser Vorgang kann nicht rückgängig gemacht und Anwendungsstatus nicht wiederhergestellt werden.

~~~
PS D:\temp> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS D:\temp> Get-ServiceFabricApplication
PS D:\temp>
~~~

Wenn Sie eine bestimmte Version der Art der Anwendung nicht mehr benötigt wird, sollten Sie es mithilfe des Befehls [' Registrierung aufheben '-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125885.aspx) aufgehoben werden. Aufheben der Registrierung nicht verwendeter Datentypen gibt der Inhalt des Pakets Anwendung dieses Typs auf dem Bild Store verwendeten Speicherplatz frei. Ein Anwendungstyp kann aufgehoben werden, solange Sie keine Programme werden davor instanziiert und nicht ausstehend Anwendung Upgrades sind darauf verweisen.

~~~
PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp> Unregister-ServiceFabricApplicationType MyApplicationType AppManifestVersion1
Unregister application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

PS D:\temp>
~~~

## <a name="troubleshooting"></a>Behandlung von Problemen

### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopieren-ServiceFabricApplicationPackage gefragt werden, für eine ImageStoreConnectionString

Die Dienst Fabric SDK Umgebung sollte die korrekten Standardeinstellungen einrichten bereits. Aber bei Bedarf sollte die ImageStoreConnectionString für alle Befehle entsprechen den Wert, den der Dienst Fabric Cluster verwendet wird. Sie können dies im Cluster Manifest über den Befehl [Get-ServiceFabricClusterManifest](https://msdn.microsoft.com/library/mt126024.aspx) abgerufen finden:

~~~
PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType

cmdlet Copy-ServiceFabricApplicationPackage at command pipeline position 1
Supply values for the following parameters:
ImageStoreConnectionString:

PS D:\temp> Get-ServiceFabricClusterManifest
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]

PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType -ImageStoreConnectionString file:D:\ServiceFabric\Data\ImageStore
Copy application package succeeded

PS D:\temp>
~~~

## <a name="next-steps"></a>Nächste Schritte

[Upgrade der Fabric Service-Anwendung](service-fabric-application-upgrade.md)

[Dienst Fabric Gesundheit Einführung](service-fabric-health-introduction.md)

[Diagnose und Problembehandlung bei einem Dienst Fabric-Dienst](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Modellieren einer Anwendungs Dienst Struktur](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
