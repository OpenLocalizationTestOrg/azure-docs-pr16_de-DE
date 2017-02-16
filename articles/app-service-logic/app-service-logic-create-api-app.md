<properties 
    pageTitle="Erstellen Sie eine API für Logik Apps" 
    description="Erstellen einer benutzerdefinierten API zur Verwendung mit Logik Apps" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na" 
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>
    
# <a name="creating-a-custom-api-to-use-with-logic-apps"></a>Erstellen einer benutzerdefinierten API zur Verwendung mit Logik Apps

Wenn Sie die Logik Apps-Plattform erweitern möchten, gibt es viele Möglichkeiten, die Sie anrufen können in APIs oder Systeme, die nicht als eines unserer viele Out-of-Box-Connectors verfügbar sind.  Diese Möglichkeiten zum Erstellen einer App API können Sie innerhalb eines Workflows Logik App aus aufrufen.

## <a name="helpful-tools"></a>Nützliche Tools

APIs am besten mit Logik Apps arbeiten wird empfohlen, ein [swagger](http://swagger.io) Dokument mit detaillierten Informationen zu den unterstützten Vorgänge und Parameter für Ihre API generieren.  Es gibt viele Bibliotheken (wie [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)), die die Swagger für Sie automatisch generiert.  Sie können auch [TRex](https://github.com/nihaue/TRex) verwenden, helfen Kommentieren der Swagger Logik Apps auch konzipiert (anzeigen Namen, Typen von usw.).  Für einige Beispiele API-Apps, die speziell für Logik Apps unbedingt unsere [GitHub Repository](http://github.com/logicappsio) oder [einen Blog](http://aka.ms/logicappsblog)Auschecken.

## <a name="actions"></a>Aktionen

Der grundlegende Aktion für eine App Logik handelt es sich um einen Controller, der eine HTTP-Anforderung akzeptiert und eine Antwort (normalerweise 200) zurückgeben.  Es gibt jedoch unterschiedliche Mustern, denen, die Sie folgen können, um Aktionen entsprechend Ihren Anforderungen zu erweitern.

Standardmäßig ist die Logik App-Engine wird Timeout eine Anforderung nach einer Minute.  Müssen Sie jedoch, Ihre API ausführen, klicken Sie auf Aktionen, die länger dauern, und das Modul für den Abschluss, warten Sie, indem Sie entweder eine asynchrone oder Webhook Muster unter detaillierte haben.

Für standard-Aktionen Schreiben von HTTP-Anforderungsmethode einfach Ihre API, die über Swagger verfügbar gemacht wird.  Beispiele von apps API erkennen, mit denen Logik Apps in unseren [GitHub Repository](https://github.com/logicappsio)zusammenarbeiten.  Nachstehend sind Verfahren, um gängige Muster mit einem benutzerdefinierten Verbinder zu erreichen.

### <a name="long-running-actions---async-pattern"></a>Lange ausgeführten Aktionen - Muster asynchronen

Wenn Sie eine lange Schritt oder eine Aufgabe ausführen zu können, wird als erstes, die Sie tun müssen, stellen Sie sicher, dass die-Engine weiß, dass Sie noch nicht überschritten. Müssen Sie auch mit der-Engine kommunizieren wie wissen wird, wenn Sie den Vorgang abgeschlossen haben, und schließlich zurückgegeben relevante Daten an die-Engine, sodass weiterhin für den Workflow werden muss. Sie können, die über eine API Folgen des Ablaufs unten durchführen. Der Punkt der Ansicht der benutzerdefinierten-API sind folgende Schritte aus:

1. Wenn eine Anforderung eingeht, sofort eine Antwort (vor dem zurück gearbeitet wird). Kann ich diese Antwort ein `202 ACCEPTED` Antwort, die-Engine wissen, haben Sie die Daten, sodass die Nutzlast akzeptiert und jetzt verarbeitet werden. Die Antwort 202 sollte die folgenden Kopfzeilen enthalten: 
 * `location`Kopfzeile (erforderlich): Dies ist ein absoluter Pfad zu der URL Logik Apps Überprüfen des Status des Projekts verwenden kann.
 * `retry-after`(optional, wird standardmäßig für Aktionen 20). Dies ist die Anzahl von Sekunden ein, die die-Engine zum Abrufen der URL des Speicherorts Kopfzeile bis sollte um den Status zu überprüfen.

2. Wenn ein Auftragsstatus aktiviert ist, führen Sie die folgenden Prüfungen: 
 * Wenn Sie der Auftrag fertig ist: Zurückgeben eines `200 OK` Antwort, mit der Antwort Nutzlast.
 * Wenn Sie der Auftrag weiterhin verarbeitet: Zurückgeben einer anderen `202 ACCEPTED` Antwort, mit der gleichen Kopfzeilen als die erste Antwort

Dieses Muster können Sie sehr lange Vorgänge innerhalb eines Thread des Ihre benutzerdefinierten API, ausführen, aber aufrechterhalten der einer aktiven Verbindungs mit der Logik Apps-Engine, damit dies nicht der Fall Timeout oder fortgesetzt werden, bevor der Arbeit abgeschlossen ist. Wenn Sie dies in Ihrer App Logik hinzufügen, ist es wichtig zu beachten, dass Sie nicht benötigen, führen Sie in der Definition für die App Logik weiterhin Umfrage, und überprüfen Sie den Status Ihrer nichts. Sobald die-Engine akzeptiert Reaktionen 202 mit einer gültigen Speicherort Kopfzeile sieht, wird das Muster asynchronen berücksichtigt, und fahren Sie mit die Position Kopfzeile Umfrage, bis ein nicht-202 zurückgegeben wird.

Sehen Sie ein Beispiel für dieses Muster in GitHub [hier](https://github.com/jeffhollan/LogicAppsAsyncResponseSample)

### <a name="webhook-actions"></a>Webhook Aktionen

Während der Workflow haben Sie die App Logik zeigen und warten, bis ein "Rückruf", um den Vorgang fortzusetzen.  Dieser Rückruf kommt in Form von HTTP POST.  Um dieses Muster zu implementieren, müssen Sie zwei Endpunkte auf Ihrem Controller bereitstellen: Ihr Abonnement.

Klicken Sie auf 'abonnieren' die Logik App erstellen und registriert einen Rückruf-URL, die Ihre API gespeichert werden kann und Rückruf sofort als HTTP POST.  Alle Inhalte/Header werden in der App Logik übergeben und in den Rest des Workflows verwendet werden können.  Die Logik App-Engine Ruft den Punkt abonnieren auf Ausführung, sobald sie diesen Schritt Treffer.

Wenn ausführen aufgehoben wurde, wird die Logik App-Engine, den Endpunkt 'Kündigen des Abonnements' aufzurufen.  Ihre API kann dann die URL der Rückruf aufgehoben werden, je nach Bedarf.

Der Logik App-Designer wird zurzeit nicht unterstützt entdecken einen Endpunkt Webhook bis Swagger, damit zum Verwenden dieser Aktion müssen Sie die Aktion "Webhook" hinzufügen und geben Sie die URL, Überschriften und Textkörper der Anforderung.  Sie können die `@listCallbackUrl()` Workflow-Funktion in einem dieser Felder je nach Bedarf, um den Rückruf-URL zu übergeben.

Sehen Sie ein Beispiel für ein Muster Webhook in GitHub [hier](https://github.com/jeffhollan/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs)

## <a name="triggers"></a>Trigger

Zusätzlich zu Aktionen können Sie Ihre benutzerdefinierte API Act als Trigger zu einer App Logik haben.  Es gibt zwei Mustern, denen Sie eine App Logik ausgelöst unter folgen können:

### <a name="polling-triggers"></a>Umfragen Trigger

Umfragen Trigger Verhalten sich ähnlich wie die lange ausgeführt asynchrone Aktionen oben.  Die Logik App-Engine Ruft den Endpunkt auslösen nach Ablauf eine bestimmten Zeitspanne (je nach 15 Sekunden für Premium, Standard, 1 Minute und 1 Stunde Free SKU).

Wenn keine Daten vorhanden sind, gibt die Auslösen eines `202 ACCEPTED` Antwort, mit einer `location` und `retry-after` Kopfzeile.  Jedoch für Trigger Es empfiehlt sich die `location` Kopfzeile enthält einen Abfrageparameter der `triggerState`.  Hier einige Bezeichner für Ihre API wissen, wann die letzte Uhrzeit der App Logik ausgelöst.  Wenn Daten zur Verfügung stehen, wird der Trigger gibt eine `200 OK` Antwort mit der Inhalt Nutzlast.  Dadurch wird die App Logik ausgelöst.

Beispiel, wenn ich abrufen wurde, um festzustellen, ob eine Datei verfügbar wurde könnten Sie einen Trigger Umfragen erstellen, der die folgenden Aktionen ausführen möchten:

* Die API würde zurückgeben, wenn eine Anforderung mit keine TriggerState empfangen wurde ein `202 ACCEPTED` mit einer `location` Header, der eine TriggerState der aktuellen Uhrzeit und ein `retry-after` 15.
* Wenn Sie eine Besprechungsanfrage mit einer TriggerState empfangen wurde:
 * Überprüfen Sie, wenn Sie alle Dateien nach der TriggerState DateTime hinzugefügt wurden. 
  * Ist 1 Datei, Zurückgeben einer `200 OK` Antwort mit dem Inhalt gefährliche Fracht, Erhöhen der TriggerState in der DateTime-Wert der Datei ich zurückgegeben, und legen Sie die `retry-after` auf 15.
  * Wenn mehrere Dateien vorhanden sind, kann ich 1 zurückgeben, jeweils mit einer `200 OK`, erhöhen Sie meine TriggerState in der `location` Kopf- und festlegen `retry-after` 0.  Auf diese Weise können die-Engine wissen weitere Daten vorhanden sind, und es wird sofort fordern Sie auf die `location` Kopfzeile angegeben haben.
  * Zurückzugeben, wenn keine Dateien vorhanden sind, eine `202 ACCEPTED` Antwort, und lassen Sie die `location` TriggerState identisch.  Festlegen von `retry-after` auf 15.

Sehen Sie ein Beispiel eines Triggers Umfragen in GitHub [hier](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)

### <a name="webhook-triggers"></a>Webhook Trigger

Webhook Trigger Verhalten sich ähnlich wie Webhook Aktionen oben.  Die Logik App-Engine Ruft den Endpunkt 'abonnieren', ein Triggers Webhook hinzugefügt und gespeichert ist.  Ihre API können Sie die URL Webhook registrieren und nennen Sie diese über HTTP POST aus, wenn Daten verfügbar sind.  Der Inhalt alle Aspekte und Überschriften werden in der Logik App ausführen übergeben.

Eine Webhook auslösen jemals gelöscht wird (der Logik-App vollständig oder nur die Webhook auslösen), die-Engine tätigen eines Anrufs an die URL 'Kündigen des Abonnements', in dem Ihre API kann aufgehoben werden die URL der Rückruf und beenden Sie alle Prozesse wie gewünscht wird.

Aktuell unterstützt im Logik App-Designer kein Triggers Webhook bis Swagger, entdecken, wenn Sie diese Art von Aktion verwenden den Trigger "Webhook" hinzufügen und geben Sie die URL, Überschriften und Textkörper der Anforderung.  Sie können die `@listCallbackUrl()` Workflow-Funktion in einem dieser Felder je nach Bedarf, um den Rückruf-URL zu übergeben.

Sehen Sie ein Beispiel eines Triggers Webhook in GitHub [hier](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)