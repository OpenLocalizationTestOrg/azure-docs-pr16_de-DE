<properties
   pageTitle="Anleitung zum Konvertieren von Web- und Worker-Rollen zu Dienst Fabric zustandsloser Dienste | Microsoft Azure"
   description="In diesem Handbuch vergleicht Cloud Services-Web und Worker-Rollen und Dienst Fabric zustandsloser Dienste zum Migrieren von Cloud Services auf Dienst Fabric-Hilfe."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>
 
# <a name="guide-to-converting-web-and-worker-roles-to-service-fabric-stateless-services"></a>Leitfaden Sie zum Konvertieren von Web und Worker-Rollen in Dienst Fabric zustandsloser Dienste

Dieser Artikel beschreibt, wie Ihre Cloud Services Web und Worker-Rollen zum Dienst Fabric zustandsloser Dienste migrieren. Dies ist die einfachste Migrationspfad aus der Cloud Services mit Service Fabric für Applikationen ungefähr unverändert bleibt, deren allgemeine Architektur vertraut ist.

## <a name="cloud-service-project-to-service-fabric-application-project"></a>Projekt, das Projekt Fabric Service-Anwendung Cloud-Dienst
 
 Eines Projekts Cloud-Dienst und eines Projekts Fabric Anwendungsdienst weisen eine ähnliche Struktur und beide darstellen die Bereitstellung Maßeinheit für eine Anwendung – d. h., sie jeweils definieren das vollständige Paket, das bereitgestellt wird, um Ihre Anwendung auszuführen. Ein Projekt Cloud-Dienst enthält Web oder Worker-Rollen. Auf ähnliche Weise enthält ein Projekt Fabric Anwendungsdienst einen oder mehrere Dienste. 

Der Unterschied besteht, dass das Projekt Cloud-Dienst die Anwendung Bereitstellung mit einer Bereitstellung einschließen und somit virtueller Computer Konfiguration Einstellungen darin enthält, während das Projekt Fabric Anwendungsdienst definiert nur eine Anwendung, die in eine Reihe von vorhandenen virtuellen Computern in einem Dienst Fabric Cluster bereitgestellt werden soll. Der Dienst Fabric Cluster selbst ist nur einmal, über einen Cloud-Vorlage oder über das Portal Azure und mehrere Dienst Fabric Applikationen können bereitgestellt werden darauf.

![Vergleich von Dienst Fabric und Cloud Services-Projekt][3]
 
## <a name="worker-role-to-stateless-service"></a>Worker-Rolle statusfreie Dienst 

Im Prinzip stellt eine Worker-Rolle eine statusfreie Arbeitsbelastung, d. h., jeder Instanz des die Arbeitsbelastung identisch ist und zu einem beliebigen Zeitpunkt Anfragen einer beliebigen Instanz weitergeleitet werden können. Beachten Sie die vorherige Anforderung wird jede Instanz nicht erwartet. Status, der für die Arbeitsbelastung ausgeführt wird, wird von einer externen Zustand Store, z. B. Azure Table Storage oder Azure Dokument DB verwaltet. Dienst Struktur wird diese Art von Arbeitsbelastung durch eine statusfreie Dienst dargestellt. Die einfachste Vorgehensweise zum Migrieren von Worker-Rolle auf Dienst Fabric kann durch Konvertieren von Worker-Rolle Code in einen statusfreie Service erfolgen.

![Worker-Rolle statusfreie Dienst][4]

## <a name="web-role-to-stateless-service"></a>Webrolle statusfreie Dienst

Ähnlich wie Worker-Rolle, eine Webrolle auch stellt eine statusfreie Arbeitsbelastung und daher im Prinzip es auch zugeordnet werden kann in einen Dienst Fabric statusfreie Service. Jedoch im Gegensatz zu Webrollen unterstützt Dienst Fabric IIS. Zum Migrieren einer Anwendung aus einer Rolle Web und einem statusfreie Dienst zuerst verschieben, um ein Web-Framework, das Self gehostet werden und hängt nicht IIS oder System.Web, z. B. ASP.NET Core 1 ist erforderlich.

**Anwendung** | **Unterstützt** | **Migrationspfad**
--- | --- | ---
ASP.NET Web Forms | Nein | Konvertieren Sie in MVC ASP.NET Core 1
ASP.NET-MVC | Mit der Migration | Upgrade auf ASP.NET Core 1 MVC
ASP.NET Web API | Mit der Migration | Verwenden Sie lokal gehosteten Server oder ASP.NET Core 1
ASP.NET Core 1 | Ja | N/V

