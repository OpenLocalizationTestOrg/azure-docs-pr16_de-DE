<properties 
pageTitle="Startaufgaben in Azure Cloud Services ausführen | Microsoft Azure" 
description="Startaufgaben können Ihre Cloud-Service-Umgebung für Ihre app vorbereiten. Dies zeigt Ihnen, wie Vorgänge beim Start funktionieren und wie zu machen" 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>



# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>So konfigurieren und Ausführen des Startaufgaben für einen Clouddienst

Startaufgaben können Sie Vorgänge ausführen, bevor eine Rolle gestartet wird. Vorgänge, die Sie möglicherweise ausführen möchten einschließen eine Komponente installieren, COM-Komponenten registrieren, Registrierungsschlüsseln festlegen oder Starten eines Prozesses zeitintensiven.

>[AZURE.NOTE] Startaufgaben sind nicht anwendbar auf virtuellen Computern, die nur für Cloud-Dienst Web und Worker-Rollen.

## <a name="how-startup-tasks-work"></a>Wie funktionieren Startaufgaben

Start Tasks sind Aktionen, die ausgeführt werden, bevor die Rollen beginnen und in der Datei [ServiceDefinition.csdef] definiert sind, mithilfe des [Task] -Elements innerhalb des [Start] -Elements. Startaufgaben werden häufig Stapelverarbeitungsdateien können zwar, jedoch auch Console Applikationen oder Stapelverarbeitungsdateien, die PowerShell-Skripts zu starten.

Umgebungsvariablen-, die Informationen in eine Startaufgabe übergeben, und lokaler Speicher kann verwendet werden, um Informationen aus einer Startaufgabe beim zu übergeben. Angenommen, eine Umgebungsvariable kann Geben Sie den Pfad zu einem Programm, die Sie installieren möchten, und Dateien geschrieben werden können, in den lokalen Speicher, der von Ihrer Rollen dann später gelesen werden kann.

Die Startaufgabe kann Informationen und Fehler in der durch die **TEMP** -Umgebungsvariable angegebene Verzeichnis protokollieren. Während der Vorgang Start aufgelöst Variable **TEMP** -Umgebung auf die *C:\\Ressourcen\\Temp\\[Guid]. [ RoleName]\\RoleTemp* Verzeichnis, wenn in der Cloud ausführen.

Startaufgaben können auch mehrmals zwischen nach einem Neustart ausgeführt werden. Beispielsweise der Start-Vorgang wird jedes Mal, wenn die Rolle-Freigabe ausgeführt werden, und darf keine Rolle archiviert immer einen Neustart enthalten. Startaufgaben sollten in einer Weise geschrieben werden, mit dem sie mehrfach ohne Probleme ausführen können.

Startaufgaben müssen Enden eines **Errorlevel** (oder Beendigungscode) von 0 (null) für den Startprozess ausführen. Wenn eine Startaufgabe eine nicht-NULL-Wert **Errorlevel**endet, wird die Rolle nicht gestartet.


## <a name="role-startup-order"></a>Rolle beim Start-Reihenfolge

Die folgende Liste enthält das Rolle Start Verfahren in Azure aus:

1. Die Instanz als **Felder starten** gekennzeichnet ist und nicht den Datenverkehr empfängt.

2. Alle Aufgaben werden gemäß ihren **TaskType** Attribut ausgeführt.
    - Die **einfachen** Aufgaben synchron, ausgeführt werden einzeln nacheinander.
    - Die **Hintergrund-** und **Vordergrundfarben** Aufgaben werden asynchrone, parallel, die dem Vorgang Start gestartet.  
       
    > [AZURE.WARNING] IIS möglicherweise nicht vollständig konfiguriert in welcher Phase Start Vorgang in den Prozess starten, damit die Rolle-spezifische Daten sind möglicherweise nicht verfügbar. Startaufgaben, die Rolle-spezifische Daten erfordern sollten [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx)verwenden.

