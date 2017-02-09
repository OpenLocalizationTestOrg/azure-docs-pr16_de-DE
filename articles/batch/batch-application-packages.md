<properties
    pageTitle="Einfache Anwendungsinstallation und Verwaltung in Azure Stapel | Microsoft Azure"
    description="Verwenden der Anwendung Pakete Feature Azure Blattnamen problemlos mehrere Applikationen und Versionen für die Installation auf Stapel verwalten Knoten zu berechnen."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="application-deployment-with-azure-batch-application-packages"></a>Bereitstellung der Anwendung mit Azure Stapel Anwendungspaketen

Die Anwendung Pakete Features des Blatts Azure bietet einfache Verwaltung von Applications Vorgang und ihre Bereitstellung auf die Datenverarbeitungsknoten in Ihrem Ressourcenpool. Mit der Anwendungspaketen können hochladen und Verwalten von mehreren Versionen der Programme, die Ihre Aufgaben ausführen, einschließlich deren Hilfsdateien. Sie können dann automatisch eine oder mehrere der folgenden Anwendungen zu den Knoten berechnen im Pool bereitstellen.

In diesem Artikel erfahren Sie, wie Sie hochladen und Verwalten der Anwendungspakete Azure-Portal. Dann werden Sie erfahren, wie diese auf einem Ressourcenpool berechnen Knoten mit den [Stapel .NET] installieren[ api_net] Bibliothek.

> [AZURE.NOTE] Die Anwendung Pakete Features, die hier beschriebenen hat Vorrang vor der "Stapel Apps" Feature in früheren Versionen des Diensts verfügbar.

## <a name="application-package-requirements"></a>Anwendung Paket Anforderungen

