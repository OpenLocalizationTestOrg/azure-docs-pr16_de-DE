<properties
   pageTitle="Behandlung von Logik Apps Ausnahmen | Microsoft Azure"
   description="Erfahren Sie, Muster zum Fehler und Behandlung mit Azure Logik Apps"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-error-and-exception-handling"></a>Logik Apps Fehler und Behandlung von Ausnahmen

Logik Apps stellt eine umfangreiche Sammlung von Tools und Mustern, um sicherzustellen, dass Ihre Integration sind robuste und robuste gegen Fehler.  Zu den Aufgaben mit einem beliebigen Integrationsarchitektur müssen Sie sicherstellen, dass Ausfallzeiten oder Problemen abhängige Systeme ordnungsgemäß verarbeitet werden.  Logik Apps macht Fehlerbehandlung einer erster Klasse zur Verfügung steht, was Ihnen die Tools, die Sie auf Ausnahmen und Fehler in Ihrer Workflows dienen müssen.

## <a name="retry-policies"></a>Wiederholen von Richtlinien

Der grundlegendsten Ausnahme und Fehlerbehandlung ist eine "Wiederholen"-Richtlinie.  Diese Richtlinie definiert werden, wenn die Aktion wiederholen soll, wenn ursprüngliche Anforderung Timeout oder fehlgeschlagen ist (eine Anforderung, die eine 429 geführt haben oder 5xx-Antwort).  Standardmäßig wiederholen Sie alle Aktionen 4 zusätzliche Zeiten über 20 Sekunden.  In diesem Fall ist die erste Anforderung empfangen eine `500 Internal Server Error` Antwort, die Workflow-Engine hält für 20 Sekunden, und versuchen Sie die Anfrage erneut.  Wenn nach Versuche die Antwort immer noch eine Ausnahme oder ein Fehler ist, der Workflow fortfahren und kennzeichnen den Aktionsstatus als wird `Failed`.