3. Der Rolle Host-Prozess wird gestartet, und die Website in IIS erstellt.

4. Die [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) wird aufgerufen.

5. Die Instanz als **bereit** markiert ist, und den Datenverkehr an die Instanz weitergeleitet wird.

6. Die [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) wird aufgerufen.


## <a name="example-of-a-startup-task"></a>Beispiel für eine Startaufgabe

Startaufgaben werden in der Datei [ServiceDefinition.csdef] im **Task** -Element definiert. Das Attribut **Befehlszeile** gibt, den Namen und die Parameter der Batchdatei Start oder Console-Befehl, das Attribut **ExecutionContext** gibt an, die Berechtigungsstufe des Vorgangs Autostart, und das Attribut **TaskType** gibt an, wie die Aufgabe ausgeführt wird.

In diesem Beispiel wird eine Umgebungsvariable, **MyVersionNumber**, für den Start-Vorgang erstellt und auf den Wert "**1.0.0.0**" festgelegt.

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

Im folgenden Beispiel schreibt die Batchdatei **Startup.cmd** der Zeile "die aktuelle Version ist 1.0.0.0" zu der Datei StartupLog.txt im Verzeichnis, indem Sie die Variable TEMP-Umgebung angegeben. Die `EXIT /B 0` Zeile wird sichergestellt, dass die Startaufgabe mit einer **Errorlevel** 0 (null) endet.

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [AZURE.NOTE] In Visual Studio die Eigenschaft **Kopieren in die Ausgabeverzeichnis** für Ihre Stapel Startdatei sollte festgelegt werden immer **Kopieren** um sicherzustellen, dass Ihre Stapel Startdatei ordnungsgemäß zu Ihrem Projekt auf Azure bereitgestellt wird (**Approot\\Papierkorb** für Webrollen und **Approot** für Worker-Rollen).

## <a name="description-of-task-attributes"></a>Beschreibung der Aufgabenattribute

Im folgenden werden die Attribute des **Task** -Elements in der Datei [ServiceDefinition.csdef] beschrieben:

**Befehlszeile** - gibt die Befehlszeile für den Start-Vorgang an:

- Der Befehl mit optional Befehlszeilenparameter, der den Start-Vorgang beginnt.
- Dies ist häufig der Dateiname einer Batchdatei cmd oder bat.
- Der Vorgang ist relativ zu den AppRoot\\Ordner Bin für die Bereitstellung. Umgebungsvariablen werden nicht in den Pfad und den Dateinamen des Vorgangs bestimmen erweitert. Wenn die Erweiterung erforderlich ist, können Sie eine kleine CMD-Skript erstellen, die die Startaufgabe ruft.
- Ist eine Console-Anwendung oder eine Batchdatei, mit die ein [PowerShell-Skript](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task)beginnt.

**ExecutionContext** - gibt die Berechtigungsstufe für den Start-Vorgang an. Die Berechtigungsstufe kann eingeschränkt oder erweiterte Berechtigungen:

- **Eingeschränkte**  
Der Start-Vorgang führt mit dieselben Berechtigungen wie die Rolle. Wenn das **ExecutionContext** -Attribut für die [Laufzeit] Element auch **beschränkt**ist, werden dann Benutzerberechtigungen verwendet.

- **erhöhten**  
Der Start-Vorgang wird mit Administratorrechten ausgeführt werden. Dadurch wird Start Programme installieren IIS-Konfiguration vorzunehmen, Registrierung Änderungen ausführen und anderen Administrator Ebene Aufgaben, ohne die Berechtigungsstufe der Rolle selbst zu erhöhen.  

> [AZURE.NOTE] Die Berechtigungsstufe eines Vorgangs Start muss nicht die Rolle selbst identisch sein.

**TaskType** - gibt an, wie, wenn ein Vorgang Start ausgeführt wird.

