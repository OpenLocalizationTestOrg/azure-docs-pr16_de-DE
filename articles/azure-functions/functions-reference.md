<properties
    pageTitle="Azure Funktionen Entwicklerreferenz | Microsoft Azure"
    description="Grundlegendes zur Azure-Funktionen Konzepte und Komponenten, die für alle Sprachen und Bindungen feldbezogene."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure-Funktionen, Funktionen, Ereignisse zu verarbeiten, Webhooks, dynamische berechnen, ohne Server Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-developer-reference"></a>Azure Funktionen Entwicklerreferenz

Azure Funktionen freigeben ein paar Core technischen Konzepte und Komponenten, unabhängig von der Sprache oder Bindung, die Sie verwenden. Bevor Sie springen in learning Details, die speziell für eine bestimmte Sprache oder Bindung, achten Sie darauf, die für alle von ihnen gilt dieser Übersicht zu lesen.

In diesem Artikel wird vorausgesetzt, dass Sie die [Übersicht über Funktionen Azure](functions-overview.md) bereits gelesen haben und [WebJobs SDK Konzepte wie Trigger, Bindungen, und die Laufzeit JobHost](../app-service-web/websites-dotnet-webjobs-sdk.md)vertraut sind. Azure Funktionen basiert auf dem WebJobs SDK. 


## <a name="function-code"></a>Code (Funktion)

Eine *Funktion* ist die primäre Konzept in Azure-Funktionen. Schreiben von Code für eine Funktion in einer anderen Sprache Ihrer Wahl, und speichern die Datei(en) Code und einer Konfigurationsdatei in demselben Ordner. Konfiguration ist in JSON und den Namen der Datei `function.json`. Eine Vielzahl von Sprachen werden unterstützt, und können Sie jeweils bietet eine etwas anders optimiert für diese Sprache am besten geeignet sind. 

Die `function.json` Datei enthält die Konfiguration, die speziell für eine Funktion, einschließlich der zugehörigen Bindungen. Die Laufzeit liest diese Datei zum bestimmen, welche Ereignisse zum Deaktivieren von auslösen welche Daten aufnehmen möchten, wenn Sie die Funktion und wohin die Daten zu senden, Anrufen von der Funktion selbst übergeben. 

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Sie können verhindern, dass die Laufzeit mit der Funktion durch Festlegen der `disabled` Eigenschaft zu `true`.

Die `bindings` Eigenschaft ist, in dem Sie Trigger und Bindungen konfigurieren. Jede Bindung verfügt über einige allgemeine Einstellungen und einige Einstellungen, die für einen bestimmten Typ von Bindung spezifisch sind. Jede Bindung erfordert die folgenden Einstellungen:

|Eigenschaft|Werte/Typen|Kommentare|
|---|-----|------|
|`type`|Zeichenfolge|Geben Sie ein Bindung. Beispielsweise `queueTrigger`.
|`direction`|'in', 'out'| Gibt an, ob die Bindung für Daten in der Funktion empfangen oder Senden von Daten aus der Funktion ist.
| `name` | Zeichenfolge | Der Name, der für die gebundenen Daten in der Funktion verwendet wird. Für c# wird dies einen Argumentnamen sein. Bei Verwendung von JavaScript werden die in einer Liste Schlüssel/Wert-Taste.

## <a name="function-app"></a>App (Funktion)

Eine Funktion app besteht eine oder mehrere einzelne Funktionen, die von der App-Verwaltungsdienst Azure zusammen verwaltet werden. Alle Funktionen in einer Funktion app Freigeben der gleichen Preisgestaltung Plan, kontinuierliche Bereitstellung und Runtime-Version. Funktionen, die in mehreren Sprachen geschrieben können alle dieselbe Funktion app freigeben. Stellen Sie sich eine Funktion app als eine Möglichkeit zum Organisieren und verwalten Ihre Funktionen zusammen. 

## <a name="runtime-script-host-and-web-host"></a>Laufzeit (Script Host und Web-Host)

Die Laufzeit oder Script Host, ist der zugrunde liegenden WebJobs SDK Host die für Ereignisse überwacht, sammelt und sendet Daten und schließlich führt den Code aus. 

Um HTTP Trigger zu erleichtern, ist ebenfalls ein Webhost ungefähr vor dem Script Host in Herstellung Szenarien vorgesehen ist. Auf diese Weise zum Isolieren des Script Hosts aus der front-End-Datenverkehr von Web-Host verwaltet werden können.

## <a name="folder-structure"></a>Ordnerstruktur

