<properties
   pageTitle="Verwenden von Azure mit Logik Apps | Microsoft Azure"
   description="Finden Sie unter Verwenden von Funktionen Azure mit Logik Apps"
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
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="using-azure-functions-with-logic-apps"></a>Verwenden von Azure mit Logik Apps

Sie können benutzerdefinierte Codeausschnitte c# oder node.js mithilfe von Azure-Funktionen aus innerhalb einer app Logik ausführen.  [Azure-Funktionen](../azure-functions/functions-overview.md) bietet in Microsoft Azure computing Server frei. Dies ist hilfreich für die folgenden Aufgaben ausführen:

* Erweiterte Formatierung oder zum Berechnen von Feldern in einer App Logik
* Durchführen von Berechnungen innerhalb eines Workflows
* Erweitern Sie die Funktionalität der Logik Apps mit Funktionen, die in c# oder node.js unterstützt werden

## <a name="create-a-function-for-logic-apps"></a>Erstellen Sie eine Funktion für Logik Apps

Es empfiehlt sich, dass Sie eine neue Funktion im Portal Azure-Funktionen mithilfe der **Generische Webhook - Knoten** oder **Webhook - generische C#-** Vorlagen erstellen. Diese automatisch eingefügt eine Vorlage, die akzeptiert `application/json` aus einer app Logik.  Wenn Sie die Registerkarte **integrieren** in Azure Funktionen auswählen sollte sie **Modus** **Webhook** und **Webhook Typ** von **Generische JSON**festgelegt haben.  Funktionen, die in diesen Vorlagen verwendet werden automatisch erkannt und aufgeführt, die im Designer Logik Apps unter **Azure-Funktionen in meiner Region.**

Webhook Funktionen annehmen einer und übergeben es an die Methode über eine `data` Variable. Sie können die Eigenschaften eines Ihrer Nutzlast zugreifen, indem Sie mit Punktnotation wie `data.foo`.  Im folgenden Beispiel wird beispielsweise sieht eine einfache JavaScript-Funktion, die einen DateTime-Wert in einer Datumszeichenfolge konvertiert:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-a-logic-app"></a>Rufen Sie eine app Logik Azure-Funktionen

Wenn Sie auf das Menü **Aktionen** klicken, können Sie im Designer **Azure-Funktionen in meiner Region**auswählen.  Listet die Container im Rahmen Ihres Abonnements, und ermöglicht es Ihnen, wählen Sie die Funktion, die Sie anrufen möchten.  

Nachdem Sie die Funktion ausgewählt haben, werden Sie aufgefordert, ein Objekt Eingabewerte Nutzlast angeben. Dies ist die Nachricht, die die Logik app für die Funktion sendet, und es muss ein JSON-Objekt. Wenn Sie das Datum der **Letzten Änderung** von einem Trigger Vertrieb übergeben möchten, könnte die Funktion Nutzlast beispielsweise wie folgt aussehen:

![Datum der letzten-Domain][1]

## <a name="trigger-logic-apps-from-a-function"></a>Auslösen Logik apps aus einer Funktion

Es ist auch möglich, eine app Logik innerhalb einer Funktion auslösen.  Dazu erstellen Sie einfach eine Logik app mit einem manuellen auslösen. Weitere Informationen finden Sie unter [Logik apps als aufgerufen Endpunkte](app-service-logic-http-endpoint.md).  Klicken Sie dann in der Funktion generieren Sie HTTP POST für die manuelle Trigger URL mit der Nutzlast, die Sie mit der Logik senden möchten.

### <a name="create-a-function-from-the-designer"></a>Erstellen Sie eine Funktion aus dem designer

Sie können auch eine Funktion Webhook node.js aus innerhalb des Designers erstellen. Zunächst **Azure-Funktionen in meiner Region** und dann Auswählen eines Containers für Ihre (Funktion).  Wenn Sie noch nicht über einen Container verfügen, müssen Sie einen vom [Azure Funktionen Portal](https://functions.azure.com/signin)erstellen. Wählen Sie dann **Neu erstellen**.  

Zum Generieren einer Vorlage auf der Grundlage der Daten, die Sie berechnen möchten, geben Sie das Kontextobjekt, das Sie an eine Funktion übergeben möchten. Dies muss ein JSON-Objekt. Wenn Sie in der Dateiinhalt aus einer FTP-Aktion übergeben, sieht die Nutzlast Kontext beispielsweise wie folgt aus:

![Kontext Nutzlast][2]

>[AZURE.NOTE] Da dieses Objekt als Zeichenfolge umgewandelt wurde nicht, wird der Inhalt direkt an die JSON-Nutzlast hinzugefügt werden. Allerdings wird dadurch Fehler zurück, wenn es sich nicht um ein JSON-Token (d. h., eine Zeichenfolge oder eine JSON Object-Array) ist. Wenn Sie es als Zeichenfolge umwandeln, fügen Sie einfach Angebote, wie in der ersten Abbildung in diesem Artikel gezeigt.

Der Designer generiert dann eine Vorlage (Funktion), dass Sie Inline erstellen können. Variablen werden basierend auf dem Kontext vorab erstellt, die Sie in der Funktion übergeben möchten.




<!--Image references-->
[1]: ./media/app-service-logic-azure-functions/callFunction.png
[2]: ./media/app-service-logic-azure-functions/createFunction.png