Sie können die **Eingaben** für eine bestimmte Aktion "Wiederholen" Richtlinien konfigurieren.  Eine "Wiederholen"-Richtlinie kann bis zu 4 Mal über 1 Stunde Intervallen versuchen konfiguriert werden.  Vollständige Details auf die Eingabewerte Eigenschaften können [auf MSDN gefunden]werden kann[retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Falls gewünscht die HTTP-Aktion wiederholen 4 Mal und warten Sie 10 Minuten zwischen den einzelnen versuchen, müssen Sie die folgende Definition:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

Weitere Informationen zu unterstützten Syntax, finden Sie im [Abschnitt "Wiederholen"-Richtlinie, auf der MSDN-][retryPolicyMSDN].

## <a name="runafter-property-to-catch-failures"></a>RunAfter-Eigenschaft, um Fehler abzufangen.

Jede Logik app Aktion deklariert welche Aktionen müssen abgeschlossen werden, bevor die Aktion gestartet werden kann.  Sie können diesen vorstellen, wie die Reihenfolge der Schritte in den Workflow.  Diese Reihenfolge ist bekannt als der `runAfter` Eigenschaft in der Aktionsdefinition.  Es ist ein Objekt, das beschreibt, welche Aktionen und die Aktion Status die Aktion ausführen möchten.  Standardmäßig sind alle Aktionen hinzugefügt, über den Designer festgelegt, dass `runAfter` im vorherigen Schritt, wenn Sie im vorherige Schritt wurde `Succeeded`.  Sie können diesen Wert, um die Aktionen ausgelöst wird, sobald die zuvor ausgeführten Aktionen werden jedoch anpassen `Failed`, `Skipped`, oder diese Wertemenge möglich.  Wenn Sie ein Element mit einem festgelegten Dienstbus Thema nach einer bestimmten Aktion hinzufügen wollten `Insert_Row` fehlschlägt, verwenden Sie die folgenden `runAfter` Konfiguration:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Hinweis Die `runAfter` auslösen soll, wenn die Eigenschaft festgelegt ist die `Insert_Row` Aktion ist `Failed`.  Die Aktion ausführen der Aktionsstatus lautet `Succeeded`, `Failed`, oder `Skipped` die Syntax wäre:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

>[AZURE.TIP] Aktionen, führen Sie nach eine vorhergehenden Aktion fehlgeschlagen sind und erfolgreich, abgeschlossen, markiert `Succeeded`.  Dieses Verhalten bedeutet, sofern Sie erfolgreich alle Fehler in einem Workflow ausführen selbst ist `Succeeded`.

## <a name="scopes-and-results-to-evaluate-actions"></a>Bereiche und Ergebnisse für Aktionen ausgewertet werden soll

Ähnlich wie die ausgeführt werden können können nach einzelne Aktionen, Sie auch Gruppe Aktionen zusammen in einen [Bereich](app-service-logic-loops-and-scopes.md) – die als eine logische Gruppierung von Aktionen fungieren.  Bereiche eignen sich zum Organisieren von Logik app Aktionen und zur Durchführung evaluieren aggregieren möchten, auf den Status eines Bereichs.  Der Bereich selbst erhalten einen Status aus, nachdem alle Aktionen in einem Bereich beendet haben.  Der Bereich Status wird mit den gleichen Kriterien als eine ausführen – bestimmt, ist die letzte Aktion in einer Verzweigung Ausführung `Failed` oder `Aborted` der Status lautet `Failed`.

Sie können `runAfter` Bereich markiert wurde `Failed` bestimmte Aktionen für alle Fehler ausgelöst wird, die innerhalb des Bereichs aufgetreten sind.  Ausgeführt werden, nachdem Sie ein Bereich schlägt fehl, können Sie eine einzelne Aktion, um Fehler abzufangen, wenn *Alle* Aktionen innerhalb des Bereichs ein Fehler auftreten, erstellen.

### <a name="getting-the-context-of-failures-with-results"></a>Abrufen des Kontexts der Fehler mit Ergebnissen

Aufspüren von Fehlern aus einem Bereich ist dann sehr hilfreich, aber es möglicherweise sinnvoll, den Kontext zu verstehen, genau welche fehlgeschlagen, Aktionen und Fehler oder Statuscodes, die zurückgegeben wurden.  Die `@result()` stellt Kontext in das Ergebnis aller Aktionen in einem Bereich Workflow-Funktion.

`@result()`einen einzelnen Parameter, Bereichsnamen dauert, und gibt eine Matrix von allen die Aktionsergebnisse in diesem Bereich.  Diese Aktionsobjekte enthalten die gleichen Attribute wie die `@actions()` Object, einschließlich Aktion Startzeit, Endzeit Aktion, Aktionsstatus, Aktion Eingaben, Aktion Korrelations-IDs und Aktion gibt.  Können Sie problemlos Verbinden einer `@result()` Funktion mit einer `runAfter` , Kontext der Aktionen, die nicht innerhalb eines Bereichs zu senden.

Wenn Sie eine Aktion *für jede* Aktion in einem Bereich ausführen möchten, die `Failed`, können Sie verbinden `@result()` mit einer Aktion **[Filter Matrix](../connectors/connectors-native-query.md)** und **[ForEach](app-service-logic-loops-and-scopes.md)** endlos wiedergegeben wird.  So können Sie die Matrix von Ergebnissen zu Aktionen zu filtern, die fehlgeschlagen ist.  Die gefilterten Ergebnis Matrix ausführen können und Ausführen eine Aktion für jeden Fehler mit der **ForEach** -Schleife.  Hier ein Beispiel unten, gefolgt von einer ausführliche Erläuterung.  In diesem Beispiel wird eine HTTP POST-Anforderung mit den Hauptteil einer Antwort von Aktionen, die nicht innerhalb des Bereichs senden `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Es folgt eine ausführliche exemplarische Vorgehensweise, was passiert:

1. **Array der Filters** Aktion zum Filtern der `@result('My_Scope')` das Ergebnis aller Aktionen innerhalb erhalten`My_Scope`
1. Bedingung des **Filters Matrix** ist eine `@result()` Element mit dem Status gleich `Failed`.  Dadurch wird die Matrix aller Aktion Ergebnisse aus Filtern `My_Scope` auf nur ein Array von fehlgeschlagene Aktionsergebnisse.
1. Führen Sie eine Aktion **für jeden** auf die Ausgaben für die **Matrix gefiltert** .  Dadurch wird eine Aktion *für jeden* Fehler beim Aktionsergebnis wir über gefiltert durchführen.
    - Wenn eine einzelne Aktion in den Bereich, der die Aktionen in Fehler aufgetreten ist die `foreach` würde nur einmal ausgeführt.  Viele fehlgeschlagene Aktionen würde eine Aktion pro Fehler auf.
1. Senden einer HTTP-POST auf die `foreach` Antwort Textbereich Element oder `@item()['outputs']['body']`.  Die `@result()` Element Form unterscheidet sich von der `@actions()` -shape, und die gleiche Weise analysiert werden können.
1. Auch im Lieferumfang von zwei benutzerdefinierte Header mit dem Namen fehlgeschlagene Aktion `@item()['name']` und der Fehler beim Ausführen von Client-ID-Überwachung `@item()['clientTrackingId']`.

Als Referenz hier ist ein Beispiel für ein einzelnes `@result()` Element.  Sie können sehen die `name`, `body`, und `clientTrackingId` Eigenschaften im obigen Beispiel analysiert.  Darauf hinzuweisen, die außerhalb von einer `foreach`, `@result()` gibt ein Array von dieser Objekte.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/foo/bar does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

Die obigen Ausdrücke können Sie andere Ausnahme behandeln von Mustern ausführen.  Sie können auch eine Ausnahme behandeln von außerhalb des Bereichs, der das gesamte gefilterte Array von Fehlern und entfernen akzeptiert Aktion ausführen der `foreach`.  Sie können auch andere nützlichen Eigenschaften einschließen der `@result()` Antwort abgebildet.

## <a name="azure-diagnostics-and-telemetry"></a>Azure Diagnose und werden

Die oben aufgeführten Muster werden hervorragende Möglichkeit zum Behandeln von Fehlern und Ausnahmen innerhalb eines Testlaufs, aber Sie können auch identifizieren und Antworten auf Fehler unabhängig von der selbst ausführen.  [Azure-Diagnose](app-service-logic-monitor-your-logic-apps.md) bietet eine einfache Möglichkeit, alle Workflowereignisse (einschließlich aller ausführen und der Aktion Status) mit Azure-Speicher Firma oder ein Ereignis Azure-Hub zu senden.  Sie können überwachen die Protokolle und Kriterien oder in einem beliebigen Überwachungstools, die Sie ausführen Status auswerten möchten, zu veröffentlichen.  Eine mögliche Möglichkeit ist, alle Ereignisse über Azure Ereignis Hub in [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)streamen.  In Stream Analytics können Sie sich von einem beliebigen Bildschirmdarstellung auftreten, Durchschnittswerte oder Fehlern live Abfragen aus den Diagnoseprotokollen schreiben.  Stream Analytics können einfach zu anderen Datenquellen wie Warteschlangen, Themen, SQL, DocumentDB und Power BI ausgeben.

## <a name="next-steps"></a>Nächste Schritte
- [Finden Sie unter wie eine Customer robuste Fehlerbehandlung mit Logik Apps erstellt](app-service-logic-scenario-error-and-exception-handling.md)
- [Suchen nach weiteren Logik Apps Beispiele und Szenarien](app-service-logic-examples-and-scenarios.md)
- [Informationen Sie zum Erstellen von automatisierter Bereitstellungen Logik Apps](app-service-logic-create-deploy-template.md)
- [Entwerfen und Logik apps aus Visual Studio bereitstellen](app-service-logic-deploy-from-vs.md)


<!-- References -->
[retryPolicyMSDN]: https://msdn.microsoft.com/library/azure/mt643939.aspx#Anchor_9