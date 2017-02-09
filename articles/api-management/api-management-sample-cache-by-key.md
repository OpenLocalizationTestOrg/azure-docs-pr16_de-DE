<properties
    pageTitle="Benutzerdefinierte Zwischenspeichern in Azure-API Management"
    description="Informationen Sie zum Zwischenspeichern von Elementen durch Schlüssel in Azure-API Management"
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

# <a name="custom-caching-in-azure-api-management"></a>Benutzerdefinierte Zwischenspeichern in Azure-API Management
Azure-API Verwaltungsdienst weist integrierten Unterstützung für mit der URL für die Ressource als die Taste [HTTP-Antwort zwischenspeichern](api-management-howto-cache.md) . Die Taste geändert werden kann, durch Anforderungsheader mithilfe der `vary-by` Eigenschaften. Dies ist nützlich zum Zwischenspeichern von gesamten HTTP-Antworten (QuickInfos Darstellungen), aber manchmal ist es sinnvoll, nur Cache einen Teil einer Darstellung. Die neuen [Cache Suchkriterium](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) und [Cache Speicherwert](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) Richtlinien bieten die Möglichkeit zum Speichern und Abrufen von Daten aus Richtliniendefinitionen beliebiger Teile. Diese Möglichkeit bereichert auch die Richtlinie zuvor eingefügten [Senden anfordern](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) , da Sie jetzt Antworten von externen Diensten zwischengespeichert werden können.

## <a name="architecture"></a>Architektur  
Verwaltungsdienst-API verwendet einen freigegebenen pro Mandant Daten-Cache aus, damit mehrere Einheiten, die Sie Zugriff auf die gleiche weiterhin aufrufen können, wie Sie bis zu skalieren Daten zwischengespeichert. Bei der Arbeit mit einer Bereitstellung mit mehreren Region gibt es jedoch unabhängig Caches innerhalb jeder der Regionen. Aus diesem Grund ist es wichtig, nicht den Cache als einen Datenspeicher behandeln, wo es die einzige Quelle von irgendeine Information ist. Wenn, und Sie später zu nutzen der Bereitstellung mehrere Region entschieden, möglicherweise Kunden Reisen-Benutzer Zugriff auf die zwischengespeicherten Daten verlieren.

## <a name="fragment-caching"></a>Zwischenspeichern von Fragmenten
Es gibt bestimmte Fälle, in dem Antworten zurückgegeben wird einen Teil der Daten, die kostet enthalten ermitteln und bleibt noch für eine angemessenen Zeitspanne frisch. Ein Beispiel sollten Sie einen Dienst erstellt, indem eine Fluglinie, die Angaben Flight Reservierungen, Flight Status usw. bereitstellt. Wenn der Benutzer ein Mitglied des Programms Airlines Punkt ist, müssten sie auch Angaben über deren aktuellen Status und mit Gesamtzeit. Diese Benutzer-bezogene Informationen möglicherweise in einem anderen System gespeichert werden, aber es möglicherweise wünschenswert zum Einschließen in Antworten zu Flight Status und Reservierungen zurückgegeben. Dies kann unter Verwendung der sogenannten Fragment Zwischenspeichern. Die primäre Darstellung kann zurückgegeben werden, aus dem Ausgangsserver einige Arten von Token verwenden, um anzugeben, wo die Benutzer-bezogene Informationen ist, eingefügt werden soll. 

Berücksichtigen Sie die folgende JSON-Antwort aus einer Back-End-API.


    {
      "airline" : "Air Canada",
      "flightno" : "871",
      "status" : "ontime",
      "gate" : "B40",
      "terminal" : "2A",
      "userprofile" : "$userprofile$"
    }  

Und sekundäre Ressource am `/userprofile/{userid}` , folgendermaßen aussieht,

     { "username" : "Bob Smith", "Status" : "Gold" }

Bei der Feststellung die entsprechenden Benutzerinformationen aufnehmen möchten, müssen wir erkennen, wer den Endbenutzer ist. Dieses Verfahren wird Implementierung ab. Als Beispiel verwende ich die `Subject` beanspruchen von einer `JWT` token. 

    <set-variable
      name="enduserid"
      value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

Wir speichern diese `enduserid` Wert in einer Variablen Kontext zur späteren Verwendung. Im nächsten Schritt wird, um festzustellen, ob eine vorherige Anforderung bereits die Benutzerinformationen abgerufen und es im Cache gespeichert. Wir verwenden die `cache-lookup-value` Richtlinie.

      <cache-lookup-value
      key="@("userprofile-" + context.Variables["enduserid"])"
      variable-name="userprofile" />

Wenn kein Eintrag vorhanden, im Cache ist, die den Schlüsselwert, und klicken Sie dann auf kein entspricht `userprofile` Kontextvariable erstellt werden. Wir überprüfen den Erfolg des Nachschlage mithilfe der `choose` Fluss Richtlinie steuern.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("userprofile"))">
            <!— If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
        </when>
    </choose>


