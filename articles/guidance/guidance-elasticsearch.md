
<properties
   pageTitle="Elasticsearch Azure Erfassung | Microsoft Azure"
   description="Elasticsearch Azure Erfassung."
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

# <a name="elasticsearch-on-azure-guidance"></a>Elasticsearch Azure Erfassung 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Elasticsearch ist eine hochgradig skalierbare öffnen Source-Suchmaschine und die Datenbank. Es empfiehlt sich für Situationen, in denen schnelle Analyse und der Suche nach Informationen in große Datasets frei, die erforderlich ist. Dieser Leitfaden untersucht einige wichtige Aspekte zu berücksichtigen beim Entwerfen eines Elasticsearch Clusters, mit Schwerpunkt auf Methoden zum Optimieren und Testen der Konfiguration aus.

> [AZURE.NOTE]Dieser Leitfaden wird davon ausgegangen einige grundlegende Kenntnisse von Elasticsearch aus. Ausführlichere Informationen finden Sie auf der [Website Elasticsearch](https://www.elastic.co/products/elasticsearch). 

- **[Elasticsearch ausgeführt wird, klicken Sie auf Azure][]** bietet eine kurze Einführung in die allgemeine Struktur der Elasticsearch und beschreibt, wie Sie einen Elasticsearch Cluster mit Azure implementieren. 
- **[Optimieren Daten Aufnahme Leistung für Elasticsearch auf Azure][]** beschreibt die Bereitstellung von einem Elasticsearch Cluster sowie die genaue Analyse der verschiedenen Konfigurationsoptionen zu berücksichtigen ist, wenn Sie einen hohen Anteil an Daten Aufnahme erwarten.
- **[Optimieren Datenaggregation und Leistung von Abfragen für Elasticsearch auf Azure][]** bietet eine genaue Analyse der Optionen zu berücksichtigende entscheiden, wie Sie Ihrem System für die Abfrage, und suchen Sie Leistung zu optimieren.
- **[Konfigurieren von Stabilität und Wiederherstellung auf Elasticsearch auf Azure][]** werden einige wichtige Features von einem Elasticsearch Cluster, der Ihnen helfen können, ist die Wahrscheinlichkeit von Datenverlust und erweiterten Daten Wiederherstellungszeiten zu minimieren.
- **[Erstellen einer Leistung testen Umgebung für Elasticsearch auf Azure][]** beschrieben, wie Sie eine Umgebung zum Testen der Leistung Daten Aufnahme und Auslastung Abfrage in einem Cluster Elasticsearch einrichten. 
- **[Planen der Verwendung von Elasticsearch Implementieren eines JMeter testen][]** werden laufenden Performance-Tests mit JMeter Testpläne zusammen mit Java-Code als JUnit Test zur Durchführung von Aufgaben, z. B. Hochladen von Daten in der Elasticsearch Cluster integriert implementiert zusammengefasst.
- **[Bereitstellen einer JMeter JUnit Demo zum Testen der Leistung Elasticsearch][]** beschrieben, wie Sie erstellen und Verwenden einer JUnit Demo, die generieren und Daten zu einem Cluster Elasticsearch hochladen kann. Auf diese Weise einen hochgradig flexiblen Ansatz zum Testen laden, der große Datenmengen Test ohne abhängig von externen Datendateien generieren können. 
- **[Ausführen der automatisierten Elasticsearch Stabilität Tests][]** zusammengefasst, wie zum Ausführen der Stabilität Tests in dieser Reihe verwendet. 
- **[Ausführen der automatisierten Elasticsearch Leistungstests][]** zusammengefasst, wie zum Ausführen der Leistungstests in dieser Reihe verwendet.


[Elasticsearch auf Windows Azure ausgeführte]: guidance-elasticsearch-running-on-azure.md
[Optimieren von Daten Aufnahme Leistung für Elasticsearch auf Azure]: guidance-elasticsearch-tuning-data-ingestion-performance.md
[Erstellen einer Umgebung für Elasticsearch auf Azure testen Leistung]: guidance-elasticsearch-creating-performance-testing-environment.md
[Implementierung eines JMeter Test-Plans für Elasticsearch]: guidance-elasticsearch-implementing-jmeter-test-plan.md
[Bereitstellen einer JMeter JUnit Demo zum Testen der Leistung Elasticsearch]: guidance-elasticsearch-deploying-jmeter-junit-sampler.md
[Optimieren von Datenaggregation und Leistung von Abfragen für Elasticsearch auf Azure]: guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md
[Konfigurieren der Stabilität und Wiederherstellung auf Elasticsearch auf Azure]: guidance-elasticsearch-configuring-resilience-and-recovery.md
[Ausführen der automatisierten Elasticsearch Stabilität Tests]: guidance-elasticsearch-running-automated-resilience-tests.md
[Ausführen der automatisierten Elasticsearch Leistungstests]: guidance-elasticsearch-running-automated-performance-tests.md
