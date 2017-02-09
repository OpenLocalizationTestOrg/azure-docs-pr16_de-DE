<properties 
   pageTitle="Stream Analytics Grenzwerte Tabelle"
   description="Beschreibt System Grenzwerte und empfohlene Größen für Verbindungen und Stream Analytics-Komponenten."
   services="stream-analytics"
   documentationCenter="NA"
   authors="jeffstokes72"
   manager="paulettm"
   editor="cgronlun" />
<tags 
   ms.service="stream-analytics"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-data"
   ms.date="07/25/2016"
   ms.author="jeffstok" />

| Grenzwert für Bezeichner | Grenzwert       | Kommentare |
|----------------- | ------------|--------- |
| Maximale Anzahl der Einheiten Streaming pro Abonnement pro region | 50 | Eine Anforderung an das streaming Einheiten für Ihr Abonnement über 50 erhöhen kann durch Kontaktieren des [Microsoft-Supports](https://support.microsoft.com/en-us)vorgenommen werden. |
| Maximale Durchsatz einer Einheit Streaming | 1MB / s * | Maximale Durchsatz pro SU hängt vom Szenario ab. Tatsächliche Durchsatz möglicherweise niedriger und richtet sich nach der Aufteilung und Komplexität der Abfrage. Weitere Details finden Sie im Artikel [Skala Azure Stream Analytics Aufträge Datendurchsatz](../articles/stream-analytics/stream-analytics-scale-jobs.md) . |
| Maximale Anzahl der Eingänge pro Projekt | 60 | Es gibt eine feste Grenze von 60 Eingaben pro Stream Analytics-Projekt aus. |
| Maximale Anzahl der Ausgänge pro Projekt | 60 | Es gibt eine feste Grenze von 60 Ausgaben pro Stream Analytics-Projekt aus. |
| Maximale Anzahl von Funktionen pro Projekt | 60 | Es gibt eine feste Grenze von 60 Funktionen pro Stream Analytics-Projekt aus. |
| Maximale Anzahl von Aufträgen pro region | 1500 | Möglicherweise müssen Sie jedes Abonnement nach Zeitphasen bis zum 1500 Stellen pro geografische Region. |