## <a name="entry-point-api-and-lifecycle"></a>Einstiegspunkt-API und Lebenszyklus

Worker-Rolle und Dienst Fabric service-APIs Angebot ähnliche Eintrittspunkte: 

**Einstiegspunkt** | **Worker-Rolle** | **Dienst Fabric service**
--- | --- | ---
Verarbeitung | `Run()` | `RunAsync()`
Virtueller Computer starten | `OnStart()` | N/V
Virtueller Computer beenden | `OnStop()` | N/V
Öffnen der Zuhörer für Client-Anfragen | N/V | <ul><li> `CreateServiceInstanceListener()`für statusfreie</li><li>`CreateServiceReplicaListener()`für stateful</li></ul>

### <a name="worker-role"></a>Worker-Rolle

```C#

using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
        }

        public override bool OnStart()
        {
        }

        public override void OnStop()
        {
        }
    }
}

```

### <a name="service-fabric-stateless-service"></a>Statusfreie Fabric-Dienst

```C#

using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace Stateless1
{
    public class Stateless1 : StatelessService
    {
        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
        }

        protected override Task RunAsync(CancellationToken cancelServiceInstance)
        {
        }
    }
}

```

Beide verfügen über eine primäre "ausführen" überschreiben, in dem die Verarbeitung beginnen soll. Dienst Fabric Services kombinieren `Run`, `Start`, und `Stop` in einem einzigen Einstiegspunkt `RunAsync`. Des Diensts beginnen soll, wann arbeiten `RunAsync` beginnt und sollte nicht mehr, sobald die `RunAsync` der Methode CancellationToken signalisiert wird. 

Es gibt verschiedene grundlegende Unterschiede zwischen den Lebenszyklus und die Gültigkeitsdauer des Worker-Rollen und Dienst Fabric Services:

 - **Lebenszyklus:** Der größte Unterschied ist, dass eine Worker-Rolle eines virtuellen Computers und daher Lebenszyklus zu den virtuellen Computer, wozu auch die Ereignisse für beim Starten und Beenden des virtuellen Computer verknüpft ist. Ein Dienst Fabric-Dienst hat einen Lebenszyklus, der von der Lebenszyklus virtueller Computer getrennt ist, damit es nicht enthalten ist Ereignisse beim Hostnamen virtuellen Computers oder der Computer wird gestartet, und beenden, während sie nicht verwandt sind.

 - **Lebensdauer:** Eine Instanz des Worker-Rolle Papierkorb wird, wenn die `Run` Methode beendet. Die `RunAsync` Methode in einem Dienst Fabric-Dienst kann jedoch bis zum Abschluss ausführen und die Instanz des Diensts verbleibt nach oben. 

Fabric-Dienst bietet optional Kommunikation Setup Einstiegspunkt für Dienste, die Client-Anfragen überwachen. Die RunAsync und die Kommunikation Einstiegspunkt sind optionale überschreibt - Ihrem Dienst möglicherweise nur Client-Anfragen Abhören oder nur Ausführen einer Verarbeitungsschleife oder beides auswählen - Dienst Fabric-Dienste weshalb die Methode RunAsync beenden, ohne einen Neustart von Service-Instanz, da das Clientanfragen empfangen weiterhin möglicherweise zulässig ist.

## <a name="application-api-and-environment"></a>Anwendung-API und -Umgebung

Die Cloud Services-Umgebung API enthält Informationen sowie Funktionen für die aktuelle Instanz der virtueller Computer sowie Informationen zu anderen virtuellen Computer Rolleninstanzen. Dienst Fabric stellt Informationen in Bezug auf seine Laufzeit und einige Informationen zu den Knoten einen Dienst wird derzeit ausgeführt. 

**-Umgebung, die Aufgabe** | **Cloud-Dienste** | **Dienst Fabric**
--- | --- | ---
Konfiguration von Einstellungen und Benachrichtigung ändern | `RoleEnvironment` | `CodePackageActivationContext`
Lokaler Speicher | `RoleEnvironment` | `CodePackageActivationContext`
Endpunktinformationen | `RoleInstance` <ul><li>Aktuelle Instanz:`RoleEnvironment.CurrentRoleInstance`</li><li>Andere Rollen und die Instanz:`RoleEnvironment.Roles`</li> | <ul><li>`NodeContext`für die Adresse der aktuellen Knoten</li><li>`FabricClient`und `ServicePartitionResolver` für die Ermittlung der Service-Endpunkts</li> 
Umgebung Emulation | `RoleEnvironment.IsEmulated` | N/V
Gleichzeitiges Änderungsereignis | `RoleEnvironment` | N/V

