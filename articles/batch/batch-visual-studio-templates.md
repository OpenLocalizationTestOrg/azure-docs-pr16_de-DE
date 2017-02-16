<properties
    pageTitle="Visual Studio-Vorlagen für Azure Stapel | Microsoft Azure"
    description="Erfahren Sie, wie diese Visual Studio-Projektvorlagen organisieren können implementieren und Ihre rechenintensiven Auslastung Azure Stapel ausgeführt"
    services="batch"
    documentationCenter=".net"
    authors="fayora"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="visual-studio-project-templates-for-azure-batch"></a>Visual Studio-Projektvorlagen für Azure Stapel

Die **Auftrags-Manager** und die **Aufgabe Prozessor Visual Studio-Vorlagen** für Stapel geben Sie Code zum helfen Ihnen, implementieren und Ihre rechenintensiven Auslastung Stapel mit wenig wie leistungsgesteuert. Dieses Dokument beschreibt diese Vorlagen und bietet einen Leitfaden für ihre Verwendung.

>[AZURE.IMPORTANT] In diesem Artikel erläutert nur Informationen auf diese zwei Vorlagen anwendbar und setzt voraus, dass Sie mit den Stapel-Dienst und Konzepte im Zusammenhang mit es vertraut sind: Pools, Knoten, Projekte und Vorgänge, Manager Projektaufgaben, Umgebungsvariablen und weitere relevante Informationen zu berechnen. Weitere Informationen finden Sie in die [Grundlagen von Azure Stapel](batch-technical-overview.md), [Stapel Übersicht über die Features für Entwickler](batch-api-basics.md)und [Erste Schritte mit der Bibliothek Azure Stapel für .NET](batch-dotnet-get-started.md).

## <a name="high-level-overview"></a>Allgemeine Übersicht

Vorlagen Auftrags-Manager und Prozessor Vorgang können zum Erstellen von zwei hilfreiche Komponenten verwendet werden:

* Eine Position Manager Aufgabe, die einem Auftrag Teiler implementiert, die eine Position in mehreren Vorgängen Teile aufzuteilen können nach unten, die unabhängig voneinander, parallel ausgeführt werden können.

* Ein Vorgang-Prozessor, der zum Ausführen der Vorbereitung und nach der Verarbeitung, um eine Anwendungsbefehlszeile verwendet werden kann.

Beispielsweise würde in einem Szenario Film Rendern im Auftrag Teiler Umwandeln einer einzelnen Film Position in Hunderte oder Tausende von einzelne Aufgaben, die einzelnen Frames separat verarbeitet werden müssten. Entsprechend der Prozessor würde die Anwendung des Renderns aufrufen und alle abhängigen Prozesse, die erforderlich sind, jede gerendert Fensterform sowie zusätzliche Aktionen (z. B. Rahmens gerenderten an einen Speicherort kopieren) durchzuführen.

>[AZURE.NOTE] Die Auftrags-Manager und Vorgang Prozessor Vorlagen sind unabhängig voneinander,, damit Sie auswählen können, eine oder beide von ihnen, je nach Anforderung Ihres Auftrags berechnen und auf Ihre Einstellungen zu verwenden.

Wie in der folgenden Abbildung gezeigt wird, werden drei Phasen einer Position berechnen, die diese Vorlagen verwendet durchlaufen:

1. Der Client-Code (z. B. Anwendung, Webdienst usw.) sendet einen Auftrag an den Stapel Dienst auf Azure angeben, wie seine Auftrags-Manager des Vorgangs im Auftrag Manager Programms.

2. Der Stapel Dienst den Auftrag Manager Vorgang auf einem Knoten berechnen und den Auftrag Teilerleiste auf die angegebene Anzahl von Vorgang Prozessor Aufgaben startet, wie viele Knoten je nach Bedarf, basierend auf den Parameter und Spezifikationen im Auftrag Teiler Code zu berechnen.

3. Die Vorgang Prozessor Aufgaben unabhängig voneinander, parallel ausgeführt, die eingegebenen Daten zu verarbeiten und die Ausgabedaten generieren.

![Diagram, in dem die Interaktion zwischen Client-Code und den Stapel-Dienst][diagram01]

## <a name="prerequisites"></a>Erforderliche Komponenten

Um die Stapelvorlagen verwenden zu können, benötigen Sie Folgendes:

* Ein Computer mit Visual Studio 2015, oder höher, bereits installiert ist.

* Stapelvorlagen, die aus der [Visual Studio Gallery-Website] zur Verfügung stehen[ vs_gallery] als Visual Studio-Erweiterungen. Es gibt zwei Methoden zum Aufrufen von Vorlagen aus:

  * Installieren Sie die über das Dialogfeld **Extensions und Aktualisierungen** in Visual Studio (Weitere Informationen finden Sie unter [Suchen und Verwenden von Visual Studio Extensions][vs_find_use_ext]). Klicken Sie im Dialogfeld **Extensions und Updates** suchen und Herunterladen der beiden folgenden Erweiterungen:

    * Azure Stapel Auftrags-Manager mit Auftrag Teiler
    * Azure Stapel Vorgang Prozessor

  * Herunterladen von Vorlagen aus dem Katalog online für Visual Studio: [Microsoft Azure Stapel Project-Vorlagen][vs_gallery_templates]

