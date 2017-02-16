<properties 
    pageTitle="Azure-API Management Richtlinie Bezug" 
    description="Informationen Sie zu den Richtlinien zur Verfügung-API Management konfigurieren." 
    services="api-management" 
    documentationCenter="" 
    authors="vladvino" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="apimpm"/>

# <a name="azure-api-management-policy-reference"></a>Azure-API Management Richtlinie Bezug

Dieser Abschnitt enthält einen Index für die Richtlinien für [API Management Richtlinie Bezug][]. Informationen über das Hinzufügen und Konfigurieren von Richtlinien finden Sie unter [Richtlinien bei API-Verwaltung][].

Richtlinienausdrücke können als Attributwerte oder Textwerte in keines der API Informationsverwaltungsrichtlinien verwendet werden, die Richtlinie nicht anders angegeben. Einige Richtlinien, wie etwa die Richtlinien [Fluss Steuerelements][] , und [Legen Sie die Variable][] basieren auf Richtlinienausdrücke. Weitere Informationen finden Sie unter [Erweiterte Richtlinien][] und [Richtlinienausdrücke][]

## <a name="policy-reference-index"></a>Richtlinie Bezug index

-   [Einschränkung-Richtlinien][]
    -   [Aktivieren Sie HTTP-Header][] - erzwingt Vorhandensein und/oder den Wert für einen HTTP-Header.
    -   [Grenzwert Anruf Zins durch Abonnement][] - API wird verhindert, dass Verwendung, indem Sie auf Basis pro Abonnement Anruf Rate Spitzen.
    -   [Grenzwert für Anruf Zins von Key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - API wird verhindert, dass Verwendung, indem Sie Anruf Rate auf Basis pro Schlüssel Spitzen.
    -   [Einschränken Anrufer IP-Adressen][] - Filter verweigert (ermöglicht /) Anrufe von bestimmter IP-Adressen und/oder Adressbereiche.
    -   [Einsatzkontingent ein Abonnement festlegen][] –, können Sie ein erneuert oder Lebensdauer Anruf Lautstärke und/oder Bandbreite Kontingent, auf Basis pro Abonnement erzwingen.
    -   [Festlegen von Key einsatzkontingent](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) -, können Sie ein erneuert oder Lebensdauer Anruf Lautstärke und/oder Bandbreite Kontingent, auf Basis pro Schlüssel erzwingen.
    -   [Überprüfen Sie die JWT][] - erzwingt Vorhandensein und die Gültigkeit einer JWT extrahiert aus einer angegebenen HTTP-Kopfzeile oder einen angegebenen Abfrageparameter.
-   [Erweiterte Richtlinien][]
    -   [Fluss Steuerelements][] - bedingt gilt für Richtlinie Anweisungen basierend auf den Ergebnissen der Auswertung von booleschen [Ausdrücken][]aus.
    -   [Anfragen weiterleiten][] – leitet die Anforderung an den Back-End-Dienst.
    -   [Log an Ereignis Verteiler][] - sendet Nachrichten im angegebenen Format in einer Nachricht als Ziel durch eine [Protokollierung](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) Entität definiert.
    -   [Wiederholen](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - Wiederholungsversuche Ausführung Anweisungen eingeschlossene Richtlinie, wenn, und die Bedingung erfüllt ist. Die Ausführung wird in den festgelegten Zeitabständen wiederholen und auf die angegebene Anzahl der Wiederholungsversuche.
    -   [Antwort zurückgegeben](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) - Abbruch Verkaufspipeline Ausführung und gibt die angegebene Antwort direkt an den Anrufer.
    -   [Senden Sie eine Möglichkeit Anfrage](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) - sendet eine Anforderung an die angegebene URL ohne warten auf Antwort.
    -   [Anfrage senden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) – sendet eine Anforderung an den angegebenen URL.
    -   [Set-Anforderungsmethode](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - können Sie die HTTP-Methode für eine Anforderung ändern.
    -   [Festlegen der Status](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) - ändert den HTTP-Code, der auf den angegebenen Wert.
    -   [Legen Sie Variable][] - einen Wert in einer Variablen benannten [Kontext][] für den späteren Zugriff beizubehalten.
    -   [Spur](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - Fügt eine Zeichenfolge in der Ausgabe [API Inspektor](../api-management/api-management-howto-api-inspector.md) an.
    -   [Warten Sie](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - wartet eingeschlossene senden, Get-Wert von Cache und Steuerelement Fluss Richtlinien zum Anforderung abgeschlossen sein, bevor Sie fortfahren.
-   [Authentifizierungsrichtlinien][]
    -   [Authentifizieren Basic][] - authentifizieren mit einem Back-End-Dienst mithilfe der Standardauthentifizierung.
    -   [Authentifizieren mit Client-Zertifikat][] - authentifizieren mit einem Back-End-Dienst Client-Zertifikate.
-   [Zwischenspeichern von Richtlinien][] 
    -   [Abrufen von Cache][] - Cache ausführen nachschlagen und eine gültige zwischengespeicherte Antwort, wenn verfügbar zurückgibt.
    -   [Store-Cache][] - Caches Antwort entsprechend der angegebenen Cache Steuerelementkonfiguration.
    -   [Abrufen der Wert aus dem Cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) – Abrufen eines zwischengespeicherten Elements von Key.
    -   [Store Wert im Cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - Speicher ein Element im Cache von Key.
    -   [Entfernen der Wert aus dem Cache](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - ein Element im Cache durch Schlüssel entfernen.
-   [Cross Domänenrichtlinien][] 
    -   [Zulassen Domain-übergreifende Anrufe][] – erleichtern den Zugriff auf die API von Adobe Flash und Microsoft Silverlight-Browser-basierte Clients.
    -   [CORS][] - addiert Cross-Origin Ressource freigeben (CORS) unterstützt ein Vorgang oder eine API Domain-übergreifende Anrufe von Clients browserbasierte dürfen.
    -   [JSONP][] - addiert JSON mit Textabstand (JSONP) Unterstützung für einen Vorgang oder eine API Domain-übergreifende Anrufe von JavaScript browserbasierte Clients zulassen.
-   [Transformation Richtlinien][] 
    -   [Konvertieren von JSON und XML][] - konvertiert Anforderung oder Antwort Textkörper von JSON in XML.
    -   [Konvertieren von XML in JSON][] - konvertiert Anforderung oder Antwort Textkörper aus XML-JSON.
    -   [Suchen und ersetzen die Zeichenfolge in Text][] - findet eine Anforderung oder Antwort Teilzeichenfolge und durch eine andere Teilzeichenfolge ersetzt.
    -   [Eingabeformat-URLs im Inhalt][] - erneut schreibt (Masken) Links in der Antwort Textkörper, damit sie auf die entsprechende Verknüpfung über das Gateway zeigen.
    -   [Legen Sie die Back-End-Service][] - ändert sich den Back-End-Dienst für eine eingehende Anforderung aus.
    -   [Festlegen der Textkörper][] - legt den Nachrichtentext für eingehende und ausgehende Anfragen.
    -   [Festlegen von HTTP-Header][] - weist einen Wert in eine vorhandene Antwort und/oder Anforderung Kopf- oder fügt einen neuen Antwort und/oder Anforderung Header.
    -   [Festlegen der Abfragezeichenfolge-Parameter][] - fügt hinzu, ersetzt der Wert oder löscht Abfrageparameter anfordern.
    -   [Schreiben Sie URL][] - konvertiert eine Anfrage-URL aus ihrer öffentlichen Form auf das Formular, das durch den Webdienst erwartet.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Ausdrücken Richtlinie finden Sie im folgende Video.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Einschränkung-Richtlinien]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Überprüfen Sie die HTTP-Kopfzeile]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Grenzwert für Anruf Zins durch Abonnement]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Einschränken der Anrufer IP-Adressen]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Festlegen von einsatzkontingent durch Abonnement]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Überprüfen Sie die JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Erweiterte Richtlinien]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Steuerelement Fluss]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Festlegen einer Variablen]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Ausdrücke]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[Kontextmenü]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Anfragen weiterleiten]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Log an Ereignis Verteiler]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Authentifizierungsrichtlinien]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Basic authentifizieren]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Authentifizieren mit Client-Zertifikat]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Zwischenspeichern von Richtlinien]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Aus Cache abrufen]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Cache speichern]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross Domänenrichtlinien]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Domain-übergreifende Anrufe zulassen]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformation Richtlinien]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Konvertieren von JSON in XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Konvertieren von XML in JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Suchen Sie und Ersetzen Sie die Zeichenfolge in Text]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Eingabeformat-URLs im Inhalt]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Festlegen Back-End-Dienst]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Festlegen von Text]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Festlegen von HTTP-header]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Abfrageparameter festlegen]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Schreiben Sie URL]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Richtlinien bei API-Verwaltung]: api-management-howto-policies.md
[Projektmanagement-API Richtlinie Bezug]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Richtlinienausdrücke]: https://msdn.microsoft.com/library/azure/dn910913.aspx

 
