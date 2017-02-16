<properties
   pageTitle="Erste Schritte mit Azure Stapel CLI | Microsoft Azure"
   description="Erhalten Sie eine kurze Einführung in die Befehle Stapel in Azure CLI für die Verwaltung von Stapel Azure Service-Ressourcen"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="09/30/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-cli"></a>Erste Schritte mit Azure Stapel CLI

Die Plattformen Azure Line-Benutzeroberfläche (Azure CLI) können Sie Ihre Konten Stapel und Ressourcen wie Pools, Projekten und Aufgaben in Windows, Mac und Linux Befehlsshells verwaltet. Mit der CLI Azure können Sie viele der Aufgaben, die Sie mit den Stapel APIs, Azure-Portal und Stapel PowerShell-Cmdlets ausführen, Skripts und ausführen.

In diesem Artikel basiert auf Azure CLI Version 0.10.5.

## <a name="prerequisites"></a>Erforderliche Komponenten

* [Installieren der Azure CLI](../xplat-cli-install.md)

* [Herstellen einer Verbindung Ihres Abonnements Azure mit Azure CLI](../xplat-cli-connect.md)

* Wechseln Sie zur **Ressourcenmanager Modus**:`azure config mode arm`

>[AZURE.TIP] Es empfiehlt sich, dass Sie Ihren Azure CLI häufig um Dienstupdates und Erweiterungen nutzen aktualisieren.

## <a name="command-help"></a>Hilfe zu dem Befehl

Sie können in der CLI Azure Hilfetext für jeden Befehl anzeigen, durch Anhängen `-h` als einzige Option nach dem Befehl. Beispiel:

* Hilfe zu den für die `azure` Befehl, geben Sie ein:`azure -h`
* Um eine Liste aller Stapel Befehle in der CLI erhalten möchten, verwenden Sie Folgendes:`azure batch -h`
* Um Hilfe zum Erstellen eines Kontos Stapel zu gelangen, geben Sie Folgendes ein:`azure batch account create -h`

Verwenden Sie im Zweifelsfall das `-h` Befehlszeilenoption zum Aufrufen von Hilfe zu den einzelnen Befehlen Azure CLI.

## <a name="create-a-batch-account"></a>Erstellen Sie ein Konto Stapel

Syntax:

    azure batch account create [options] <name>

Beispiel:

    azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

Erstellt ein neues Blatt Konto mit den angegebenen Parametern an. Sie müssen mindestens einen Speicherort, Ressourcengruppe und Kontonamen angeben. Wenn Sie bereits über eine Ressourcengruppe besitzen, erstellen Sie eine durch Ausführen `azure group create`, und geben Sie eine der Azure Regionen (beispielsweise "" Westen "USA") für die `--location` Option. Beispiel:

    azure group create --name "resgroup001" --location "West US"

> [AZURE.NOTE] Der Blattname Konto muss innerhalb der Azure Region eindeutig sein, die das Konto erstellt wurde. Es kann nur Kleinbuchstaben alphanumerische Zeichen enthalten, und 3-24 Zeichen lang sein. Sie können keine Sonderzeichen wie `-` oder `_` in Stapel Kontonamen.

### <a name="linked-storage-account-autostorage"></a>Verknüpfte Speicher-Konto (Autostorage)

Sie können bei der Erstellung (optional) eine **Allgemeine** Speicher-Konto bei Ihrem Konto Stapel verknüpfen. Das Feature [Anwendungspakete](batch-application-packages.md) des Blatts verwendet Blob-Speicher in einer verknüpften allgemeine Speicher-Konto, wie die [Stapel Datei Konventionen .NET](batch-task-output.md) Bibliothek. Diese optionale Features hilft Ihnen bei der Bereitstellung Programme, die durch die Ihre Aufgaben Stapel ausgeführt und Beibehalten von Daten, die sie verursachen.

Geben Sie zum Verknüpfen eines vorhandenen Azure-Speicher-Kontos in ein neues Blatt-Konto bei der Erstellung der `--autostorage-account-id` Option. Diese Option erfordert die vollständig qualifizierte Ressourcen-ID des Speicherkontos.

Zeigen Sie Ihres Speicherkontos Details an:

    azure storage account show --resource-group "resgroup001" "storageaccount001"

Verwenden Sie den **Url** -Wert für die `--autostorage-account-id` Option. Der Url-Wert beginnt mit "/ Abonnements /" und Ihre Abonnement-ID und Ressourcen Pfad mit dem Konto Speicher enthält:

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## <a name="delete-a-batch-account"></a>Löschen eines Kontos Stapel

Syntax:

    azure batch account delete [options] <name>

Beispiel:

    azure batch account delete --resource-group "resgroup001" "batchaccount001"