- **einfache**  
Vorgänge werden synchron, ausgeführt einzeln nacheinander in der Reihenfolge in der Datei [ServiceDefinition.csdef] angegeben. Eine **einfache** Start-Aufgabe mit einer **Errorlevel** 0 (null) endet, wird die nächste **einfache** Startaufgabe ausgeführt. Es gibt keine weitere **einfache** Startaufgaben ausführen, wird die Rolle selbst gestartet.   

    > [AZURE.NOTE] Wenn ein nicht-NULL-Wert **Errorlevel**der **einfachen** Vorgang endet, wird die Instanz blockiert. Nachfolgende **einfache** Startaufgaben und die Rolle Kopien werden nicht gestartet.

    Um sicherzustellen, dass Ihre Batchdatei mit einer **Errorlevel** 0 (null) endet, führen Sie den Befehl `EXIT /B 0` am Ende des Prozesses Stapel Datei.

- **Hintergrund**  
Aufgaben werden asynchrone parallel mit den Start der Rolle ausgeführt werden.

- **Vordergrund**  
Aufgaben werden asynchrone parallel mit den Start der Rolle ausgeführt werden. Der wesentliche Unterschied zwischen einem **Vordergrund** und **Hintergrund** ist, dass ein Vorgang **Vordergrund** wird verhindert, dass die Rolle aus Wiederverwendung oder beendet werden soll, bis der Vorgang beendet ist. **Die Hintergrundaufgaben** müssen diese Einschränkung nicht.

## <a name="environment-variables"></a>Umgebungsvariablen

Umgebungsvariablen sind eine Möglichkeit, Informationen zu einem Vorgang Start übergeben. Beispielsweise können Sie den Pfad ein Blob mit einem Programm für die Installation oder Portnummern, mit denen Ihre Rolle oder Einstellungen auf die Funktionen der Startaufgabe setzen.

Es gibt zwei Arten von Umgebungsvariablen für Vorgänge beim Start aus. statische Umgebungsvariablen und Umgebungsvariablen auf Grundlage der Mitglieder der [RoleEnvironment] -Klasse. Beide werden im Abschnitt [Umgebung] der Datei [ServiceDefinition.csdef] , und beide das [Variable] Element und den **Namen** Attribut verwenden.

Statische Umgebungsvariablen verwendet das **Value** -Attribut des Elements [Variable] . Im oben genannten Beispiel wird die Umgebungsvariable **MyVersionNumber** dem statischen Wert**1.0.0.0"**erstellt. Ein weiteres Beispiel wäre eine **StagingOrProduction** Umgebungsvariable erstellen, die Sie manuell zu "**staging**" oder "**Fertigung**" Werte festlegen können, basierte auf dem Wert der Variablen **StagingOrProduction** -Umgebung, die andere Startdatei-Aktionen ausführen.

Umgebungsvariablen auf Grundlage der Mitglieder der RoleEnvironment-Klasse verwenden Sie das Attribut der **Wert** des Elements [Variable] nicht. Stattdessen werden das [RoleInstanceValue] untergeordnetes Element mit dem entsprechenden Wert der **XPath** -Attribut, zum Erstellen einer Umgebungsvariable basierend auf ein bestimmtes Mitglied [RoleEnvironment] -Klasse verwendet. Werte für das Attribut **XPath** Zugriff auf verschiedenen [RoleEnvironment] Werte finden Sie [hier](cloud-services-role-config-xpath.md).



Angenommen, um eine Umgebungsvariable zu erstellen, die "**true**" ist, wenn die Instanz Serveremulator, und "**false**" ausgeführt wird, wenn in der Cloud ausgeführt, verwenden Sie die folgenden [Variable] und [RoleInstanceValue] Elemente an:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
    
            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->
    
            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>
    
        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie einige [häufige Startaufgaben](cloud-services-startup-tasks-common.md) mit der Cloud-Dienst ausführen möchten.

[Verpacken](cloud-services-model-and-package.md) der Cloud-Dienst.  


[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Aufgabe]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Beim Start]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Laufzeit]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Umgebung]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Variable]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx