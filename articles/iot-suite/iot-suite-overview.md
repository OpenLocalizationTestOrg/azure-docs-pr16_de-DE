<properties
    pageTitle="Überblick über die Microsoft Azure IoT Suite | Microsoft Azure"
    description="Übersicht über bietet Azure IoT Suite Internet der Dinge vorkonfigurierten Lösungen zum Sammeln, analysieren, und Speichern von Daten, Bereitstellen von Visualisierungen und Integration mit anderen Systemen."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/09/2016"
     ms.author="dobett"/>

# <a name="what-is-azure-iot-suite"></a>Was ist Azure IoT Suite?

Im Internet Azure Dinge (IoT) Dienste bieten eine Vielzahl von Funktionen. Diese Enterprise Noten Services ermöglichen Folgendes:

- Sammeln von Daten von Geräten
- Analysieren von Daten Streams in Bewegung
- Speichern und großen Datenmengen Abfragen
- Visualisieren von Daten in Echtzeit und zurückliegende
- Mit Back Office-Systeme integrieren

Diese Funktionen, Azure IoT Suite-Paketen zusammen mit benutzerdefinierter Erweiterungen mehrere Azure-Dienste als *vorkonfigurierten Lösungen*vorführen. Diese vorkonfigurierten Lösungen sind Basis Implementierungen mit häufigen IoT Lösungsmustern, die Ihnen helfen, um die Zeit zu verringern, die Sie ergreifen, um Ihre IoT Solutions bieten. Verwenden der [IoT Softwareentwicklungskits][lnk-sdks], können Sie anpassen und erweitern Sie die folgenden Lösungsvorschlägen Ihre eigenen Bedürfnisse zugeschnitten. Sie können auch diese Lösungen als Beispiele oder Vorlagen verwenden, bei der Entwicklung neuer IoT Lösungen.

Das folgende Video bietet eine Einführung in Azure IoT Suite:

> [AZURE.VIDEO azurecon-2015-introducing-the-microsoft-azure-iot-suite]

## <a name="azure-iot-services-in-azure-iot-suite"></a>Azure IoT Dienste in Azure IoT Suite

In der Regel gehen die vorkonfigurierten Lösungen Sie folgendermaßen vor:

- Core Azure IoT der Suite ist der [Azure IoT Hub] [ lnk-iot-hub] Dienst. Dieser Dienst bietet die messaging Gerät Cloud und Cloud-zu-Gerät-Funktionen und fungiert als des Gateways auf der Cloud und anderen wichtigen IoT Suite Dienste. Der Dienst ermöglicht es Ihnen Befehle an Ihren Geräten senden und Empfangen von Nachrichten aus Ihren Geräten bei.

- [Azure Stream Analytics] [ lnk-asa] bietet Datenanalyse in Bewegung. IoT Suite nutzt diesen Dienst, um eingehenden telemetrieprotokoll zu verarbeiten, Aggregation ausführen und Ereignisse zu erkennen. Die vorkonfigurierten Lösungen verwenden Stream Analytics auch zum Verarbeiten von informativer Nachrichten, die Daten wie Metadaten oder Befehl Antworten von Geräten enthalten. Die Lösungen verwenden Stream Analytics zum Verarbeiten von Nachrichten von Ihren Geräten und übermitteln die Nachrichten mit anderen Diensten.

- [Azure-Speicher] [ lnk-azure-storage] und [Azure DocumentDB] [ lnk-document-db] Daten das Speicher bieten. Die vorkonfigurierten Lösungen verwenden Blob-Speicher gespeichert werden und für die Analyse verfügbar zu machen. Die Lösungen verwenden DocumentDB zum Speichern von Gerätemetadaten, und aktivieren die Geräteverwaltungsfunktionen von Solutions.

- [Azure Web Apps] [ lnk-web-apps] und [Microsoft Power BI] [ lnk-power-bi] Daten das Visualisierung bieten. Die Flexibilität von Power BI können Sie schnell interaktive Dashboards erstellen, die IoT Suite Daten verwenden.

Eine Übersicht über die Architektur einer normalen IoT Lösung, finden Sie unter [Microsoft Azure und das Internet der Dinge (IoT)][iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>Vorkonfigurierten Lösungen

IoT Suite enthält vorkonfigurierten Lösungen, die Sie mit schnellen Einstieg und Untersuchen von den gängigen IoT Szenarios wie *Remote-Überwachung* aktivieren und *Vorhersage Wartung*, die Azure IoT Suite ermöglicht. Sie können diese Lösungen für Ihr Abonnement Azure bereitstellen, und führen Sie ein vollständiges, End-to-End-IoT Szenario.

## <a name="next-steps"></a>Nächste Schritte

Nun stehen Ihnen einen Überblick über die Möglichkeiten IoT Suite, und welche seiner Hauptkomponenten sind, können Sie weitere Informationen zu den vorkonfigurierten Lösungen in IoT Suite, finden Sie unter [Was die Azure IoT sind vorkonfiguriert Lösungen?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
