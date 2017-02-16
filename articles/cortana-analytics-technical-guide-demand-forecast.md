<properties
    pageTitle="Fordern Sie SCHÄTZER Aufwand technischen Leitfaden | Microsoft Azure"
    description="Technischen Leitfaden zur Lösung Vorlage mit Microsoft Cortana Intelligence für SCHÄTZER in Energy Demand."
    services="cortana-analytics"
    documentationCenter=""
    authors="yijichen"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="inqiu;yijichen;ilanr9"/>

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-demand-forecast-in-energy"></a>Technischen Leitfaden für die Cortana Intelligence Lösungsvorlage für bei Bedarf in Energy SCHÄTZER

## <a name="overview"></a>**(Übersicht)**

Lösungsvorlagen sind für die Erstellung einer E2E Demo auf Cortana Intelligence Suite Beschleunigung. Bereitgestellte Vorlage Ihres Abonnements notwendigen Cortana Intelligence Komponente bereitstellt und die Beziehungen zwischen erstellen. Außerdem wird der Verkaufspipeline Daten basiert, mit Beispieldaten erste aus eine Anwendung zur Simulation generiert. Herunterladen Sie den Simulator Daten über den bereitgestellten Link und installieren Sie es auf Ihrem lokalen Computer, finden Sie in der Infodatei laut: Verwenden des Simulators. Daten aus der Simulator generiert werden unterbrechen, die Daten Verkaufspipeline und Computer Learning Vorhersage der klicken Sie dann auf dem Power BI-Dashboard visualisiert werden soll, kann zu erzeugen.

