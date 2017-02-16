<properties
    pageTitle="Verwenden von Verwaltungsdienst-API zum Generieren von HTTP-Anfragen"
    description="Erfahren Sie, Anfrage und Antwort Richtlinien bei API-Verwaltung, mit externe Diensten aus Ihrer API aufzurufen"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="using-external-services-from-the-azure-api-management-service"></a>Verwenden von externen Diensten vom Verwaltungsdienst Azure-API

Die Richtlinien in Azure-API Verwaltungsdienst verfügbar, die eine Vielzahl von nützliche Arbeit Grundlage rein eingehende Anfrage, die ausgehende Antwort und grundlegende Konfigurationsinformationen ausführen können. Richtlinien wird Interaktion mit externen Diensten aus der API Verwaltung von vielen weitere Verkaufschancen geöffnet.

Wir haben bereits gesehen, wie wir mit dem [Ereignis-Hub Azure Service für Protokollierung, Überwachung und Analytics](api-management-log-to-eventhub-sample.md)interagieren können. In diesem Artikel werden wir, dass die Richtlinien, die für die Interaktion mit einem beliebigen externen HTTP ermöglichen Service-basierte vorführen. Diese Richtlinien können verwendet werden, für das Auslösen der remote Ereignisse oder zum Abrufen von Informationen, die zum Bearbeiten der ursprünglichen Anforderung und Antwort in irgendeiner Weise verwendet werden.

## <a name="send-one-way-request"></a>Senden eine Möglichkeit anfordern
Möglicherweise die einfachste externe Interaktion wird der Stil auslösen und vergessen, der Anfrage, der einen externen Dienst einige Arten von wichtiges Ereignis benachrichtigt werden sollen. Wir können die Kontrolle Fluss Richtlinie `choose` um jede Art von Bedingung zu erkennen, dass wir interessiert sind, und klicken Sie dann, wenn die Bedingung erfüllt ist, wir umso einer externen HTTP-Anforderung mithilfe der Richtlinie [senden eine Möglichkeit anfordern](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) . Dies kann eine Anforderung an eine Nachrichtensystems wie Hipchat oder Pufferzeit oder eine e-Mail-API wie SendGrid oder MailChimp, oder für kritische Support-Fälle wie im folgenden PagerDuty. Alle diese messaging-Systemen haben einfachen HTTP-APIs, die wir einfach aufgerufen werden können.

### <a name="alerting-with-slack"></a>Benachrichtigung mit Pufferzeit
Im folgenden Beispiel wird veranschaulicht, wie eine Nachricht zu einem Chatroom Pufferzeit zu senden, ist die Zahl größer als oder gleich 500. Ein 500-Bereichsfehler weist darauf hin ein Problem mit unseren Back-End-API, die der Client unsere API selbst auflösen können. Es ist in der Regel eine Art von Eingriff Ihrerseits erforderlich.  

    <choose>
        <when condition="@(context.Response.StatusCode >= 500)">
          <send-one-way-request mode="new">
            <set-url>https://hooks.slack.com/services/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</set-url>
            <set-method>POST</set-method>
            <set-body>@{
                    return new JObject(
                            new JProperty("username","APIM Alert"),
                            new JProperty("icon_emoji", ":ghost:"),
                            new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                    context.Request.Method,
                                                    context.Request.Url.Path + context.Request.Url.QueryString,
                                                    context.Request.Url.Host,
                                                    context.Response.StatusCode,
                                                    context.Response.StatusReason,
                                                    context.User.Email
                                                    ))
                            ).ToString();
                }</set-body>
          </send-one-way-request>
        </when>
    </choose>

Pufferzeit weist das Konzept eines eingehenden Web Haken. Wenn Sie ein Häkchen eingehende Web konfigurieren, generiert Pufferzeit eine spezielle URL dem können Sie eine einfache Beitrag Anforderung ausführen und eine Nachricht in den Pufferzeit Kanal zu übergeben. Der JSON-Text, den Sie erstellen basiert auf ein Format von Pufferzeit definiert.

