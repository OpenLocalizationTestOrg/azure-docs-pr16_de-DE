<properties
   pageTitle="Verwenden Elasticsearch als ein Dienst Fabric Spur Anwendungsspeicher | Microsoft Azure"
   description="Beschreibt, wie Dienst Fabric Applikationen mit Elasticsearch und Kibana zu speichern, Index und Durchsuchen der Anwendung Spuren (Protokolle)"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/09/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Verwenden von Elasticsearch als eine Fabric Service-Anwendung Spur speichern
## <a name="introduction"></a>Einführung
Dieser Artikel beschreibt, wie [Azure Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric/) Applications **Elasticsearch** und **Kibana** für Spur Anwendungsspeicher, Indizierung und Suche verwenden können. [Elasticsearch](https://www.elastic.co/guide/index.html) ist eine Open Source, verteilt und skalierbare in Echtzeit suchen und Analytics-Engine, die für diese Aufgabe gut geeignet ist. Sie können auf Windows und Linux virtuellen Computern in Microsoft Azure installiert sein. Elasticsearch kann *strukturierte* Spuren mit Technologien wie **Event Tracing for Windows (ETW)**erzeugt effizient verarbeiten.

ETW wird vom Dienst Fabric Runtime Quelle Diagnoseinformationen (Spuren) verwendet. Es empfiehlt sich für Dienst Fabric Applikationen, deren Diagnoseinformationen, auch Quelle. Verwenden das gleiche Verfahren ermöglicht Korrelationskoeffizienten zwischen Laufzeit gelieferten und Anwendung bereitgestellten Spuren und Problembehebung wird erleichtert. Dienst Fabric Project-Vorlagen in Visual Studio gehören eine Protokollierung API (basierend auf der Klasse .NET **Quelle** ), die auf ETW standardmäßig gibt aus. Eine allgemeine Übersicht über Fabric Service-Anwendung Tracing ETW verwenden finden Sie unter [Überwachung und Diagnose von Diensten in einer lokalen Computer Entwicklung einrichten](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

Für die Spuren in Elasticsearch angezeigt werden soll müssen sie erfasst an den Dienst Fabric Cluster-Knoten in Echtzeit (während der Ausführung der Anwendung) und an einen Endpunkt Elasticsearch gesendet werden. Es gibt zwei Bereiche Optionen zum Erfassen von Spur aus:

+ **Erfassen von Spur in Bearbeitung**  
Die Anwendung, oder genauer, Service-Prozess ist verantwortlich für das Senden von Diagnoseprotokollen Daten im Store Spur (Elasticsearch).

+ **Erfassen von Out-of-Process Spur**  
Ein separaten-Agent ist auf aus dem Dienstprozess oder Prozesse erfassen und Senden an den Store Spur.

Im folgenden, wir beschreiben, wie Elasticsearch auf Azure einrichten, die Experten diskutieren und Nachteile für die beiden Optionen zu erfassen und erläutert, wie Sie einen Dienst Fabric-Dienst zum Senden von Daten an Elasticsearch zu konfigurieren.


## <a name="set-up-elasticsearch-on-azure"></a>Einrichten von Elasticsearch auf Azure
Am einfachsten zum Einrichten des Elasticsearch-Diensts auf Azure ist [**Azure Ressourcenmanager Vorlagen**](../azure-resource-manager/resource-group-overview.md)durch. Eine umfassende [Schnellstart Azure Ressourcenmanager Vorlage für Elasticsearch](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) steht Azure Schnellstart Vorlagen Repository zur Verfügung. Diese Vorlage verwendet separate Speicherkonten für die Maßeinheiten (Gruppen von Knoten). Sie können auch eine separate Client- und Serverknoten mit unterschiedlichen Konfigurationen und verschiedenen Zahlen der verbundenen Daten Datenträger bereitstellen.

Hier verwenden wir eine andere Vorlage, **ES-MultiNode** aus dem [Azure von Diagnosetools](https://github.com/Azure/azure-diagnostics-tools)bezeichnet. Diese Vorlage ist einfacher zu verwenden, und erstellt einen Elasticsearch Cluster durch HTTP-Standardauthentifizierung geschützt ist. Bevor Sie fortfahren, herunterladen Sie Repository aus GitHub auf den Computer (durch entweder Klonen Repository oder Herunterladen einer Zipdatei). Die Vorlage ES-MultiNode befindet sich in den Ordner mit demselben Namen.

### <a name="prepare-a-machine-to-run-elasticsearch-installation-scripts"></a>Vorbereiten eines Computers zum Ausführen von Elasticsearch Installationsskripts
Die einfachste Möglichkeit, die ES MultiNode Vorlage verwenden, wird durch ein bereitgestellten Azure PowerShell-Skript namens `CreateElasticSearchCluster`. Wenn Sie dieses Skript verwenden zu können, müssen Sie PowerShell-Module und einem Tool namens **Openssl**installieren. Letztere ist erforderlich, zum Erstellen einer SSH-Taste, die verwendet werden kann, um Ihren Cluster Elasticsearch Remote zu verwalten.

`CreateElasticSearchCluster`Center für erleichterte Bedienung mit der ES MultiNode Vorlage aus einem Windows-Computer dient Skript. Es ist möglich, die Vorlage auf eine nicht-Windows-Computer verwenden, aber dieses Szenario ist nicht Gegenstand dieses Artikels.

1. Wenn Sie diese bereits installiert haben, installieren Sie [**Azure PowerShell-Module**](http://aka.ms/webpi-azps). Wenn Sie dazu aufgefordert werden, klicken Sie auf **Ausführen**, klicken Sie dann auf **Installieren**. Azure PowerShell 1.3 oder höher ist erforderlich.

2. Das Tool **Openssl** ist in die Verteilung der [**Git für Windows**](http://www.git-scm.com/downloads)enthalten. Wenn Sie dies noch nicht getan haben, installieren Sie jetzt [Git für Windows](http://www.git-scm.com/downloads) . (Die Standardoptionen für die Installation sind in Ordnung.)

3. Unter der Voraussetzung, dass Git installiert, aber nicht in den Systempfad eingeschlossen wurde, öffnen Sie eine Microsoft Azure PowerShell-Fenster zu, und führen Sie die folgenden Befehle:

    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```

    Ersetzen der `<Git installation folder>` mit dem Git Speicherort auf Ihrem Computer; Die Standardeinstellung ist **"c:\Programme\Microsoft Files\Git"**. Beachten Sie das Semikolon am Anfang der ersten Pfad ein.

4. Stellen Sie sicher, dass Sie Azure angemeldet sind (über [`Add-AzureRmAccount`](https://msdn.microsoft.com/library/mt619267.aspx) Cmdlet) und dass Sie das Abonnement ausgewählt haben, die zum Erstellen Ihrer Suche flexible Clusters verwendet werden soll. Sie können überprüfen, ob der richtige Abonnement verwenden ausgewählt ist `Get-AzureRmContext` und `Get-AzureRmSubscription` Cmdlets.

5. Wenn dies nicht erfolgt noch, ändern Sie zu dem Ordner ES-MultiNode des aktuellen Verzeichnisses.

### <a name="run-the-createelasticsearchcluster-script"></a>Führen Sie das Skript CreateElasticSearchCluster
Bevor Sie das Skript ausführen, öffnen Sie die `azuredeploy-parameters.json` Datei und stellen Sie sicher, oder geben Sie Werte für die Skriptparameter. Die folgenden Parameter stehen zur Verfügung:

|Parameternamen           |Beschreibung|
|-----------------------  |--------------------------|
|dnsNameForLoadBalancerIP |Der Name, der den öffentlich sichtbaren DNS-Namen für den Cluster flexible Suchen erstellen (durch die Azure Region-Domäne anhängen, das dem bereitgestellten Namen) verwendet wird. Wenn diese übergebener Wert ist "MyBigCluster" und der ausgewählten Azure Region Westen US, wird der resultierende DNS-Name für den Cluster myBigCluster.westus.cloudapp.azure.com. <br /><br />Dieser Name dient auch als Name für einen Stamm für viele Elemente, die dem Cluster flexible suchen, wie z. B. Daten Knotennamen zugeordnet.|
|adminUsername           |Der Name des Administratorkontos für die Verwaltung von Cluster flexible Suche (entsprechende SSH Tasten werden automatisch generiert).|
|dataNodeCount           |Die Anzahl der Knoten im Cluster flexible suchen. Die aktuelle Version des Skripts unterscheidet nicht zwischen Daten und Abfragen Knoten; alle Knoten wiedergeben beide Rollen an. Der Standardwert ist 3 Knoten.|
|dataDiskSize            |Die Größe der Datenfestplatten (in GB), die für die einzelnen Datenknoten zugeordnet wird. Jeder Knoten erhält 4 Laufwerken zu flexible Suchdienst ausschließlich dedizierter an.|
|Region                  |Der Name des Azure Region, in dem der flexible Suche Cluster befinden soll.|
|esUserName              |Der Benutzername des Benutzers, die so konfiguriert ist, dass Zugriff auf ES Cluster (unterliegen HTTP-Standardauthentifizierung) haben. Das Kennwort ist nicht Teil einer Parameterdatei und muss bereitgestellt werden, wenn `CreateElasticSearchCluster` Skript aufgerufen wird.|
|vmSizeDataNodes         |Die Größe der Azure-virtuellen Computern flexible Suche Cluster-Knoten. Standardmäßig ist Standard_D2.|

Jetzt sind Sie bereit sind, führen Sie das Skript. Geben Sie den folgenden Befehl aus:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

WHERE 

|Parameternamen Skript    |Beschreibung|
|-----------------------  |--------------------------|
|`<es-group-name>`        |Der Name der Azure Ressourcengruppe, die alle flexible Suche Clusterressourcen enthalten sollen.|
|`<azure-region>`         |Der Name des Azure Region, in dem der flexible Suche Cluster erstellt werden soll.|         
|`<es-password>`          |Das Kennwort für den Benutzer flexible suchen.|

>[AZURE.NOTE] Wenn Sie eine NullReferenceException des Cmdlets Test-AzureResourceGroup erhalten haben, vergessen haben Azure anmelden (`Add-AzureRmAccount`).

Wenn Sie eine Fehlermeldung erhalten, aus der Sie das Skript ausführen, und Sie feststellen, dass der Fehler durch einen falschen Vorlage Parameterwert verursacht wurde, korrigieren Sie die Parameterdatei, und führen Sie das Skript erneut mit einer anderen Gruppe Ressourcennamen. Können Sie auch den gleichen Ressource Gruppennamen wiederverwenden und haben Sie das Skript Bereinigen von das alte Konto durch Hinzufügen der `-RemoveExistingResourceGroup` -Parameter der Aufrufen des Skripts.

### <a name="result-of-running-the-createelasticsearchcluster-script"></a>Ergebnis der Ausführung des Skripts CreateElasticSearchCluster
Nach dem Ausführen der `CreateElasticSearchCluster` Skript, die folgenden-Hauptfenster Elemente erstellt werden. In diesem Beispiel davon ausgegangen, dass Sie als Wert des "MyBigCluster" verwendet haben die `dnsNameForLoadBalancerIP` Parameter und die Stelle, an der Sie den Cluster erstellt Region Westen US ist.

|Element|Name, Position und Hinweise|
|----------------------------------|----------------------------------|
|SSH Schlüssel für remote-administration |myBigCluster.key-Datei (im Verzeichnis aus dem der CreateElasticSearchCluster ausgeführt wird). <br /><br />Diese Taste Datei kann (über den Administrator-Knoten) und den Knoten Admin Verbindung zu Datenknoten im Cluster verwendet werden.|
|Admin-Knoten                        |MyBigCluster-admin.westus.cloudapp.azure.com <br /><br />Dedizierte eines virtuellen Computers für remote Elasticsearch Cluster Administration – die einzige, die externe SSH Verbindungen zulässt. Keine Elasticsearch Services ausgeführt werden, aber in demselben virtuellen Netzwerk als alle Elasticsearch Clusterknoten ausgeführt.|
|Datenknoten                        |myBigCluster1... MyBigCluster*N* <br /><br />Datenknoten, die Elasticsearch und Kibana Services ausgeführt werden. Sie können über SSH für jeden Knoten, aber nur über den Administrator Knoten verbinden.|
|Elasticsearch cluster             |http://myBigCluster.westus.cloudapp.Azure.com/es/ <br /><br />Der primäre Endpunkt für den Cluster Elasticsearch (Beachten Sie das Suffix/es einsetzen). Es ist durch HTTP-Standardauthentifizierung geschützt (die Anmeldeinformationen wurden die angegebenen EsUserName/EsPassword-Parameter der ES MultiNode Vorlage). Der Cluster hat auch der Leiter-Plug-In installiert (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head) für grundlegende Cluster Administration.|
|Kibana-Dienst                    |http://myBigCluster.westus.cloudapp.Azure.com <br /><br />Der Dienst Kibana wird zum Anzeigen von Daten aus dem erstellten Elasticsearch Cluster eingerichtet. Es wird durch die gleichen Authentifizierungsinformationen als den Cluster selbst geschützt.|

## <a name="in-process-versus-out-of-process-trace-capturing"></a>Im Vergleich zu erfassen von Out-of-Process Spur Prozess
In der Einführung erwähnt zwei grundlegende zum Sammeln von Daten von Diagnoseprotokollen: in Bearbeitung und Out-of-Process. Jede hat Stärken und Schwächen.

Veranschaulicht den **Prozess Spur erfassen** bietet folgende Vorteile:

1. *Einfache Konfiguration und Bereitstellung*

    * Die Konfiguration des diagnostic Datensammlung ist nur für einen Teil der Anwendungskonfiguration. Es ist einfach immer "synchron" mit den Rest der Anwendung behalten.

    * Pro Anwendung oder jeden Dienst Konfiguration ist einfach erzielt.

    * Erfassen von Out-of-Process Spur erfordert in der Regel eine separate Bereitstellung und Konfiguration von der Diagnose-Agent, der eine zusätzliche administrative Vorgänge und eine mögliche Quelle der Fehler ist. Die bestimmten Agent-Technologie ermöglicht häufig nur eine Instanz des Agents pro virtuellen Computern (Knoten). Dies bedeutet die Konfiguration für die Sammlung der Diagnose Konfiguration von Applications alle gemeinsam verwendet wird und Dienste auf diesem Knoten ausgeführt.

2. *Flexibilität*

    * Die Anwendung kann die Daten senden, wo diese benötigt werden, wie es ist eine Clientbibliothek, die das gezielte Speicher Datensystem unterstützt. Neue senken hinzugefügt werden können wie gewünscht.

    * Komplexe erfassen, Filtern und Daten-Aggregation Regeln können implementiert werden.

    * Eine Out-of-Process Spur erfassen werden häufig durch die Daten senken beschränkt, die der Agent unterstützt. Einige Agents sind erweiterbar.

3. *Zugriff auf interne Anwendungsdaten und den Kontext*

    * Diagnose Teilsystems innerhalb des Anwendungsdienst/ausgeführt kann einfach die Spuren mit Kontextinformationen erweitern.

    * In der Ansatz Out-of-Process müssen die Daten für einen Agent über einige Kommunikationsmechanismus, wie z. B. Event Tracing for Windows gesendet werden. Dieses Verfahren kann zusätzliche Einschränkungen vorgangseinschränkung.

Die **Out-of-Process-Spur erfassen** bietet folgende Vorteile:

1. *Die Möglichkeit zum Überwachen der Anwendungs und sammeln Absturz bildet ab.*

    * In Bearbeitung Spur erfassen möglicherweise nicht erfolgreich, wenn die Anwendung nicht gestartet oder stürzt ab. Ein unabhängiger Agent verfügt über eine bessere Chance von erfassen wichtige Informationen, die zur Problembehandlung.<br /><br />

2. *Fälligkeit, Stabilität und Leistung bewährte*

    * Agent von einem Lieferanten Plattform (beispielsweise einem Microsoft Azure-Diagnose-Agent) entwickelt wurde unterliegen strengen testen und statt-zum Sichern des.

    * Mit in Bearbeitung Spur erfassen muss Achten Sie sicherstellen, dass die Aktivität Senden von Diagnoseprotokollen Daten aus einem Anwendungsprozess nicht der Anwendung wichtigsten Aufgaben stören oder Anzeigedauer oder Leistungsprobleme vorstellen. Agent unabhängig voneinander ausgeführt wird diese Probleme weniger häufig und dient speziell zum deren Einfluss auf das System beschränken.

Es ist möglich, kombinieren und beide Ansätze nutzen. Dies möglicherweise tatsächlich um, die bestmögliche Lösung für viele Clientanwendungen verwenden.

Hier verwenden wir die **Bibliothek Microsoft.Diagnostic.Listeners** und der in Bearbeitung Spur zum Senden von Daten aus einer Fabric Service-Anwendung zu einem Cluster Elasticsearch erfassen.

## <a name="use-the-listeners-library-to-send-diagnostic-data-to-elasticsearch"></a>Verwenden der Bibliothek Listener diagnostische Daten an Elasticsearch senden
Die Bibliothek Microsoft.Diagnostic.Listeners PartyCluster Beispiel Fabric Service-Anwendung gehört. Möglichkeit zur gemeinsamen Nutzung:

1. Herunterladen [der Stichprobe PartyCluster](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) aus GitHub an.

2. Kopieren Sie die Microsoft.Diagnostics.Listeners und Microsoft.Diagnostics.Listeners.Fabric Projekte (gesamten Ordner) aus dem PartyCluster Beispiel zu dem Ordner Lösung der Anwendung, das um die Daten an Elasticsearch zu senden.

3. Öffnen Sie die Ziel-Lösung, mit der rechten Maustaste in des Lösung-Knotens in der Lösung-Explorer, und wählen Sie **Vorhandenes Projekt hinzufügen**. Fügen Sie das Projekt Microsoft.Diagnostics.Listeners in der Lösung hinzu. Wiederholen Sie die gleiche für das Projekt Microsoft.Diagnostics.Listeners.Fabric aus.

4. Die beiden hinzugefügten Projekte einen Projektverweis aus Ihrem Projekt Dienst hinzuzufügen. (Jedem Dienst, das Senden von Daten an Elasticsearch sollten Microsoft.Diagnostics.EventListeners und Microsoft.Diagnostics.EventListeners.Fabric verweisen).

    ![Projektverweise auf Microsoft.Diagnostics.EventListeners und Microsoft.Diagnostics.EventListeners.Fabric-Bibliotheken][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Dienst Fabric allgemeine Verfügbarkeit Release und Microsoft.Diagnostics.Tracing Nuget-Paket
Mit Service Fabric allgemeine Verfügbarkeit Version (2.0.135, 31 März 2016 freigegeben) entwickelte adressieren **.NET Framework 4.5.2**. Dies ist der höchsten Version von .NET Framework von Azure, die zum Zeitpunkt der Veröffentlichung GA unterstützt werden. Diese Version von Framework verfügt leider nicht über bestimmte EventListener-APIs, die die Bibliothek Microsoft.Diagnostics.Listeners muss. Da die Quelle (die Komponente, die die Grundlage für die Protokollierung APIs in Clientanwendungen Fabric bildet) und EventListener eng verknüpft sind, muss in jedem Projekt, die die Bibliothek Microsoft.Diagnostics.Listeners verwendet eine alternative Implementierung der Quelle verwenden. Diese Implementierung wird durch das **Microsoft.Diagnostics.Tracing Nuget-Paket**, verfasst von Microsoft bereitgestellt. Das Paket ist vollständig abwärtskompatibel mit Quelle im Framework, enthalten ist, sodass keine Änderungen Code als verwiesen wird Namespace Änderungen erforderlich sein soll.

Um mit der Microsoft.Diagnostics.Tracing-Implementierung der Klasse Quelle zu starten, für jedes Service-Projekt, das zum Senden von Daten zu Elasticsearch folgendermaßen Sie vor:

1. Mit der rechten Maustaste auf das Projekt-Dienst, und wählen Sie **Nuget-Pakete verwalten**.

2. Wechseln Sie zu der Datenquelle "NuGet.org" Paket (wenn es nicht bereits aktiviert ist), und suchen Sie nach "**Microsoft.Diagnostics.Tracing**".

3. Installieren der `Microsoft.Diagnostics.Tracing.EventSource` -Paket (und die zugehörigen Dateien).

4. Öffnen Sie die Datei **ServiceEventSource.cs** oder **ActorEventSource.cs** im Dienstprojekt und ersetzen die `using System.Diagnostics.Tracing` die Richtlinie auf die Datei mit der `using Microsoft.Diagnostics.Tracing` Richtlinie.

Folgendermaßen werden nicht notwendig sein, nachdem die **.NET Framework 4.6** von Microsoft Azure unterstützt wird.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Elasticsearch Zuhörer Instanziierung und Konfiguration
Zum Senden von Diagnoseprotokollen Daten an Elasticsearch der letzte Schritt besteht im Erstellen einer Instanz von `ElasticSearchListener` und konfigurieren Sie ihn mit Elasticsearch Verbindungsdaten. Die Zuhörer erfasst automatisch alle über Quelle Klassen im Dienstprojekt definierten ausgelöste Ereignisse. Es muss während der Gültigkeitsdauer des Dienst aktiv sein, damit am besten Erstellung im Dienst Initialisierungscode ist. So der Initialisierungscode für einen statusfreie Dienst nach die notwendigen Änderungen vor aussehen konnte (Ergänzungen hingewiesen beginnend mit Kommentaren `****`):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add the following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>        
        private static void Main()
        {
            try
            {
                // **** Instantiate ElasticSearchListener
                var configProvider = new FabricConfigurationProvider("ElasticSearchEventListener");
                ElasticSearchListener esListener = null;
                if (configProvider.HasConfiguration)
                {
                    esListener = new ElasticSearchListener(configProvider);
                }

                // The ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name to a .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of the class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that the ElasticSearchListner instance is not garbage-collected prematurely
                GC.KeepAlive(esListener);
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e);
                throw;
            }
        }
    }
}
```

Setzen Sie Elasticsearch Verbindungsdaten in einer separaten Abschnitt in der Konfiguration Dienstdatei (**PackageRoot\Config\Settings.xml**). Der Name des Abschnitts muss auf den Wert zu übergeben entsprechen den `FabricConfigurationProvider` Konstruktor, beispielsweise:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
Die Werte von `serviceUri`, `userName` und `password` Parameter entsprechen Elasticsearch Cluster Endpunktadresse, Elasticsearch Benutzername und Kennwort. `indexNamePrefix`ist das Präfix für Elasticsearch Indizes. die Bibliothek Microsoft.Diagnostics.Listeners erstellt einen neuen Index für ihre Daten täglich aus.

### <a name="verification"></a>Überprüfung
Das war's schon! Jetzt immer, wenn der Dienst ausgeführt wird, wird gestartet, es auf dem Elasticsearch-Dienst in der Konfiguration angegebenen senden. Sie können überprüfen, dass dies durch Öffnen der Kibana UI die Ziel-Instanz Elasticsearch zugeordnet. In diesem Beispiel wird die Seitenadresse http://myBigCluster.westus.cloudapp.azure.com/. Überprüfen, die mit dem Namenspräfix für ausgewählte indiziert die `ElasticSearchListener` Instanz tatsächlich um erstellt und mit Daten gefüllt wurden.

![Kibana mit PartyCluster Anwendungsereignisse][2]

## <a name="next-steps"></a>Nächste Schritte
- [Weitere Informationen zu Diagnosezwecken und einen Dienst Fabric-Dienst für die Überwachung](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png