Wenn die `userprofile` Kontextvariable nicht vorhanden, und wir werden müssen, dass eine HTTP-Anforderung Abrufen dieser Daten sind.

    <send-request
      mode="new"
      response-variable-name="userprofileresponse"
      timeout="10"
      ignore-error="true">

      <!-- Build a URL that points to the profile for the current end-user -->
      <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
          (string)context.Variables["enduserid"]).AbsoluteUri)
      </set-url>
      <set-method>GET</set-method>
    </send-request>

Wir verwenden die `enduserid` auf die URL, die der Benutzer Profil Ressource zu erstellen. Wenn die Antwort vorliegt, können wir ziehen Sie den Textkörper aus der Antwort und wieder in einem Kontextvariable zu speichern.

    <set-variable
        name="userprofile"
        value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

Zur Vermeidung von uns müssen diese HTTP-Anforderung erneut stellen, wenn derselbe Benutzer eine andere anfordert, können wir das Benutzerprofil im Cache speichern.

    <cache-store-value
        key="@("userprofile-" + context.Variables["enduserid"])"
        value="@((string)context.Variables["userprofile"])" duration="100000" />

Wir speichern den Wert im Cache über die genauen desselben Schlüssels, den wir ursprünglich versucht, es abzurufen. Die Dauer, die wir zum Speichern des Werts auswählen sollten Grundlage wie oft die Änderungen Informationen und wie gegeben Benutzer Informationen zu veraltet sind. 

Es ist wichtig, zu verstehen ist immer noch eine Out-of-Process, Netzwerk Anforderung und potenziell kann weiterhin hinzufügen Dutzende von Millisekunden die Anforderung aus dem Cache abrufen. Die Vorteile kommen entscheiden, ob die Benutzerprofildaten erheblich länger als, die aufgrund der Notwendigkeit, Abfragen oder Aggregieren Daten aus mehreren Back-enden Database-dauert.

Der letzte Schritt im Prozess ist die zurückgegebene Antwort mit unseren Benutzerprofildaten aktualisiert.

    <!—Update response body with user profile-->
    <find-and-replace
        from='"$userprofile$"'
        to="@((string)context.Variables["userprofile"])" />

Ich habe die Anführungszeichen als Teil des Token aufnehmen möchten, damit auch, wenn das Ersetzen nicht auftritt, die Antwort noch gültig JSON wurde. Dies wurde hauptsächlich soll das Debuggen vereinfacht.

Nachdem Sie alle Schritte miteinander kombinieren, ist das Ergebnis einer Richtlinie, die aussieht, wie im folgenden.

     <policies>
        <inbound>
            <!-- How you determine user identity is application dependent -->
            <set-variable
              name="enduserid"
              value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

            <!--Look for userprofile for this user in the cache -->
            <cache-lookup-value
              key="@("userprofile-" + context.Variables["enduserid"])"
              variable-name="userprofile" />

            <!-- If we don’t find it in the cache, make a request for it and store it -->
            <choose>
                <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                    <!—Make HTTP request to get user profile -->
                    <send-request
                      mode="new"
                      response-variable-name="userprofileresponse"
                      timeout="10"
                      ignore-error="true">

                       <!-- Build a URL that points to the profile for the current end-user -->
                        <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                        <set-method>GET</set-method>
                    </send-request>

                    <!—Store response body in context variable -->
                    <set-variable
                      name="userprofile"
                      value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                    <!—Store result in cache -->
                    <cache-store-value
                      key="@("userprofile-" + context.Variables["enduserid"])"
                      value="@((string)context.Variables["userprofile"])"
                      duration="100000" />
                </when>
            </choose>
            <base />
        </inbound>
        <outbound>
            <!—Update response body with user profile-->
            <find-and-replace
                  from='"$userprofile$"'
                  to="@((string)context.Variables["userprofile"])" />
            <base />
        </outbound>
     </policies>

Dieser Ansatz Zwischenspeichern wird hauptsächlich in Websites, in dem HTML-Code auf dem Server besteht aus, damit diese als Einzelseite gerendert werden kann. Jedoch, sie können auch in nützlich sein, in dem nicht Clients Client, APIs seitliche HTTP Zwischenspeichern oder es empfiehlt sich nicht, um diese Aufgabe auf dem Client setzen.

Diese derselben Art von Zwischenspeichern von Fragmenten kann auch auf den Back-End-Webservern mit einer Redis Zwischenspeichern Server durchgeführt werden, jedoch mit der API Verwaltungsdienst zum Durchführen dieser Aufgaben ist nützlich, wenn die zwischengespeicherte Fragmente von anderen Back-endet als die primären Antworten stammen.