Sie müssen [Link ein Konto Azure-Speicher](#link-a-storage-account) bei Ihrem Konto Stapel Anwendungspakete verwenden.

Das Anwendung Pakete Feature in diesem Artikel beschriebenen ist kompatibel *nur* mit Stapel-Pools bereitgestellt, die nach 10 März 2016 erstellt wurden. Anwendungspakete werden nicht bereitgestellt werden, um Knoten in Pools erstellt vor diesem Datum zu berechnen.

Diese Funktion wurde in [Stapel REST-API] eingeführt[ api_rest] Version 2015-12-01.2.2 und den entsprechenden [Stapel .NET] [ api_net] Bibliotheksversion 3.1.0. Es empfiehlt sich, dass Sie die neueste Version von API bei der Arbeit mit Stapel immer verwenden.

> [AZURE.IMPORTANT] Derzeit unterstützt nur *CloudServiceConfiguration* Pools Anwendungspakete. Anwendung-Paketen können keine in Pools mit VirtualMachineConfiguration Bilder erstellt. Finden Sie die [Konfiguration des virtuellen Computers](batch-linux-nodes.md#virtual-machine-configuration) Abschnitt [Bereitstellen von Linux berechnen Knoten in Azure Stapel Pools](batch-linux-nodes.md) für Weitere Informationen über die zwei verschiedenen Konfigurationen.

## <a name="about-applications-and-application-packages"></a>Informationen zu Anwendungen und Anwendungspakete

Bezieht sich in Azure Stapel eine *Anwendung* auf einen Satz von Versionsnummern Binärdateien, die zum Berechnen Knoten in der Ressourcenpool automatisch heruntergeladen werden kann. Ein *Anwendungspaket* bezieht sich auf eine *bestimmte Gruppe* von diese Binärdateien und einer bestimmten *Version* der Anwendung darstellt.

![Auf hoher Ebene Diagramm von Applications und Anwendungspaketen][1]

### <a name="applications"></a>Applikationen

Anwendung in Stapel enthält einen oder mehrere Anwendung verpackt und gibt die von Konfigurationsoptionen für die Anwendung an. Beispielsweise kann eine Anwendung geben Sie die Standard Anwendung Paketversion installieren Datenverarbeitungsknoten und ob Pakete, die aktualisiert oder gelöscht werden können.

### <a name="application-packages"></a>Anwendungspaketen

Eine Anwendungspaket ist eine ZIP-Datei, die die Anwendungsbinärdateien enthält und Hilfsdateien, die zum Ausführen Ihrer Aufgaben erforderlich sind. Jedes Anwendungspaket steht für eine bestimmte Version der Anwendung.

Sie können die Anwendungspakete Ebene Ressourcenpool und der Aufgabe angeben. Wenn Sie ein Ressourcenpool oder eine Aufgabe erstellen, können Sie eine oder mehrere der folgenden Pakete und (optional) eine Version angeben.

* **Ressourcenpool Anwendungspakete** sind für *jeden* Knoten im Pool bereitgestellt. Bereitstellen einer Anwendung, wenn ein Knoten einem Ressourcenpool Beitritt und neu gestartet oder einem neuen Image versehen.

    Ressourcenpool Anwendungspakete sind geeignet, wenn alle Knoten in einem Pool eines Auftrags Aufgaben ausführen. Sie können ein oder mehrere Anwendungspakete angeben, wenn Sie einem Ressourcenpool erstellen und können Sie hinzufügen oder Aktualisieren von einem vorhandenen Pool Pakete. Wenn Sie einem vorhandenen Pool Anwendungspakete aktualisieren, müssen Sie dessen Knoten, um das neue Paket installieren neu starten.

* **Aufgabe Anwendungspakete** sind nur für einen berechnen Knoten geplant, führen Sie eine Aufgabe vor der Ausführung des Vorgangs Befehlszeile bereitgestellt. Wenn das Paket angegebenen Anwendung und Version bereits auf dem Knoten, es nicht erneut bereitgestellt und das vorhandene Paket verwendet wird.

    Aufgabe Anwendungspakete eignen sich in freigegebenen Pool Umgebungen, wobei unterschiedliche Stellen auf einem Ressourcenpool ausgeführt werden und der Pool wird nicht gelöscht werden, wenn ein Projekt abgeschlossen ist. Wenn der Auftrag im Pool weniger Vorgänge als Knoten aufweist, können Vorgang Anwendungspakete Datenübertragung minimiert werden, da die Anwendung nur auf Knoten bereitgestellt wird, die Vorgänge ausgeführt werden.

    Andere Szenarien, die von Vorgang Anwendungspakete profitieren werden Einzelvorgänge, die eine besonders große Anwendung, sondern auch für nur eine geringe Anzahl von Aufgaben. Eine Phase vor der Verarbeitung oder einen Zusammenführen-Vorgang, wo finde ich die Anwendung vorab verarbeiten oder Zusammenführen schwer.

> [AZURE.IMPORTANT] Es gibt hinsichtlich der Anzahl von Programmen und Anwendungspaketen in einem Stapel-Konto als auch die maximale Größe des Pakets. Weitere Informationen zu diesen Grenzen finden Sie unter [Kontingente und Grenzwerte für den Stapel Azure-Dienst](batch-quota-limit.md) .

### <a name="benefits-of-application-packages"></a>Vorteile der Anwendungspakete

Anwendungspaketen können vereinfachen Sie den Code in Ihre Lösung Stapel und senken den Aufwand erforderlich, um die Anwendungen zu verwalten, die Ihre Aufgaben ausgeführt werden.

Der Ressourcenpool des Start-Aufgabe verfügt nicht über eine lange Liste mit einzelne Ressourcendateien auf den Knoten installieren angeben. Sie müssen nicht mehrere Versionen Ihrer Anwendungsdateien im Azure-Speicher auf Ihrem Knoten oder manuell zu verwalten. Und Sie brauchen kümmern [SAS-URLs](../storage/storage-dotnet-shared-access-signature-part-1.md) für den Zugriff auf die Dateien in Ihrem Konto Speicher generieren. Stapel funktioniert in den Hintergrund mit Azure-Speicher speichern Anwendungspakete und bereitstellen, um den Knoten zu berechnen.

## <a name="upload-and-manage-applications"></a>Hochladen und Verwalten von applications

Sie können mithilfe der [Azure-Portal] [ portal] oder den [Stapel Management .NET](batch-management-dotnet.md) Bibliothek der Anwendungspakete in Ihr Konto Stapel verwalten. In den nächsten Abschnitten wir Verknüpfen einer Firma Speicher zunächst diskutieren, Hinzufügen von Applications und Pakete und diese mit dem Portal verwalten.

### <a name="link-a-storage-account"></a>Verknüpfen einer Firma Speicher

Wenn Anwendungspakete verwenden möchten, müssen Sie zuerst ein Konto Azure-Speicher bei Ihrem Konto Stapel verknüpfen. Wenn Sie ein Konto Speicherplatz für Ihr Konto Stapel noch nicht konfiguriert haben, wird das Azure-Portal eine Warnung beim ersten Verwenden Sie die Kachel **Applications** in das Blade **Stapel Konto** klicken Sie auf angezeigt.

> [AZURE.IMPORTANT] Stapelverarbeitung aktuell unterstützt *nur* **Allgemeine** Speicher Kontotyp wie in Schritt 5, [Speicherkonto erstellen](../storage/storage-create-storage-account.md#create-a-storage-account), in [zur Azure-Speicherkonten](../storage/storage-create-storage-account.md)beschrieben. Beim Verknüpfen einer Azure-Speicher-Konto bei Ihrem Stapel Konto Link *nur* eine **Allgemeine** Speicher-Konto.

![Keine Speicherkonto konfiguriert Warnung Azure-Portal][9]

Der Stapel-Dienst verwendet das zugeordnete Speicherplatz-Konto für das Speichern und Abrufen von Anwendungspakete. Nachdem Sie die beiden Konten verknüpft haben, kann Stapel automatisch in das verknüpfte Speicher-Konto zu Ihrer Datenverarbeitungsknoten gespeicherte Pakete bereitstellen. Klicken Sie auf **kontoeinstellungen Speicher** auf das Blade **Warnung** , und klicken Sie dann auf das **Konto Speicher** Blade zum Verknüpfen von Speicher-Konto bei Ihrem Konto Stapel **Speicher-Konto** .

![Wählen Sie Speicher Konto Blade Azure-Portal][10]

Es empfiehlt sich, dass ein Speicher Konto *speziell* für die Verwendung mit Ihrem Konto Stapel erstellen, und wählen Sie ihn hier. Ausführliche Informationen zum Erstellen eines Kontos Speicher finden Sie unter "erstellen ein Kontos Speicher in [zur Azure-Speicher-Konten"](../storage/storage-create-storage-account.md). Nachdem Sie ein Speicherkonto erstellt haben, können Sie dann bei Ihrem Konto Stapel verknüpfen mithilfe des Blades **Speicher-Konto** .

> [AZURE.WARNING] Da Stapel Azure-Speicher zum Speichern Ihrer Anwendungspakete verwendet, werden Sie [wie üblich belastet] [ storage_pricing] für den Zugriffsschutz BLOB-Daten. Achten Sie darauf, sollten die Größe und Anzahl Ihrer Anwendungspaketen und entfernen veraltete Pakete Kosten minimiert werden regelmäßig.

### <a name="view-current-applications"></a>Aktuelle Ansicht-Anwendungen

Um die Anwendungen in Ihr Konto Stapel anzeigen möchten, klicken Sie auf das Menüelement **Applications** im linken Menü, während das **Konto Stapel** Blade anzeigen.

![Applikationen-Kachel][2]

Daraufhin wird das **Applikationen** Blade:

![Liste von applications][3]

Das Blade **Applikationen** zeigt die ID des jede Anwendung in Ihr Konto und die folgenden Eigenschaften:

* **Pakete**- die Anzahl der Versionen dieser Anwendung zugeordnet.
* **Standard-Version**– die Version, die installiert werden, wenn Sie eine Version nicht angeben, wenn Sie die Anwendung für einen Pool festlegen. Diese Einstellung ist optional.
* **Updates zulassen**– der Wert, der angibt, ob Paket aktualisiert, Löschvorgängen und Ergänzungen zulässig sind. Wenn dies auf **Nein**festgelegt ist, werden Paket-Updates und löschungen für die Anwendung deaktiviert. Nur neue Anwendung Paketversionen können hinzugefügt werden. Die Standardeinstellung ist **Ja**.

### <a name="view-application-details"></a>Anzeigen von Details zu Anwendung

Klicken Sie auf eine Anwendung in das Blade **Applikationen** , das Blade zu öffnen, das die Details für diese Anwendung enthält.

![Anwendungsdetails][4]

Die Anwendung Details Blade können Sie die folgenden Einstellungen für eine Anwendung konfigurieren.

* **Updates zulassen**– geben an, ob Anwendungspakete, die aktualisiert oder gelöscht werden können. In diesem Artikel finden Sie unter "Aktualisieren oder löschen ein Anwendungspakets".
* **Standardversion**– Geben Sie ein Standard-Anwendung-Paket bereitstellen, um den Knoten zu berechnen.
* **Anzeigename**– ein "" Anzeigename, die Ihren Lösung können verwendet werden, wenn sie Informationen zu der Anwendung, wie in der Benutzeroberfläche von einem Dienst anzeigt, die Sie angeben, dass Ihre Kunden durch den Stapel Stapel angeben.

### <a name="add-a-new-application"></a>Hinzufügen einer neuen Anwendungs

Zum Erstellen einer neuen Anwendungs fügen Sie ein Anwendungspakets hinzu, und geben Sie eine neue, eindeutige Anwendung-ID. Das erste Paket der Anwendung, das Sie mit der neuen Anwendung ID hinzufügen kann auch die neue Anwendung erstellen.

Klicken Sie auf das **Applikationen** Blade So öffnen Sie das **neue Anwendung** Blade auf **Hinzufügen** .

![Neue Anwendung-vorher in Azure-portal][5]

Das **neue Anwendung** Blade bietet die folgenden Felder aus, um die Einstellungen der neuen Anwendung und Anwendungspaket anzugeben.

**Id der Anwendung**

Dieses Feld gibt an, die ID der neuen Anwendung, für die standard Azure Stapel ID Gültigkeitsprüfungsregeln gilt:

* Kann eine beliebige Kombination von alphanumerischen Zeichen, einschließlich Bindestriche und Unterstriche enthalten.
* Kann nicht mehr als 64 Zeichen enthalten.
* Muss innerhalb des Kontos Stapel eindeutig sein.
* Ist Groß-/Kleinschreibung beibehalten und die Groß-/Kleinschreibung.

**Version**

Gibt die Version des Anwendungspakets, die Sie hochladen. Versionszeichenfolgen werden die folgenden Gültigkeitsprüfungsregeln:

* Kann eine beliebige Kombination von alphanumerischen Zeichen, einschließlich Bindestriche, Unterstriche und Punkte enthalten.
* Kann nicht mehr als 64 Zeichen enthalten.
* Muss innerhalb der Anwendung eindeutig sein.
* Fall beibehalten und zwischen Groß-und Kleinschreibung.

**Anwendungspaket**

Dieses Feld gibt an, die ZIP-Datei, die die Anwendungsbinärdateien enthält und Hilfsdateien, die erforderlich sind, um die Anwendung ausführen. Klicken Sie auf das Feld **Wählen Sie eine Datei** oder das Ordnersymbol zu navigieren, und wählen eine ZIP-Datei, die die Dateien der Anwendung enthält.

Nachdem Sie eine Datei ausgewählt haben, klicken Sie auf **OK** , um den Upload zu Azure-Speicher zu beginnen. Wenn der Uploadvorgang abgeschlossen ist, werden Sie benachrichtigt, und das Blade wird geschlossen. Abhängig von der Größe der Datei, die Sie hochladen und der Geschwindigkeit Ihrer Netzwerk-Verbindung, kann dieser Vorgang einige Zeit dauern.

> [AZURE.WARNING] Schließen Sie das **neue Anwendung** Blade nicht, bevor der Uploadvorgang abgeschlossen ist. Auf diese Weise wird während des Uploads gestoppt.

### <a name="add-a-new-application-package"></a>Hinzufügen eines neuen Anwendungspakets

Um eine neue Version der Anwendung-Pakets für eine vorhandene Anwendung hinzuzufügen, wählen Sie eine Anwendung in das Blade **Applications** aus, klicken Sie auf **Pakete**, dann klicken Sie auf **Hinzufügen** , um das Blade **Paket hinzufügen** zu öffnen.

![Hinzufügen der Anwendung Paket Blade Azure-Portal][8]

Wie Sie sehen können, wird das **neue Anwendung** Blade entsprechen die Feldern, aber das Feld **Id der Anwendung** ist deaktiviert. Als für die neue Anwendung Meinten, geben Sie die **Version** für Ihr neues Paket, navigieren Sie zu Ihrer **Anwendung** ZIP-Paketdatei, und klicken Sie auf **OK** , um das Paket hochladen.

### <a name="update-or-delete-an-application-package"></a>Aktualisieren oder Löschen eines Anwendungspakets

Aktualisieren oder löschen ein vorhandenes Anwendungspaket, das Blade Details für die Anwendung geöffnet ist, klicken Sie auf **Pakete** , um das Blade **Pakete** zu öffnen, klicken Sie auf die **drei Punkte** in der Zeile des Anwendungspakets, die Sie ändern möchten, und wählen Sie die Aktion, die Sie ausführen möchten.

![Aktualisieren oder Löschen von Paket Azure-Portal][7]

**Aktualisieren**

Wenn Sie auf **Aktualisieren**klicken, wird das *Update-Paket* Blade angezeigt. Diese Blade ähnelt dem das *neue Anwendungspaket* Blade, jedoch nur im Paket Auswahlfeld aktiviert ist, sodass Sie eine neue Zipdatei zum Hochladen angeben.

![Update-Paket vorher in Azure-portal][11]

**Löschen**

Wenn Sie auf **Löschen**klicken, werden Sie aufgefordert, das Löschen der Paketversion zu bestätigen, und Stapel löscht das Paket aus Azure-Speicher. Wenn Sie die Standardversion von Anwendung löschen, wird die **Standardversion** Einstellung für die Anwendung entfernt.

![Löschen Sie die Anwendung][12]

## <a name="install-applications-on-compute-nodes"></a>Installieren von Applications auf berechnen Knoten

Jetzt, da Sie zum Verwalten der Anwendungspaketen mit dem Portal Azure gesehen haben, können wir erörtern bereitstellen, um Knoten zu berechnen, und führen Sie diese mit Stapelverarbeitungsaufgaben.

### <a name="install-pool-application-packages"></a>Installieren Sie die Anwendungspakete Ressourcenpool

Um ein Anwendungspaket auf allen berechnen-Knoten in einem Ressourcenpool zu installieren, geben Sie einen oder mehrere Anwendung Paket *Verweise* für den Pool an. Anwendungspakete an, denen Sie für einen Pool angeben werden auf den einzelnen Knoten berechnen installiert, wenn sich der Knoten im Ressourcenpool Beitritt und Knoten neu gestartet oder einem neuen Image versehen ist.

Geben Sie einen oder mehrere [CloudPool]Stapel .NET,[net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref] beim Erstellen einer neuen Ressourcenpool oder für einen vorhandenen Pool aus. Die [ApplicationPackageReference] [ net_pkgref] Klasse gibt eine Anwendung-ID und Version auf einem Ressourcenpool installieren Knoten zu berechnen.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicated: "1",
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package will be installed on each.
await myCloudPool.CommitAsync();
```

>[AZURE.IMPORTANT] Eine Anwendung Paket Bereitstellung aus irgendeinem Grund fehlschlägt, der Stapel Dienst den Knoten [installiertes]kennzeichnet[net_nodestate], und werden keine Aufgaben zur Ausführung auf diesem Knoten geplant. In diesem Fall sollten Sie **neu starten** zu die Bereitstellung des Pakets dieses Knotens. Einen Neustart von den Knoten wird ebenfalls aktiviert Vorgang Planung erneut auf dem Knoten.

### <a name="install-task-application-packages"></a>Installieren Sie die Anwendungspakete Aufgabe

Ähnlich wie in einem Ressourcenpool, geben Sie die Anwendung Paket *Verweise* für einen Vorgang wird. Wenn Sie ein Vorgang geplant wurde, auf einem Knoten ausführen, wird das Paket heruntergeladen und extrahiert kurz vor Ausführung des Vorgangs Befehlszeile. Wenn eine angegebene Paket und eine Version wird auf dem Knoten bereits installiert, das Paket ist nicht heruntergeladen, und das vorhandene Paket verwendet wird.

Um ein Vorgang Anwendungspaket zu installieren, Konfigurieren des Vorgangs [CloudTask][net_cloudtask]. [ApplicationPackageReferences] [net_cloudtask_pkgref] Eigenschaft:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a>Ausführen der installierten Programme

Die Pakete, die Sie für ein Ressourcenpool oder eine Aufgabe angegeben haben heruntergeladen und extrahiert an ein benanntes Verzeichnis innerhalb der `AZ_BATCH_ROOT_DIR` des Knotens. Stapel erstellt auch eine Umgebungsvariable mit, die den Pfad zu der benannten Verzeichnis enthält. Der Befehl Aufgabenzeilen verwenden diese Umgebungsvariable aus, wenn Sie die Anwendung auf dem Knoten verweisen. Die Variable befindet sich in folgendem Format:

`AZ_BATCH_APP_PACKAGE_APPLICATIONID#version`

`APPLICATIONID`und `version` sind Werte, die Sie, für die Bereitstellung angegeben haben der Anwendung und Paket Version entsprechen. Beispielsweise, wenn der angegebene dieser Version 2.7 Anwendung *Blender* sollten installiert haben, der Befehl Aufgabenzeilen verwenden diese Umgebungsvariable-, die auf ihre Dateien zugreifen:

`AZ_BATCH_APP_PACKAGE_BLENDER#2.7`

Wenn Sie eine Standardversion für eine Anwendung angeben, können Sie das Versionssuffix weglassen. Angenommen, wenn Sie als die Standardversion für Anwendung *Blender*"2.7" festlegen, Aufgaben können die folgende Umgebungsvariable verweisen, und sie Version 2.7 ausgeführt:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Der folgende Codeausschnitt zeigt eine Beispiel Vorgang Befehlszeile, die die Standardversion der *Blender* Anwendung gestartet wird:

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [AZURE.TIP] Finden Sie unter [umgebungseinstellungen für Vorgänge](batch-api-basics.md#environment-settings-for-tasks) in der [Stapel Übersicht über die Features](batch-api-basics.md) für Weitere Informationen zum Berechnen Knoten umgebungseinstellungen.

## <a name="update-a-pools-application-packages"></a>Ein Ressourcenpool Anwendungspakete aktualisieren

Wenn Sie ein vorhandenes Pool mit einer Anwendungspaket bereits konfiguriert wurde, können Sie ein neues Paket für den Pool angeben. Wenn Sie eine neue Paket über einem Ressourcenpool, gelten die folgenden angeben:

* Alle neuen Knoten, die dem Pool teilnehmen an und alle vorhandenen Knoten, der neu gestartet oder einem neuen Image versehen ist, werden das neu angegebene Paket installiert.
* Berechnen Sie, dass Knoten, die sich bereits im Pool, wenn Sie die Bezüge Paket aktualisieren das neuen Anwendungspaket nicht automatisch installiert. Diese berechnen Knoten müssen neu gestartet oder einem neuen Image versehen, um das neue Paket empfangen werden.
* Wenn ein neues Paket bereitgestellt wird, geben die erstellte Umgebungsvariablen die neuen Anwendung Paketverweise wieder.

In diesem Beispiel wurde das vorhandene Pool Version 2.7 der Anwendung *Blender* als eine der zugehörigen [CloudPool]konfiguriert[net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref]. Um den Pool Knoten mit Version 2.76b zu aktualisieren, geben Sie einen neuen [ApplicationPackageReference] [ net_pkgref] für die neue Version, und klicken Sie auf Commit ändern.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Nachdem Sie nun die neue Version konfiguriert wurde, stehen alle *neuen* Knoten, der den Pool verknüpft Version 2.76b bereitgestellt, damit zur Verfügung. Um 2.76b auf den Knoten installieren zu können, die sich *bereits* im Pool befinden, neu zu starten, oder sie ein neues Abbild. Beachten Sie, dass neu gestartet Knoten die Dateien aus vorherigen Paket Bereitstellungen beibehalten werden.

## <a name="list-the-applications-in-a-batch-account"></a>Liste der Anwendungen in einem Stapel-Konto

Sie können die Anwendungen und Softwarepaketen in einem Stapel-Konto mithilfe der [ApplicationOperations]Liste[net_appops]. [ListApplicationSummaries] [net_appops_listappsummaries] Methode.

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Umbrechen von

Mit der Anwendungspaketen helfen Ihnen, Ihre Kunden wählen Sie die Anwendungsmöglichkeiten für ihre Arbeit, und geben Sie die genaue Version verwenden, bei der Verarbeitung von Projekten mit dem Dienst Stapel aktiviert. Sie können auch die Möglichkeit, dass Ihre Kunden hochladen und Nachverfolgen ihrer eigenen Applications in Ihrem Dienst bereitstellen.

## <a name="next-steps"></a>Nächste Schritte

* Die [REST-API Stapel] [ api_rest] bietet auch Unterstützung für die Arbeit mit der Anwendungspakete. Beispielsweise finden Sie in der [ApplicationPackageReferences] [ rest_add_pool_with_packages] Element hinzufügen [ein Pool mit einer Firma] [ rest_add_pool] Informationen zu installieren, indem Sie die REST-API Pakete angeben. Finden Sie unter [Applications] [ rest_applications] ausführliche Informationen zum Anwendungsinformationen zu erhalten, indem Sie die Stapel REST-API.

* Erfahren Sie, wie programmgesteuert [Azure Stapel Konten und Kontingente mit Stapel Management .NET verwalten](batch-management-dotnet.md). Der [Stapel Management .NET] [ api_net_mgmt] Bibliothek Konto erstellen und Löschen von Features für Ihre Stapel Anwendung oder den Dienst aktivieren kann.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Anwendung-Paketen auf hoher Ebene Diagramm"
[2]: ./media/batch-application-packages/app_pkg_02.png "Applikationen Kachel Azure-Portal"
[3]: ./media/batch-application-packages/app_pkg_03.png "Applikationen vorher in Azure-portal"
[4]: ./media/batch-application-packages/app_pkg_04.png "Anwendung Details vorher in Azure-portal"
[5]: ./media/batch-application-packages/app_pkg_05.png "Neue Anwendung-vorher in Azure-portal"
[7]: ./media/batch-application-packages/app_pkg_07.png "Aktualisieren oder Löschen von Paketen Dropdown-Azure-Portal"
[8]: ./media/batch-application-packages/app_pkg_08.png "Neue Anwendung Paket-vorher in Azure-portal"
[9]: ./media/batch-application-packages/app_pkg_09.png "Keine verknüpften Speicher Konto Benachrichtigung"
[10]: ./media/batch-application-packages/app_pkg_10.png "Wählen Sie Speicher Konto Blade Azure-Portal"
[11]: ./media/batch-application-packages/app_pkg_11.png "Update-Paket vorher in Azure-portal"
[12]: ./media/batch-application-packages/app_pkg_12.png "Löschen von Paket Bestätigungsdialogfeld Azure-Portal"