* Wenn Sie das Feature für die [Anwendungspakete](batch-application-packages.md) Bereitstellen des Auftrags-Managers verwenden möchten, und Vorgang Prozessor zum Stapel Knoten zu berechnen, müssen Sie ein Speicherkonto bei Ihrem Konto Stapel verknüpfen.

## <a name="preparation"></a>Vorbereitung

Es wird empfohlen, erstellen eine Lösung, die Ihren Vorgesetzten Auftrag als auch Ihren Aufgabenprozessor enthalten kann ein, da dies Code zwischen Ihrem Auftrags-Manager und die Aufgabe Prozessor Programme freigeben erleichtern kann. Gehen Sie folgendermaßen vor, um diese Lösung zu erstellen:

1. Öffnen Sie Visual Studio 2015, und wählen Sie die **Datei** > **neu** > **Projekt**.

2. Klicken Sie unter **Vorlagen**erweitern Sie **Andere Projekttypen**, klicken Sie auf **Visual Studio-Lösungen**, und wählen Sie dann auf **Leere Lösung**.

3. Geben Sie einen aussagekräftigen Namen für die Anwendung und den Zweck dieser Lösung (z. B. "LitwareBatchTaskPrograms").

4. Um die neue Lösung erstellen, klicken Sie auf **OK**.

## <a name="job-manager-template"></a>Auftrags-Manager-Vorlage

Die Vorlage Auftrags-Manager können Sie eine Aufgabe-Manager implementieren, die die folgenden Aktionen ausführen können:

* Eine Position in mehreren Vorgängen aufteilen.
* Senden Sie diese Aufgaben für Stapel ausgeführt.

