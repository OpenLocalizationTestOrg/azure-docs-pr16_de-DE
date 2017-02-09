<properties
   pageTitle="Logik apps als aufgerufen Endpunkte"
   description="So erstellen und Konfigurieren von Endpunkten auslösen und deren Verwendung in einer app Logik in Azure-App-Verwaltungsdienst"
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


# <a name="logic-apps-as-callable-endpoints"></a>Logik apps als aufgerufen Endpunkte

Logik Apps kann systembedingt einen synchronen HTTP-Endpunkt als Trigger verfügbar machen.  Das Serienmuster aufgerufen Endpunkte können Sie auch Logik Apps als geschachtelte Workflow durch die Aktion "Workflow" in einer App Logik aufzurufen.

Es gibt 3 Arten von Triggern, die Anfragen empfangen werden können:

* Anfordern
* ApiConnectionWebhook
* HttpWebhook

Für den Rest dieses Artikels wir verwenden **Anforderung** aus, wie im Beispiel, jedoch alle der Grundsätze identisch auf die anderen 2 Typen von Triggern anwenden.

## <a name="adding-a-trigger-to-your-definition"></a>Der Definition hinzufügen ein Triggers
Dieser erste Schritt besteht ein Triggers in der Definition Ihrer Logik app hinzufügen, die eingehende Anfragen empfangen können.  Sie können im Designer für "HTTP anfordern", um die Karte Trigger hinzuzufügen suchen. Sie können eine Anforderungstexts JSON-Schema definieren, und der Designer Token, um Ihnen zu analysieren und übergeben von Daten aus der manuellen Trigger durch den Workflow zu generieren.  Es wird empfohlen, mithilfe von Tools wie [jsonschema.net](http://jsonschema.net) Generieren eines JSON-Schemas aus einer Stichprobe Textkörper Nutzlast.

![Anfordern der Trigger Karte][2]

Nachdem Sie Ihre App Logik Definition zu speichern, wird eine Rückruf-URL wie dieses Beispiel generiert werden:
 
``` text
https://prod-03.eastus.logic.azure.com:443/workflows/080cb66c52ea4e9cabe0abf4e197deff/triggers/myendpointtrigger?*signature*...
```

Diese URL enthält eine SAS-Taste in die Abfrageparameter für die Authentifizierung verwendet.

Sie können auch diesen Endpunkt Azure-Portal erhalten:

![][1]

Oder durch Einwahl:

``` text
POST https://management.azure.com/{resourceID of your logic app}/triggers/myendpointtrigger/listCallbackURL?api-version=2015-08-01-preview
```

### <a name="security-for-the-trigger-url"></a>Sicherheit für die Trigger-URL

Logik App Rückruf URLs sind sicher mit einer freigegebenen Access-Signatur generiert.  Die Signatur wird als Abfrageparameter durch geleitet, und überprüft werden muss, bevor die app Logik ausgelöst wird.  Es wird durch eine eindeutige Kombination aus einen geheimen Schlüssel pro Logik app, den Triggernamen und die Durchführung des Vorgangs generiert.  Wenn jemand Zugriff auf den geheimen Logik app Schlüssel hat, möchten sie keine gültige Signatur generieren sein.

## <a name="calling-the-logic-app-triggers-endpoint"></a>Aufrufen der Logik app Trigger Endpunkt

Nachdem Sie den Endpunkt für Ihre Trigger erstellt haben, können Sie es über Auslösen eines `POST` auf die vollständige URL. Sie können im Textkörper zusätzliche Kopfzeilen und alle Inhalte einschließen.

Ist der Inhaltstyp `application/json` dann Bezug Eigenschaften von innerhalb der Anfrage werden kann. Andernfalls wird diese als einzelne Einheit binäre behandelt werden, die an andere APIs übergeben werden kann, aber kann nicht ohne Konvertieren des Inhalts innerhalb des Workflows verwiesen werden.  Angenommen, Sie übergeben `application/xml` Inhalte können `@xpath()` Ausführen einer Extraktion XPath- oder `@json()` Umwandlung von XML in JSON.  Weitere Informationen zum Arbeiten mit Inhalt Datentypen [werden können, finden Sie hier](app-service-logic-content-type.md)

Darüber hinaus können Sie ein JSON-Schema in der Definition angeben. Dadurch wird den Designer Token zu erzeugen, die Sie dann in Schritte übergeben können.  Beispielsweise die folgenden Stellen werden eine `title` und `name` Token im Designer zur Verfügung:

```
{
    "properties":{
        "title": {
            "type": "string"
        },
        "name": {
            "type": "string"
        }
    },
    "required": [
        "title",
        "name"
    ],
    "type": "object"
}
```

## <a name="referencing-the-content-of-the-incoming-request"></a>Bezüge auf den Inhalt der eingehenden Anforderung

Die `@triggerOutputs()` Funktion des Inhalts der eingehenden Anforderung ausgegeben wird. Sie möchten beispielsweise wie aussehen:

```
{
    "headers" : {
        "content-type" : "application/json"
    },
    "body" : {
        "myprop" : "a value"
    }
}
```

Sie können die `@triggerBody()` Kontextmenü für den Zugriff auf die `body` Eigenschaft speziell. 

## <a name="responding-to-the-request"></a>Antworten auf die Anfrage

Für einige Anfragen, die eine Logik app starten, sollten Sie mit der ein Teil des Inhalts der Anrufer beantworten. Es wird ein neuer Aktionstyp aufgerufen **Antwort** , die verwendet werden können, um den Statuscode, Text und Überschriften für Ihre Antwort zu erstellen. Beachten Sie, dass wenn kein Shape **Antwort** vorhanden ist, wird der app-Endpunkt Logik *sofort* Antworten mit **202 akzeptiert**.

![HTTP-Antwort Aktion][3]

``` json
"Response": {
            "conditions": [],
            "inputs": {
                "body": {
                    "name": "@{triggerBody()['name']}",
                    "title": "@{triggerBody()['title']}"
                },
                "headers": {
                    "content-type": "application/json"
                },
                "statusCode": 200
            },
            "type": "Response"
        }
```

Antworten stehen die folgenden:

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| statusCode | Die HTTP-Statuscode reagieren auf eingehende Anfrage. Es kann keinen gültigen Statuscode sein, der mit 2xx, 4xx oder 5xx beginnt. 3xx Statuscodes sind nicht zulässig. | 
| Textkörper | Ein Textkörperobjekt, das eine Zeichenfolge sein kann, ein JSON-Objekt oder sogar binäre Inhalte, die von einem vorherigen Schritt verwiesen wird. | 
| Kopfzeilen | Sie können eine beliebige Anzahl von Überschriften, die in die Antwort aufgenommen werden definieren. | 

Alle Schritte in der Logik app, die für die Antwort erforderlich sind, müssen innerhalb von *60 Sekunden* für die ursprüngliche Anforderung erhalten die Antwort **, wenn der Workflow als geschachtelte Logik App aufgerufen wird**ausführen. Wenn keine Antwortaktion erreicht ist innerhalb von 60 Sekunden und dann die eingehende Anforderung ab, und erhalten Sie Reaktionen **408 Client Timeout** HTTP.  Für geschachtelte Logik Apps, die übergeordnete, die Logik App warten, bis eine Antwort bis abgeschlossen ist, werden weiterhin dauert unabhängig von der Menge an.

## <a name="advanced-endpoint-configuration"></a>Erweiterte Endpunktkonfiguration

Logik apps über eine integrierte Unterstützung für den direkten Zugriff Endpunkt und verwenden Sie immer die `POST` Methode, um ein Ausführen der Logik app starten. Die **Zuhörer HTTP-** API unterstützte Plattformen für zuvor auch den URL-Segmenten und die HTTP-Methode ändern. Könnten Sie gerade richten Sie zusätzliche Sicherheit oder eine benutzerdefinierte Domäne durch Hinzufügen zum API app Host (die Web-app, die die app API gehostet) an. 

Diese Funktion ist durch **API Management**verfügbar:
* [Ändern Sie die Methode der Anfrage](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod)
* [Ändern Sie die URL-Segmenten der Anfrage](https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL)
* Einrichten von Domänen Management API auf der Registerkarte " **Konfigurieren** " in der klassischen Azure-portal
* Einrichten von Richtlinie zu prüfen, ob Standardauthentifizierung (**Link erforderlich**)

## <a name="summary-of-migration-from-2014-12-01-preview"></a>Zusammenfassung der Migration von 2014-12-01-Vorschau

|  2014-12-01-Vorschau | 2016-06-01 |
|---------------------|--------------------|
| Klicken Sie auf **HTTP-Zuhörer** API-App | Klicken Sie auf **Manuelle Trigger** (keine API app erforderlich) |
| HTTP-Zuhörer einrichten "*automatisch sendet Antwort*" | Eine **Antwort** Aktion aufzunehmen oder nicht in die Workflowdefinition |
| Konfigurieren von Standard- oder OAuth-Authentifizierung | per-API management |
| Konfigurieren von HTTP-Methode | über Management API |
| Konfigurieren der relativen Pfad | über API management |
| Über eingehenden Textkörper verweisen`@triggerOutputs().body.Content` | Bezug über`@triggerOutputs().body` |
| **Senden von HTTP-Antwort** Aktion auf dem HTTP-Zuhörer | Klicken Sie auf **Antworten auf HTTP-Anforderung** (keine API app erforderlich)


[1]: ./media/app-service-logic-http-endpoint/manualtriggerurl.png
[2]: ./media/app-service-logic-http-endpoint/manualtrigger.png
[3]: ./media/app-service-logic-http-endpoint/response.png