Die Lösungsvorlage finden Sie [hier](https://gallery.cortanaintelligence.com/SolutionTemplate/Demand-Forecasting-for-Energy-1) 

Verfahren führt Sie durch die verschiedenen Schritte, um Ihre Anmeldeinformationen Lösung einzurichten. Stellen Sie sicher, dass Sie diese Anmeldeinformationen wie Lösungsname, Benutzername und Kennwort ein, das Sie während der Bereitstellung bieten aufzeichnen.

Das Ziel dieses Dokuments wird die Verweis-Architektur und unterschiedlichen Komponenten nach der Bereitstellung in Ihrem Abonnement als Teil dieser Vorlage Lösung erläutert. Das Dokument spricht auch über so ersetzen Sie die Beispieldaten mit realen Daten eigene Einsichten/Vorhersagen von Ihnen gewonnenen Daten sehen können. Darüber hinaus spricht Dokument über die Teile der Vorlage Lösung, die muss geändert werden, wenn Sie die Lösung für Ihre eigenen Daten anpassen möchten. Anweisungen zum Erstellen des Power BI-Dashboards für diese Vorlage Lösung werden am Ende bereitgestellt.

## <a name="big-picture"></a>**Großes Bild**

![](media\cortana-analytics-technical-guide-demand-forecast\ca-topologies-energy-forecasting.png)

### <a name="architecture-explained"></a>Erläuterung der Architektur
Verschiedene Azure Dienste Cortana Analytics-Suite sind aktiviert, wenn die Lösung bereitgestellt wird, (*d. h.* Ereignis Hub *Stream Analytics, HDInsight, Daten Factory, Computer-Schulung usw.*). Das Architekturdiagramm oben angezeigt wird, auf hoher Ebene, wie die Anforderung Planung für Energy Lösungsvorlage von End-to-End erstellt wird. Sie werden können diese Dienste ermitteln, indem Sie auf die Lösung Vorlage Diagramm mit für die Bereitstellung der Lösung erstellt wurden. Den folgenden Abschnitten werden die jeweiligen Textabschnitts.

## <a name="data-source-and-ingestion"></a>**Datenquelle und Gliederung**

### <a name="synthetic-data-source"></a>Synthetische Datenquelle

Für diese Vorlage wird die Datenquelle verwendet aus einer desktop-Anwendung generiert, die Sie herunterladen und nach der erfolgreichen Bereitstellung lokal ausgeführt werden. Finden Sie die Anweisungen zum Herunterladen und installieren diese Anwendung in der Eigenschaften-Leiste, wenn Sie auf dem ersten Knoten Aufwand prognostizieren Daten Simulator im Diagramm der Lösung Vorlage auswählen. Diese Anwendung feeds [Azure Ereignis Hub](#azure-event-hub) Dienst mit Datenpunkten oder Ereignissen, die in den Rest der Lösung illustrieren verwendet werden soll.

Die Ereignis Generation Anwendung wird das Ereignis Azure-Hub füllen Sie nur während der auf dem Computer ausgeführt wird.

### <a name="azure-event-hub"></a>Azure Ereignis Hub

Der Dienst [Azure Ereignis Hub](https://azure.microsoft.com/services/event-hubs/) ist der Empfänger der Eingabe von oben beschriebenen synthetische Datenquelle bereitgestellt.

## <a name="data-preparation-and-analysis"></a>**Vorbereiten der Daten und Analyse**


### <a name="azure-stream-analytics"></a>Azure Stream Analytics

Der [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -Dienst wird verwendet, bieten eine nahezu in Echtzeit Analytics im Eingabewerte Stream vom Dienst [Azure Ereignis Hub](#azure-event-hub) und die Ergebnisse auf einer [Power BI](https://powerbi.microsoft.com) -Dashboard sowie alle unformatierten eingehende Ereignisse in der [Azure](https://azure.microsoft.com/services/storage/) -Speicherdienst für eine spätere Verarbeitung vom Dienst [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) Archivierung veröffentlichen.

### <a name="hd-insights-custom-aggregation"></a>HD Einsichten benutzerdefinierte Aggregation

Der Dienst Azure HD Einblicke dient zum [Struktur](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) Skripts (von Azure Daten Factory koordiniert) ausführen, um Aggregationen auf die unformatierten Ereignisse bereitzustellen, die mithilfe des Diensts für Azure Stream Analytics archiviert wurden.

### <a name="azure-machine-learning"></a>Learning Azure-Computern

Der [Azure maschinellen Learning](https://azure.microsoft.com/services/machine-learning/) -Dienst wird verwendet (von Azure Daten Factory koordiniert) zukünftigen Stromverbrauch eines bestimmten Bereichs angegebenen Eingaben empfangen Prognose vornehmen.

## <a name="data-publishing"></a>**Daten für die Veröffentlichung**


### <a name="azure-sql-database-service"></a>SQL Azure-Datenbank-Dienst

Der Dienst [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) wird verwendet, um speichern (verwaltet von Azure Daten Factory) die Vorhersagen vom Dienst Azure maschinellen Schulung, die in der [Power BI](https://powerbi.microsoft.com) -Dashboard verwendet werden.

## <a name="data-consumption"></a>**Daten Verbrauch**

### <a name="power-bi"></a>Power BI

Der [Power BI](https://powerbi.microsoft.com) -Dienst wird verwendet, um ein Dashboard anzuzeigen, die Aggregationen enthält, sofern durch die [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) Dienst ebenso wie bei Bedarf Ergebnisse in [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) gespeichert SCHÄTZER, die mit den [Azure maschinellen Learning](https://azure.microsoft.com/services/machine-learning/) -Dienst erstellt wurden. Anweisungen zum Erstellen des Power BI-Dashboards für diese Vorlage Lösung finden Sie weiter unten im Abschnitt.

## <a name="how-to-bring-in-your-own-data"></a>**So zeigen Sie Ihre eigenen Daten**

In diesem Abschnitt beschrieben, wie Sie Ihre eigenen Daten an Azure bringen, und welche Bereiche müssten Änderungen für die Daten, die Sie in diese Architektur bringen.

Es ist wahrscheinlich nicht, dass alle Dataset, Sie zeigen, das Dataset für diese Lösungsvorlage verwendet übereinstimmen würden. Grundlegendes zu Ihren Daten und die Anforderungen ist werden entscheidend in, wie Sie diese Vorlage für die Arbeit mit Ihren eigenen Daten ändern. Ist dies der ersten anzeigen zum Dienst Azure maschinellen Schulung, können Sie eine Einführung zu erhalten, anhand des Beispiels in [zum Erstellen Ihrer ersten experimentieren](machine-learning\machine-learning-create-experiment.md).

In den folgenden Abschnitten werden in den Abschnitten der Vorlage an, die Änderungen erfordern wird, wenn ein neues Dataset eingeführt wird.

### <a name="azure-event-hub"></a>Azure Ereignis Hub

Der Dienst [Azure Ereignis Hub](https://azure.microsoft.com/services/event-hubs/) ist sehr generische, sodass Daten an den Hub im JSON oder CSV-Format veröffentlicht werden können. Keine besondere Verarbeitung im Hub Azure Ereignis eintritt, aber es ist wichtig, dass Sie die Daten verstehen, die darin eingezogen wird.

Dieses Dokument beschreibt nicht, wie Sie Ihre Daten Aufnahme, aber eine kann einfach senden Ereignisse oder Daten an einen Hub an Azure Ereignis, verwenden das [Ereignis Hub-API](event-hubs\event-hubs-programming-guide.md).

### <a name="azure-stream-analytics"></a>Azure Stream Analytics

Der [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -Dienst wird verwendet, um durch die Datenstreams lesen und Ausgeben von Daten in eine beliebige Anzahl von Datenquellen in der Nähe in Echtzeit Analytics zu ermöglichen.

Für die Anforderung Planung für Energy Lösungsvorlage besteht aus zwei Unterabfragen, jede Verarbeitung Ereignisse vom Dienst Azure Ereignis Hub als Eingaben und Ausgaben zwei unterschiedlichen Speicherorten müssen die Azure Stream Analytics-Abfrage. Diese Ausgaben bestehen aus einer Power BI-Dataset und einen Speicherort für die Azure.

Die Abfrage [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) kann vom gefunden werden:

-   Anmeldung beim [Azure-Verwaltungsportal](https://manage.windowsazure.com/)

-   Speicherorte der Stream Analytics Aufträge ![](media\cortana-analytics-technical-guide-demand-forecast\icon-stream-analytics.png) , generiert wurden, wenn die Lösung bereitgestellt wurde. Eine ist für das Senden der Daten zu BLOB-Speicher (z. B. mytest1streaming432822asablob), und das andere ist für das Senden der Daten an der Power BI (z. B. mytest1streaming432822asapbi).


-   Auswählen

    -   ***EINGABEN*** für die Eingabe der Abfrage anzeigen

    -   ***Abfrage*** die Abfrage selbst anzeigen

    -   ***Gibt*** die verschiedenen Ausgaben anzeigen

Informationen zum Erstellen der Abfrage Azure Stream Analytics finden Sie im [Stream Analytics Abfrage Bezug](https://msdn.microsoft.com/library/azure/dn834998.aspx) auf MSDN.

Azure Stream Analytics Auftrags der Dataset mit in der Nähe in Echtzeit Analytics Informationen zu den eingehenden Datenstream zu einem Power BI-Dashboard ausgegeben wird in dieser Lösung als Teil dieser Lösungsvorlage bereitgestellt. Da es implizit Kenntnisse über das eingehende Datenformat ist, müssten diese Abfragen geändert werden, basierend auf Ihrer Datenformat.

Die anderen Azure Stream Analytics Position Gibt alle [Ereignis-Hub](https://azure.microsoft.com/services/event-hubs/) Ereignisse zu [Azure-Speicher](https://azure.microsoft.com/services/storage/) aus und erfordert daher keine Änderung, unabhängig von der Datenformat aus, wie die vollständige Ereignisinformationen zu Speicher gestreamt werden.

### <a name="azure-data-factory"></a>Factory Azure-Daten

Der Dienst [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) koordiniert Bewegung und Verarbeitung von Daten. In der Demand Planung für Energy Lösungsvorlage die Daten Factory besteht von zwölf [Pipelines](data-factory\data-factory-create-pipelines.md) , das Verschieben und die Daten mit verschiedenen Technologien verarbeiten.

  Sie können Ihre Daten Factory durch Öffnen des Daten Factory-Knotens am unteren Rand der Lösung Vorlage Diagramm erstellt haben, klicken Sie mit der Bereitstellung der Lösung zugreifen. Dadurch gelangen Sie auf Ihre Azure-Verwaltungsportal Fabrik Daten. Wenn unter Ihrem Datasets Fehler angezeigt wird, können Sie die ignorieren, wie sie sind aufgrund von Daten Factory bereitgestellt wird, bevor der Daten-Generator eingeleitet wurde. Dieser Fehler verhindern Ihre Daten Factory nicht funktioniert nicht.

In diesem Abschnitt wird erläutert, die erforderlichen [Rohrleitungen](data-factory\data-factory-create-pipelines.md) und [Aktivitäten](data-factory\data-factory-create-pipelines.md) , die in der [Factory der Azure-Daten](https://azure.microsoft.com/documentation/services/data-factory/)enthalten sind. Im folgenden finden Sie die Diagrammansicht der Lösung.

![](media\cortana-analytics-technical-guide-demand-forecast\ADF2.png)

Fünf der von dieser Factory Pipelines enthalten [Struktur](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) Skripts, mit denen unterteilen und Aggregieren von Daten aus. Wenn notiert haben, werden die Skripts in das während der Installation erstellte [Azure-Speicher](https://azure.microsoft.com/services/storage/) Konto befinden. Kann ich die Standortinformationen: Demandforecasting\\\\Skript\\\\Struktur\\ \\ (oder https://[Your Lösung name].blob.core.windows.net/demandforecasting).

Ähnlich wie der [Azure Stream Analytics](#azure-stream-analytics-1) Abfragen, die [Struktur](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) Skripts implizit Kenntnisse über das eingehende Datenformat haben, diese Abfragen möchten müssen geändert werden basierend auf Ihren Anforderungen Format und [Features technisch](machine-learning\machine-learning-feature-selection-and-engineering.md) .

#### <a name="aggregatedemanddatato1hrpipeline"></a>*AggregateDemandDataTo1HrPipeline*

Diese [Verkaufspipeline](data-factory\data-factory-create-pipelines.md) Verkaufspipeline enthält eine einzelne Aktivität: eine [HDInsightHive](data-factory\data-factory-hive-activity.md) Aktivität mit einer [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , die ein Skript [Struktur](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) , die alle 10 Sekunden aggregieren ausgeführt wird bei Bedarf Daten in Unterwerk Ebene stündlich Region Ebene gestreamt und in [Azure-Speicher](https://azure.microsoft.com/services/storage/) durch den Auftrag Azure Stream Analytics setzen.

Das Skript [Struktur](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) für diese vorherigen Vorgang ist ***AggregateDemandRegion1Hr.hql***


#### <a name="loadhistorydemanddatapipeline"></a>*LoadHistoryDemandDataPipeline*

Diese [Verkaufspipeline](data-factory\data-factory-create-pipelines.md) enthält zwei Aktivitäten:
- [HDInsightHive](data-factory\data-factory-hive-activity.md) Aktivitäten mit einem [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , das ausgeführt wird eine Struktur Skript zum Aggregieren stündlich Verlauf Demand Daten in Unterwerk Ebene stündlich Region Ebene und setzen Sie während des Azure Stream Analytics Auftrags in Azure-Speicher

- [Kopieren](https://msdn.microsoft.com/library/azure/dn835035.aspx) von Aktivitäten, die aggregierten Daten aus Azure-Speicher Blob zur Azure SQL-Datenbank verschoben wird, das als Teil der Installation der Lösung Vorlage bereitgestellt wurde.

Das Skript [Struktur](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) für diese Aufgabe ist ***AggregateDemandHistoryRegion.hql***.


#### <a name="mlscoringregionxpipeline"></a>*MLScoringRegionXPipeline*

Diese [Pipelines](data-factory\data-factory-create-pipelines.md) enthalten mehrere Aktivitäten und, deren Ergebnis ist die bewertete Vorhersagen aus Azure maschinellen Learning-Versuch, die mit dieser Lösungsvorlage verknüpft ist. Sind fast identisch mit Ausnahme von jeder von ihnen nur das andere Region verarbeitet die verschiedenen RegionID in der Verkaufspipeline ADF und das Skript Struktur für die einzelnen Regionen übergeben durchgeführt wird.  
Die Aktivitäten, die diese enthalten sind:
-   [HDInsightHive](data-factory\data-factory-hive-activity.md) Aktivität mit einer [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , die eine Struktur-Skripts zum Ausführen von Aggregationen und Features technisch für den Versuch Azure maschinellen Learning erforderlich ausgeführt wird. Die Struktur Skripts für diese Aufgabe sind die entsprechenden ***PrepareMLInputRegionX.hql***.

-   [Kopieren](https://msdn.microsoft.com/library/azure/dn835035.aspx) von Aktivitäten, die die Ergebnisse aus der Aktivität [HDInsightHive](data-factory\data-factory-hive-activity.md) in ein einzelnes Speicher Azure Blob verschiebt, die Zugriff durch die Aktivität [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) werden können.

-   [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) Aktivitäten, die die Azure maschinellen Learning ruft experimentieren, welche Ergebnisse in die Ergebnisse in einer einzelnen Azure-Speicher abgelegt BLOB.

#### <a name="copyscoredresultregionxpipeline"></a>*CopyScoredResultRegionXPipeline*
Diese [Pipelines](data-factory\data-factory-create-pipelines.md) enthalten eine einzelne Aktivität - eine Aktivität [Kopieren](https://msdn.microsoft.com/library/azure/dn835035.aspx) , die die Ergebnisse der Azure maschinellen Learning experimentieren aus der jeweiligen ***MLScoringRegionXPipeline*** zur Azure SQL-Datenbank verschoben wird, das als Teil der Installation der Lösung Vorlage bereitgestellt wurde.

#### <a name="copyaggdemandpipeline"></a>*CopyAggDemandPipeline*
Diese [Pipelines](data-factory\data-factory-create-pipelines.md) enthalten eine einzelne Aktivität - eine Aktivität [Kopieren](https://msdn.microsoft.com/library/azure/dn835035.aspx) , die die Daten zusammengefasster laufenden Demand aus ***LoadHistoryDemandDataPipeline*** zur Azure SQL-Datenbank verschoben wird, das als Teil der Installation der Lösung Vorlage bereitgestellt wurde.

#### <a name="copyregiondatapipeline-copysubstationdatapipeline-copytopologydatapipeline"></a>*CopyRegionDataPipeline, CopySubstationDataPipeline, CopyTopologyDataPipeline*
Diese [Pipelines](data-factory\data-factory-create-pipelines.md) enthalten eine einzelne Aktivität - eine Aktivität [Kopieren](https://msdn.microsoft.com/library/azure/dn835035.aspx) , die die Daten für Bezug der Region/Unterwerk/Topologygeo, die auf Speicher Azure Blob als Teil der Installation der Lösung Vorlage zur Azure SQL-Datenbank hochgeladen werden, die als Teil der Installation der Lösung Vorlage bereitgestellt wurde verschoben.

### <a name="azure-machine-learning"></a>Learning Azure-Computern
Die für diese Lösungsvorlage verwendet [Azure maschinellen Learning](https://azure.microsoft.com/services/machine-learning/) experimentieren bietet Vorhersagen zu Demand des Bereichs. Der Versuch gilt nur für den Datensatz ein verbraucht und daher erfordern Änderung oder Ersatz speziell für die Daten, die in eingebracht wurde.

## <a name="monitor-progress"></a>**Überwachen des Fortschritts**
Nach dem Start der Daten-Generator der Verkaufspipeline beginnt hydrated erhalten, und die verschiedenen Komponenten des Ihre Lösung starten eigene in Aktion folgenden die Befehle von der Factory Daten ausgestellt. Es gibt zwei Möglichkeiten, die Sie der Verkaufspipeline überwachen können.

1. Überprüfen Sie die Daten aus Azure BLOB-Speicher.

    Eine des Streams Analytics Auftrags schreibt die unformatierten eingehenden Daten zu Blob-Speicher. Wenn Sie auf **Azure BLOB-Speicher** Komponente Ihrer Lösung auf dem Bildschirm Sie erfolgreich auf die Lösung bereitgestellt, und klicken Sie dann im rechten Bereich auf **Öffnen** , gelangen Sie zum [Azure-Verwaltungsportal](https://portal.azure.com). Einmal vorhanden ist, klicken Sie auf **Blobs**. Im nächsten Bereich wird eine Liste der Container angezeigt. Klicken Sie auf **"Energysadata"**. Klicken Sie in der nächsten Systemsteuerung sehen Sie den Ordner **"Demandongoing"** . In den Ordner Rawdata sehen Sie Ordner mit dem Namen Datum = 2016-01-28 usw.. Wenn Sie diese Ordner angezeigt wird, gibt an, dass die unformatierten Daten erfolgreich ist auf Ihrem Computer generiert und im Blob-Speicher gespeichert wird. Sie sollten Dateien angezeigt werden, die Größen begrenzte in MB in diesen Ordnern sein soll.

2. Überprüfen Sie die Daten aus Azure SQL-Datenbank.

    Der letzte Schritt darin, von der Verkaufspipeline ist zum Schreiben von Daten (z. B. Vorhersagen von maschinellen Learning) in der SQL-Datenbank. Sie müssen möglicherweise warten, bis zu 2 Stunden für die Daten in SQL-Datenbank angezeigt werden. Eine Möglichkeit zum Überwachen, wie viele Daten in einer SQL-Datenbank verfügbar ist, erfolgt über [Azure-Verwaltungsportal](https://manage.windowsazure.com/). Suchen Sie auf klicken Sie im linken Bereich SQL-Datenbanken![](media\cortana-analytics-technical-guide-demand-forecast\SQLicon2.png) , und klicken Sie darauf. Klicken Sie dann suchen Sie Ihre Datenbank (d. h. demo123456db), und klicken Sie darauf. Klicken Sie auf der nächsten Seite im Abschnitt **"Verbinden zur Datenbank"** auf **"Ausführen Transact-SQL-Abfragen für die SQL-Datenbank"**.

    Sie können hier, klicken Sie auf neue Abfrage und Abfrage für die Anzahl von Zeilen (z. B. "select count(*) aus DemandRealHourly)" während Ihrer Datenbank immer umfangreicher wird, die Anzahl der Zeilen in der Tabelle sollte vergrößern)

3. Überprüfen Sie die Daten aus der Power BI-Dashboard.

    Sie können Power BI langsamste Pfad Dashboard einrichten, die eingehenden unformatierten Daten zu überwachen. Führen Sie die Anweisung im Abschnitt "Power BI-Dashboard".



## <a name="power-bi-dashboard"></a>**Power BI-Dashboard**

### <a name="overview"></a>(Übersicht)

In diesem Abschnitt beschrieben, wie Sie die Power BI-Dashboard zu visualisieren von Ihrem Echtzeitdaten aus Azure Stream Analytics (wichtiges Pfad) sowie SCHÄTZER Ergebnisse von Azure maschinellen learning (kalt Pfad) einrichten.


### <a name="setup-hot-path-dashboard"></a>Setup langsamste Pfad Dashboard

Die folgenden Schritte führt Sie wie Echtzeit Datenausgabe Stream Analytics Aufträge visualisiert werden sollen, die zum Zeitpunkt der Bereitstellung der Lösung generiert wurden. Die folgenden Schritte ausführen, ist ein [Power BI online](http://www.powerbi.com/) -Konto erforderlich. Wenn Sie kein Konto haben, können Sie [einen erstellen](https://powerbi.microsoft.com/pricing).

1.  Fügen Sie Power BI-Ausgabe in Azure Stream Analytics (ASA) hinzu.

    -  Sie müssen so folgen Sie die Anweisungen in [Azure Stream Analytics und Power BI: ein Dashboard in Echtzeit Analytics in Echtzeit Sichtbarkeit von streaming-Daten](stream-analytics-power-bi-dashboard.md) zum Einrichten der Ausgabe Ihres Auftrags Azure Stream Analytics als Ihrer Power BI-Dashboard.

    - Suchen Sie nach den Stream Analytics Auftrag Ihrer [Azure-Verwaltungsportal](https://manage.windowsazure.com). Der Name des Projekts sollte sein: YourSolutionName + "Streamingjob" + zufällige Zahl + "Asapbi" (d. h. demostreamingjob123456asapbi).

    - Fügen Sie eine PowerBI Ausgabe für das Projekt ASA hinzu. Legen Sie die **Ausgabealias** als **'PBIoutput'**ein. Legen Sie Ihren **Dataset Namen** und **Tabellennamen** als **'EnergyStreamData'**ein. Nachdem Sie die Ausgabe hinzugefügt haben, klicken Sie auf am unteren Rand der Seite **"Start"** , um den Auftrag Stream Analytics zu starten. Sie sollten eine bestätigungsmeldung (*z. B.*, "Starten Stream Analytics Auftrag myteststreamingjob12345asablob wurde erfolgreich abgeschlossen") erhalten.

2. Melden Sie sich bei [Power BI online](http://www.powerbi.com)

    -   Klicken Sie auf den linken Bereich Datasets Abschnitt Meine Arbeitsbereich sollten Sie ein neues Dataset mit auf den linken Bereich des Power BI finden Sie unter sein. Dies ist das streaming Daten, die Sie im vorherigen Schritt aus Azure Stream Analytics abgelegt.

    -   Vergewissern Sie sich im Bereich ***Visualisierungen*** geöffnet ist, und klicken Sie auf der rechten Seite des Bildschirms angezeigt wird.

3. Erstellen Sie die Kachel "Demand durch Timestamp":
    -   Klicken Sie auf Dataset **'EnergyStreamData'** auf der linken Bereich Datasets Abschnitt.

    -   Klicken Sie auf **"Liniendiagramm"** Symbol ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic8.png).

    -   Klicken Sie im Bereich **Felder** auf 'EnergyStreamData'.

    -   Klicken Sie auf **"Zeitstempel"** , und stellen Sie sicher, dass es unter "Achse" zeigt. Klicken Sie auf **"Laden"** , und stellen Sie sicher, dass es unter "Werte" zeigt.

    -   Klicken Sie oben auf **Speichern** , und benennen Sie den Bericht als "EnergyStreamDataReport". Der Bericht mit dem Namen "EnergyStreamDataReport" wird im Abschnitt Berichte im Bereich Navigator auf der linken Seite angezeigt.

    -   Klicken Sie auf **"Pin visuell"** ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic6.png) -Symbol auf der oberen rechten Ecke der dieses Liniendiagramm, ein "Pin-Dashboard" Fenster möglicherweise zur Anzeige von für Sie zum Auswählen eines Dashboards. Wählen Sie "EnergyStreamDataReport" aus, und klicken Sie dann klicken Sie auf "Pin".

    -   Bewegen Sie die Maus über diese Kachel auf dem Dashboard, klicken Sie auf "Bearbeiten" Symbol in der oberen rechten Ecke, um seinen Titel als "Demand durch Timestamp" ändern

4.  Andere Dashboard Kacheln basierend auf den entsprechenden Datasets zu erstellen. Nachfolgend finden Sie die endgültige Dashboard-Ansicht.
        ![](media\cortana-analytics-technical-guide-demand-forecast\PBIFullScreen.png)


### <a name="setup-cold-path-dashboard"></a>Setup kalt Pfad Dashboard
Befindet, kalt Pfad Daten Verkaufspipeline muss vor allem um die Demand Planung der einzelnen Regionen zu erhalten. Power BI stellt eine Verbindung mit einer SQL Azure-Datenbank als Datenquelle, die Vorhersageergebnisse gespeichert sind.

> [AZURE.NOTE] 1) dauert einige Stunden genug Wettervorhersage angezeigt werden Ergebnisse für das Dashboard sammeln. Es empfiehlt sich, dass Sie diesen Vorgang starten 2 und 3 Stunden, nachdem Sie den Generator Mittagessen. 2) in diesem Schritt ist die Voraussetzung herunterladen und Installieren von kostenlosen [Power BI-Desktop](https://powerbi.microsoft.com/desktop)-Software aus.



1.  Erhalten Sie die Datenbankanmeldeinformationen ein.

    **Datenbankservername, Datenbankname, Benutzername und Kennwort** benötigen Sie vor dem Verschieben in den nächsten Schritten fort. Hier sind die Schritte können Sie problemlos finden wie führen.

    -   Nachdem **"Azure SQL-Datenbank"** in Ihrem Diagramm der Lösung Vorlage grünen verwandelt, klicken Sie darauf, und klicken Sie dann auf **"Öffnen"**. Der Assistent unterstützt Sie zum Azure-Verwaltungsportal und Ihre Datenbank Informationsseite wird ebenfalls geöffnet werden.

    -   Suchen Sie auf der Seite einen Abschnitt "Datenbank". Aufgelistet, die Datenbank, die Sie erstellt haben. Der Namen der Datenbank sollten **"Ihrer Lösungsnamen + Zufallszahl +"Db""** (z. B. "mytest12345db").

    -   Klicken Sie auf die Datenbank in das neue Pop, Pannel, finden Sie Ihre Datenbankservername oben. Ihre Datenbank Server Name Name muss werden **"Ihrer Lösungsnamen + Zufallszahl + 'database.windows.net,1433'"** (z. B. "mytest12345.database.windows.net,1433").

    -   Ihre Datenbank- **Benutzernamen** und Ihr **Kennwort** stimmen mit den Benutzernamen und Kennwort zuvor aufgezeichnet werden, während der Bereitstellung der Lösung.

2.  Aktualisieren der Datenquelle der kalt Pfad Power BI-Datei
    -  Stellen Sie sicher, dass Sie die neueste Version von [Power BI-Desktop](https://powerbi.microsoft.com/desktop)installiert haben.

    -   Klicken Sie im Ordner **"DemandForecastingDataGeneratorv1.0"** , die, den Sie heruntergeladen haben mit der Doppelklicken auf die Datei **' Power BI-Template\DemandForecastPowerBI.pbix'.** Anfänglichen Visualisierungen basieren auf-platzhalterprodukt Daten. **Hinweis:** Wenn einen Fehler Suchdaten, stellen Sie sicher, dass angezeigt wird, haben Sie die neueste Version von Power BI-Desktop installiert.

        Nachdem Sie es am oberen Rand der Datei öffnen, klicken Sie auf **' Abfragen bearbeiten '**. Im Fenster Pop Doppelklick **'Quelle'** auf den rechten Bereich.
    ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic1.png)

    -   Ersetzen Sie im Fenster Pop **"Server"** und **"Database"** mit Ihren eigenen Server und den Datenbanknamen, und klicken Sie dann auf **"OK"**. Stellen Sie für Servername angezeigt sicher, dass Sie den Port 1433 (**YourSolutionName.database.windows.net, 1433**) angeben. Ignorieren der Warnung Nachrichten, die auf dem Bildschirm angezeigt werden.

    -   In den nächsten Pop, Fenster sehen Sie zwei Optionen im linken Bereich (**Windows** und **Datenbank**). Klicken Sie auf **"Datenbank"**, "Füllen Sie aus" auf Ihren **"Username"** und **"Kennwort"** (Dies ist der Benutzername und Kennwort ein, das Sie eingegeben haben, wenn die Lösung bereitgestellt und eine SQL Azure-Datenbank erstellt). ***Wählen Sie die Ebene, um diese Einstellungen anwenden***möchten aktivieren Sie Ebene Datenbankoption. Klicken Sie auf **"Verbinden"**.

    -   Sobald Sie zurück zur vorherigen Seite geführt haben, schließen Sie das Fenster. Wird eine Nachricht pop-out – klicken Sie auf **Übernehmen**. Klicken Sie abschließend auf **Speichern** , um die Änderungen zu speichern. Die Power BI-Datei hat die Verbindung mit dem Server jetzt eingerichtet. Wenn Ihre Visualisierungen leer sind, stellen Sie sicher, dass Sie die Auswahlmöglichkeiten in der Visualisierungen alle Daten visualisiert werden sollen, indem Sie auf das Symbol Radierer, klicken Sie auf der oberen rechten Ecke des die Legenden deaktivieren. Verwenden Sie die Aktualisierungsschaltfläche, um neue Daten auf Visualisierungen wiederzugeben. Anfangs nur sehen die Ausgangswerte für Ihre Visualisierungen Sie, wie die Daten Factory aktualisieren alle 3 Stunden geplant wurde. Nach 3 Stunden sehen Sie neue Vorhersagen in Ihrer Visualisierungen wiedergegeben wird, wenn Sie die Daten aktualisieren.

3. (Optional) Veröffentlichen Sie das Dashboard kalt Pfad [online Power BI](http://www.powerbi.com/). Beachten Sie, dass dieses Schritts Power BI-Konto (oder Office 365-Konto) benötigt.

    -   Klicken Sie auf **"Veröffentlichen"** und einige Sekunden später ein Fensters angezeigt wird "Veröffentlichung für Power BI Erfolg!" mit einem grünen Häkchen angezeigt. Klicken Sie auf den Link unter "Öffnen demoprediction.pbix in Power BI". Ausführliche Anweisungen finden Sie unter [Veröffentlichen von Power BI-Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).

    -   So erstellen ein neues Dashboard: Klicken Sie auf die **+** (+) neben dem Abschnitt " **Dashboards** " im linken Bereich. Geben Sie den Namen "Demand prognostizieren Demo" für das neue Dashboard.

    -   Nachdem Sie den Bericht zu öffnen, klicken Sie auf ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic6.png) auf alle Visualisierungen zum Dashboard anheften. Finden Sie Wenn Sie ausführliche Anweisungen [Anheften einer Kachel zu einem Power BI-Dashboard von einem Bericht](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
        Wechseln Sie zu der Dashboardseite und passen Sie der Größe und Position des Ihrer Visualisierungen an und bearbeiten Sie deren Titel zu. Ausführliche Anweisungen zum Bearbeiten der Kacheln finden Sie unter [Bearbeiten einer Kachel – Ändern der Größe, verschieben, umbenennen, Pin, löschen, fügen Sie Links hinzu](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Hier ist ein Beispieldashboard mit einigen kalt Pfad Visualisierungen angehefteten darauf.

        ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic7.png)

4. (Optional) Aktualisieren der Datenquelle zu planen.
    -     Zeigen Sie auf den Terminplan Aktualisieren der Daten, mit dem Mauszeiger über das Dataset **EnergyBPI-abgeschlossen** , klicken Sie auf ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic3.png) und wählen Sie dann auf **Zeitplan zu aktualisieren**.
    **Hinweis:** Wenn Sie sehen, dass eine Warnung Massage, klicken Sie auf **Anmeldeinformationen bearbeiten** , und stellen Sie sicher sind Ihre Datenbankanmeldeinformationen entspricht den in Schritt 1 beschriebenen aus.

    ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic4.png)

    -   Erweitern Sie im Abschnitt **Terminplan aktualisieren** aus. Aktivieren Sie "die Daten auf dem neuesten Stand halten".

    -   Planen Sie die Aktualisierung entsprechend Ihren Anforderungen. Weitere Informationen finden Sie unter [Aktualisieren von Daten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/).


## <a name="how-to-delete-your-solution"></a>**So löschen Sie Ihre Lösung**
Stellen Sie sicher, dass den Daten-Generator beenden, wenn die Lösung nicht aktiv verwenden, während der Ausführung des Daten-Generators höhere Kosten anfallen. Löschen Sie die Lösung, wenn Sie nicht verwendet werden. Ihre Lösung löschen werden alle Komponenten, die in Ihrem Abonnement bereitgestellt werden, wenn Sie die Lösung bereitgestellt. Löschen die Lösung auf Ihren Namen der Lösung im linken Bereich der Lösungsvorlage, und klicken auf Löschen.

## <a name="cost-estimation-tools"></a>**Kosten Abschätzung Tools**

Die folgenden beiden Tools stehen Ihnen die Gesamtkosten für Energy Lösungsvorlage im Rahmen Ihres Abonnements ausgeführt Prognostizieren von Demand beteiligt besser zu erleichtern:

-   [Microsoft Azure Kosten Rechner Tool (online)](https://azure.microsoft.com/pricing/calculator/)

-   [Microsoft Azure Kosten Rechner Tool (Desktop)](http://www.microsoft.com/download/details.aspx?id=43376)

## <a name="acknowledgements"></a>**Empfangsbestätigungen**
In diesem Artikel wird von Daten Scientist Yijing Chen und Softwareentwickler Qiu Min bei Microsoft verfasst.