## <a name="configuration-settings"></a>Konfiguration von Einstellungen

Konfiguration von Einstellungen in der Cloud Services sind für eine Rolle festgelegt und gelten für alle Instanzen des diese Rolle. Diese Einstellungen sind Schlüssel-Wert-Paare festlegen in ServiceConfiguration.*.cscfg Dateien und direkt über RoleEnvironment zugegriffen werden können. Dienst Struktur Einstellungen angewendet einzeln für jeden Dienst und jede Anwendung statt eines virtuellen Computers, da ein virtuellen Computers mehreren Diensten und Anwendungen gehostet werden kann. Ein Dienst besteht aus drei Pakete:

 - **Code:** enthält die Programmdateien Service, Binärdateien, DLL-Dateien und alle anderen Dateien, die ein Dienst ausgeführt werden muss.
 - **Config:** alle Konfigurationsdateien und Einstellungen für einen Dienst.
 - **Daten:** statische Datendateien, die mit dem Dienst verknüpft ist.

Jedes dieser Pakete kann unabhängig voneinander Versionsnummern und aktualisierten sein. Ähnlich wie in der Cloud Services, ein Paket Config zugegriffen werden kann programmgesteuert über eine API und Ereignisse sind für den Dienst Config Paket ändert benachrichtigen verfügbar. Eine Datei Settings.xml kann für Schlüsselwert Konfiguration und programmgesteuerten Zugriff ähnlich wie im Abschnitt der app-Einstellungen für eine App verwendet werden. Im Gegensatz zu Cloud Services kann ein Dienst Fabric Config Paket jedoch keine Konfigurationsdateien in einem beliebigen Format, enthalten, ob es sich um XML, JSON, YAML oder einem benutzerdefinierten Binärformat ist. 


### <a name="accessing-configuration"></a>Zugreifen auf die Konfiguration
#### <a name="cloud-services"></a>Cloud-Dienste

Konfiguration von Einstellungen von ServiceConfiguration.*.cscfg zugegriffen werden können, bis `RoleEnvironment`. Diese Einstellungen stehen global für alle Rolleninstanzen in der gleichen Bereitstellung von Cloud-Dienst aus.

```C#

string value = RoleEnvironment.GetConfigurationSettingValue("Key");

```

#### <a name="service-fabric"></a>Dienst Fabric

Jeder Dienst verfügt über eine eigene Paket für die einzelnen. Indem Sie alle Programme, die in einem Cluster kann keine integrierten Verfahren der Einstellungen für globale Konfiguration zugegriffen werden. Bei der Verwendung des Fabric Service spezielle Settings.xml Konfiguration-Datei in einem Konfigurationspaket können Werte in Settings.xml Ebene der Anwendung, wodurch Anwendung Ebene Konfiguration Einstellungen möglich überschrieben.

Konfiguration von Einstellungen sind Zugriffe innerhalb jeder Dienstinstanz Durchlaufen des Diensts `CodePackageActivationContext`.

```C#

ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");

// Access Settings.xml
KeyedCollection<string, ConfigurationProperty> parameters = configPackage.Settings.Sections["MyConfigSection"].Parameters;

string value = parameters["Key"]?.Value;

// Access custom configuration file:
using (StreamReader reader = new StreamReader(Path.Combine(configPackage.Path, "CustomConfig.json")))
{
    MySettings settings = JsonConvert.DeserializeObject<MySettings>(reader.ReadToEnd());
}

```

### <a name="configuration-update-events"></a>Konfiguration Update Ereignisse
#### <a name="cloud-services"></a>Cloud-Dienste

Die `RoleEnvironment.Changed` Ereignis wird verwendet, um alle Rolleninstanzen benachrichtigt werden, wenn eine Änderung in der Umgebung, z. B. eine Konfigurations Änderung vorgenommen wird. Hiermit wird Konfiguration Updates ohne Wiederverwendung Rolleninstanzen oder einen Neustart von einem Worker-Prozess nutzen.

```C#

RoleEnvironment.Changed += RoleEnvironmentChanged;

private void RoleEnvironmentChanged(object sender, RoleEnvironmentChangedEventArgs e)
{
   // Get the list of configuration changes
   var settingChanges = e.Changes.OfType<RoleEnvironmentConfigurationSettingChange>();
foreach (var settingChange in settingChanges) 
   {
      Trace.WriteLine("Setting: " + settingChange.ConfigurationSettingName, "Information");
   }
}

```

