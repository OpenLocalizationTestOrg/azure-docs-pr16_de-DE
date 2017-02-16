<properties 
    pageTitle="Stream Analytics Versionsinformationen | Microsoft Azure" 
    description="Stream Analytics – Versionsinformationen" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="stream-analytics-release-notes"></a>Stream Analytics Freigeben von Notizen

## <a name="notes-for-04152016-release-of-stream-analytics"></a>Notizen für 04/15/2016 Version von Stream Analytics ##

Diese Version enthält das folgende Update.

Titel | Beschreibung
---|---
Allgemeine Verfügbarkeit für Power BI-Ausgaben  | [Power BI gibt](stream-analytics-power-bi-dashboard.md) sind jetzt in der Regel verfügbar. Der Ablauf von 90 Tagen Autorisierung für Power BI wurde entfernt. Weitere Informationen zu Szenarien, in denen Autorisierung erneuert werden muss, finden Sie unter Abschnitt [Erneuern Autorisierung](stream-analytics-power-bi-dashboard.md#Renew-authorization) des Erstellens eines Dashboards Power BI.

## <a name="notes-for-03032016-release-of-stream-analytics"></a>Notizen für 03/03/2016 Version von Stream Analytics ##

Diese Version enthält das folgende Update.

Titel | Beschreibung
---|---
Neue Stream Analytics-Abfragesprache Elemente  | SAQL verfügt jetzt über [GetType](https://msdn.microsoft.com/library/azure/mt643890.aspx "GetType MSDN-Seite"), [TRY_CAST](https://msdn.microsoft.com/library/azure/mt643735.aspx "TRY_CAST MSDN-Seite") und [REGEXMATCH](https://msdn.microsoft.com/library/azure/mt643891.aspx "REGEXMATCH MSDN-Seite").

## <a name="notes-for-12102015-release-of-stream-analytics"></a>Notizen für 12/10/2015 Version von Stream Analytics ##

Diese Version enthält das folgende Update.

Titel | Beschreibung
---|---
REST-API Version aktualisieren | Die REST-API Version wurde 2015-10-01 aktualisiert. Details finden Sie auf MSDN unter [Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx) und [Computer-Learning-Integration in Stream Analytics](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md).
Azure-Computern Learning-Integration | Mit dieser Version erhalten Sie Unterstützung für benutzerdefinierte Funktionen Learning Azure-Computern aus. Finden Sie das [Lernprogramm](stream-analytics-machine-learning-integration-tutorial.md) für Weitere Informationen sowie die [Allgemeine Blog Ankündigung](http://blogs.technet.com/b/machinelearning/archive/2015/12/10/apply-azure-ml-as-a-function-in-azure-stream-analytics.aspx)aus.

## <a name="notes-for-11122015-release-of-stream-analytics"></a>Notizen für 11/12/2015 Version von Stream Analytics ##

Diese Version enthält das folgende Update.

Titel | Beschreibung
---|---
Neue Verhalten der SELECT-Anweisung | Wählen Sie in Stream Analytics wurde erweitert, um zulassen * als ein Eigenschaftenaccessor eines Datensatzes geschachtelte. Weitere Informationen finden Sie unter [http://msdn.microsoft.com/library/mt622759.aspx](http://msdn.microsoft.com/library/mt622759.aspx "Komplexer Datentypen").

## <a name="notes-for-10222015-release-of-stream-analytics"></a>Notizen für 10/22/2015 Version von Stream Analytics ##

Diese Version enthält die folgenden Updates.

Titel | Beschreibung
---|---
Zusätzliche Abfrage-Sprachen | Stream Analytics weist die Abfragesprache erweitert, indem Sie die folgenden Features: [ABS](https://msdn.microsoft.com/library/azure/mt574054.aspx), [CEILING](https://msdn.microsoft.com/library/azure/mt605286.aspx), [EXP](https://msdn.microsoft.com/library/azure/mt605289.aspx), [FLOOR](https://msdn.microsoft.com/library/azure/mt605240.aspx), [POWER](https://msdn.microsoft.com/library/azure/mt605287.aspx), [Melden Sie sich](https://msdn.microsoft.com/library/azure/mt605290.aspx), [QUADRAT](https://msdn.microsoft.com/library/azure/mt605288.aspx)und [Wurzel](https://msdn.microsoft.com/library/azure/mt605238.aspx).
Aggregieren Einschränkungen entfernt  | Diese Version entfernt die Einschränkung der 15 Aggregate in einer Abfrage an. Es ist jetzt keine hinsichtlich der Anzahl der Aggregate pro Abfrage.
Zusätzliche Gruppe durch System.Timestamp-Funktion | Die [GROUP BY](https://msdn.microsoft.com/library/azure/dn835023.aspx) -Funktion ermöglicht jetzt entweder Window_type oder [System.Timestamp](https://msdn.microsoft.com/library/azure/mt598501.aspx).
Hinzugefügten Versatz für Tumbling und Windows Hopping | Standardmäßig werden [Tumbling](https://msdn.microsoft.com/library/azure/dn835055.aspx) und [Hopping](https://msdn.microsoft.com/library/azure/dn835041.aspx) Windows gegen 0 (null) Zeit ausgerichtet (1/1/0001 12:00:00 Uhr UTC). Der neue (optionale) Parameter 'Offsetsize' ermöglicht das Angeben einer benutzerdefinierten Offset (oder Ausrichtung).


## <a name="notes-for-09292015-release-of-stream-analytics"></a>Notizen für 09/29/2015 Version von Stream Analytics ##

Diese Version enthält die folgenden Updates.

Titel | Beschreibung
---|---
Azure IoT Suite Public Preview | Stream Analytics ist in der öffentlichen Vorschau der Azure IoT Suite enthalten.
Azure-Portal-integration | Zusätzlich zu Vorhandensein im Verwaltungsportal Azure ist Stream Analytics jetzt im [Portal Azure](https://azure.microsoft.com/overview/preview-portal/)integriert. Beachten Sie, dass Stream Analytics Funktionalität im Vorschau-Portal aktuell ist eine Teilmenge der Funktionen im Verwaltungsportal Azure, ohne Unterstützung für die Abfrage im Browser zu testen, angeboten ausgeben Power BI-Konfiguration und zum Durchsuchen oder Erstellen neuer Eingabe- und Ausgabe Ressourcen in Abonnements, die, denen Sie Zugriff haben.
Unterstützung für DocumentDB Ausgabe | Stream Analytics Aufträge können jetzt [DocumentDB](https://azure.microsoft.com/services/documentdb/)ausgeben.
Unterstützung für die Eingabe IoT-Hub | Stream Analytics Aufträge können Daten aus IoT Hubs jetzt Aufnahme.
TIMESTAMP nach für heterogene Ereignisse | Wenn ein einzelnes Data Stream mehrere Ereignis Datentypen werden in verschiedenen Feldern Probleme enthält, jetzt können [Zeitstempel durch](http://msdn.microsoft.com/library/mt573293.aspx) mit Ausdrücken Sie verschiedene zeitstempelfeldern für jeden Fall angeben.

## <a name="notes-for-09102015-release-of-stream-analytics"></a>Notizen für 09/10/2015 Version von Stream Analytics ##

Diese Version enthält die folgenden Updates.

Titel|Beschreibung
---|---
Unterstützung für PowerBI Gruppen|Klicken Sie zum Aktivieren der Freigabe von Daten mit anderen Benutzern Power BI können Stream Analytics Aufträge jetzt [PowerBI](stream-analytics-define-outputs.md#power-bi) Gruppen innerhalb Ihrer Power BI-Konto schreiben.

## <a name="notes-for-08202015-release-of-stream-analytics"></a>Notizen für 08/20/2015 Version von Stream Analytics ##

Diese Version enthält die folgenden Updates.

Titel|Beschreibung
---|---
Hinzugefügten letzten (Funktion) |Die [letzte](http://msdn.microsoft.com/library/mt421186.aspx) Funktion ist jetzt in Stream Analytics Einzelvorgänge, aktivieren Sie das letzte Ereignis in einem Stream von Ereignissen innerhalb eines bestimmten Zeitrahmens abrufen verfügbar.
Neue Array-Funktionen|Matrixfunktionen [GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx), [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx) und [GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx) sind jetzt verfügbar.
Neuen Datensatz-Funktionen|Datensatz-Funktionen [GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx) und [GetRecordPropertyValue](http://msdn.microsoft.com/library/mt270220.aspx) stehen jetzt zur Verfügung.

## <a name="notes-for-07302015-release-of-stream-analytics"></a>Notizen für 07/30/2015 Version von Stream Analytics ##

Diese Version enthält die folgenden Updates.

Titel|Beschreibung
---|---
Power BI Organisations-Id abgekoppelt aus Azure-Id|Dieses Feature ermöglicht die [Ausgabe der Power BI](stream-analytics-power-bi-dashboard.md) für ASA Aufträge unter jedem Azure Kontotyp (Live Id oder Organisations-Id). Darüber hinaus können Sie ein Organisations-Id für Ihr Konto Azure haben und eine andere für die Power BI-Ausgabe Autorisierung verwenden.
Unterstützung für Bus Servicewarteschlangen Ausgabe|[Bus Servicewarteschlangen](stream-analytics-define-outputs.md#service-bus-queues) Ausgaben sind jetzt in Stream Analytics Aufträge verfügbar.
Unterstützung für Service Bus Topics Ausgabe|[Service Bus Topics](stream-analytics-define-outputs.md#service-bus-topics) Ausgaben sind jetzt in Stream Analytics Aufträge verfügbar.

## <a name="notes-for-07092015-release-of-stream-analytics"></a>Notizen für 07/09/2015 Version von Stream Analytics ##

Diese Version enthält die folgenden Updates.


Titel|Beschreibung
---|---
Benutzerdefinierte BLOB-Ausgabe Partitionierung|BLOB-Speicher Ausgaben können nun einer Option aus, um die Häufigkeit anzugeben, die Ausgabe, die Blobs geschrieben werden und die Struktur und das Format der Ausgabe Pfad Ordner Datenstruktur. 

## <a name="notes-for-05032015-release-of-stream-analytics"></a>Notizen für 05/03/2015 Version von Stream Analytics ##

Diese Version enthält die folgenden Updates.


Titel|Beschreibung
---|---
Höhere Maximalwert für außerhalb der Reihenfolge Fehlertoleranz Fenster|Die maximale Größe für das Fenster der Reihenfolge von Out-Of-Fehlertoleranz ist jetzt 59:59 (mm: ss)
JSON-Ausgabeformat: Zeile getrennt oder einem Array|Es ist jetzt eine Option bei der Ausgabe an Blob-Speicher oder Ereignis Hub als ein Array von JSON-Objekten oder extrahieren JSON-Objekte mit einer neuen Zeile ausgegeben. 

## <a name="notes-for-04162015-release-of-stream-analytics"></a>Notizen für 04/16/2015 Version von Stream Analytics ##


Titel|Beschreibung
---|---
Verzögerung bei der Kontokonfiguration Azure-Speicher|Beim Erstellen eines Auftrags Stream Analytics in einem Bereich zum ersten Mal werden Sie aufgefordert werden, zum Erstellen einer neuen Speicher Firma oder ein vorhandenes Kontos für die Überwachung Stream Analytics Aufträge in diesem Bereich angeben. Aufgrund von Wartezeit für die Überwachung konfigurieren fordert erstellen einen anderen Stream Analytics Position in derselben Region innerhalb von 30 Minuten das Angeben eines ein zweites Speicherkonto statt mit der zuletzt konfigurierten Zeile in der Dropdownliste für die Überwachung von Speicher-Konto. Um zu vermeiden, erstellen ein nicht benötigter Speicherkonto, warten Sie 30 Minuten nach dem Erstellen eines Projekts in einem Bereich zum ersten Mal vor der Bereitstellung zusätzliche Aufträge in diesem Bereich ein.
Upgrade der Position|Zu diesem Zeitpunkt unterstützt Stream Analytics live Bearbeitungen die Definition oder die Konfiguration eines laufenden Auftrags. Um die Eingabe, Ausgabe, Abfrage, Skalierung oder Konfiguration eines laufenden Auftrags ändern möchten, müssen Sie zuerst den Auftrag beenden.
Datentypen abgeleitet aus einer Quelle|Wenn CREATE TABLE-Anweisung nicht verwendet wird, wird der Eingabe Typ von Eingabeformat abgeleitet, beispielsweise alle Felder aus CSV Zeichenfolge sind. Felder in den richtigen Typ die CAST-Funktion verwenden, um Konflikt Typfehler zu vermeiden explizit konvertiert werden müssen.
Fehlende Felder werden als null-Werte ausgegeben.|Verweisen auf ein Feld, das in der Datenquelle nicht vorhanden ist, führt zu null-Werten in der Ausgabeereignis.
ANWEISUNGEN für die muss SELECT-Anweisungen setzen.|In der Abfrage müssen SELECT-Anweisungen Unterabfragen definiert mit Anweisungen folgen.
Out-of-Memory-Problem|Streaming Analytics-Projekten mit einer großen Fehlertoleranz für außerhalb der Reihenfolge Ereignisse und/oder komplexe Abfragen warten, dass eine große Datenmenge Zustand des Auftrags nicht mehr genügend Arbeitsspeicher, die in einem Auftrag resultierender bewirken möglicherweise neu starten. Die Start- und Vorgänge werden in den Auftrag des Vorgangs Protokolle angezeigt. Skalieren Sie die Abfrage, um dieses Verhalten zu vermeiden, mehrere partitionsübergreifend. In zukünftigen Versionen wird diese Einschränkung behoben werden, indem beeinträchtigt die Leistung von betroffenen Aufträge statt neu zu.
Große Blob Eingaben ohne Nutzlast Timestamp können Out-of-Memory Problem verursachen.|Verarbeitung große Dateien von Blob-Speicher möglicherweise Stream Analytics Aufträge zum Absturz, wenn ein Timestamp-Feld nicht über Zeitstempel von angegeben ist. Um dieses Problem zu vermeiden, lassen Sie jede Blob unter 10MB Größe aus.
SQL-Datenbank Ereignis Lautstärke Einschränkung|Wenn Sie ein Ziel für die Ausgabe SQL-Datenbank zu verwenden, möglicherweise sehr große Datenmengen Ausgabedaten des Auftrags Stream Analytics Timeout bewirken. Um dieses Problem zu beheben, verringern der Lautstärke mithilfe von Aggregate oder Filteroperatoren, oder wählen Sie Azure Blob-Speicher oder Ereignis Hubs als Ausgabeziel stattdessen.
PowerBI Datasets kann nur eine Tabelle enthalten.|PowerBI unterstützt nicht mehr als eine Tabelle in einen bestimmten Datensatz unterdrückt.

## <a name="get-help"></a>Anfordern von Hilfe
Für weitere Unterstützung zu erhalten versuchen Sie es unsere [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren Sie Azure Stream Analytics Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Bezug](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
