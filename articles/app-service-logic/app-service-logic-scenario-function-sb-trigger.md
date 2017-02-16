<properties
   pageTitle="Logik app Szenario: erstellen einen Azure Funktionen Dienstbus Trigger | Microsoft Azure"
   description="Verwenden Sie zum Erstellen eines Triggers Dienstbus für eine app Logik Azure-Funktionen"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/23/2016"
   ms.author="jehollan"/>

# <a name="logic-app-scenario-create-an-azure-service-bus-trigger-by-using-azure-functions"></a>Logik app Szenario: erstellen einen Azure-Dienstbus Trigger mithilfe von Azure-Funktionen

Azure-Funktionen können Sie einen Trigger für eine app Logik erstellen, wenn Sie ein langer Zuhörer oder eine Aufgabe bereitstellen möchten. Beispielsweise können Sie eine Funktion erstellen, die eine Warteschlange über Verkaufsgespräche und dann sofort eine app Logik als Pushbenachrichtigungen Trigger ausgelöst werden.

## <a name="build-the-logic-app"></a>Erstellen Sie die app Logik

In diesem Beispiel müssen Sie eine Funktion ausführen für jede Logik-app, die ausgelöst werden muss. Erstellen Sie zuerst eine Logik app, die einen HTTP-Anforderung Trigger aufweist. Die Funktion ruft diesen Endpunkt aus, wenn eine Warteschlange-Nachricht empfangen wird.  

1. Erstellen Sie eine neue Logik app; Wählen Sie den Trigger **manuell – Wenn ein HTTP-Anforderung empfangen wird** .  
   Optional können Sie mit der Meldung Warteschlange mithilfe eines Tools wie [jsonschema.net](http://jsonschema.net)ein JSON-Schema angeben. Fügen Sie das Schema der auslösen. Dadurch wird des Designers die Form der Daten zu verstehen und weitere Datenfluss einfach Eigenschaften durch den Workflow.
1. Fügen Sie eine zusätzliche Schritte, die auftreten, nachdem eine Warteschlange-Nachricht empfangen werden soll. Senden Sie beispielsweise eine e-Mail über Office 365 an.  
1. Speichern Sie die app Logik, um den Rückruf-URL für den Trigger diese App Logik zu generieren. Die URL, die auf der Karte Trigger wird angezeigt.

![Der Rückruf wird URL auf der Karte trigger][1]

## <a name="build-the-function"></a>Erstellen Sie die Funktion

Als Nächstes müssen Sie eine Funktion zu erstellen, die als Trigger fungiert und der Warteschlange abhören.

1. Im [Portal Azure-Funktionen](https://functions.azure.com/signin)wählen Sie **Neue Funktion**, und wählen Sie dann die **ServiceBusQueueTrigger - C#-** Vorlage aus.

    ![Azure Funktionen-portal][2]

2. Konfigurieren Sie die Verbindung zu der Warteschlange Dienstbus (die wird der Azure Service Bus SDK verwendet `OnMessageReceive()` Zuhörer).
3. Schreiben Sie eine einfache Funktion, um den Logik app-Endpunkt (zuvor erstellt) mithilfe der Warteschlange Nachricht als Trigger anzurufen. Hier ist ein vollständiges Beispiel einer Funktion aus. Im Beispiel wird mit einer `application/json` Inhaltstyp der Nachricht, aber Sie können dies bei Bedarf ändern.

   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";

   public static void Run(string myQueueItem, TraceWriter log)
   {

       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Klicken Sie zum Testen fügen Sie Warteschlange Mitteilung über ein Tool wie [Service Bus Explorer hinzu](https://github.com/paolosalvatori/ServiceBusExplorer). Finden Sie unter der Logik app Auslösen sofort nach die Funktion die Nachricht empfängt.

<!-- Image References -->
[1]: ./media/app-service-logic-scenario-function-sb-trigger/manualTrigger.PNG
[2]: ./media/app-service-logic-scenario-function-sb-trigger/newQueueTriggerFunction.PNG