![Pufferzeit Web Häkchen](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Ist Fire festgelegt und gut genug vergessen?
Es gibt bestimmte Kompromisse bei Verwendung der Formatvorlage auslösen und vergessen der Anforderung. Wenn aus irgendeinem Grund die Anforderung fehlschlägt, und klicken Sie dann der Fehler gemeldet werden. In diesem Fall bestimmten garantiert ist nicht die Komplexität der Probleme eines sekundären Fehlers reporting System und die zusätzliche Leistung soll-Kosten für die Antwort zu warten. Szenarien, in denen sie unbedingt die Antwort zu überprüfen, ist die Richtlinie [die Anforderung zum Senden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) einer besser geeignet.

## <a name="send-request"></a>Anfrage senden
Die `send-request` Richtlinie ermöglicht, die mit einem externen Dienst um komplexe Verarbeitungsfunktionen und zurückgeben, die Daten für die Verwaltung API-service für weitere Verarbeitung von Richtlinie verwendet werden können.

### <a name="authorizing-reference-tokens"></a>Verweis Token autorisieren
Haupt-Funktion der Management-API ist Back-End-Ressourcen schützen. Wenn der Autorisierung Server verwendet, die für Ihre API [JWT Token](http://jwt.io/) als Teil des zugehörigen Fluss OAuth2 erstellt wie [Azure Active Directory](../active-directory/active-directory-aadconnect.md) der Fall ist, können Sie verwenden die `validate-jwt` Richtlinie die Gültigkeit der Token überprüfen. Einige Server Autorisierung jedoch erstellen, sogenannten [Bezug Token](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) , die überprüft werden kann, ohne tätigen eines Anrufs wieder auf dem Server für die Autorisierung.

### <a name="standardized-introspection"></a>Standardisierte introspection
In der Vergangenheit wurde keine standardisierten Verfahren, um einen Bezug Token mit einem Server Autorisierung bestätigt. Jedoch eine zuletzt vorgeschlagenen standard [RFC 7662](https://tools.ietf.org/html/rfc7662) von der IETF veröffentlicht wurde, die definiert, wie ein Ressourcenserver die Gültigkeit einer Token überprüfen kann.

### <a name="extracting-the-token"></a>Extrahieren des Tokens
Der erste Schritt besteht in der Kopfzeile Autorisierung Token zu extrahieren. Der Kopfzeile Wert formatiert werden sollen, mit der `Bearer` Autorisierungsschema, ein einzelnes Leerzeichen, und klicken Sie dann auf die Autorisierungstoken gemäß [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Es gibt leider Fällen, wo die Autorisierung des Farbschemas ausgelassen wird. Um dies zu berücksichtigen beim Analysieren, wir teilen den Wert der Kopfzeile auf ein Leerzeichen und wählen Sie die letzte Zeichenfolge aus der gelieferten Matrix von Zeichenfolgen. Dies stellt das Problem zu umgehen, für die Autorisierung falsch formatierte Überschriften.

    <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

### <a name="making-the-validation-request"></a>Die Überprüfung Anforderung ausführenden
Wenn wir das Autorisierungstoken haben, können wir die Anforderung zur Überprüfung des Tokens vornehmen. RFC 7662 ruft dieser Prozess Introspection und erfordert, dass Sie `POST` ein HTML-Formular, die der Ressource Introspection. Das HTML-Formular muss mindestens ein Schlüssel/Wert-Paar enthalten, mit dem Schlüssel `token`. Diese Anforderung auf dem Server Autorisierung muss auch authentifiziert werden, um sicherzustellen, dass bösartiger Clients für gültige Token Schleppnetzfangs wechseln können nicht.

    <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
      <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
      <set-method>POST</set-method>
      <set-header name="Authorization" exists-action="override">
        <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
      </set-header>
      <set-header name="Content-Type" exists-action="override">
        <value>application/x-www-form-urlencoded</value>
      </set-header>
      <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
    </send-request>

### <a name="checking-the-response"></a>Aktivieren die Antwort
Die `response-variable-name` Attribut wird verwendet, um den Zugriff auf die zurückgegebene Antwort gewähren. Der in dieser Eigenschaft definierte Name kann verwendet werden, als Schlüssel in der `context.Variables` Wörterbuch für den Zugriff auf die `IResponse` Objekt.

Vom Antwortobjekt können wir den Textkörper abrufen und RFC 7622 können Sie feststellen, dass die Antwort muss ein JSON-Objekt muss mindestens eine Eigenschaft namens enthalten `active` d. h. boolesche Wert. Wenn `active` wahr ist, und klicken Sie dann das Token gültig ist.

### <a name="reporting-failure"></a>Fehler meldet
Wir verwenden eine `<choose>` Richtlinie erkennen, wenn das Token ungültig ist und eine 401 Antwort zurückgeben.

    <choose>
      <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
        <return-response response-variable-name="existing response variable">
          <set-status code="401" reason="Unauthorized" />
          <set-header name="WWW-Authenticate" exists-action="override">
            <value>Bearer error="invalid_token"</value>
          </set-header>
        </return-response>
      </when>
    </choose>

Gemäß [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) die beschreibt wie `bearer` Token verwendet werden sollte, wir auch Zurückgeben eines `WWW-Authenticate` Kopfzeile mit der 401 Antwort. "Www" authentifizieren soll anweisen, einen Client zum eine ordnungsgemäß autorisierte Anforderung erstellen. Aufgrund der Vielzahl von Ansätze mit OAuth2 Framework möglich ist es schwierig, alle erforderliche Informationen zu kommunizieren. Es besteht Glücklicherweise unter Umständen laufender [Clients erfahren Sie, wie Besprechungsanfragen auf einem Ressourcenserver ordnungsgemäß autorisiert](http://tools.ietf.org/html/draft-jones-oauth-discovery-00)helfen.

### <a name="final-solution"></a>Lösung abgeschlossen
Kombinieren alle der einzelnen Komponenten, erhalten wir die folgenden Richtlinien:

    <inbound>
      <!-- Extract Token from Authorization header parameter -->
      <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

      <!-- Send request to Token Server to validate token (see RFC 7662) -->
      <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
        <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
        <set-method>POST</set-method>
        <set-header name="Authorization" exists-action="override">
          <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
        </set-header>
        <set-header name="Content-Type" exists-action="override">
          <value>application/x-www-form-urlencoded</value>
        </set-header>
        <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
      </send-request>

      <choose>
            <!-- Check active property in response -->
            <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
                <!-- Return 401 Unauthorized with http-problem payload -->
                <return-response response-variable-name="existing response variable">
                    <set-status code="401" reason="Unauthorized" />
                    <set-header name="WWW-Authenticate" exists-action="override">
                        <value>Bearer error="invalid_token"</value>
                    </set-header>
                </return-response>
            </when>
        </choose>
      <base />
    </inbound>

Dies ist nur eines von vielen Beispielen gezeigt, wie die `send-request` Richtlinie kann verwendet werden, in den Prozess der Besprechungsanfragen und Antworten, bis der API Verwaltungsdienst parallelen nützliche externe Dienste integriert werden soll.

## <a name="response-composition"></a>Antwort Komposition
Die `send-request` Richtlinie kann verwendet werden, für die Anforderung einer primären zu einem Back-End-System zu verbessern, wie wir im vorherigen Beispiel gesehen haben, oder sie als eine vollständige ersetzen für die Back-End-Anrufe verwendet werden kann. Mit diesem Verfahren können wir einfach erstellen composite Ressourcen, die aggregiert werden von mehreren unterschiedlichen Systemen.

### <a name="building-a-dashboard"></a>Erstellen eines Dashboards   
Manchmal möchten in der Lage Informationen verfügbar machen, die in mehreren Back-End-Systemen, beispielsweise vorhanden ist, um ein Dashboard versorgen. KPIs aus allen anderen Back-End stammen, aber Sie bevorzugen nicht zum Bereitstellen von direkten Zugriffs auf diese und es wäre toll, wenn alle Informationen in einer Anforderung abgerufen werden konnte. Möglicherweise einige der Back-End-Informationen benötigen einige segmentieren und beim Aufteilen und etwas Bereinigen von zuerst! Die zusammengesetzte Ressource zwischengespeichert würde sein hilfreich, wenn Sie die Back-End-Last zu verringern, wie Sie wissen, dass Benutzer haben Gewohnheit hammering die Taste F5, um festzustellen, ob deren dieser Kriterien ändern können.    

### <a name="faking-the-resource"></a>Abfangen der Ressource
Dieser erste Schritt bis hin zum Erstellen von unserem Dashboard Ressource ist so konfigurieren Sie einen neuen Vorgang in Publisher-API Verwaltungsportal. Dies ist eine Platzhalter Vorgang verwendet, um unser Komposition Richtlinie unsere dynamische Ressource erstellen konfigurieren werden.

![Dashboard-Vorgang](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>Ausführen einer der Anforderung
Nachdem die `dashboard` Vorgang erstellt wurde können wir eine Richtlinie speziell für diesen Vorgang konfigurieren. 

![Dashboard-Vorgang](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

Dieser erste Schritt besteht, alle Abfrageparameter aus der eingehenden Anforderung extrahieren, damit wir zur unsere Back-End-Weiterleitung können. In diesem Beispiel unsere Dashboard Informationen basierend auf einer bestimmten Zeitspanne angezeigt wird eine daher weist eine `fromDate` und `toDate` Parameter. Wir können die `set-variable` Richtlinie die Informationen aus der Anforderung URL zu extrahieren.

    <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
    <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

Nachdem wir haben, dass diese Informationen können wir Anfragen an allen Back-End-Systemen vornehmen. Jede Anforderung erstellt eine neue URL mit den Parameterinformationen und den eigenen Server ruft und speichert die Antwort in einer Variablen Kontext.

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

Diese Anfragen werden in der Reihenfolge ausgeführt, die nicht ideal ist. Eine bevorstehende Version wir werden werden Einführung in eine neue Richtlinie mit `wait` wird, die alle diese Anfragen parallel ausführen aktivieren.

### <a name="responding"></a>Reagiert

Um die zusammengesetzte Antwort zu generieren, können wir die [zurück-Antwort](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) Richtlinie verwenden. Die `set-body` Element einen Ausdruck verwenden kann, eine neue `JObject` mit allen als Eigenschaften eingebetteten Komponente Darstellungen.

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>

Die vollständige Richtlinie sieht wie folgt aus:

    <policies>
        <inbound>

      <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
      <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

        <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
          <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
          <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <return-response response-variable-name="existing response variable">
          <set-status code="200" reason="OK" />
          <set-header name="Content-Type" exists-action="override">
            <value>application/json</value>
          </set-header>
          <set-body>
            @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                          new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                          new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                          new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                          ).ToString())
          </set-body>
        </return-response>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
    </policies>

In der Konfiguration des Platzhalters bedeutet Operation können wir die Dashboard-Ressource für mindestens eine Stunde zwischengespeichert werden sollen, da wir wissen, dass die Art der Daten konfigurieren, die auch ist eine Stunde veraltet, er ausreichend effektiver zu vermitteln wertvollen Informationen an die Benutzer weiterhin angezeigt wird.

## <a name="summary"></a>Zusammenfassung
Verwaltungsdienst Azure-API bietet flexible Richtlinien, die Selektives auf HTTP-Datenverkehr angewendet werden können, und ermöglicht Zusammenstellung der Back-End-Dienste. Gibt an, ob Sie Optimieren Ihrer API Gateway mit Funktionen, die Überprüfung, Überprüfung Funktionen warnen, oder erstellen Sie neue zusammengesetzte Ressourcen auf Grundlage mehrerer Back-End-Dienste möchten die `send-request` und verwandte Richtlinien öffnen eine Welt der mögliche Werte.

## <a name="watch-a-video-overview-of-these-policies"></a>Schauen Sie sich einen Überblick über diese Richtlinien video
Weitere Informationen zum [senden eine Möglichkeit anfordern](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [Anfrage senden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest)und [zurück-Antwort](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) Richtlinien, die in diesem Artikel behandelt schauen Sie sich das folgende Video.

> [AZURE.VIDEO send-request-and-return-response-policies]
