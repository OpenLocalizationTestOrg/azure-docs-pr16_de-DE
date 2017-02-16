<properties
    pageTitle="Grundlagen von Azure Stapel Dienst | Microsoft Azure"
    description="Weitere Informationen Sie zur Verwendung des Stapel Azure-Diensts für umfangreiche Parallel und HPC Auslastung"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/22/2016"
    ms.author="marsma"/>

# <a name="basics-of-azure-batch"></a>Grundlagen des Blatts Azure

Azure Stapel können Sie effizient Ausführen von Applications umfangreiche parallele und High Performance computing (HPC) in der Cloud. Es ist ein Plattformdienst, der in geplant rechenintensiver Arbeit für eine verwaltete Gruppe von virtuellen Computern ausgeführt werden, und kann automatisch skalieren berechnen von Aufträgen Anforderungen Ressourcen.

Mit dem Dienst Stapel definieren Sie Ressourcen Azure berechnen, um parallel und bei der Ihre Programme ausgeführt. Sie können bei Bedarf oder geplante ausführen Aufträge, und Sie müssen nicht manuell erstellen, konfigurieren und Verwalten eines HPC Cluster, einzelne virtuelle Maschinen, virtuelle Netzwerke oder eine komplexe Auftrag und Infrastruktur Planung des Vorgangs.

## <a name="use-cases-for-batch"></a>Verwenden von Fällen für Stapel

Stapel ist eine verwaltete Azure Service, der für *Stapelverarbeitung* oder *Stapel computing*– eine große Anzahl von einige gewünschte Ergebnis erzielen ähnliche Aufgaben ausführen, verwendet wird. Stapel computing wird von Organisationen, die regelmäßig zu verarbeiten, Transformieren und Analysieren große Datenmengen, am häufigsten verwendet.

Stapel funktioniert auch mit systembedingt parallele (auch bekannt als "sehr parallel") Anwendungen und Auslastung. Systembedingt parallele Auslastung werden mehrere Aufgaben einfach unterteilt, die Arbeit gleichzeitig auf mehreren Computern ausführen.

![Parallele Aufgaben][1]<br/>

Einige Beispiele für Auslastung, die häufig verarbeitet werden mit diesem Verfahren sind:

* Finanzielle Risiken Modellierung
* Klima und Hydrology Datenanalyse
* Abbildung des Renderns, Analyse und Verarbeitung
* Codieren von Medien und Umcodierung
* Genetischen Sequenzanalyse
* Technisch betonen Analyse
* Testen von Software

Stapel kann parallele Berechnungen durch einen Schritt verringern am Ende ausführen und komplexere HPC Auslastung z. B. Applikationen [Nachricht übergeben Interface (MPI)](batch-mpi.md) ausführen.

Einen Vergleich zwischen Stapel und anderen Lösungsmöglichkeiten HPC in Azure finden Sie unter [Stapel und HPC Lösungen](batch-hpc-solutions.md).

## <a name="developing-with-batch"></a>Entwickeln mit Stapel

Parallele Auslastung mit Blattnamen Verarbeitung programmgesteuert erfolgt normalerweise mithilfe der [Stapel APIs](#batch-development-apis). Mit den Stapel-APIs, Sie erstellen, verwalten Speicherpools Datenverarbeitungsknoten (virtuelle Maschinen) und Planen von Aufträgen und Aufgaben auf diesen Knoten ausführen. Eine Clientanwendung oder einen bestimmten Dienst, die Sie schreiben verwendet die Stapel-APIs zur Kommunikation mit dem Dienst Stapel.

Effizientes können Sie umfangreiche Auslastung für Ihre Organisation zu verarbeiten, oder ein Dienst front-End an Ihre Kunden bieten, sodass sie Projekte und Aufgaben – bei Bedarf oder nach einem Zeitplan – auf einem, Hunderte oder sogar Tausende von Knoten ausgeführt werden können. Sie können auch als Teil eines größeren Workflows, die mithilfe von Tools wie [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md)verwaltete Stapel verwenden.

> [AZURE.TIP] Wenn Sie bereit sind, an die Stapel API für ausführlichere Informationen zu den Features zu erhalten, die ihn enthält, schauen Sie sich den [Stapel Übersicht über die Features für Entwickler](batch-api-basics.md).

### <a name="azure-accounts-youll-need"></a>Benötigen Sie Azure-Konten

Bei der Entwicklung von Lösungen Stapel, verwenden Sie die folgenden Konten in Microsoft Azure.

- **Azure-Konto und Ihr Abonnement** – Wenn Sie bereits über ein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnent profitieren]aktivieren[msdn_benefits], oder melden Sie sich für ein [kostenloses Azure-Konto][free_account]. Wenn Sie ein Konto erstellen, wird ein Standardabonnement für Sie erstellt.

