<properties 
    pageTitle="Einführung in Stream Analytics | Microsoft Azure" 
    description="Lernen Sie Stream Analytics, einen verwalteten Dienst, die Ihnen die streaming über das Internet der Dinge (IoT) Datenanalyse in Echtzeit erleichtert." 
    keywords="Analytics als Dienst, verwaltete Services, der während der Verarbeitung, streaming Analytics, was Stream Analytics ist"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="what-is-stream-analytics"></a>Was ist Stream Analytics?

Azure Stream Analytics ist eine vollständig verwaltete, kostengünstiger in Echtzeit Ereignis Verarbeitung-Engine, die trägt dazu bei, Tiefe Einblicke aus Daten entsperren. Stream Analytics erleichtert in Echtzeit analytisches Berechnungen auf streaming von Geräten, Sensoren, Websites, soziale Medien, Applikationen, Infrastruktur Systeme und weitere Daten einrichten.

Mit wenigen Mausklicks Azure-Portal können Sie auf einen Stream Analytics Auftrag angeben der Eingabewerte Quelle streaming Daten, die Ausgabe Empfänger für die Ergebnisse des Projekts erstellen und eine Datentransformation in einer SQL-ähnliche Sprache ausgedrückt. Sie können überwachen und Maßstab Geschwindigkeit Ihrer Position im Azure-Portal aus wenigen KB auf einem Gigabyte oder mehr pro Sekunde verarbeiteten Ereignisse skalieren anpassen.

Stream Analytics nutzt Jahre von Microsoft Research Arbeit bei der Entwicklung streaming TTS für dringende Verarbeitung und Integration der Sprache für die intuitive Spezifikationen eines solchen optimiert.

## <a name="what-can-i-use-stream-analytics-for"></a>Was kann ich Stream Analytics für werden verwendet?
Große Datenmengen werden heute mit hoher Geschwindigkeit über das Netzwerk entdeckt. Organisationen, die verarbeiten, und klicken Sie auf diese streaming Daten in Echtzeit dienen können können erheblich Verbesserung der Effizienz und sich in dem Markt voneinander unterscheiden. Der in Echtzeit streaming Analytics Szenarien finden Sie in allen Branchen: personalisierte, in Echtzeit Stock trading Analyse und Benachrichtigungen angebotenen Finanzdienstleister; in Echtzeit Betrugsversuche; Daten und Identität Schutz-Dienste; zuverlässigen Aufnahme und Analyse von Daten, die auf Sensoren und Aktuatoren eingebettet in physische Objekte (Internet der Dinge oder IoT); Web Clickstream Analytics; und den Kunden Beziehung (Relationship Management) Applications ausgeben Benachrichtigungen, wenn Customer Experience innerhalb eines Zeitrahmens beschädigt ist. Unternehmen suchen nach der flexible, zuverlässigen und kostengünstiger Weg solche in Echtzeit Ereignis-Stream Datenanalyse in der Welt hochgradig Mitbewerber moderne Business erfolgreich ausführen.

## <a name="key-capabilities-and-benefits"></a>Key-Funktionen und Vorteilen
-   **Center für erleichterte Bedienung:** Stream Analytics unterstützt ein Modell einfachen, deklarativen Abfrage für die Beschreibung von Transformationen. Center für erleichterte Bedienung optimieren möchten, Stream Analytics verwendet eine Variante T-SQL, und die Notwendigkeit der Kunden für den Umgang mit den technischen Aspekten der Verarbeitung Systeme Stream entfernt. Verwenden der [Stream Analytics-Abfragesprache](https://msdn.microsoft.com/library/azure/dn834998.aspx) im im Browser Abfrage-Editor, erhalten Sie IntelliSense AutoVervollständigen, mit deren Hilfe Sie können schnell und einfach implementieren Zeit Reihe Abfragen, einschließlich zeitliche-basierten Verknüpfungen, Fenster Aggregate, zeitliche Filter und andere allgemeinen Operationen wie Verknüpfungen, Aggregate, Projektionen und Filter. Darüber hinaus kann im Browser Abfrage testen anhand einer Stichprobe Datendatei schnelle, iterative Entwicklung.  

-   **Skalierbarkeit:** Stream Analytics ist hohe Ereignis Durchsatz von bis zu 1GB pro Sekunde verarbeiten. Integration in [Azure Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/) und [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/) ermöglichen die Lösung für Millionen Ereignisse pro vom verbundenen Geräten Clickstreams, zweiten kommen Aufnahme und Protokolldateien, um ein paar zu nennen. Um dies zu erreichen, nutzt Stream Analytics der vorherigen Videofunktionen von Ereignis Hubs, die 1 MB/s pro Partition ergeben können. Benutzer können die Berechnung in eine Zahl innerhalb der Abfragedefinition logische Schritte aufgeteilt werden, jede mit der Möglichkeit weitergegeben werden zum Erhöhen der Skalierbarkeit aufgeteilt.  

-   **Zuverlässigkeit, Wiederholbarkeit und schnelle Wiederherstellung:** Ein verwalteter Dienst in der Cloud, Stream Analytics verhindert Datenverlust, sowie die Geschäftskontinuität Ausfall durch integrierte Wiederherstellungsfunktionen. Die Möglichkeit, intern Status beibehalten werden soll bietet der Dienst wiederholt, um sicherzustellen, dass es ist möglich, Ereignisse archivieren und erneutes Anwenden, in der Zukunft, immer erste dieselben Ergebnisse Verarbeitung Ergebnisse. Dies kann Kunden zeitlich zurückgehen und Berechnungen ermitteln, wenn Analyse der ursprünglichen Ursache, was-wäre-wenn-Analyse, usw. ausführen.  

-   **Niedrigen Kosten:** Als Clouddienst ist Stream Analytics optimiert, um Benutzern ermöglichen sehr niedrig Kosten um jetzt geht's los und Verwalten von in Echtzeit Analytics Lösungen. Der Dienst ist für Sie Interesse basierend auf Streaming Einheit Verwendung und der Menge an das System verarbeiteten Daten durcharbeiten integriert. Verwendung wird basierend auf die Lautstärke der verarbeiteten Ereignisse und der Menge an berechnen Power im Cluster bereitgestellt, die entsprechenden Stream Analytics Einzelvorgänge behandeln abgeleitet.  

-   **Verweisen auf Daten:** Stream Analytics bietet Benutzern die Möglichkeit zum angeben und Bezug Daten verwenden. Möglicherweise zurückliegende Daten oder einfach nicht streaming Daten, die weniger häufig im Zeitverlauf ändern. Das System vereinfacht die Verwendung von Verweisdaten wie andere eingehende Ereignisstream mit anderen Ereignisstreams in Echtzeit zum Ausführen der Transformationen aufgenommen beitreten behandelt werden sollen.  

-   **Benutzerdefinierte Funktionen:** Stream Analytics verfügt Integration in Azure maschinellen Schulung zum Definieren von Funktion Anrufe in den Computer Learning-Dienst als Teil einer Abfrage Stream Analytics Erstreckt sich über die Funktionen von Stream Analytics vorhandene Azure maschinellen Learning Lösungen zu nutzen. Weitere Informationen hierzu finden Sie in der [Computer Learning Integration Lernprogramm](stream-analytics-machine-learning-integration-tutorial.md).

-   **Konnektivität:** Stream Analytics verbindet direkt mit Azure Ereignis Hubs und Azure IoT Hubs für Stream Aufnahme und Azure BLOB-Dienst Aufnahme zurückliegende Daten. Ergebnisse können aus Stream Analytics Azure-Speicher Blobs oder Tabellen, Azure SQL-DB, Azure Lake Datenspeicher, DocumentDB, Ereignis Hubs, Azure Service Bus Topics oder Warteschlangen und Power BI, in dem dann visualisiert, weiteren verarbeiteten Workflows, in Stapel Analytics über [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) verwendet oder möglich erneut verarbeitet als eine Reihe von Ereignissen geschrieben werden. Bei Verwendung von Hubs Ereignis ist es möglich, mehrere Stream Analytics zusammen mit anderen Datenquellen und Verarbeitung TTS ohne Verlust der streaming Art von den Berechnungen verfassen.  

## <a name="get-help"></a>Anfordern von Hilfe
Für weitere Unterstützung zu erhalten versuchen Sie es unsere [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
Sie haben eine Einführung in Stream Analytics, einen verwalteten Dienst für das streaming Analytics auf Daten aus dem Internet der Dinge wurde. Weitere Informationen zu diesem Dienst finden Sie unter:

- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren Sie Azure Stream Analytics Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Bezug](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)

