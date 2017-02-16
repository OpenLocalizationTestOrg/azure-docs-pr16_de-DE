
<properties
   pageTitle="Ausführen der automatisierten Elasticsearch Leistungstests | Microsoft Azure"
   description="Beschreibung der, wie Sie die Leistungstests in Ihrer eigenen Umgebung ausführen können."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>
   
# <a name="running-the-automated-elasticsearch-performance-tests"></a>Ausführen der automatisierten Elasticsearch Leistungstests

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel ist [Teil einer Serie](guidance-elasticsearch.md). 

Die Dokumente [Optimieren Daten Aufnahme Leistung für Elasticsearch auf Azure] und [Optimieren Datenaggregation und Leistung von Abfragen für Elasticsearch auf Azure] beschreiben eine Reihe von Performance-Tests, die für eine Stichprobe Elasticsearch Cluster ausgeführt wurden.

Diese Tests wurden Skripts verwendet, damit sie automatisierte Verfahren ausgeführt werden können. Dieses Dokument beschreibt, wie Sie die Tests in Ihrer eigenen Umgebung wiederholen können.

## <a name="prerequisites"></a>Erforderliche Komponenten

Die automatisierten Tests erfordern die folgenden Elemente:

-  Ein Elasticsearch Cluster.

- Eine JMeter Umgebung einrichten wie durch das [Erstellen einer Leistung testen Umgebung für Elasticsearch auf Azure]Dokument beschrieben.