## <a name="transparent-versioning"></a>Transparente versioning
Es ist üblich für mehrere andere Implementierung Versionen einer API zu einem beliebigen Zeitpunkt unterstützt werden. Dies ist vielleicht zur Unterstützung von verschiedenen Umgebungen, wie Entwickler, testen, Fertigung, usw., oder es möglicherweise zur Unterstützung von älterer Versionen von der-API, um Zeit für API Nutzer auf neuere Versionen migrieren zu verleihen. 

Ein Verfahren zur Behandlung von dies statt mit Anforderung der Client-Entwickler so ändern Sie die URLs von `/v1/customers` zu `/v2/customers` besteht darin, die Consumer Profil Daten welche Version der API speichern sie aktuell verwenden, und rufen Sie die entsprechenden Back-End-URL möchten. Bei der Feststellung die richtigen Back-End-URL für einen bestimmten Client aufrufen, ist es erforderlich, vor einiger Konfigurationsdaten Abfragen. Durch Zwischenspeichern von dieser Konfigurationsdaten, können wir die Leistung beeinträchtigt, der den Nachschlagevorgang ausführen minimieren.

Der erste Schritt besteht die ID verwendet, um die gewünschte Version konfigurieren zu ermitteln. In diesem Beispiel habe ich die Version mit dem Abonnement Product Key zugeordnet werden soll. 

        <set-variable name="clientid" value="@(context.Subscription.Key)" />

Wir führen Sie dann eine Cache-Suche, um festzustellen, ob wir die gewünschten Client-Version bereits abgerufen haben.

        <cache-lookup-value
        key="@("clientversion-" + context.Variables["clientid"])"
        variable-name="clientversion" />

Klicken Sie dann geprüft, wenn es keine im Cache gefunden haben.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">

Wenn wir haben, und klicken Sie dann wechseln, und nehmen Sie ihn.

    <send-request
        mode="new"
        response-variable-name="clientconfiguresponse"
        timeout="10"
        ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
    </send-request>

Extrahieren Sie den Textkörper der Antwort aus der Antwort ein.

    <set-variable
          name="clientversion"
          value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />

Speichern Sie es in den Cache für eine zukünftige Verwendung.

    <cache-store-value
          key="@("clientversion-" + context.Variables["clientid"])"
          value="@((string)context.Variables["clientversion"])"
          duration="100000" />

Und schließlich aktualisieren die Back-End-URL, um die Version der vom Client gewünschten Dienst auswählen.

    <set-backend-service
          base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />

Die Richtlinie vollständig sieht wie folgt aus.

     <inbound>
        <base />
        <set-variable name="clientid" value="@(context.Subscription.Key)" />
        <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("clientversion"))">
                <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                    <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>
                <!-- Store response body in context variable -->
                <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
                <!-- Store result in cache -->
                <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
            </when>
        </choose>
        <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
    </inbound>


Aktivieren API Nutzer transparent zu steuern, welche Back-End-Version zugegriffen wird von Clients ohne zu aktualisieren und erneut bereitstellen Clients ist eine elegante Lösung, die viele API Versioning Bedenken Adressen.

## <a name="tenant-isolation"></a>Grad der Isolation Mandanten

In größeren, mit mehreren Mandanten Bereitstellungen erstellen einige Unternehmen eigene Gruppen von Mandanten auf distinct-Bereitstellungen mit Back-End-Hardware. Dadurch wird die Anzahl der Benutzer, die durch ein Hardwareproblem auf die Back-End-beeinträchtigt werden minimiert. Darüber hinaus können neue Softwareversionen in Phasen einführen. Idealerweise sollte diese Back-End-Architektur für API Nutzer transparent sein. Dies kann auf ähnliche Weise auf transparente Versionskontrolle fest erreicht werden, da sie auf das gleiche Verfahren für die Bearbeitung von die Back-End-URL mit Konfigurationszustand pro API Schlüssel basiert.  

Statt Zurückgeben einer bevorzugten Version der API für jedes Abonnementschlüssel, würde Sie einen Bezeichner zurückgeben, der in der Hardwaregruppe zugeordneten einen Mandanten verbunden ist. Dieser Bezeichner kann verwendet werden, in die entsprechenden Back-End-URL zu erstellen.

## <a name="summary"></a>Zusammenfassung
Der Freiheitsgrade im Cache Management Azure-API zum Speichern von beliebige Daten verwenden aktiviert Optimierung des Zugriffs auf Konfigurationsdaten, die die Anzeige eine eingehende Anforderung verarbeitet wird beeinflussen können. Sie können auch Daten Satzteile gespeichert, die Antworten, die aus einer Back-End-API zurückgegeben erweitern können verwendet werden.

## <a name="next-steps"></a>Nächste Schritte
Liefern Sie uns bitte Ihr Feedback im Thread Disqus für dieses Thema ist es gibt andere Szenarien, die folgenden Richtlinien für Sie aktiviert haben, oder wenn es Szenarien gibt möchte Sie zu erreichen, aber nicht den Eindruck sind derzeit möglich führen.
