<properties 
    pageTitle="In Echtzeit Verarbeitung mit Stream Analytics Ereignisse zu verarbeiten von Ereignissen | Microsoft Azure" 
    description="Erfahren Sie, wie eine Reihe von Azure Services zum Aktivieren der Verarbeitung von Ereignissen in Echtzeit und Analytics zusammenarbeiten kann." 
    keywords="in Echtzeit Verarbeitung, Ereignisse zu verarbeiten, Verweis Architektur"
    services="stream-analytics,event-hubs,storage,sql-database" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="stream-analytics" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Architektur verweisen: in Echtzeit mit Microsoft Azure Stream Analytics Verarbeitung von Ereignissen

Die Verweis-Architektur für in Echtzeit mit Azure Stream Analytics Verarbeitung von Ereignissen soll generischen Entwurf für die Bereitstellung von einer in Echtzeit Plattform as a Service (PaaS) Stream-Verarbeitung-Lösung mit Microsoft Azure bereitstellen.

## <a name="summary"></a>Zusammenfassung

In der Vergangenheit haben Lösungen Analytics Funktionen wie ETL (extrahieren, Transformation, laden) und Datawarehousing, basierend auf wurde, in dem Daten vor der Analyse gespeichert ist. Veränderliche Anforderungen, einschließlich weiterer schnell eingehenden Daten, werden diese vorhandene Modell bis zum Limit ablegen. Die Möglichkeit zum Analysieren von Daten in Streams vor dem Speichern verschieben ist eine Lösung, und gedrückt, während sie sich nicht um eine neue Funktion ist, der Ansatz weist keine wurde ausweitet auf allen Ebenen der Branche. 

Microsoft Azure bietet einen umfassende Katalog von Analytics-Technologien, die ein Array von anderen Lösung Szenarien und Anforderungen unterstützen können. Auswählen der Azure-Dienste für eine End-to-End-Lösung bereitstellen kann zur Herausforderung die große Bandbreite der Angebote angegeben werden. In diesem Dokument soll Beschreiben der Funktionen und Interoperabilität von verschiedenen Azure Dienste, die eine Lösung Ereignis-streaming unterstützen. Es wird erläutert, einige Szenarien, in denen Kunden von diese Art von Ansatz profitieren können.

## <a name="contents"></a>Inhalt

- Geschäftsleitung Zusammenfassung
- Einführung in Echtzeit Analytics
- Nutzen des Echtzeitdaten in Azure
- Häufige Szenarien beim in Echtzeit Analytics
- Architektur und-Komponenten
    - Datenquellen
    - Daten-Integration Layer
    - In Echtzeit Analytics Layer
    - Speicher Datenebene
    - Präsentation / Verbrauch Layer
- Abschluss

**Autor:** Charles Feddersen, Lösungsarchitekt, Data Einsichten Center von Excellence, vorbehalten.

**Veröffentlicht:** Januar 2015

**Überarbeitung:** 1.0

**Herunterladen:** [In Echtzeit mit Microsoft Azure Stream Analytics Verarbeitung von Ereignissen](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)


## <a name="get-help"></a>Anfordern von Hilfe
Für weitere Unterstützung zu erhalten versuchen Sie es unsere [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren Sie Azure Stream Analytics Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Bezug](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)

 