- [Python 3.5.1](https://www.python.org/downloads/release/python-351/) auf dem Master JMeter virtuellen Computer installiert ist.


## <a name="how-the-tests-work"></a>Wie funktionieren die tests
Die Tests werden JMeter verwendet. Ein JMeter master-Server lädt einen Testplan und übergibt diese an eine Gruppe von untergeordneten JMeter-Servern, die die Tests tatsächlich ausgeführt werden. Der JMeter master-Server koordiniert die untergeordneten JMeter-Server und sammelt die Ergebnisse.

Die folgenden Testpläne stehen zur Verfügung:

* [elasticsearchautotestplan3nodes.jmx](https://github.com/mspnp/azure-guidance/blob/master/ingestion-and-query-tests/templates/elasticsearchautotestplan3nodes.jmx). Wird den Erfassung Test über einen Cluster mit 3 Knoten ausgeführt.

* [elasticsearchautotestplan6nodes.jmx](https://github.com/mspnp/azure-guidance/blob/master/ingestion-and-query-tests/templates/elasticsearchautotestplan6nodes.jmx). Wird den Erfassung Test über einen Cluster mit 6 Knoten ausgeführt.

* [elasticsearchautotestplan6qnodes.jmx](https://github.com/mspnp/azure-guidance/blob/master/ingestion-and-query-tests/templates/elasticsearchautotestplan6qnodes.jmx). Wird die Aufnahme und Abfrage testen über einen Cluster mit 6 Knoten ausgeführt.

* [elasticsearchautotestplan6nodesqueryonly.jmx](https://github.com/mspnp/azure-guidance/blob/master/ingestion-and-query-tests/templates/elasticsearchautotestplan6nodesqueryonly.jmx). Wird die Teststatistik eines Abfrage nur über einen Cluster mit 6 Knoten ausgeführt.


Verwenden Sie diese Pläne als Grundlage für eigene Szenarien testen, wenn Sie weniger oder mehr Knoten benötigen.

Testpläne verwenden eine JUnit Anforderung Demo zu generieren und die Testdaten hochladen. Der JMeter Testplan erstellt und ausgeführt wird diese Demo und überwacht die einzelnen Knoten Elasticsearch für Performance-Daten.  

## <a name="building-and-deploying-the-junit-jar-and-dependencies"></a>Erstellen und Bereitstellen von JUnit JAR und Abhängigkeiten
Vor dem Ausführen der Leistungstests, die heruntergeladen werden soll, kompilieren und die JUnit Tests befinden sich in der Leistung/Junitcode Ordner bereitstellen. Diese Tests werden von den JMeter Testplan verwiesen werden. Weitere Informationen finden Sie unter dem Verfahren "Importieren ein vorhandenes JUnit Testprojekt in" Ellipse "ein" im Dokument [Bereitstellen einer JMeter JUnit Demo zum Testen der Leistung Elasticsearch].

Es gibt zwei Versionen der JUnit Tests aus: 

- [Elasticsearch1.73](https://github.com/mspnp/azure-guidance/tree/master/ingestion-and-query-tests/junitcode/elasticsearch1.73). Verwenden Sie diesen Code zur Durchführung der Erfassung überprüft. Verwenden diese Tests Elasticsearch 1,73.

- [Elasticsearch2](https://github.com/mspnp/azure-guidance/tree/master/ingestion-and-query-tests/junitcode/elasticsearch2). Verwenden Sie diesen Code zum Ausführen der Abfrage überprüft. Verwenden diese Tests Elasticsearch 2.1 und höher.

Kopieren Sie die entsprechende Java (JAR) Archivdatei zusammen mit den restlichen die Abhängigkeiten auf Ihren Computern JMeter. Der Vorgang ist in [Bereitstellen einer JMeter JUnit Demo zum Testen der Leistung Elasticsearch][]beschrieben. 

> **Wichtige** Verwenden Sie nach der Bereitstellung von einer JUnit testen JMeter zum Laden und den Testpläne, die dieser Test JUnit verweisen, und stellen Sie sicher, dass die BulkInsertLarge Threadgruppe verweist auf die richtige JAR-Datei, die JUnit Klassennamen konfigurieren und Testen Sie Methode:
> 
> ![](./media/guidance-elasticsearch/performance-tests-image1.png)
> 
> Speichern Sie die aktualisierten Testpläne vor dem Ausführen der Tests.

## <a name="creating-the-test-indexes"></a>Erstellen die Testindizes
Jeder Test führt Aufnahme und/oder Abfragen in einem einzigen Index angegeben, wenn der Test ausgeführt wird. Sie sollten mithilfe von Schemas beschrieben, die in den Anhängen zu den Dokumenten [Optimieren Daten Aufnahme Leistung für Elasticsearch auf Azure] und [Optimieren Datenaggregation und Leistung von Abfragen für Elasticsearch auf Azure] Index zu erstellen und konfigurieren diese entsprechend Ihrer Testszenario (Doc Werte aktiviert/deaktiviert, mehrere Replikate und usw.). Beachten Sie, dass die Testpläne wird davon ausgegangen, dass der Index einen einzelnen Typ mit dem Namen *Ctip*enthält.

## <a name="configuring-the-test-script-parameters"></a>Konfigurieren von Parametern der Test-Skript
Kopieren Sie die folgenden Test-Skript Parameter-Dateien auf dem Servercomputer JMeter aus:

* [run.properties](https://github.com/mspnp/azure-guidance/blob/master/ingestion-and-query-tests/run.properties). Diese Datei gibt die Anzahl der JMeter Test Threads, die Dauer der Prüfung (in Sekunden), die IP-Adresse von einem Knoten (oder ein Lastenausgleich im Cluster Elasticsearch) verwenden und den Namen der Cluster an:

  ```ini
  nthreads=3
  duration=300
  elasticip=<IP Address or DNS Name Here>
  clustername=<Cluster Name Here>
  ```
  
  Bearbeiten Sie diese Datei, und geben Sie die entsprechenden Werte für Ihre testen und Cluster.

* [Abfrage-Config-Win](https://github.com/mspnp/azure-guidance/blob/master/ingestion-and-query-tests/query-config-win.ini) und [Abfrage-Config-nix.ini](https://github.com/mspnp/azure-guidance/blob/master/ingestion-and-query-tests/query-config-nix.ini). Diese beiden Dateien enthalten dieselben Informationen; die Datei *gewonnen* für Windows-Dateinamen und Pfade formatiert ist und die Datei *Nix* für Linux Dateinamen und Pfade formatiert ist:

  ```ini
  [DEFAULT]
  debug=true #if true shows console logs.

  [RUN]
  pathreports=C:\Users\administrator1\jmeter\test-results\ #path where tests results are saved.
  jmx=C:\Users\administrator1\testplan.jmx #path to the JMeter test plan.
  machines=10.0.0.1,10.0.0.2,10.0.0.3 #IPs of the Elasticsearch data nodes separated by commas.
  reports=aggr,err,tps,waitio,cpu,network,disk,response,view #Name of the reports separated by commas.
  tests=idx1,idx2 #Elasticsearch index(es) name(s) to test, comma delimited if more than one.
  properties=run.properties #Name of the properties file.
  ```

  Bearbeiten Sie diese Datei zum Angeben der Speicherorte der Testergebnisse, den Namen des Plans JMeter Test ausgeführt werden, die IP-Adressen der Knoten Daten Elasticsearch Performance-Werte aus gesammelten die unformatierten Leistungsdaten, die generiert werden, enthalten Berichte und den Namen (oder Namen Trennzeichen-getrennt) für die Indizes unter Test ausgeführt wird, wenn mehr als ein , Tests einzeln nacheinander ausgeführt werden. Wenn die run.properties-Datei in einem anderen Ordner oder Verzeichnis befindet, geben Sie den vollständigen Pfad zu dieser Datei ein.

## <a name="running-the-tests"></a>Ausführen der tests

* Kopieren Sie die Datei [Abfrage-test.py](https://github.com/mspnp/azure-guidance/blob/master/ingestion-and-query-tests/query-test.py) auf dem Servercomputer JMeter in denselben Ordner wie die run.properties und Abfrage-Config-Win (Abfrage Config nix.ini) Dateien ein.

* Sicherstellen Sie, dass jmeter.bat (Windows) oder jmeter.sh (Linux) auf den Pfad für die ausführbare Datei für Ihre Umgebung.

* Führen Sie das Skript Abfrage-test.py, über die Befehlszeile zum Ausführen von Tests:

  ```cmd
  py query-test.py
  ```

* Wenn der Test abgeschlossen wurde, werden die Ergebnisse der Satz von durch Kommas getrennte (CSV) Dateien in der Datei Abfrage Config Win (Abfrage Config nix.ini) angegebenen Werte gespeichert. Sie können Excel zu analysieren und diese Daten von Aktienkursen.


[Optimieren von Daten Aufnahme Leistung für Elasticsearch auf Azure]: guidance-elasticsearch-tuning-data-ingestion-performance.md
[Optimieren von Datenaggregation und Leistung von Abfragen für Elasticsearch auf Azure]: guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md
[Erstellen einer Umgebung für Elasticsearch auf Azure testen Leistung]: guidance-elasticsearch-creating-performance-testing-environment.md
[Bereitstellen einer JMeter JUnit Demo zum Testen der Leistung Elasticsearch]: guidance-elasticsearch-deploying-jmeter-junit-sampler.md
