<properties 
    pageTitle="Zum Hinzufügen von Vorgängen zu einer API in Azure-API Management | Microsoft Azure" 
    description="Informationen Sie zum Vorgänge eine API in Azure-API Management hinzufügen." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Zum Hinzufügen von Vorgängen an eine API in Azure-API Management

Bevor eine API in API Management verwendet werden kann, müssen die Vorgänge hinzugefügt werden. Mit diesem Leitfaden veranschaulicht zum Hinzufügen und Konfigurieren der verschiedene Arten von Vorgängen an eine API-API Management.

## <a name="add-operation"> </a>Hinzufügen ein Vorgangs

Vorgänge werden hinzugefügt und so konfiguriert, dass eine API im Portal Publisher. Wenn Sie das Publisher-Portal zugreifen zu können, klicken Sie auf **Verwalten** im klassischen Azure-Portal für Ihre API Verwaltungsdienst.

![Publisher-portal][api-management-management-console]

>Wenn Sie eine Instanz der API Management-Dienst noch nicht erstellt haben, finden Sie unter [Erstellen einer API Management Service-Instanz][] , in die [Erste Schritte mit Azure-API Management][] Lernprogramm.

Wählen Sie die gewünschten-API im Publisher-Portal aus, und wählen Sie dann auf die Registerkarte **Vorgänge** . 

![Vorgänge][api-management-operations]

Klicken Sie auf **Vorgang hinzufügen** , um einen neuen Vorgang hinzufügen. Die **neuen Vorgang** wird angezeigt, und die Registerkarte **Signatur** wird standardmäßig aktiviert werden.

![Vorgang hinzufügen][api-management-add-operation]

Geben Sie das **http-Verb** durch auswählen aus der Dropdownliste aus.

![HTTP-Methode][api-management-http-method]

<a name="url-template"></a>

Definieren Sie die URL-Vorlage durch Eingabe in ein URL-Fragment aus einem oder mehreren Segmenten der URL-Pfad und NULL oder mehr Zeichenfolge Abfrageparameter. Die URL-Vorlage, auf der Basis der API-URL angefügt identifiziert einen einzigen HTTP-Vorgang. Er enthält oder benannte Variable Teile, die geschweiften Klammern identifiziert werden. Diese Variable Teile werden Vorlagenparameter bezeichnet, und es werden die Werte aus der URL der Anforderung extrahiert wurden, bei der Verarbeitung der Anforderung durch die API Management-Plattform dynamisch zugewiesen.

![URL-Vorlage][api-management-url-template]

<a name="rewrite-url-template"></a>

Geben Sie bei Bedarf die **Schreiben Sie URL-Vorlage**. So können Sie die Standardvorlage URL für die Verarbeitung von eingehender Anfragen auf der Front-End-, während Sie die Back-End über eine konvertierte URL entsprechend der erneuten Vorlage verwenden. In der Vorlage zum erneuten Schreiben von sollte Vorlagenparameter aus der URL-Vorlage verwendet werden. Im folgenden Beispiel wird gezeigt, wie Inhaltstyp codierte als Pfadsegment im Webdienst aus dem vorherigen Beispiel als Abfrageparameter in der API veröffentlicht über die API Management-Plattform, die unter Verwendung der URL-Vorlagen bereitgestellt werden kann.

![URL-Vorlage zum erneuten Schreiben von][api-management-url-template-rewrite]

Verwenden Sie Anrufer des Vorgangs werden das Format `/customers?customerid=ALFKI` und dies wird zugeordnet werden `/Customers('ALFKI')` Wenn Back-End-Dienst aufgerufen wird.


**Anzeigenamen** und **Beschreibung** Geben Sie eine Beschreibung des Vorgangs und werden verwendet, um den Entwicklern mit dieser API in der Entwicklerportal Dokumentation zur Verfügung.

![Beschreibung][api-management-description]

Die Beschreibung des Vorgangs kann als nur-Text- oder HTML-Code in das Textfeld **Beschreibung** angegeben werden.

## <a name="operation-caching"> </a>Vorgang Zwischenspeichern.

Antwort zwischenspeichern verringert Wartezeit vom API Nutzer, geringere Bandbreitenverbrauch und verringert die laden im Web HTTP-service Implementieren der-API angesehen. 

Wenn Sie schnell und einfach für den Vorgang Zwischenspeichern aktivieren, wählen Sie die Registerkarte **Zwischenspeichern** und aktivieren Sie das Kontrollkästchen **Aktivieren** .

![Zwischenspeichern][api-management-caching-tab]

**Dauer** gibt den Zeitraum an, bei dem die Antwort auf einen Vorgang im Cache bleibt. Der Standardwert ist 3.600 Sekunden oder 1 Stunde.