- **Stapel Konto** – Wenn Interaktion Ihrer Anwendung mit dem Stapel-Dienst, den Namen des Kontos, die URL der das Konto sowie eine Zugriffstaste dienen als Anmeldeinformationen. Alle Stapel Ressourcen wie Pools, Knoten, Projekte zu berechnen und Vorgänge mit einem Stapel-Konto verknüpft sind. Sie können im Portal Azure [Stapel Konto erstellen](batch-account-create-portal.md) .

- **Speicher-Konto** - Stapel verfügt über integrierte Unterstützung für das Arbeiten mit Dateien in [Azure-Speicher][azure_storage]. Nahezu jeder Stapel Szenario verwendet Azure-Speicher für das staging von den Programmen, die Ihre Aufgaben ausgeführt werden und die Daten, die sie verarbeiten und für die Speicherung Ausgabedaten, die sie generieren. Zum Erstellen eines Speicher-Kontos finden Sie unter [Informationen zum Azure-Speicherkonten](./../storage/storage-create-storage-account.md).

### <a name="batch-development-apis"></a>Stapel Entwicklung APIs

Von Applications und Dienstleistungen können direkte REST-API Aufrufe Emission, verwenden Sie eine oder mehrere der folgenden Clientbibliotheken oder eine Kombination beider zum Verwalten von Ressourcen und Ausführen parallele Auslastung bei mithilfe des Diensts für Stapel zu berechnen.

| API    | -API-Referenz | Herunterladen | Codebeispielen |
| ----------------- | ------------- | -------- | ------------ |
| **Stapel REST** | [MSDN][batch_rest] | N/V | [MSDN][batch_rest] |
| **Stapel .NET**    | [MSDN][api_net] | [NuGet][api_net_nuget] | [GitHub][api_sample_net] |
| **Stapel Python**  | [readthedocs.IO][api_python] | [PyPI][api_python_pypi] |[GitHub][api_sample_python] |
| **Stapel Node.js** | [github.IO][api_nodejs] | [npm][api_nodejs_npm] | - |
| **Stapel Java** (Preview) | [github.IO][api_java] | [Maven][api_java_jar] | [GitHub][api_sample_java] |

### <a name="batch-resource-management"></a>Ressourcenverwaltung Stapel

Zusätzlich zu den Client-APIs können Sie auch die folgenden zum Verwalten von Ressourcen in Ihr Konto Stapel verwenden.

- [PowerShell-Cmdlets Stapel][batch_ps]: der Azure Stapel Cmdlets im [Azure PowerShell](../powershell-install-configure.md) -Modul aktivieren Sie Stapel Ressourcen mit PowerShell verwalten.

- [Azure CLI](../xplat-cli-install.md): der Azure Line Interface (CLI Azure) ist ein Kreuz-Plattformtoolset, die Shell-Befehle für die Interaktion mit vielen Azure Dienste einschließlich Stapel bereitstellt.

- [Stapel Management .NET](batch-management-dotnet.md) Client-Bibliothek: auch verfügbar über [NuGet][api_net_mgmt_nuget], können die Stapel Management .NET Client-Bibliothek Stapel Konten, Kontingente und Anwendungspakete programmgesteuert verwalten. Bezug für die Verwaltung Bibliothek befindet sich auf der [MSDN-][api_net_mgmt].

### <a name="batch-tools"></a>Stapel-tools