[AZURE.INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Wenn ein Projekt für die Bereitstellung von Funktionen mit einer Funktion Azure-App-Verwaltungsdienst einrichten, können Sie diese Ordnerstruktur als Ihre Website Code behandeln. Können vorhandene Tools, wie fortlaufende Integration und Bereitstellung oder benutzerdefinierte Bereitstellung Skripts dafür Fehlercode Transpilation oder Zeit Paketinstallation bereitstellen.

>[AZURE.NOTE] Die `wwwroot` hier Ordner befindet, in dem Ihre Dateien zu bereitgestellt werden. Allerdings müssen Sie diesen Ordner nicht enthalten, in die Sie bereitstellen, Dateien einhandeln würde `wwwroot\wwwroot`. In diesem Fall Ihre `host.json` sollten Datei- und Funktion Ordner direkt im Stammverzeichnis was Sie bereitstellen.

## <a name="a-idfileupdatea-how-to-update-function-app-files"></a><a id="fileupdate"></a>So aktualisieren Sie die app-Dateien (Funktion)

Der in der Azure-Portal integriert Funktions-Editor können Sie die *function.json* und die Codedatei für eine Funktion zu aktualisieren. Zum Hochladen oder andere Dateien, wie z. B. *package.json* oder *project.json* oder Abhängigkeiten aktualisieren möchten, müssen Sie andere Methoden für die Bereitstellung verwenden.

Funktion apps sind App-Dienst integriert, sodass alle [Bereitstellungsoptionen standard-Web apps zur Verfügung](../app-service-web/web-sites-deploy.md) für Funktion apps sowie zur Verfügung stehen. Hier sind einige Methoden, die Sie hochladen, oder aktualisieren die app-Dateien (Funktion) verwenden können. 

#### <a name="to-use-app-service-editor"></a>Verwenden des App-Editor-Dienst

1. Klicken Sie im Portal Azure Funktionen auf **app-Einstellungen (Funktion)**.

2. Klicken Sie im Abschnitt **Erweiterte Einstellungen** auf **App Service-Einstellungen**.

3. Klicken Sie unter **DEVELOPMENT TOOLS**auf **App-Editor-Dienst** in der App-Menü menübeschriftungen an.

4.  Klicken Sie auf **OK**.

    Nach dem Laden der App-Editor-Dienst, sehen Sie die *host.json* Datei- und Funktion Ordner unter *' wwwroot '*. 

5. Öffnen von Dateien zu bearbeiten, oder ziehen und Ablegen von Ihrem Entwicklungscomputer zum Hochladen von Dateien.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>Verwenden der Funktion-app SCM (Kudu) Endpunkt

1. Navigieren Sie zu: `https://<function_app_name>.scm.azurewebsites.net`.

2. Klicken Sie auf **Debuggen Console > CMD**.

3. Navigieren Sie zu `D:\home\site\wwwroot\` *host.json* aktualisieren oder `D:\home\site\wwwroot\<function_name>` einer Funktion Dateien zu aktualisieren.

4. Ziehen und Ablegen einer Datei, die Sie in den entsprechenden Ordner im Raster Datei hochladen möchten. Es gibt zwei Bereiche im Datei-Raster, in dem Sie eine Datei löschen können. Für *ZIP-* Dateien wird ein Feld mit der Bezeichnung "Ziehen hier hochladen, und Entzippen Sie ihn." angezeigt Legen Sie für andere Dateitypen im Raster Datei jedoch außerhalb des Felds "Entzippen" aus.

#### <a name="to-use-ftp"></a>Verwenden von FTP

1. Folgen Sie den Anweisungen [hier](../app-service-web/web-sites-deploy.md#ftp) abzurufenden FTP konfiguriert.

2. Wenn Sie mit der Funktion app-Website verbunden sind, kopieren eine Datei aktualisierte *host.json* `/site/wwwroot` oder Kopieren von Dateien (Funktion) `/site/wwwroot/<function_name>`.

#### <a name="to-use-continuous-deployment"></a>Fortlaufender Bereitstellung verwenden.

Führen Sie die Schritte im Thema [fortlaufender Bereitstellung für Azure-Funktionen](functions-continuous-deployment.md)aus.

## <a name="parallel-execution"></a>Parallele Ausführung

Wenn mehrere auslösenden Ereignisse schneller eintreten als eine Funktion Single threaded Laufzeit verarbeitet werden kann, kann die Laufzeit die Funktion mehrmals parallel geltend.  Wenn eine app für die Funktion des [Dynamischen planen Service](functions-scale.md#dynamic-service-plan)verwendet wird, konnte die Funktion app automatisch skalieren.  Jede Instanz von der Funktion-app, ob die app auf der dynamischen Dienst planen oder eine normale [App Dienst planen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), ausgeführt wird möglicherweise gleichzeitige Funktionsaufrufe Verwendung mehrerer parallel verarbeiten.  Die maximale Anzahl von gleichzeitigen Funktionsaufrufe in jede Funktion app-Instanz hängt von der Arbeitsspeichergröße der app (Funktion). 

## <a name="azure-functions-pulse"></a>Pulse Azure-Funktionen  

Pulse ist live Ereignisstream, der anzeigt, wie oft die Funktion ausgeführt wird, sowie Erfolge und Fehlern. Sie können auch die durchschnittliche Ausführung Zeit überwachen. Wir werden weitere Features und Anpassung darauf über einen Zeitraum hinzufügen. Sie können die **Pulse** -Seite auf der Registerkarte **Überwachung** zugreifen.

## <a name="repositories"></a>Repositorys

Der Code für Azure Funktionen führt öffnen und in GitHub Repositorys Speicherorten abgelegt:

* [Azure Funktionen Laufzeit](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure Funktionen-portal](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure Funktionen Vorlagen](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs SDK Erweiterungen](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Bindungen

Hier ist eine Tabelle mit allen unterstützten Bindungen aus.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Melden von Problemen

[AZURE.INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)] 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter den folgenden Ressourcen:

* [Azure Funktionen C#-Entwicklerreferenz](functions-reference-csharp.md)
* [Azure Funktionen F Nr. Entwicklerreferenz](functions-reference-fsharp.md)
* [Azure Funktionen NodeJS Entwicklerreferenz](functions-reference-node.md)
* [Azure Funktionen Trigger und Bindungen](functions-triggers-bindings.md)
* [Azure Funktionen: der Reise](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) auf den App-Verwaltungsdienst Azure-Teamblog. Aufzeichnung der wie Azure Funktionen entwickelt wurde.