Löscht das angegebene Stapel-Konto an. Wenn Sie dazu aufgefordert werden, bestätigen Sie das Konto entfernen möchten (Entfernen des Kontos kann einige Zeit in Anspruch nehmen).

## <a name="manage-account-access-keys"></a>Verwalten von Konto-Tastenkombinationen

Sie benötigen eine Tastenkombination [Erstellen](#create-and-modify-batch-resources) und Ändern von Ressourcen in Ihr Konto Stapel aus.

### <a name="list-access-keys"></a>Liste von Tastenkombinationen

Syntax:

    azure batch account keys list [options] <name>

Beispiel:

    azure batch account keys list --resource-group "resgroup001" "batchaccount001"

Listet die Tasten Konto für die angegebenen Stapel-Konto.

### <a name="generate-a-new-access-key"></a>Generieren einer neuen Zugriffstaste

Syntax:

    azure batch account keys renew [options] --<primary|secondary> <name>

Beispiel:

    azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

Erneut generiert die angegebene Konto-Taste für das angegebene Stapel-Konto ein.

## <a name="create-and-modify-batch-resources"></a>Erstellen und Ändern von Stapel Ressourcen

Die CLI Azure können Sie erstellen, lesen, aktualisieren und löschen (CRUD) Stapel Ressourcen wie Pools, Knoten, Aufträgen und Aufgaben zu berechnen. Dieser Vorgänge erforderlich, Ihr Konto Blattname, Zugriffstaste und Endpunkt. Sie können angeben, diese mit der `-a`, `-k`, und `-u` Optionen oder festgelegten [Umgebungsvariablen-, die](#credential-environment-variables) die CLI verwendet werden, automatisch (wenn belegt).

### <a name="credential-environment-variables"></a>Anmeldeinformationen Umgebungsvariablen

Sie können festlegen, `AZURE_BATCH_ACCOUNT`, `AZURE_BATCH_ACCESS_KEY`, und `AZURE_BATCH_ENDPOINT` Umgebungsvariablen anstatt `-a`, `-k`, und `-u` Optionen in der Befehlszeile für jede Befehl ausführen. Diese Variablen verwendet, die Stapel CLI (wenn festlegen), damit Sie nicht angeben, können die `-a`, `-k`, und `-u` Optionen. In diesem Artikel wird davon ausgegangen dieser Variablen verwenden.

>[AZURE.TIP] Liste die Tasten mit `azure batch account keys list`, und zeigen Sie den Endpunkt des Kontos mit `azure batch account show`.

### <a name="json-files"></a>JSON-Dateien

Wenn Sie Stapel Ressourcen wie Pools und Aufträge erstellen, können Sie eine JSON-Datei, die mit der neuen Ressource Konfiguration statt zu übergeben Funktionsparameter als Befehlszeilenoptionen angeben. Beispiel:

`azure batch pool create my_batch_pool.json`

Während Sie viele Ressourcen Erstellung Vorgänge mithilfe von nur Befehlszeilenoptionen ausführen können, erfordern einige Features eine JSON-formatierte Datei, die die Ressourcendetails enthält. Beispielsweise müssen Sie eine JSON-Datei verwenden, wenn Sie Ressourcendateien für einen Vorgang Start angeben möchten.

Um den JSON erforderlich zum Erstellen einer Ressource finden Sie finden Sie in den [Stapel REST-API-Referenz] [ rest_api] Dokumentation auf MSDN. Jedes Thema "hinzufügen *Ressourcenart"*enthält JSON-Beispiel für das Erstellen der Ressource, die Sie für Ihre JSON-Dateien als Vorlagen verwenden können. Beispielsweise JSON für die Erstellung von Ressourcenpool finden Sie unter [Hinzufügen einer Ressourcenpool mit einer Firma][rest_add_pool].

>[AZURE.NOTE] Wenn Sie beim Erstellen einer Ressource eine JSON-Datei angeben, werden alle Parameter, die Sie in der Befehlszeile für diese Ressource angeben ignoriert.

## <a name="create-a-pool"></a>Erstellen einer Ressourcenpool

Syntax:

    azure batch pool create [options] [json-file]

Beispiel (Konfiguration des virtuellen Computers):

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

Beispiel (Cloud Services-Konfiguration):

    azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

Erstellt einen Pool von Computeknoten im Stapel-Dienst an.

Wie in der [Übersicht über die Features Stapel](batch-api-basics.md#pool)erwähnt, stehen zwei Optionen bei der Auswahl eines Betriebssystems für die Knoten in der Ressourcenpool: **Konfiguration des virtuellen Computers** und **Cloud Services-Konfiguration**. Verwenden der `--image-*` Optionen zum Erstellen von Pools Konfiguration des virtuellen Computers, und `--os-family` um Cloud Services-Konfiguration Pools zu erstellen. Sie können nicht gleichzeitig angegeben `--os-family` und `--image-*` Optionen.

Sie können Pool [Anwendungspakete](batch-application-packages.md) und die Befehlszeile für einen [Vorgang beginnen](batch-api-basics.md#start-task)angeben. Um Ressourcendateien für den Start-Vorgang angeben möchten, müssen Sie jedoch stattdessen eine [JSON-Datei](#json-files)verwenden.

Löschen Sie einen Pool mit:

    azure batch pool delete [pool-id]

>[AZURE.TIP] Überprüfen Sie die [Liste der virtuellen Computern Bilder](batch-linux-nodes.md#list-of-virtual-machine-images) aus, für die entsprechenden Werte für die `--image-*` Optionen.

## <a name="create-a-job"></a>Erstellen Sie ein Projekt

Syntax:

    azure batch job create [options] [json-file]

Beispiel:

    azure batch job create --id "job001" --pool-id "pool001"

Fügt einen Auftrag mit dem Konto Stapel hinzu, und gibt den Pool Grundlage ihrer Aufgaben ausführen.

Löschen Sie ein Projekt mit:

    azure batch job delete [job-id]

## <a name="list-pools-jobs-tasks-and-other-resources"></a>Liste Pools, Projekte, Aufgaben und weitere Ressourcen

Jeder Stapel Ressourcentyp unterstützt ein `list` Befehl, fragt Ihr Konto Stapel und Ressourcen dieses Typs sind. Beispielsweise können Sie die Pools in Ihr Konto und die Aufgaben eines Projekts Liste:

    azure batch pool list
    azure batch task list --job-id "job001"

### <a name="listing-resources-efficiently"></a>Effizientes Auflisten von Ressourcen

Für schnellere Abfragen, können Sie angeben, **auswählen**, **Filtern**und **Erweitern** -Klauseloptionen für `list` Vorgänge. Verwenden Sie diese Optionen, um die Menge der durch den Stapel Dienst zurückgegebenen Daten zu beschränken. Da alle Filter serverseitigen auftritt, schneidet nur die Daten, die Sie interessiert, das Kabel an. Verwenden Sie diese Klauseln Bandbreite speichern (und daher Anzeigedauer) Wenn Sie die Liste Vorgänge ausführen.

Beispielsweise wird nur Pools zurückgegeben, deren Ids mit "RenderTask" beginnen:

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

Die Stapel CLI unterstützt alle drei Klauseln vom Dienst Stapel unterstützt:

* `--select-clause [select-clause]`Eine Teilmenge der Eigenschaften für jede Entität zurückzugeben
* `--filter-clause [filter-clause]`Nur Personen, die den angegebenen OData-Ausdruck entsprechen zurück
* `--expand-clause [expand-clause]`Die Entitätsinformationen in einer einzelnen zugrunde liegenden REST Anruf zu erhalten. Die erweitern-Klausel unterstützt nur die `stats` Eigenschaft zu diesem Zeitpunkt.

Details auf die drei Klauseln und leistungsfähigeren Liste Abfragen mit ihnen finden Sie unter [den Stapel Azure-Dienst effizient Abfragen](batch-efficient-list-queries.md).

## <a name="application-package-management"></a>Verwalten von Paketen

Anwendungspakete bieten eine vereinfachte Möglichkeit zum Bereitstellen von Applications zu berechnen Knoten in der Pools. Mit der CLI Azure können Sie Anwendungspakete hochladen, Paketversionen verwalten und Pakete löschen.

So erstellen Sie eine neue Anwendung, und fügen eine Version des Pakets:

**Erstellen** einer Anwendung:

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

**Hinzufügen** eines Anwendungspakets:

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

**Aktivieren** des Pakets:

    azure batch application package activate "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" zip

Legen Sie die **Standardversion** der Anwendung:

    azure batch application set "resgroup001" "batchaccount001" "MyTaskApplication" --default-version "1.10-beta3"

### <a name="deploy-an-application-package"></a>Ein Anwendungspaket bereitgestellt

Sie können eine oder mehrere Anwendungspakete für die Bereitstellung beim Erstellen eines neuen Pools angeben. Wenn Sie ein Paket zum Zeitpunkt der Erstellung Pool angeben, wird als Knoten Verknüpfungen Pool einzelnen Knoten bereitgestellt. Pakete sind auch bereitgestellt werden, wenn ein Knoten neu gestartet oder einem neuen Image versehen ist.

Geben Sie die `--app-package-ref` option beim Erstellen eines Pools so, dass ein Anwendungspaket zu dem Pool des Knoten bereitstellen, wie er im Ressourcenpool teilnehmen an. Die `--app-package-ref` Option akzeptiert eine durch Semikolons getrennte Liste der Anwendung Ids für die Bereitstellung auf die Knoten berechnen.

    azure batch pool create --pool-id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

Bei der Erstellung einer Ressourcenpool mithilfe von Befehlszeilenoptionen können nicht Sie aktuell *die* Anwendung Paketversion bereitstellen zu den Knoten berechnen, beispielsweise "1.10-von Beta 3" angeben. Daher müssen Sie zuerst eine Standardversion für die Anwendung mit angeben `azure batch application set [options] --default-version <version-id>` vor dem Erstellen von des Pools (siehe vorherigen Abschnitt). Sie können, jedoch eine Version des Pakets für den Pool angeben, wenn Sie einer [JSON-Datei](#json-files) anstelle von Befehlszeilenoptionen verwenden, wenn Sie den Pool erstellen.

Weitere Informationen zum Anwendungspaketen finden Sie in der [Bereitstellung der Anwendung mit Azure Stapel Anwendungspakete](batch-application-packages.md).

>[AZURE.IMPORTANT] Sie müssen [Link ein Konto Azure-Speicher](#linked-storage-account-autostorage) bei Ihrem Konto Stapel Anwendungspakete verwenden.

### <a name="update-a-pools-application-packages"></a>Ein Ressourcenpool Anwendungspakete aktualisieren

Klicken Sie zum Aktualisieren der Anwendungen zu einem vorhandenen Pool zugewiesen Emission der `azure batch pool set` -Befehl mit den `--app-package-ref` Option:

    azure batch pool set --pool-id "pool001" --app-package-ref "MyTaskApplication2"

Zur Bereitstellung der neuen Anwendungspakets um Knoten bereits in einem vorhandenen Pool zu berechnen, müssen Sie neu starten oder ein neues Abbild diese Knoten:

    azure batch node reboot --pool-id "pool001" --node-id "tvm-3105992504_1-20160930t164509z"

>[AZURE.TIP] Sie erhalten eine Liste der Knoten in einem Ressourcenpool, sowie deren Ids Knoten mit `azure batch node list`.

Denken Sie daran, dass Sie bereits die Anwendung mit einer Standardversion vor der Bereitstellung konfiguriert haben, müssen (`azure batch application set [options] --default-version <version-id>`).

## <a name="troubleshooting-tips"></a>Tipps zur Problembehandlung

In diesem Abschnitt soll Sie Ressourcen, die bei der Behandlung von Problemen mit Azure CLI zur Verfügung zu stellen. Es wird nicht unbedingt alle Probleme lösen, aber kann Ihnen Grenzen Sie die Ursache, und zeigen Sie Ressourcen helfen.

* Verwenden Sie `-h` beim Abrufen von **Hilfe Text** für jeden CLI-Befehl

* Verwenden Sie `-v` und `-vv` **ausführliche** Ausgabe des Befehls; angezeigt werden. `-vv` ist "zusätzliche" ausführlich und zeigt die tatsächliche REST Besprechungsanfragen und Antworten. Diese Optionen sind nützlich für die Anzeige der vollständigen Fehler ausgegeben wurden.

* Sie können die **Ausgabe des Befehls als JSON** mit Anzeigen der `--json` Option. Beispielsweise `azure batch pool show "pool001" --json` zeigt den pool001 Eigenschaften im JSON-Format. Sie können dann kopieren und ändern Sie diese Ausgabe zur Verwendung in einem `--json-file` (siehe [JSON-Dateien](#json-files) in diesem Artikel).

* [Stapel-Forum im MSDN] [ batch_forum] ist eine große Hilferessource, und wird durch den Stapel Teammitglieder genau überwacht. Achten Sie darauf, dass Sie Ihre Fragen es Posten, wenn Sie Probleme ausführen oder Hilfe mit einem bestimmten Vorgang wünschen.

* Nicht jeder Stapel Ressource Vorgang wird von der CLI Azure derzeit unterstützt. Beispielsweise können nicht Sie aktuell eine Anwendung Paket *Version* für einen Pool, nur die Paket-ID angeben In diesem Fall müssen Sie möglicherweise angeben einer `--json-file` , damit der Befehl anstelle von Befehlszeilenoptionen. Achten Sie darauf, um auf dem neuesten Stand mit der neuesten Version von CLI zu zukünftigen Erweiterungen zu bleiben.

## <a name="next-steps"></a>Nächste Schritte

*  Finden Sie unter [Bereitstellung der Anwendung mit Azure Stapel Anwendungspaketen](batch-application-packages.md) zu erfahren, wie Sie mit diesem Feature können verwalten und Programme, die Sie, klicken Sie auf Blatt berechnen Knoten ausführen bereitstellen.

* Unter [den Stapel Dienst effizient Abfragen](batch-efficient-list-queries.md) für Weitere Informationen zur Verringern der Anzahl von Elementen und die Art der Informationen, die für Abfragen Stapel zurückgegeben wird.

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx