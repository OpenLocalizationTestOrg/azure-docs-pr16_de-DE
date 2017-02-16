<properties
    pageTitle="Azure Funktionen Timer Trigger | Microsoft Azure"
    description="Verstehen Sie, wie Timer Trigger in Azure-Funktionen verwenden."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure-Funktionen, Funktionen Verarbeitung von Ereignissen, dynamische berechnen, ohne Server Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-timer-trigger"></a>Azure Funktionen Timer trigger

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In diesem Artikel wird erläutert, wie Timer Trigger Azure Funktionen konfigurieren. Zeitgeber ausgelöst Anruf Funktionen basierend auf einem Zeitplan, einmaliges oder als Terminserie.  

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-timer-trigger"></a>Function.JSON für Timer trigger

Die Datei *function.json* stellt einen Zeitplan-Ausdruck. Der folgende Zeitplan läuft beispielsweise die Funktion pro Minute:

```json
{
  "bindings": [
    {
      "schedule": "0 * * * * *",
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Der Zeitgeber Trigger mit mehreren Instanzen Skalierung automatisch verarbeitet: nur eine Instanz einer bestimmten Timer-Funktion wird für alle Instanzen ausgeführt werden.

## <a name="format-of-schedule-expression"></a>Formatieren von Expression Terminplan

Planen der Ausdruck ein [CRON-Ausdruck](http://en.wikipedia.org/wiki/Cron#CRON_expression) , der 6 Felder enthält: `{second} {minute} {hour} {day} {month} {day of the week}`. 

Beachten Sie, dass viele der Cron Ausdrücke, die Sie online finden Sie im Feld {zweite} weglassen, sodass beim Kopieren von einem von Ihnen werden für das zusätzliche Feld anpassen müssen. 

Hier sind einige andere Terminplan Beispiele für Ausdrücke:

Alle 5 Minuten ausgelöst:

```json
"schedule": "0 */5 * * * *"
```

Am oberen Rand jeder Stunde einmal ausgelöst wird:

```json
"schedule": "0 0 * * * *",
```

Alle zwei Stunden ausgelöst:

```json
"schedule": "0 0 */2 * * *",
```

Stündlich von 9: 00 Uhr 5 ausgelöst:

```json
"schedule": "0 0 9-17 * * *",
```

Um 9:30 Uhr jeden Tag ausgelöst wird:

```json
"schedule": "0 30 9 * * *",
```

Bei jeder Wochentag 9:30 Uhr ausgelöst wird:

```json
"schedule": "0 30 9 * * 1-5",
```

## <a name="timer-trigger-c-code-example"></a>Beispiel für einen Zeitgeber Trigger C#-code

In diesem C#-Code-Beispiel schreibt ein einzelnes Protokoll jedes Mal, wenn die Funktion ausgelöst wird.

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");    
}
```

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
