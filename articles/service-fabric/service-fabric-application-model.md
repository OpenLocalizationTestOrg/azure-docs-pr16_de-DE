<properties
   pageTitle="Dienst Fabric Anwendungsmodell | Microsoft Azure"
   description="Informationen zum Modellieren und Anwendungen und Dienste Service Struktur zu beschreiben."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor="mani-ramaswamy"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"   
   ms.author="seanmck"/>

# <a name="model-an-application-in-service-fabric"></a>Modellieren einer Anwendungs Dienst Struktur

Dieser Artikel enthält eine Übersicht über das Modell Azure Service Fabric Anwendung. Auch erläutert, wie eine Anwendung und der Dienst über Manifestdateien zu definieren, und erhalten die Anwendung verpackt und für die Bereitstellung bereit.

## <a name="understand-the-application-model"></a>Grundlagen des Datenmodells Anwendung

Eine Anwendung ist eine Sammlung von enthaltenen Diensten, die eine bestimmte Funktion oder Funktionen ausführen. Ein Dienst führt eine Funktion abgeschlossen und eigenständigen (sie können Anfangs- und unabhängig von anderen Diensten ausgeführt) und Code, Konfiguration und Daten besteht. Klicken Sie für jeden Dienst Code besteht aus den ausführbaren Binärdateien, Konfiguration besteht aus diensteinstellungen, die zur Laufzeit geladen werden können, Daten aus beliebiger statische Daten und von der Dienst genutzt werden. Jede Komponente in dieses Anwendungsmodell hierarchische kann Versionsnummern und aktualisierten unabhängig sein.

![Der Dienst Fabric Anwendungsmodell][appmodel-diagram]


Ein Anwendungstyp ist eine Kategorisierung der Anwendung und bündeln Service-Typen besteht aus. Ein Diensttyp ist eine Kategorisierung eines Diensts. Die Kategorisierung kann unterschiedliche Einstellungen und Konfigurationen haben, aber die Kernfunktionalität bleibt unverändert. Die Instanzen eines Diensts sind unterschiedliche Service Konfiguration Variationen der gleichen Diensttyp.  

Klassen (oder "Typen") werden von Applications und-Diensten über XML-Dateien (Anwendungsmanifeste und Dienst Manifeste), die die Vorlagen sind, anhand derer Applikationen die Cluster Bild Store kann werden, beschrieben. Die Schemadefinition für die Datei ServiceManifest.xml und ApplicationManifest.xml wird mit dem Dienst Fabric SDK und Tools *C:\Programme c:\Programme\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*installiert.

Als separate Prozesse, auch wenn Sie von demselben Dienst Fabric Knoten gehostet wird der Code für andere Anwendungsinstanzen ausgeführt. Darüber hinaus kann des Lebenszyklus von jede Anwendungsinstanz (d. h. verwaltet werden Upgrade) unabhängig voneinander. Das folgende Diagramm veranschaulicht, wie ein Anwendungstypen von Service-Typen bestehen die wiederum Code, Konfiguration und Pakete bestehen. Um das Diagramm zu vereinfachen, werden nur die Code/Config/Daten für Pakete `ServiceType4` angezeigt werden, obwohl jeder Diensttyp einige zählen oder alle diese Typen verpacken.

![Fabric Service-Anwendung und Servicetypen][cluster-imagestore-apptypes]

Zwei verschiedene Manifestdateien werden verwendet, um Anwendungen und Dienste beschreiben: die Servicemanifest und Anwendungsmanifest. Diese werden in den folgenden Abschnitten ausführlich behandelt.

Es kann eine oder mehrere Instanzen von einen Diensttyp im Cluster aktiv sein. Beispielsweise erzielen dynamische Dienstinstanzen oder Replikations hohen Zuverlässigkeit durch die Replikation Zustand zwischen Replikaten, die sich auf anderen Knoten im Cluster befinden. Diese Replikation stellt im Wesentlichen Redundanz für den Dienst zur Verfügung, auch wenn ein Knoten in einem Cluster fehlschlägt. Eine [partitionierte Dienst](service-fabric-concepts-partitioning.md) weiteren dividiert seinem Zustand (und Access Mustern in diesen Status) über Knoten im Cluster.

Das folgende Diagramm zeigt die Beziehung zwischen Anwendungen und Dienstinstanzen, Partitionen und Replikate.

![Partitionen und der in einem Dienst][cluster-application-instances]


>[AZURE.TIP] Sie können das Layout von Applications in einem Cluster verwenden Sie das Dienst Fabric Explorer-Tool verfügbar unter http:// anzeigen&lt;Yourclusteraddress&gt;: 19080/Explorer. Weitere Informationen hierzu finden Sie unter [Cluster mit Service Fabric Explorer Visualisierung](service-fabric-visualizing-your-cluster.md).

## <a name="describe-a-service"></a>Beschreiben Sie eine Dienstleistung

Servicemanifests definiert deklarativ den Diensttyp und die Version. Es gibt Dienstmetadaten wie Diensttyp, Gesundheit Eigenschaften, den Lastenausgleich Kennzahlen, Dienstbinärdateien und Konfigurationsdateien an.  Setzen eine weitere Möglichkeit, werden die Code, Konfiguration und Daten Pakete, aus die ein Service-Paket einen oder mehrere Servicetypen unterstützt zusammensetzt beschrieben. Es folgt ein einfaches Beispiel Servicemanifest aus:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
~~~

**Version** Attribute unstrukturierte Zeichenfolgen werden und nicht vom System analysiert. Hierbei handelt es sich für die Version jeder Komponente nach Upgrades verwendet.

**ServiceTypes** deklariert, welche Arten von Dienst in diesem Manifest von **CodePackages** unterstützt werden. Beim Erstellen ein Dienst anhand eines dieser Dienst werden alle in diesem Manifest deklarierte Code-Paketen durch den Eintrittspunkte ausführen aktiviert. Die resultierende Prozesse erwarten die unterstützten Servicetypen zur Laufzeit zu registrieren. Beachten Sie, dass bei der Manifesten und nicht den Code Paketebene Service-Typen deklariert sind. Also Vorliegens mehrere Code Pakete, werden diese alle aktiviert, wenn das System für eine der deklarierte Typen sieht aus.

**SetupEntryPoint** ist eine berechtigte Einstiegspunkt, der mit den gleichen Anmeldeinformationen als Dienst Fabric *(in der Regel das lokale Systemkonto)* vor einem beliebigen anderen Einstiegspunkt ausgeführt wird. Die ausführbare Datei durch **Einstiegspunkt** angegeben ist normalerweise der langer Diensthost. Das Vorhandensein eines anderen Setup Einstiegspunkt wird vermieden, den Diensthost mit hohen Berechtigungen für längere Zeit ausführen müssen. Die ausführbare Datei durch **Einstiegspunkt** angegeben wird ausgeführt, nach dem **SetupEntryPoint** erfolgreich beenden. Der resultierende Prozess wird überwacht und neu gestartet (beginnend mit **SetupEntryPoint**), wenn es jemals beendet oder stürzt ab.

**DataPackage ab** deklariert einen Ordner, der mit dem Namen durch das Attribut **Name** , das durch den Prozess zur Laufzeit verbraucht werden statische Daten enthält.

**ConfigPackage** deklariert einen Ordner, der mit dem Namen durch das Attribut **Name** , das eine Datei *Settings.xml* enthält. Diese Datei enthält Abschnitte User defined, Schlüssel / Wert-Paare-Einstellungen, die der Prozess wieder zur Laufzeit lesen kann. Bei einer Aktualisierung Wenn nur die **ConfigPackage** **Version** geändert hat, wird dann laufende Prozess nicht neu gestartet. Ein Rückruf benachrichtigt stattdessen den Prozess, den Konfiguration Einstellungen geändert haben, damit er dynamisch neu geladen werden können. Hier ist eine Beispiel *Settings.xml* Datei ein:

~~~
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSecion">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
~~~

> [AZURE.NOTE] Eine Servicemanifest kann mehrere Code, Konfiguration und Datenpakete enthalten. Jede einzelne kann unabhängig voneinander Versionsnummern werden.

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Beschreiben Sie die Anwendung


Das Anwendungsmanifest werden deklarativ Anwendungstyp und die Version. Es gibt Dienst Komposition Metadaten wie unveränderliche Namen, Partitionierung Schema, Instanz zählen/Replikation Faktor, Sicherheit/Isolation Richtlinie, Platzierung Einschränkungen, Konfiguration überschreibt und enthaltenen Service-Typen an. Darüber hinaus werden die Lastenausgleich Domänen, in denen die Anwendung platziert wird, beschrieben.

Auf diese Weise ein Anwendungsmanifest beschreibt Elemente Ebene der Anwendung und verweist auf eine oder mehrere Service-Manifeste um ein Anwendungstyp verfassen. Es folgt ein einfaches Beispiel Anwendungsmanifest aus:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
~~~

Wie Dienst Manifeste **Version** Attribute unstrukturierte Zeichenfolgen werden und nicht vom System analysiert werden. Dies sind auch auf Version jede Komponente für Upgrades verwendet.

**ServiceManifestImport** enthält Verweise auf Dienst-Manifeste, die diese Anwendungstyp verfassen. Importierte Dienst Manifeste bestimmen, welche Service-Typen innerhalb dieser Anwendungstyp gültig sind.

**DefaultServices** deklariert dar, die automatisch erstellt werden, sobald eine Anwendung anhand dieser Anwendungstyp instanziiert wird. Standarddienste werden lediglich eine Vereinfachung und Verhalten sich wie normale Dienste in jeder Hinsicht, nachdem sie erstellt wurden. Gemeinsam mit allen anderen Diensten in der Anwendungsinstanz aktualisiert werden, und auch entfernt werden kann.

> [AZURE.NOTE] Ein Anwendungsmanifest kann mehrere Dienst Manifesten Importe und Standarddienste enthalten. Jeder Dienst Manifestimports kann unabhängig voneinander Version gespeichert werden.

Weitere Informationen zum Verwalten von anderen Anwendung und Dienstparameter für einzelne Umgebungen finden Sie unter [Verwalten von Anwendungsparameter für mehrere Umgebungen](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->

## <a name="package-an-application"></a>Packen einer Anwendung

### <a name="package-layout"></a>Paket layout

Anwendungsmanifest, Service/die Anwendungsmanifeste und andere Paketdateien erforderlichen müssen in einem bestimmten Layout für die Bereitstellung in einem Dienst Fabric Cluster angeordnet werden. Die Manifeste Beispiel in diesem Artikel müssen in der folgenden Verzeichnisstruktur angeordnet werden:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
~~~

Die Ordner sind benannte die Attribute der **Name** der entsprechenden Elemente anpassen. Beispielsweise Servicemanifests zwei Code-Paketen mit den Namen **MyCodeA** und **MyCodeB**enthalten, würde dann zwei Ordnern mit den gleichen Namen die erforderlichen bin-Dateien für jedes Paket Code enthalten.

### <a name="use-setupentrypoint"></a>Verwenden von SetupEntryPoint

Folgende gängige Szenarien für die Verwendung von **SetupEntryPoint** sind, wenn Sie eine ausführbare Datei vor dem Dienst gestartet ausführen müssen, oder Sie zur Durchführung eines Vorgangs mit erhöhten benötigen. Beispiel:

- Einrichten und Initialisierung Umgebungsvariablen, die den Dienst Programm muss. Dies ist nicht auf Dateien, die über die Programmierung Fabric Service-Modelle geschrieben beschränkt. Beispielsweise benötigt npm.exe einige Umgebungsvariablen zum Bereitstellen einer Anwendung node.js konfiguriert.

- Einrichten von Access-Steuerelement nach der Installation von Sicherheitszertifikate.

### <a name="build-a-package-by-using-visual-studio"></a>Erstellen eines Pakets mit Visual Studio

Wenn Sie Visual Studio 2015 verwenden, um eine Anwendung zu erstellen, können Sie den Befehl Verpacken, automatisch ein Paket erstellen, die das Layout oben beschriebenen entspricht.

Klicken Sie zum Erstellen eines Pakets mit der rechten Maustaste in des Anwendung Projekts im Solution Explorer, und wählen Sie den Befehl Paket aus, wie unten dargestellt:

![Packen einer Anwendung mit Visual Studio][vs-package-command]

Nach Abschluss der Verpackung finden Sie die Position des Pakets im **Ausgabefenster** angezeigt. Beachten Sie, dass der Verpackung Schritt beim Bereitstellen oder Debuggen Ihrer Anwendung in Visual Studio automatisch ausgeführt wird.

### <a name="test-the-package"></a>Testen des Pakets

Sie können die Paketstruktur lokal über PowerShell mithilfe des Befehls **Test-ServiceFabricApplicationPackage** überprüfen. Dieser Befehl überprüft Manifest Probleme beim Analysieren und alle Verweise überprüfen. Beachten Sie, dass dieser Befehl nur überprüft, die strukturelle Richtigkeit der Verzeichnisse und Dateien in das Paket ob ein. Es wird nicht Überprüfen des Inhalts Code oder Daten Paket jenseits überprüfen, dass alle notwendigen Dateien vorhanden sind.

~~~
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
~~~

Dieser Fehler zeigt, dass die referenzierte Servicemanifests **SetupEntryPoint** *MySetup.bat* -Datei aus dem Paket Code fehlt. Nachdem die fehlende Datei hinzugefügt wurde, wird die Anwendung Überprüfung übergeben:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
~~~

Nachdem Sie die Anwendung korrekt zugewiesen ist, und übergibt Überprüfung, ist es für die Bereitstellung bereit.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen und Entfernen von applications][10]

[Verwalten von Anwendungsparameter für mehrere Umgebungen][11]

[RunAs: Ausführen einer Fabric Service-Anwendung mit anderen sicherheitsgrupe Berechtigungen][12]

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png
[vs-package-command]: ./media/service-fabric-application-model/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