>[AZURE.NOTE] Weitere Informationen zu Projektaufgaben-Manager finden Sie unter [Übersicht über die Stapel Features für Entwickler](batch-api-basics.md#job-manager-task).

### <a name="create-a-job-manager-using-the-template"></a>Erstellen eines Auftrags-Manager mithilfe der Vorlage

Um die Lösung einen Auftrags-Manager hinzuzufügen, die Sie zuvor erstellt haben, gehen Sie folgendermaßen vor:

1. Öffnen Sie Ihre vorhandene Lösung in Visual Studio 2015.

2. Klicken Sie im Explorer-Lösung mit der rechten Maustaste in der Lösung, klicken Sie auf **Hinzufügen** > **Neues Projekt**.

3. Unter **Visual c#** **Cloud**klicken Sie auf, und klicken Sie dann auf **Azure Stapel Auftrags-Manager mit Auftrag Teiler**.

4. Geben Sie einen Namen, der Ihrer Anwendung beschreibt und identifiziert dieses Projekt als der Auftrags-Manager (z. B. "LitwareJobManager").

5. Klicken Sie auf **OK**, um das Projekt zu erstellen.

6. Erstellen Sie abschließend das Projekt, um Visual Studio laden Sie alle NuGet-Paketen verwiesen wird, und stellen Sie sicher, dass das Projekt gültig ist, bevor Sie beginnen, ändern sie erzwingen.

### <a name="job-manager-template-files-and-their-purpose"></a>Vorlagendateien Auftrags-Manager und ihre Verwendung

Wenn Sie ein Projekt mit der Vorlage Auftrags-Manager erstellen, generiert es drei Gruppen von Codedateien:

* Die wichtigsten Programmdatei (Program.cs). Dies enthält das Programmeinstiegspunkt und die Behandlung von Ausnahmen auf oberster Ebene. Sie dürfen nicht normalerweise müssen, um dies zu ändern.

* Frameworkverzeichnis. Diese Datei enthält die Dateien, die für die Arbeit 'häufig verwendeten' vom Programm Manager Auftrag – Entpacken Parameter, Hinzufügen von Vorgängen zur Stapelverarbeitung usw. verantwortlich. Sie dürfen nicht normalerweise müssen diese Dateien zu ändern.

* Die Position Teiler Datei (JobSplitter.cs). Dies ist, wo Sie Ihre anwendungsspezifische Logik für ein Projekt in Aufgaben aufteilen platziert werden.

Natürlich können Sie zusätzliche Dateien nach Bedarf zur Unterstützung von Auftrag Teiler Code, abhängig von der Komplexität der Position aufteilen Logik hinzufügen.

Die Vorlage wird auch standard .NET Projektdateien wie eine CSPROJ-Datei, app.config, packages.config usw. generiert.

Die restlichen in diesem Abschnitt werden die verschiedenen Dateien und deren Codestruktur und wird erläutert, was bedeutet, dass jede Klasse.

![Visual Studio Solution Explorer mit der Auftrags-Manager Vorlage Lösung][solution_explorer01]

**Framework-Dateien**

* `Configuration.cs`: Das Laden von Auftrag Konfigurationsdaten wie Stapel Kontodetails, verknüpfte Speicher Anmeldeinformationen, Position und Informationen zum Vorgang und Position Parameter kapselt. Darüber hinaus Zugriff auf Stapel defined Umgebungsvariablen (Siehe-Umgebung, die Einstellungen für Aufgaben, die in der Dokumentation Stapel) über die Klasse Configuration.EnvironmentVariable.

* `IConfiguration.cs`: Fasst die Implementierung der Klasse Konfiguration, damit Sie Ihre Position Teiler mithilfe eines Konfigurationsobjekts gefälschte oder simulierten Einheit testen können.

* `JobManager.cs`: Koordiniert die Komponenten des Programms Manager Position. Es ist für die Initialisierung der Position Teiler, den Auftrag Teiler aufrufen und verteilen die Aufgaben, die zurückgegebene im Auftrag Teilerleiste an dem Absender Vorgang verantwortlich ist.

* `JobManagerException.cs`: Stellt einen Fehler, der Auftrags-Manager beenden. JobManagerException dient zum Fehler 'erwarteten' umbrechen, wo bestimmte Diagnoseinformationen als Teil der Terminierung bereitgestellt werden.

* `TaskSubmitter.cs`: Diese Klasse ist verantwortlich zum Hinzufügen von Aufgaben, die von den Auftrag Teilerleiste an die Stapelverarbeitung zurückgegeben. Die Auftrags-Manager Klasse Aggregate die Abfolge von Vorgängen in Stapel für die effiziente aber schnell Ergänzung zu den Auftrag, klicken Sie dann ruft TaskSubmitter.SubmitTasks auf einen Hintergrund-Thread für jedes Blatt.

**Position Teiler**

`JobSplitter.cs`: Diese Klasse enthält anwendungsspezifische Logik für den Auftrag in Aufgaben aufzuteilen. Das Framework Ruft die JobSplitter.Split-Methode, um eine Abfolge von Aufgaben, die sie für den Auftrag hinzufügt, wie die Methode, gibt zu erhalten. Dies ist die Klasse, in dem Sie die Logik der Ihre Arbeit einfügen werden. Implementieren der geteilten Methode zur Rückkehr eine Abfolge von CloudTask Instanzen für die Aufgaben, die in denen den Auftrag aufgeteilt werden soll.

**Standard .NET Befehlszeile Project-Dateien**

* `App.config`: Standard .NET Konfigurationsdatei der Anwendung.

* `Packages.config`: Standard NuGet Abhängigkeit Paketdatei.

* `Program.cs`: Enthält das Programmeinstiegspunkt und die Behandlung von Ausnahmen auf oberster Ebene.

### <a name="implementing-the-job-splitter"></a>Implementieren der Position Teiler

Beim Öffnen der Projektvorlage Auftrags-Manager, wird das Projekt die JobSplitter.cs-Datei, die standardmäßig geöffnet haben. Sie können geteilten Implementieren der Logik für die Vorgänge in Ihrem Arbeitsbelastung mithilfe der folgenden Split() Methode anzeigen:

```csharp
/// <summary>
/// Gets the tasks into which to split the job. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The job manager framework invokes the Split method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and the "yield return" statement; this allows tasks to be added
/// and to start running while splitting is still in progress.
/// </summary>
/// <returns>The tasks to be added to the job. Tasks are added automatically
/// by the job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for the split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

>[AZURE.NOTE] Kommentierten Abschnitt in der `Split()` Methode ist der einzige Abschnitt des Vorlagencodes Auftrags-Manager, das Sie ändern, indem Sie die Logik für unterschiedliche Aufgaben Ihre Aufträge Teilung vorgesehen ist. Wenn Sie einen anderen Abschnitt der Vorlage ändern möchten, stellen Sie sicher Sie sind vertraut mit der Funktionsweise der Stapelverarbeitung geworden und Testen Sie nur einige der [Stapel Codebeispielen][github_samples].

Ihre Implementierung Split() hat Zugriff auf:

* Die Position Parameter, über den `_parameters` Feld.
* Das CloudJob-Objekt, die die Position darstellt, über die `_job` Feld.
* Das CloudTask-Objekt, das den Auftrag Manager Vorgang darstellt, über die `_jobManagerTask` Feld.

Ihre `Split()` Implementierung muss nicht Aufgaben direkt zu den Auftrag hinzufügen. Stattdessen Code sollte eine Abfolge von CloudTask Objekte zurückgeben, und diese werden dem Projekt automatisch hinzugefügt durch die Framework-Klassen, die die Position Teiler aufrufen. Im Allgemeinen Verwenden # des Iterators (`yield return`) Feature Auftrag Teiler implementiert wird, wie auf diese Weise die Aufgaben zu starten, warten auf alle Vorgänge berechnet werden, statt so früh wie möglich ausführen können.

**Fehler bei der Auftrags Teiler**

Wenn Ihre Position Teiler ein Fehler auftritt, sollten sie:

* Beenden die Sequenz mithilfe der C#- `yield break` -Anweisung in der Fall der Auftrags-Manager als erfolgreich behandelt werden; oder

* Eine Ausnahme, in der Fall, die der Auftrags-Manager als behandelt werden fehlgeschlagen ist und wiederholt werden kann, je nachdem, wie Sie der Kunden konfiguriert hat).

In beiden Fällen werden alle Aufgaben, die bereits im Auftrag Teiler zurückgegebene und die Stapelverarbeitung hinzugefügt berechtigt ausführen. Wenn Sie dies nicht möchten, klicken Sie dann können Sie:

* Vor dem Zurückgeben aus der Teiler Auftrag beenden Sie den Auftrag

* Die gesamte Vorgang Auflistung vor der Rückgabe Formulierung (d. h. zurückgegeben, eine `ICollection<CloudTask>` oder `IList<CloudTask>` statt zu Ihrem Auftrag Teiler mit einer C#-Iterator implementieren)

* Verwenden Sie anordnungsbeziehungen, um alle Vorgänge, die vom erfolgreichen Abschluss der Auftrags-Manager abhängig sind

**Position Manager Wiederholungsversuche**

Wenn der Auftrags-Manager fehlschlägt, möglicherweise es durch den Stapel Dienst je nach der Clienteinstellungen "Wiederholen" wiederholt werden. Dies ist im Allgemeinen sicherer, da Wenn im Rahmen der Auftrag Aufgaben hinzufügt, sie alle Aufgaben werden ignoriert, die bereits vorhanden sind. Jedoch ist die Berechnung von Vorgängen teure, möchten Sie möglicherweise nicht Neuberechnen von Aufgaben, die bereits mit dem Projekt hinzugefügt wurden die Kosten entstehen; umgekehrt, wenn erneut ausführen nicht unbedingt die gleiche Aufgabe IDs generieren wird dann das Verhalten 'Duplikate ignorieren' nicht starten eines. In diesen Fällen sollten Sie Ihre Position Teiler zu erkennen, die Arbeit, die bereits durchgeführt wurde und nicht wiederholen aus, beispielsweise nach Durchführung einer CloudJob.ListTasks, bevor Sie anfangen, Vorgänge Rendite entwerfen.

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>Codes zum Beenden und Ausnahmen in der Vorlage Auftrags-Manager

Codes zum Beenden und Ausnahmen bieten ein Verfahren, um das Ergebnis der Ausführung eines Programms zu ermitteln und verhelfen können um Probleme mit der Ausführung des Programms zu identifizieren. Die Vorlage Auftrags-Manager implementiert die Codes zum Beenden und die in diesem Abschnitt beschriebenen Ausnahmen.

Drei Codes zum Beenden der möglichen kann eine Position Manager Aufgabe, die mit der Vorlage Auftrags-Manager implementiert wird zurückgegeben werden:

| Code | Beschreibung |
|------|-------------|
| 0    | Der Auftrags-Manager wurde erfolgreich abgeschlossen. Ihre Position Teiler Code ausgeführt wird, bis zum Abschluss, und alle Vorgänge dem Projekt hinzugefügt wurden. |
| 1    | Fehler bei der Position Manager Vorgang mit einer Ausnahme in einem Webpart 'erwarteten' des Programms Die Ausnahme wurde in einer JobManagerException mit Diagnoseinformationen übersetzt und, soweit möglich, Vorschläge für den Fehler zu beheben. |
| 2    | Fehler bei der Position Manager Vorgang mit 'unerwartete' Ausnahme Die Ausnahme wurde in standard Ausgabe protokolliert, aber der Auftrags-Manager konnte keine zusätzlichen Informationen ein Diagnose- oder Behebung hinzugefügt. |

Wenn Fehler bei Vorgang des Auftrags-Manager einige Vorgänge möglicherweise weiterhin zum Dienst hinzugefügt wurden, bevor der Fehler aufgetreten ist. Die folgenden Aufgaben werden normal ausgeführt. Informationen zu dieser Codepfad finden Sie unter "Position der Teilerleiste-Fehler" über.

Alle Ausnahmen zurückgegebene Informationen wird in stdout.txt und stderr.txt-Dateien geschrieben. Weitere Informationen finden Sie unter [Fehlerbehandlung](batch-api-basics.md#error-handling).

### <a name="client-considerations"></a>Client-Aspekte

In diesem Abschnitt werden einige Client Implementierung Anforderungen beim Aufrufen eines Auftrags-Managers auf der Grundlage dieser Vorlage. Details zum Übergeben von Parametern und umgebungseinstellungen finden Sie unter [wie Parameter und Umgebungsvariablen aus dem Clientcode zu übergeben](#pass-environment-settings) .

**Obligatorische Anmeldeinformationen**

Um die Azure Stapelverarbeitung Aufgaben hinzugefügt haben, erfordert der Auftrag Manager Vorgang Ihre Azure Stapel Konto URL und Schlüssel. Sie müssen diese Elemente in Umgebungsvariablen mit dem Namen YOUR_BATCH_URL und YOUR_BATCH_KEY übergeben. Sie können diese im Auftrag-Manager umgebungseinstellungen Vorgang festlegen. Angenommen, in einem C#-Client:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Speicher-Anmeldeinformationen**

In der Regel, muss der Client nicht die Anmeldeinformationen des Kontos verknüpfte Speicher an die Position Manager Aufgabe zur, da Buchstabe a am häufigsten Auftrag Projektmanager nicht das Speicherkonto verknüpfte explizit zugreifen müssen und (b) das verknüpfte Speicherkonto wird als eine allgemeine-Umgebung, die Einstellung für das Projekt häufig auf alle Aufgaben bereitgestellt. Wenn das verknüpfte Speicherkonto über die allgemeine umgebungseinstellungen nicht für Sie bereit sind, und der Auftrags-Manager Zugriff auf verknüpfte Speicher benötigt, sollten Sie die Anmeldeinformationen verknüpfte Speicher wie folgt angeben:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**Position-Manager vorgangseinstellungen**

Der Client sollte die Position Manager *KillJobOnCompletion* auf **falsch**gesetzt.

Es ist gewöhnlich sicherer für den Client *RunExclusive* auf **false**festlegen.

Der Client sollte die Sammlung *ResourceFiles* oder *ApplicationPackageReferences* verwenden, um den Auftrag haben Manager ausführbare (und dessen erforderlichen DLL-Dateien) zum Berechnungsknoten bereitgestellt.

Standardmäßig wird der Auftrags-Manager nicht wiederholt schlägt. Je nach der Position Logik Manager, der Client möglicherweise Wiederholungsversuche über *Einschränkungen*aktivieren möchten/*MaxTaskRetryCount*.

**Position-Einstellungen**

Wenn Sie den Auftrag Teiler Aufgaben mit Abhängigkeiten gibt aus, muss der Client des Projekts UsesTaskDependencies true festgelegt.

Im Modell Position Teilerleiste ist es ungewöhnliche von Clients mit Aufgaben für Projekte zusätzlich zu was im Auftrag Teiler erstellt hinzufügen möchten. Der Client sollten daher normalerweise des Projekts *OnAllTasksComplete* auf **Terminatejob**festgelegt.

## <a name="task-processor-template"></a>GUID des Prozessor-Vorlage

Eine Aufgabenprozessor Vorlage hilft Ihnen einen Aufgabenprozessor implementiert wird, der die folgenden Aktionen ausführen können:

* Richten Sie die Informationen zum Ausführen von jedem Vorgang Stapel erforderlich.
* Führen Sie alle Aktionen, die durch jeden Vorgang Stapel erforderlich.
* Aufgabenausgaben dauerhaft zu speichern.

Zwar ein Vorgang Prozessor auszuführende Aufgaben auf Stapel nicht erforderlich ist, ist der wesentlicher Vorteil der Verwendung eines Aufgabe Prozessors an, dass es sich um einen Wrapper zum Implementieren der alle Aufgabenaktionen Ausführung an einem Ort bietet. Beispielsweise, wenn Sie mehrere Programme im Kontext der einzelnen Aufgaben ausführen müssen oder wenn Sie die zu kopierenden müssen Daten dauerhaft nach Abschluss der jeweiligen Aufgabe.

Die von der Aufgabenprozessor ausgeführten Aktionen können als einfache oder komplexe, und wie viele oder wenige, je nach Bedarf, indem Sie Ihre Arbeitsbelastung sein. Darüber hinaus alle Aufgabenaktionen in einem Vorgang Prozessor implementiert wird, können leicht aktualisieren oder hinzufügen Aktionen auf Änderungen Applikationen oder Arbeitsbelastung Anforderungen basieren. In einigen Fällen ein Aufgabenprozessor nicht die optimale Lösung für Ihre Implementierung möglicherweise jedoch hinzufügen unnötigen Komplexität, beispielsweise beim Ausführen von Aufträgen, die über eine einfache Befehlszeile schnell gestartet werden können.

### <a name="create-a-task-processor-using-the-template"></a>Erstellen einer Aufgabenprozessor mit der Vorlage

Um eine Aufgabenprozessor die Lösung hinzuzufügen, die Sie zuvor erstellt haben, gehen Sie folgendermaßen vor:

1. Öffnen Sie Ihre vorhandene Lösung in Visual Studio 2015.

2. Im Lösung-Explorer mit der rechten Maustaste in der Lösung, klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Neues Projekt**.

3. Unter **Visual c#** **Cloud**klicken Sie auf, und klicken Sie dann auf **Azure Stapel Vorgang Prozessor**.

4. Geben Sie einen Namen, der Ihrer Anwendung beschreibt und identifiziert dieses Projekt als Aufgabenprozessor (z. B. "LitwareTaskProcessor").

5. Klicken Sie auf **OK**, um das Projekt zu erstellen.

6. Erstellen Sie abschließend das Projekt, um Visual Studio laden Sie alle NuGet-Paketen verwiesen wird, und stellen Sie sicher, dass das Projekt gültig ist, bevor Sie beginnen, ändern sie erzwingen.

### <a name="task-processor-template-files-and-their-purpose"></a>Aufgabe Prozessor Vorlagendateien und ihre Verwendung

Wenn Sie ein Projekt mit der Aufgabe Prozessor Vorlage erstellen, generiert es drei Gruppen von Codedateien:

* Die wichtigsten Programmdatei (Program.cs). Dies enthält das Programmeinstiegspunkt und die Behandlung von Ausnahmen auf oberster Ebene. Sie dürfen nicht normalerweise müssen, um dies zu ändern.

* Frameworkverzeichnis. Diese Datei enthält die Dateien, die für die Arbeit 'häufig verwendeten' vom Programm Manager Auftrag – Entpacken Parameter, Hinzufügen von Vorgängen zur Stapelverarbeitung usw. verantwortlich. Sie dürfen nicht normalerweise müssen diese Dateien zu ändern.

* Der Vorgang-Prozessor-Datei (TaskProcessor.cs). Dies ist, wo Sie Ihre anwendungsspezifische Logik für das Ausführen einer Aufgabe (in der Regel durch Standardtelefonnummern an eine vorhandene ausführbare Datei) gespeichert werden. Vor und nach dem Code, z. B. weitere Daten herunterladen oder Hochladen von Ergebnisdateien, steht auch hier aus.

Natürlich können Sie zusätzliche Dateien nach Bedarf zur Unterstützung von Ihrer Aufgabe Prozessor Codes, abhängig von der Komplexität der Position aufteilen Logik hinzufügen.

Die Vorlage wird auch standard .NET Projektdateien wie eine CSPROJ-Datei, app.config, packages.config usw. generiert.

Die restlichen in diesem Abschnitt werden die verschiedenen Dateien und deren Codestruktur und wird erläutert, was bedeutet, dass jede Klasse.

![Visual Studio Solution Explorer mit der Aufgabenprozessor Vorlage Lösung][solution_explorer02]

**Framework-Dateien**

* `Configuration.cs`: Das Laden von Auftrag Konfigurationsdaten wie Stapel Kontodetails, verknüpfte Speicher Anmeldeinformationen, Position und Informationen zum Vorgang und Position Parameter kapselt. Darüber hinaus Zugriff auf Stapel defined Umgebungsvariablen (Siehe-Umgebung, die Einstellungen für Aufgaben, die in der Dokumentation Stapel) über die Klasse Configuration.EnvironmentVariable.

* `IConfiguration.cs`: Fasst die Implementierung der Klasse Konfiguration, damit Sie Ihre Position Teiler mithilfe eines Konfigurationsobjekts gefälschte oder simulierten Einheit testen können.

* `TaskProcessorException.cs`: Stellt einen Fehler, der Auftrags-Manager beenden. TaskProcessorException dient zum Fehler 'erwarteten' umbrechen, wo bestimmte Diagnoseinformationen als Teil der Terminierung bereitgestellt werden.

**Aufgabenprozessor**

* `TaskProcessor.cs`: Führt die Aufgabe aus. Das Framework Ruft die Methode TaskProcessor.Run. Dies ist die Klasse, in dem Sie die anwendungsspezifische Logik der Aufgabe eingefügt werden. Implementieren der Methode ausführen, um:
  * Analysieren Sie und überprüfen Sie die Aufgabenparameter
  * Verfassen Sie die Befehlszeile für externe Programme, die Sie aufrufen möchten.
  * Melden Sie sich alle Diagnoseinformationen, die für das Debuggen erforderlich machen
  * Starten eines Prozesses über die Befehlszeile
  * Beenden des Prozesses warten
  * Erfassen Sie den Beendigungscode des Prozesses zu bestimmen, ob es erfolgreich durchgeführt wurde
  * Speichern Sie die Ausgabedateien dauerhaft beibehalten werden soll

**Standard .NET Befehlszeile Project-Dateien**

* `App.config`: Standard .NET Konfigurationsdatei der Anwendung.
* `Packages.config`: Standard NuGet Abhängigkeit Paketdatei.
* `Program.cs`: Enthält das Programmeinstiegspunkt und die Behandlung von Ausnahmen auf oberster Ebene.

## <a name="implementing-the-task-processor"></a>Implementieren des Prozessors

Wenn Sie die Aufgabenprozessor Vorlage Project öffnen, wird das Projekt die TaskProcessor.cs-Datei, die standardmäßig geöffnet haben. Sie können ausführen Implementieren der Logik für die Vorgänge in Ihrem Arbeitsbelastung Run() Abschreibung abgebildet:

```csharp
/// <summary>
/// Runs the task processing logic. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The task processor framework invokes the Run method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check the exit code of that program and
/// save output files to persistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for the task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
>[AZURE.NOTE] Kommentierte Abschnitt in die Run()-Methode ist nur Abschnitt des Vorgangs Prozessor Vorlagencodes, die Sie ändern, indem Sie die ausführen Logik für die Vorgänge in Ihrem Arbeitsbelastung vorgesehen ist. Wenn Sie einen anderen Abschnitt der Vorlage ändern möchten, wenden Sie sich bitte zuerst Vertrautmachen Sie mit Stapel Funktionsweise durch den Stapel Dokumentation überprüfen und wenige Stapel Codebeispielen ausprobieren.

Die Run()-Methode ist für die Befehlszeile, warten auf alle Vorgang abgeschlossen ist, eine oder mehrere Prozesse starten ebenso verantwortlich, speichern die Ergebnisse, und klicken Sie abschließend mit folgendem Ergebniswert zurückgeben. Die Run()-Methode ist, in dem Sie die von Verarbeitungslogik für Ihre Vorgänge implementieren. Das Vorgang Prozessor Framework ruft Run()-Methode für Sie. Sie müssen nicht es selbst aufrufen.

Ihre Implementierung Run() hat Zugriff auf:

* Die Aufgabenparameter, über den `_parameters` Feld.
* Projekt- und Aufgabenzeilen-Ids, über die `_jobId` und `_taskId` Felder.
* Die Konfiguration des Vorgangs, über die `_configuration` Feld.

**Fehler bei der Aufgabe**

Im Fall eines Fehlers Sie können die Methode Run() beenden, indem Sie eine Ausnahme, aber dies bewirkt, dass den obersten Ebene Ausnahme Ereignishandler Kontrolle über den Vorgang Beendigungscode. Wenn Sie müssen den Beendigungscode, damit Sie verschiedene Fehlertypen, beispielsweise um zu Diagnosezwecken unterscheiden können, oder weil einige Ausfall-Modi den Auftrag beenden sollte und andere nicht sollten zu steuern, sollten Sie die Run()-Methode beenden, indem Sie zurückgeben eines Codes ungleich NULL beenden. Dies wird den Code der Aufgabe beenden.

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>Codes zum Beenden und Ausnahmen in der Vorlage Vorgang Prozessor

Codes zum Beenden und Ausnahmen bieten ein Verfahren, um das Ergebnis der Ausführung eines Programms zu ermitteln und verhelfen können Probleme mit der Ausführung des Programms zu identifizieren. Die Vorlage Vorgang Prozessor implementiert die Codes zum Beenden und die in diesem Abschnitt beschriebenen Ausnahmen.

Eine Aufgabe Prozessor Aufgabe, die mit der Vorlage für die Aufgabenprozessor implementiert wird, kann drei Codes zum Beenden der möglichen zurück:

| Code | Beschreibung |
|------|-------------|
|  [Process.ExitCode][process_exitcode] | Der Prozessor ist abgeschlossen. Notiz, die diese nicht impliziert das Programm, das Sie aufgerufen – nur den Erfolg der Aufgabenprozessor es erfolgreich aufgerufen, und alle nach der Verarbeitung ohne Ausnahmen ausgeführt. Die Bedeutung des Codes beenden hängt von des aufgerufenen Programms – Beendigungscode 0 bedeutet normalerweise das Programm erfolgreich war und anderen Beendigungscode bedeutet, dass das Programm fehlgeschlagen ist. |
| 1    | Fehler bei der Prozessor mit einer Ausnahme in einem Webpart 'erwarteten' des Programms Die Ausnahme zu umgewandelt wurde ein `TaskProcessorException` mit Diagnoseinformationen und, soweit möglich, Vorschläge für den Fehler zu beheben. |
| 2    | Fehler bei der Prozessor mit 'unerwartete' Ausnahme Die Ausnahme wurde in standard Ausgabe protokolliert, aber der Prozessor konnte keine zusätzlichen Informationen ein Diagnose- oder Behebung hinzugefügt. |

>[AZURE.NOTE] Wenn das Programm, das Sie aufrufen Codes zum Beenden der 1 und 2 an, dass bestimmte Fehlermodi verwendet wird, ist die dann mithilfe von Codes zum Beenden der 1 und 2 für Aufgabe Prozessorfehler nicht eindeutig. Sie können diese Aufgabe Prozessor Fehlercodes zu Codes zum Beenden der besondere ändern, indem Sie Ausnahmefälle, in der Datei Program.cs bearbeiten.

Alle Ausnahmen zurückgegebene Informationen wird in stdout.txt und stderr.txt-Dateien geschrieben. Weitere Informationen finden Sie in der Dokumentation Stapel Fehlerbehandlung.

### <a name="client-considerations"></a>Client-Aspekte

**Speicher-Anmeldeinformationen**

Wenn Ihrer Aufgabenprozessor Azure Blob-Speicher verwendet, um Ausgaben, beispielsweise mithilfe der Bibliothek Konventionen Helper Datei beizubehalten benötigt sie Zugriff auf *entweder* die Cloud-Speicher Firma Anmeldeinformationen *oder* eine BLOB-Container URL, die eine freigegebenen Access-Signatur (SAS) enthält. Die Vorlage enthält Unterstützung für die Bereitstellung von Anmeldeinformationen über gemeinsame Umgebungsvariablen. Ihren Kunden kann die Anmeldeinformationen Speicher wie folgt übergeben:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

Das Speicherkonto steht dann in der Klasse TaskProcessor über die `_configuration.StorageAccount` Eigenschaft.

Wenn Sie eine URL Container mit SAS verwenden lieber, können Sie diese auch über eine Position allgemeine Umgebung Einstellung übergeben, aber die Aufgabe Prozessor Vorlage schließt derzeit nicht integrierten Unterstützung für diese.

**Einrichten von Speicher**

Es wird empfohlen, dass der Client oder Auftrag Manager Vorgang alle Container von Aufgaben erforderlich, vor dem Hinzufügen von Aufgaben, die im Auftrag erstellen. Dies ist erforderlich, wenn Sie eine URL Container mit SAS verwenden, als solche eine URL nicht über die Berechtigung zum Erstellen des Containers enthalten ist. Es wird empfohlen, auch wenn Sie Anmeldeinformationen für Speicher-Konto, senden, wie sie jede Aufgabe CloudBlobContainer.CreateIfNotExistsAsync für den Container anzurufen speichert.

## <a name="pass-parameters-and-environment-variables"></a>Übergeben Sie Parameter und Umgebungsvariablen

### <a name="pass-environment-settings"></a>Übergeben umgebungseinstellungen

Ein Client kann Informationen an die Position Manager Aufgabe in Form von umgebungseinstellungen übergeben. Diese Informationen kann dann vom Task-Manager Position verwendet werden, wenn die Aufgabe Prozessor Aufgaben generieren, die als Teil der Aufgabe berechnen ausgeführt wird. Beispiele für die Informationen, die Sie als umgebungseinstellungen übergeben, sind:

* Speicher Konto Servername und die Kontoinformationen Tasten
* Stapel Konto URL
* Kontoschlüssel Stapel

Der Dienst Stapel weist eine einfache Methode zum umgebungseinstellungen an eine Position Manager Aufgabe mithilfe übergeben die `EnvironmentSettings` Eigenschaft [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Um beispielsweise erhalten die `BatchClient` Instanz für ein Konto Stapel können Sie als Umgebungsvariablen vom Client der URL und dem freigegebenen Key Anmeldeinformationen für das Konto Stapel Fehlercode übergeben. Ebenso Zugriff auf die Speicherkonto, das mit dem Stapel-Konto verknüpft ist, können Sie den Kontonamen für den Speicher und die Speicher kontoschlüssel als Umgebungsvariablen übergeben.

### <a name="pass-parameters-to-the-job-manager-template"></a>Übergeben von Parametern an die Vorlage Auftrags-Manager

In vielen Fällen ist es sinnvoll, pro Projekt Parameter an den Auftrag Task Manager, können Sie den Auftrag Teilen Prozess steuern oder so konfigurieren Sie die Aufgaben für das Projekt zu übergeben. Sie können hierzu Hochladen einer JSON-Datei mit dem Namen parameters.json als Ressourcendatei für den Auftrag-Task-Manager. Die Parameter können dann in zur Verfügung stehen die `JobSplitter._parameters` -Feld in der Vorlage Auftrags-Manager.

>[AZURE.NOTE] Der integrierten Parameter Ereignishandler unterstützt nur Zeichenfolge-Wörterbücher. Wenn Sie komplexe JSON-Werte als Parameterwerte übergeben möchten, müssen Sie diese als Zeichenfolgen übergeben und in die Teilerleiste Position zu analysieren oder Ändern des Rahmen `Configuration.GetJobParameters` Methode.

### <a name="pass-parameters-to-the-task-processor-template"></a>Übergeben von Parametern an die Vorlage Vorgang Prozessor

Sie können auch auf einzelne Vorgänge, die mit der Vorlage Vorgang Prozessor implementiert Parameter übergeben. Genauso wie sieht mit der Vorlage für den Auftrag-Manager, die Aufgabe Prozessor Vorlage für eine Ressourcendatei mit dem Namen

Parameters.JSON, und wenn gefundenen lädt es als das Wörterbuch Parameter. Es gibt verschiedene Optionen für Parameter an die Aufgabe Prozessor Aufgaben zu übergeben:

* Wiederverwenden Sie die Position Parameter JSON. Dies funktioniert auch, wenn der Parameter nur Auftrag organisationsweite (z. B., eine Rendern Höhe und Breite) sind. Hierfür beim Erstellen einer CloudTask im Auftrag Teiler, fügen Sie einen Verweis auf das Objekt parameters.json Ressource File aus den Auftrag Manager Vorgang ResourceFiles (`JobSplitter._jobManagerTask.ResourceFiles`) die CloudTasks ResourceFiles-Auflistung.

* Generieren und Hochladen eines Dokuments Task-spezifische parameters.json als Teil der Ausführung des Auftrags Teiler und die Blob in den Vorgang, Ressource Dateien Auflistung verweisen. Dies ist erforderlich, wenn unterschiedliche Aufgaben verschiedene Parameter haben. Beispiel für möglicherweise ein Szenario 3D Rendern Stelle, an der der Index Rahmen, die dem Vorgang als Parameter weitergegeben wird.

>[AZURE.NOTE] Der integrierten Parameter Ereignishandler unterstützt nur Zeichenfolge-Wörterbücher. Wenn Sie komplexe JSON-Werte als Parameterwerte übergeben möchten, müssen Sie diese als Zeichenfolgen übergeben und im Aufgabenbereich Prozessor zu analysieren oder Ändern der Frameworks `Configuration.GetTaskParameters` Methode.

## <a name="next-steps"></a>Nächste Schritte

### <a name="persist-job-and-task-output-to-azure-storage"></a>Projekt- und Aufgabenzeilen Ausgabe in Azure-Speicher beibehalten

Ein weiteres nützliche Tool in Stapel Lösungsentwicklung ist [Azure Stapel Datei Konventionen][nuget_package]. Verwenden Sie diese .NET Class-Bibliothek (aktuell in der Seitenansicht) in Ihren Stapel, auf einfache Weise speichern und Vorgang Ausgaben aus und in Azure-Speicher abzurufen. [Azure-Stapelverarbeitung beibehalten werden und ein Vorgang ausgegeben](batch-task-output.md) enthält eine umfassende Erläuterung der Bibliothek und deren Verwendung.

### <a name="batch-forum"></a>Stapel-Forum

Das [Azure Stapel Forum] [ forum] auf MSDN finden Sie hervorragende Stapel diskutieren, und stellen Sie Fragen zu dem Dienst. Am auf über für hilfreiche Beiträge "dauerhaft", und stellen Sie Ihre Fragen auftretende, während Sie Ihre Lösungen Stapel erstellen.

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
