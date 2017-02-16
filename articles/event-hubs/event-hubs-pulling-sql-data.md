<properties
   pageTitle="Ziehen von SQL-Daten in Azure Ereignis Hubs | Microsoft Azure"
   description="Übersicht über den Ereignis untergeordneten Servern importieren aus SQL-Beispiel"
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor=""/>

<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="pulling-data-from-sql-into-an-azure-event-hub"></a>Ziehen von Daten aus SQL in ein Ereignis Azure-Hub

Eine typische Architektur für eine Anwendung für die Verarbeitung von Echtzeitdaten umfasst, drücken Sie es zuerst nach einer Azure-Hub Ereignis. Ein Szenario IoT beispielsweise den Datenverkehr auf verschiedenen gestreckt von einer Autobahnen, Überwachung oder ein Spiele-Szenario, die Aktionen einer Trümpfe der frenzied Wettbewerber, Überwachung oder Enterprise-Szenario, wie z. B. Überwachung Gebäude Belegung möglicherweise. In diesen Fällen die Daten Hersteller sind im allgemeinen externen Objekte erzeugt Zeit Datenreihen, die Sie benötigen, um zu erfassen, analysieren, speichern und ändern sind, sowie Sie möglicherweise haben investiert viel Arbeit in die Infrastruktur für folgende Prozesse Gebäude. Was Sie tun, wenn Sie Daten von einem Element wie eine Datenbank statt einer Quelle Streaming Daten einfügen möchten, und verwenden Sie es zusammen mit Ihren anderen streaming Daten? Berücksichtigen der Groß-/Kleinschreibung Azure Stream Analytics, Remote-Daten-Explorer (RDX) oder einem anderen Tool zum Analysieren und dienen langsam veränderliche Daten von Microsoft Dynamics oder einen benutzerdefinierten Fertigungsanlagen-Management-System verwendet werden soll? Um dieses Problem zu lösen, haben wir geschrieben und öffnen intern eine kleine Cloud Stichprobe, die Sie ändern und bereitstellen können, die die Daten aus einer SQL-Tabelle und schieben Sie ihn an eine Azure Ereignis Verteiler als Eingabe in der analytischen Verwendungen verwendet wird. Führen erkennen Sie, dass dies ist eine seltene Szenario und die entgegengesetzt, was Sie normalerweise mit einem Ereignis-Hub ausführen. Wenn Sie sich in der Lage finden, wenn dies ist, was Sie tun müssen, können Sie den Code im Azure Beispiele Katalog [hier](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-import-from-sql/)suchen.  

Beachten Sie, dass der Code in diesem Beispiel nur mit einer Stichprobe wird. Es ist **nicht** als eine Anwendung Herstellung anzusehen und keine Versuche unternommen wurden, für die Verwendung geeignet auf diese Weise wird – es ist Stricly eine DIY, Beispiel Entwicklertools konzentrieren. Sie müssen alle Arten von Sicherheit, Leistung, Funktionalität überprüfen und Kosten Faktoren, bevor Sie auf eine End-to-End-Architektur aus.

## <a name="application-structure"></a>Anwendungsstruktur

Die Anwendung in c# geschrieben ist, und die Datei readme.md in der Stichprobe enthält alle Informationen, die Sie ändern, erstellen und veröffentlichen Sie die Anwendung müssen. Die folgenden Abschnitte enthalten einen auf hoher Ebenen Überblick über die Funktionsweise der Anwendungs.

Zunächst unter der Voraussetzung, dass Sie Zugriff auf eine SQL Azure-Tabelle verfügen. Sie müssen außerdem einen Azure Ereignis Hub eingerichtet haben, und die Verbindung wissen Zeichenfolge erforderlich, darauf zuzugreifen.

Wenn die Lösung SqlToEventHub startet, wird eine Konfigurationsdatei (App.config), um eine Reihe von Dingen, erhalten, wie in der Datei readme.md beschrieben gelesen. Hierbei handelt es sich um mehr oder weniger selbsterklärend, wie der Name des Datentabelle usw., und es ist nicht erforderlich, um die erläuterungen hier hierzu. 

Nachdem Sie die Anwendung die Konfigurationsdatei lesen verfügt, wechselt es in einer Schleife, lesen die SQL-Tabelle drücken Sie Datensätze nach dem Ereignis Hub, und dann ein benutzerdefinierter Ruheintervall warten, bevor Sie es über alle erneut. Zu beachten sind einige Merkmale auf:

1. Die Anwendung basiert auf der Annahme, die von einem externen Prozess die SQL-Tabelle aktualisiert wird, und Sie alle senden möchten, und nur die Updates für ein Ereignis Hub.
2. Die SQL-Tabelle muss ein Feld vorhanden ist, eine eindeutige und zunehmende Zahl, beispielsweise ein Eintrag Anzahl. Dies kann so einfach wie das ein Feld namens "Id", oder Sonstiges, die als den Inhalt dieser Datenbank aktualisiert erhöht wird Fügt Datensätze wie "Creation_time" oder "Sequence_number" hinzu. Die Anwendung von Notizen und speichert den Wert für dieses Feld in jeder Iteration. In jeder nachfolgenden Pass-through-Schleife, die Anwendung in der Tabelle im Wesentlichen Abfragen für alle Datensätze, wenn der Wert dieses Felds den Wert überschreitet, im Browser angezeigt wurden die Uhrzeit der letzten der Schleife. Wir werden diesen letzten Wert der "Offset" aufrufen.
3. Die Anwendung erstellt eine Tabelle "TableOffsets", wenn es nach oben, beginnt den Versatz zu speichern. Die Tabelle wird in der Abfrage erstellt, die "CreateOffsetTableQuery" in der Konfigurationsdatei definiert. 
4. Es gibt mehrere Abfragen verwendet für das Arbeiten mit der Versatz Tabelle als "OffsetQuery", "UpdateOffsetQuery" und "InsertOffsetQuery" in der Konfigurationsdatei definiert. Sie sollten diese nicht ändern.
5. Schließlich ist die Abfrage, die "Datenabfrage" in der Konfigurationsdatei definiert die Abfrage, die ausgeführt werden soll, um die Einträge aus der SQL-Tabelle ziehen. Es ist derzeit beschränkt an oberen 1.000 Datensätze in jeder Pass-through-Schleife zur Optimierung verwendet – Wenn beispielsweise 25.000 Datensätze in der Datenbank seit der letzten Abfrage hinzugefügt wurden Es konnte eine Weile dauern zum Ausführen der Abfrage. Beschränken Sie die Abfrage an 1.000 Datensätze jedes Mal, werden die Abfragen viel schneller ausgeführt. Auswahl im oberen Bereich 1.000 einfache legt aufeinanderfolgende Stapeln von 1.000 Datensätze an den Hub Ereignis.    

## <a name="next-steps"></a>Nächste Schritte

Um die Lösung bereitstellen, Klonen Sie oder Herunterladen Sie die Anwendung SqlToEventHub, bearbeiten Sie die App, erstellen sie und schließlich veröffentlichen. Nachdem Sie die Anwendung veröffentlicht haben, können Sie finden sie in der klassischen Azure-Portal unter Cloud Services ausgeführt und überwachen die Ereignisse an Ihre Ereignis Verteiler in Kürze. Beachten Sie, dass die Häufigkeit durch zwei Faktoren bestimmt wird: der Häufigkeit von die Updates für die SQL-Tabelle und der Ruheintervall, die Sie in der Konfigurationsdatei für die Anwendung angegeben haben.