Cacheschlüssel werden verwendet, um zwischen Antworten zu unterscheiden, damit die Antwort, die jeder anderen Cacheschlüssel entspricht einen eigenen separaten Cache Wert zurückgegeben wird. Geben Sie optional bestimmte Zeichenfolge Abfrageparameter und/oder HTTP-Header im Cache Schlüsselwerte in die Textfelder **Unterscheidung nach Parameter der Abfragezeichenfolge** und **Unterscheidung nach Überschriften** Hilfethemas computing verwendet werden. Wenn keiner der angegebenen, vollständige Anforderung URL ist und die folgenden HTTP-Headerwerte werden in Cache Key Generation verwendet: **akzeptieren** und **Akzeptieren-Zeichensatz**.

>Weitere Informationen zum Zwischenspeichern im und Richtlinien Zwischenspeichern Informationen Sie [zum Zwischenspeichern von Ergebnissen in Azure-API Management Vorgang][].


## <a name="request-parameters"> </a>Parameter anfordern

Klicken Sie auf die Registerkarte Parameter werden Vorgangsparameter verwaltet. Parameter in der **URL-Vorlage** auf der Registerkarte **Signatur** angegeben werden automatisch hinzugefügt und können nur durch Bearbeiten der URL-Vorlage geändert werden. Zusätzliche Parameter können manuell eingegeben werden.

Zum Hinzufügen eines neuen Abfrageparameter **Abfrageparameter hinzufügen,** klicken Sie auf, und geben Sie die folgenden Informationen ein:

-   **Name** - Parametername.
-   **Beschreibung** - eine kurze Beschreibung des Parameters (optional).
-   **Typ** - Parametertyp, ausgewählt in der Dropdownliste nach unten.
-   **Werte** - Werte, die für diesen Parameter zugeordnet werden kann. Einer der Werte kann als Standard (optional) gekennzeichnet werden.
-   **Erforderlich** - Parameters durch Aktivieren des Kontrollkästchens notwendig machen. 

![Anfordern von Parametern][api-management-request-parameters]

## <a name="request-body"> </a>Textkörper anfordern

Wenn der Vorgang zulässt (z. B. sich, Beitrag) und erfordert eine Stelle können Sie ein Beispiel, in der alle der Darstellung der unterstützten Formate (z. B. Json, XML) bereitstellen. 

>Die Anforderungstexts ist nur für Dokumentation verwendet und ist nicht überprüft.

Wenn eine Anforderungstexts eingeben möchten, wechseln Sie zur Registerkarte **Text** .

Klicken Sie auf **Darstellung hinzufügen**, eingeben Sie des gewünschten Inhaltstypnamen (z. B. Anwendung/Json), wählen sie in der Dropdown-Liste, und fügen Sie das gewünschte Anforderung Textkörper-Beispiel in das ausgewählte Format, in das Textfeld ein. 

![Anforderungstexts][api-management-request-body]

In können zusätzlich zu Darstellungen, Sie auch eine optionale Beschreibung in das Textfeld **Beschreibung** angeben.

## <a name="responses"> </a>Antworten

Es empfiehlt sich Beispiele Antworten für alle Statuscodes bereitstellen, die die Operation führen kann. Jeder Statuscode möglicherweise mehr als eine Antwort Textkörper Beispiel eine für jede der unterstützten Inhaltstypen. 

Um eine Antwort hinzuzufügen, klicken Sie auf **Hinzufügen** , und starten Sie den gewünschten Statuscode eingeben. In diesem Beispiel wird der Statuscode **200 OK**. Nachdem Sie der Code in der Dropdown-Liste angezeigt wird, wählen Sie ihn aus, und der Antwortcode erstellt und für Ihr Geschäft hinzugefügt.

![Antwortcode][api-management-response-code]

Klicken Sie auf **Darstellung hinzufügen**, starten Sie den Namen der gewünschten Inhaltstyp (z. B. Anwendung/Json) eingeben und aus, die in der Dropdownliste nach unten.

![Textkörper Inhaltstyp][api-management-response-body-content-type]

Fügen Sie im Textkörper-Beispiel Antwort im ausgewählten Format in das Textfeld ein. 

![Antwort Textkörper][api-management-response-body]

Falls gewünscht, fügen Sie in das Textfeld **Beschreibung** eine optionale Beschreibung hinzu.

Nachdem der Vorgang konfiguriert ist, klicken Sie auf **Speichern**.


## <a name="next-steps"> </a>Nächste Schritte

Nachdem die Vorgänge an eine API hinzugefügt haben, besteht der nächste Schritt zum Zuordnen der API mit einem Produkt und veröffentlichen, sodass Entwickler Vorgänge aufrufen können.

-   [So erstellen und Veröffentlichen eines Produkts][]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Erste Schritte mit Azure-API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management-Dienst]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[So erstellen und Veröffentlichen eines Produkts]: api-management-howto-add-products.md
[Zum Zwischenspeichern von Ergebnissen des Vorgangs in Azure-API Management]: api-management-howto-cache.md