Beim Erstellen von Lösungen mit Stapel nicht erforderlich, sind hier einige nützliche Tools, die beim Erstellen und Debuggen von Applications Stapel und Dienstleistungen verwenden.

 - [Azure-Portal][portal]: können Sie erstellen, überwachen und Löschen von Stapel Pools, Aufträge und Aufgaben in der Azure-Portal Stapel Blades. Sie können die Statusinformationen für diese anzeigen und weitere Ressourcen, während Sie Ihre Aufträge ausführen, und sogar Dateien herunterladen, aus den Datenverarbeitungsknoten in Ihrem Pools (Herunterladen einer fehlgeschlagenen Aufgabe `stderr.txt` bei der Problembehandlung, beispielsweise). Sie können auch Remote Desktop (RDP) Dateien herunterladen, mit denen Sie sich anmelden, um Knoten zu berechnen.

 - [Azure Stapel Explorer][batch_explorer]: Stapel Explorer bietet ähnliche Stapel Ressource Managementfunktionen wie Azure-Portal, jedoch in einem eigenständigen Clientanwendung Windows Presentation Foundation (WPF). Eine Stichprobe Stapel .NET Anwendung auf [GitHub]verfügbar[github_samples], Sie können mit Visual Studio 2015 oder höher zu erstellen und diese zu durchsuchen und Verwalten von Ressourcen in Ihr Konto Stapel, während Sie entwickeln und Debuggen von Ihrem Stapel Lösungen verwenden. Ansicht Auftrag, Pool und Aufgabendetails, Herunterladen von Dateien aus Datenverarbeitungsknoten und Herstellen einer Verbindung mit Knoten Remote mit Stapel Explorer mithilfe von RDP (Remote Desktop)-Dateien, die Sie herunterladen können.

 - [Microsoft Azure-Speicher-Explorer][storage_explorer]: während nicht unbedingt Stapel Azure-Tool, ist im Speicher-Explorer ein weiteres wertvolles Tool, während Sie entwickeln und Debuggen Ihrer Stapel Lösungen.

## <a name="scenario-scale-out-a-parallel-workload"></a>Szenario: Skalierung eine parallele Arbeitsbelastung

Eine allgemeine Lösung, die die APIs Stapel verwendet für die Interaktion mit dem Dienst Stapel umfasst systembedingt parallele Arbeit – wie etwa die Darstellung von Bildern für 3D Szenen – auf einem Ressourcenpool von Computeknoten Skalierung. Dieser Pool von Computeknoten Ihrer Farm"Rendern" werden kann, Dutzende, Hunderte oder sogar Tausende von Kernen an der Position des Renderns beispielsweise bereitstellt.

Das folgende Diagramm veranschaulicht einen allgemeinen Stapel Workflow mit einem Clientanwendung oder gehosteten Dienst Stapel zum Ausführen einer parallele Arbeitsbelastung verwenden.

![Stapel Lösung workflow][2]

In diesem Szenario allgemeine verarbeitet Ihrer Anwendung oder einen bestimmten Dienst eine Berechnung Arbeitsbelastung in Azure Stapel durch Ausführen der folgenden Schritte:

1. Hochladen Sie **von Dateien** und die **Anwendung** , die diese Dateien bei Ihrem Konto Azure-Speicher verarbeitet werden. Die Eingabewerte Dateien können keine Daten, die Ihrer Anwendung verarbeitet, werden beispielsweise financial Modelldaten oder Videodateien transcodiert werden. Die Anwendungsdateien können eine beliebige Anwendung handeln, die für die Verarbeitung von Daten, wie 3D Rendern Anwendung oder Medien Kodierungsprogramm verwendet wird.