#### <a name="service-fabric"></a>Dienst Fabric

Jeder der drei Pakettypen in einem Dienst - Code, Config und Daten - haben Ereignisse, die eine Dienstinstanz benachrichtigt werden, wenn ein Paket aktualisiert, hinzugefügt oder entfernt wird. Ein Dienst kann mehrere Pakete jedes Typs enthalten. Ein Dienst möglicherweise beispielsweise mehrere Config-Paketen jeweils einzeln Versionsnummern und erweiterbaren müssen. 

Diese Ereignisse stehen Änderungen in Service-Paketen zu nutzen, ohne die Instanz des Diensts neu zu starten.
 
```C#

this.Context.CodePackageActivationContext.ConfigurationPackageModifiedEvent +=
                    this.CodePackageActivationContext_ConfigurationPackageModifiedEvent;

private void CodePackageActivationContext_ConfigurationPackageModifiedEvent(object sender, PackageModifiedEventArgs<ConfigurationPackage> e)
{
    this.UpdateCustomConfig(e.NewPackage.Path);
    this.UpdateSettings(e.NewPackage.Settings);
}

```

## <a name="startup-tasks"></a>Von Startaufgaben

Startaufgaben sind die Aktionen, die ausgeführt werden, bevor eine Anwendung gestartet wird. Ein Start-Vorgang wird in der Regel zum Einrichten Skripts erhöhten mithilfe ausführen. Sowohl Cloud Services-Dienst Fabric Start Aufgaben zu unterstützen. Der wichtigste Unterschied ist, dass in der Cloud Services ein Vorgangs beim Start eines virtuellen Computers verbunden ist, da es Teil einer Rolleninstanz ist während der Dienst Struktur ein Vorgangs Start auf einen Dienst gebunden ist, die nicht mit bestimmten alle virtuellen Computer verknüpft ist.

 | Cloud-Dienste | Dienst Fabric
--- | --- | ---
Pfad der Konfiguration | ServiceDefinition.csdef | ServiceManifest.xml
Berechtigungen | "eingeschränkte" oder "Erweitert" | alle Benutzer oder Computerkonto
Sequenzielles | "einfach", "Hintergrund", "Vordergrund" | Ausführung der Start-Aufgabe erfolgreich durchgeführt werden, bevor der Dienst gestartet wird.

### <a name="cloud-services"></a>Cloud-Dienste
In der Cloud Services ist ein Start-Einstiegspunkt pro Rolle in ServiceDefinition.csdef konfiguriert. 

```xml

<ServiceDefinition>
    <Startup>
        <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
            <Environment>
                <Variable name="MyVersionNumber" value="1.0.0.0" />
            </Environment>
        </Task>
    </Startup>
    ...
</ServiceDefinition>

```

### <a name="service-fabric"></a>Dienst Fabric

Dienst Struktur ist ein Start-Einstiegspunkt pro Service ServiceManifest.xml konfiguriert:

```xml

<ServiceManifest>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>Startup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    ...
</ServiceManifest>

``` 

## <a name="a-note-about-development-environment"></a>Eine Notiz zu Entwicklungsumgebung

Sowohl Cloud Services-Dienst Fabric sind in Visual Studio mit Project-Vorlagen und Unterstützung für das Debuggen, konfigurieren und Bereitstellen von beide lokal und in Azure integriert. Sowohl Cloud Services-Dienst Fabric bieten auch eine lokale Entwicklung Runtime-Umgebung. Der Unterschied ist, dass während die Cloud-Dienst Entwicklung Runtime Azure-Umgebung für die ausgeführt wird emuliert, Fabric Service einen Emulator keine verwendet – sie die vollständige Fabric Service-Laufzeit verwendet. Die Dienst Fabric-Umgebung, die Sie auf Ihrem Computer lokale Entwicklung ausführen ist der gleichen Umgebung, die in der Herstellung ausgeführt wird.

##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Dienst Fabric zuverlässigen Dienste und die grundlegenden Unterschiede zwischen Cloud Services und Architektur Fabric Service-Anwendung zu verstehen, wie sämtlicher Fabric Service-Funktionen nutzen.

 - [Erste Schritte mit zuverlässigen Dienst Fabric-Dienste](./service-fabric-reliable-services-quick-start.md)

 - [Konzeptionelle Leitfaden für die Unterschiede zwischen Cloud Services und Fabric Service](./service-fabric-cloud-services-migration-differences.md)
 
<!--Image references-->
[3]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/service-fabric-cloud-service-projects.png
[4]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/worker-role-to-stateless-service.png