2. Erstellen Sie einen Stapel **Ressourcenpool** von Computeknoten in Ihr Konto Stapel – diese Knoten entsprechen den virtuellen Computern, die Ihre Vorgänge ausgeführt wird. Geben Sie Eigenschaften wie die [Knotengröße](./../cloud-services/cloud-services-sizes-specs.md), deren Betriebssystem und den Speicherort, in Azure-Speicher der Anwendung zu installieren, wenn die Knoten teilnehmen an der Ressourcenpool (die Anwendung, die Sie in Schritt 1 # hochgeladen). Sie können auch den Pool [automatisch skalieren](batch-automatic-scaling.md)– dynamisch konfigurieren passen Sie die Anzahl von Computeknoten im Pool – als Antwort auf die Arbeitsbelastung, die Ihre Aufgaben generieren.

3. Erstellen einer mit dem aus **Auftrag** , um die Arbeitsbelastung auf dem Pool von Computeknoten ausgeführt werden. Wenn Sie ein Projekt erstellen, ordnen Sie es mit einem Stapel Ressourcenpool an.

4. Hinzufügen von **Vorgängen** im Projekt. Wenn Sie ein Projekt Vorgänge hinzufügen, plant der Stapel-Dienst automatisch die Aufgaben für die Ausführung auf den Knoten berechnen im Pool aus. Die Anwendung, die Sie zum Verarbeiten der Eingabewerte Dateien hochgeladen jeden Vorgang verwendet wird.

    - 4a. Bevor eine Aufgabe ausgeführt wird, können sie die Daten (die Eingabewerte Dateien) herunterladen, die sie den Prozess zum Berechnungsknoten ist, die sie zugewiesen wurde. Wenn die Anwendung nicht bereits auf dem Knoten installiert wurde (siehe Schritt #2), sie hier herunterladen kann stattdessen. Wenn die Downloads abgeschlossen haben, führen Sie die Aufgaben auf ihre zugeordneten Knoten.

5. Wie die Aufgaben ausführen möchten, können Sie den Stapel, um den Status des Auftrags und die zugehörigen Aufgaben überwachen Abfragen. Der Clientanwendung oder einen bestimmten Dienst kommuniziert mit dem Dienst Stapel über HTTPS und, da Sie Tausende von Aufgaben ausführen auf Tausende von Computeknoten Überwachung möglicherweise, müssen Sie [den Stapel Dienst effizient Abfragen](batch-efficient-list-queries.md).

6. Wie die Vorgänge abgeschlossen ist können sie ihre Ergebnisdaten in Azure-Speicher hochladen. Sie können Dateien auch direkt aus Datenverarbeitungsknoten abrufen.

7. Wenn Ihre Überwachung erkennt, dass die Vorgänge in Ihrem Projekt abgeschlossen haben, kann Ihre Clientanwendung oder einen bestimmten Dienst die Ausgabedaten für die weitere Verarbeitung oder Auswertung herunterladen.

Beibehalten berücksichtigen Dies ist nur eine Möglichkeit zum Blattnamen verwenden und diesem Szenario wird nur ein Paar der zugehörigen verfügbaren Features beschrieben. Beispielsweise können Sie [mehrere Aufgaben parallel](batch-parallel-node-tasks.md) auf den einzelnen Knoten berechnen ausführen, und Sie können [Projektaufgaben Vorbereitung und den Abschluss](batch-job-prep-release.md) verwenden, um die Knoten für Ihre Aufträge vorbereiten und dann später aufräumen.

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie einen detaillierten Überblick über den Stapel Dienst haben, ist es Zeit, um zu erfahren, wie Sie Ihre rechenintensiven parallele Auslastung Verarbeitungszeit verwenden tiefer gehende aus.

- Lesen Sie den [Stapel Übersicht über die Features für Entwickler](batch-api-basics.md), wichtige Informationen für jeden Vorbereiten der Blattnamen verwenden. Der Artikel enthält ausführliche Informationen zu den Stapel Dienstressourcen wie Pools, Knoten, Projekte und Vorgänge und die viele API-Funktionen, die Sie beim Erstellen Ihrer Anwendungs Stapel verwenden können.

- Informationen zum Verwenden von c# und der Bibliothek Stapel .NET zum Ausführen einer einfachen Arbeitsbelastung mit einem gemeinsamen Stapel Workflow [Erste Schritte mit der Bibliothek Azure Stapel für .NET](batch-dotnet-get-started.md) . In diesem Artikel sollten eine Ihrer ersten Stopps, während Sie lernen, wie Sie den Stapel-Dienst verwenden. Es gibt auch eine [Python Version](batch-python-tutorial.md) des Lernprogramms.

- Herunterladen der [Codebeispielen auf GitHub] [ github_samples] zu sehen, wie c# und Python Stapel zu planen und Prozess Stichprobe Auslastung Videokonfigurationen können.

- Schauen Sie sich den [Stapel Learning Path] [ learning_path] , um Sie eine Vorstellung davon den verfügbaren Ressourcen zu gelangen, wie Sie Informationen für die Arbeit mit Stapel.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png
