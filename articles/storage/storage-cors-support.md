<properties
    pageTitle="Cross-Origin Ressource freigeben (CORS) Support | Microsoft Azure"
    description="Informationen Sie zum CORS Unterstützung für die Microsoft Azure-Speicher-Dienste zu aktivieren."
    services="storage"
    documentationCenter=".net"
    authors="cbrooks"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="cbrooks"/>

# <a name="cross-origin-resource-sharing-cors-support-for-the-azure-storage-services"></a>Gemeinsame Nutzung von (CORS) Unterstützung für die Dienste Azure-Speicher Cross Origin Ressourcen

Ab der Version 2013-08-15, Supportdienste der Azure-Speicher Cross-Origin Ressource freigeben (CORS) für die Dienste Blob, Tabelle, Warteschlange und Datei. CORS ist eine HTTP-Feature, mit eine Webanwendung ausgeführt unter einer Domäne auf Ressourcen in einer anderen Domäne zugreifen können. Webbrowser implementieren bekannte als [same Origin Policy](http://www.w3.org/Security/wiki/Same_Origin_Policy) , die eine Webseite von Aufrufen von APIs in eine andere Domäne wird verhindert, dass eine Einschränkung für die Sicherheit. CORS bietet eine sichere Möglichkeit, eine Domäne (die Origin) in eine andere Domäne APIs aufrufen zulassen. Informationen finden Sie im [CORS Spezifikation](http://www.w3.org/TR/cors/) auf CORS.

Regeln können Sie CORS einzeln für jeden der Speicherdienste festlegen durch [Blob-Service-Eigenschaften festlegen](https://msdn.microsoft.com/library/hh452235.aspx), [Warteschlange Diensteigenschaften festlegen](https://msdn.microsoft.com/library/hh452232.aspx)und [Festlegen von Tabelleneigenschaften Dienst](https://msdn.microsoft.com/library/hh452240.aspx)aufrufen. Nachdem Sie die CORS Regeln für den Dienst festlegen, wird eine ordnungsgemäß authentifizierte Anforderung für den Dienst aus einer anderen Domäne ausgewertet werden, um festzustellen, ob er nach den Regeln zulässig ist, die Sie angegeben haben.

>[AZURE.NOTE] Beachten Sie, dass CORS keiner Authentifizierungsmethode ist. Antrag für eine Speicherressource bei aktiviertem CORS muss entweder eine entsprechende Authentifizierungssignatur oder anhand einer öffentlichen Ressource erfolgen muss.

## <a name="understanding-cors-requests"></a>Grundlegendes zu CORS Anfragen

Die Anforderung einer CORS aus einer Domäne Origin kann aus zwei separate Anfragen bestehen:

- Eine Preflight-Anforderung, die die CORS Einschränkungen, die auf den Dienst abfragt. Die Preflight-Anforderung ist erforderlich, es sei denn, die Anforderungsmethode eine [einfache Methode](http://www.w3.org/TR/cors/), d. h., abrufen, Kopf oder Beitrag.

- Die eigentliche Anforderung, für die gewünschte Ressource vorgenommen.

### <a name="preflight-request"></a>Preflight-Anforderung

Preflight-Anforderung Abfragen, die CORS Einschränkungen, die für den Speicherdienst vom Kontobesitzer eingerichtet wurden. Webbrowser (oder andere Benutzer-Agents) sendet eine Anforderung Optionen, die die Anforderung Überschriften, Methode und Origin Domäne enthält. Der Speicherdienst wertet den beabsichtigten Vorgang basierend auf einem vorkonfigurierten Satz CORS Regeln, die angeben, welche Domänen Origin, Anforderungsmethoden und Anfrage-Header ein ist-Anforderung für eine Speicherressource angegeben werden können.

Wenn CORS für den Dienst aktiviert ist, und es ist eine CORS-Regel, die die Anforderung preflight entspricht, wird der Dienst reagiert mit Statuscode 200 (OK) und enthält die erforderlichen Access-Control-Header in der Antwort.

Wenn CORS für den Dienst nicht aktiviert ist, oder keine CORS Regel erfüllt ist die Preflight-Anforderung, wird der Dienst Statuscode 403 (Verboten) zurück.

Wenn die Anfrage Optionen nicht die erforderlichen CORS Header (Kopfzeilen Origin und Access-Steuerelement-Anforderung-Methode) enthält, wird der Dienst mit Statuscode 400 (Ungültige Anforderung) reagieren.

Beachten Sie, dass die Anforderung eine Preflight-Dienst (Blob, Warteschlange und Tabellen) und nicht mit den angeforderten Ressource ausgewertet wird. Der Kontobesitzer muss CORS als Teil der Dienst Kontoeigenschaften in Reihenfolge für die Anforderung erfolgreich aktiviert haben.

### <a name="actual-request"></a>Ist-Anforderung

Sobald die Preflight-Anforderung akzeptiert wird, und die Antwort zurückgegeben wird, wird im Browser die ist-Anforderung für die Speicherressource weiterzuleiten. Im Browser wird die tatsächliche verweigern sofort anfordern, wenn die Preflight-Anforderung abgelehnt wird.

Die eigentliche Anforderung wird als normaler Anfrage anhand der Speicherdienst behandelt. Das Vorhandensein der Kopfzeile Origin gibt an, dass die Anforderung eine Anforderung CORS ist und der Dienst die passenden CORS Regeln überprüfen soll. Wenn eine Übereinstimmung gefunden wird, werden die Kopfzeilen Access-Steuerelemente die Antwort hinzugefügt und an den Client gesendet. Wenn eine Übereinstimmung gefunden wird, werden die Kopfzeilen CORS Access-Steuerelement nicht zurückgegeben.

## <a name="enabling-cors-for-the-azure-storage-services"></a>Aktivieren von CORS für die Dienste Azure-Speicher

CORS Regeln sind Ebene der Dienst festgelegt, daher müssen Sie aktivieren oder Deaktivieren von CORS für jeden Dienst (Blob, Warteschlange und Tabelle) getrennt. Standardmäßig ist die CORS für jeden Dienst deaktiviert. Um CORS zu aktivieren, müssen Sie die entsprechenden Diensteigenschaften mit der Version 2013-08-15 festlegen oder höher, und fügen Sie den Diensteigenschaften CORS Regeln. Details zum Aktivieren oder Deaktivieren von CORS für einen Dienst und zum Festlegen von Regeln CORS Näheres [Blob-Service-Eigenschaften festlegen](https://msdn.microsoft.com/library/hh452235.aspx), [Warteschlange Diensteigenschaften festlegen](https://msdn.microsoft.com/library/hh452232.aspx)und [Festlegen von Tabelleneigenschaften Dienst](https://msdn.microsoft.com/library/hh452240.aspx).

Hier ist ein Beispiel für eine einzige CORS Regel, angegeben über eines Vorgangs zum Diensteigenschaften festlegen:

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Jedes Element in der Regel CORS eingeschlossen wird nachstehend beschrieben:

- **AllowedOrigins**: die Origin-Domänen, die eine Anforderung anhand der Speicherdienst über CORS zulässig sind. Die Origin-Domäne ist die Domäne, von der die Anforderung stammt. Beachten Sie, dass der Ursprung exakter Übereinstimmung mit den Ursprung Groß-/Kleinschreibung beachtet werden muss, die das Alter des Benutzers an den Dienst sendet. Sie können auch das Platzhalterzeichen ' *' alle Origin Domänen Anfragen über CORS vornehmen dürfen. Im oben genannten Beispiel können die Domänen [http://www.contoso.com](http://www.contoso.com) und [http://www.fabrikam.com](http://www.fabrikam.com) Anfragen gegen den Dienst mit CORS vornehmen.

- **AllowedMethods**: die Methoden (HTTP-Anforderung Verben), die Domäne Origin für eine Anforderung CORS verwenden kann. Im Beispiel oben werden nur für sich und GET-Anfragen zulässig.

- **AllowedHeaders**: die Anforderung Überschriften, die die Ursprungsdomäne der Anforderung CORS angeben können. Im Beispiel oben werden alle X-ms-Metadaten, X-ms-Metatag-Ziel und X-ms-Metatag-Abc angefangen Metadaten-Header zulässig. Beachten Sie, dass das Platzhalterzeichen "*" gibt an, dass alle Kopfzeile Anfang mit dem angegebenen Präfix zulässig ist.

- **ExposedHeaders**: die Antwort Überschriften, die möglicherweise in der Antwort auf die Anforderung CORS gesendet und vom Browser zu der Anfrage Herausgeber verfügbar gemacht werden. Im Beispiel oben wird im Browser angewiesen, um eine Kopfzeile Anfang mit X-ms-Metatag verfügbar zu machen.

- **MaxAgeInSeconds**: die maximale Menge Zeit, ein Browser Preflight-Optionen zwischenspeichern soll anfordern.

Die Dienste Azure-Speicher unterstützen vorangestellte Kopfzeilen für die **AllowedHeaders** und die **ExposedHeaders** Elemente angeben. Um eine Kategorie von Kopfzeilen zu ermöglichen, können Sie ein gemeinsames Präfix zu dieser Kategorie angeben. Angeben von *X-ms-Metatag*beispielsweise * vorangestellte Kopfzeile eine Regel fest, die alle Kopfzeilen entsprechen, die mit X-ms-Metatag beginnen.

Die folgenden Einschränkungen gelten für CORS Regeln:

- Sie können bis zu fünf CORS Regeln pro Speicherdienst (Blob, Tabelle und Warteschlange) angeben.
- Die maximale Größe von allen CORS Regeln Einstellungen auf die Anfrage, ohne XML-Tags, sollten 2 KB nicht überschreiten.
- Die Länge der eine zulässige Kopfzeile, zugänglicher Kopf- oder zulässige Origin darf 256 Zeichen nicht überschreiten.
- Zulässige Kopf- und zugänglicher Kopfzeilen möglicherweise entweder:
  - Literaler Kopfzeilen, wo des Spaltennamens genauen, wie z. B. **X-ms-Metatag-verarbeitet bereitgestellt wird**. Klicken Sie auf die Anfrage kann maximal 64 literaler Kopfzeilen angegeben werden.
  - Mit dem Präfix Überschriften, sofern ein Präfix der Kopfzeile, wie z. B. **X-ms-Metadaten vorgesehen ist***. Auf diese Weise die Angabe eines Präfix ermöglicht oder macht eine Kopfzeile, die mit dem angegebenen Präfix beginnt. Klicken Sie auf die Anfrage kann bis zu zwei vorangestellter Kopfzeilen angegeben werden.
- Das Element **AllowedMethods** angegebenen Methoden (oder HTTP-Verben) müssen durch Azure-Speicherdienst APIs unterstützten Methoden entsprechen. Unterstützte Methoden werden löschen, abrufen, Kopf, verbinden, Beitrag, Optionen und sich an.

## <a name="understanding-cors-rule-evaluation-logic"></a>Grundlegendes zu CORS Regel Auswertung Logik

Wenn eine Speicherdienst eine preflight oder tatsächlichen eingeht, Auswertung die Anforderung basierend auf den CORS Regeln, die Sie für den Dienst über die entsprechende Diensteigenschaften festlegen Operation hergestellt haben. CORS Regeln werden in der Reihenfolge ausgewertet, in denen im Hauptteil Anforderung des Vorgangs Diensteigenschaften festlegen festgelegt wurden.

CORS Regeln werden wie folgt ausgewertet:

1. Zunächst aktiviert ist die Ursprungsdomäne der Anfrage gegen den Domänen für das Element **AllowedOrigins** aufgeführt. Wenn die Domäne Origin in der Liste enthalten ist, oder alle Domänen mit dem Platzhalterzeichen zulässig sind ' *', dann Regeln Auswertung ausgeführt wird. Wenn die Domäne Origin nicht enthalten ist, schlägt die Anforderung fehl.

2. Als Nächstes ist Methode (oder des HTTP-Verb) der Anfrage für das Element **AllowedMethods** aufgeführten Methoden aktiviert. Wenn die Methode in der Liste enthalten ist, wird die Auswertung von Regeln fortgesetzt; Wenn dies nicht der Fall ist, klicken Sie dann die Anforderung fehlschlägt.

3. Wenn die Anforderung einer Regel in ihrer Domäne Origin und die zugehörige Methode entspricht, ist diese Regel den Prozess ausgewählt, die Anfrage und keine weiteren Regeln ausgewertet werden. Bevor die Anfrage erfolgreich sein kann, werden jedoch alle Überschriften angegeben haben, klicken Sie auf die Anfrage anhand der Überschriften im **AllowedHeaders** -Element aufgeführt überprüft. Wenn die Spaltenüberschriften gesendet nicht zulässigen Header übereinstimmen, schlägt die Anforderung fehl.

Da die Regeln in der Reihenfolge, dass sie im Hauptteil Anforderung vorhanden sind verarbeitet werden, empfiehlt sich bewährte Methoden, dass Sie die am meisten einschränkenden Regeln in Bezug auf Ursprung zuerst in der Liste angeben, dass diese zuerst ausgewertet werden. Geben Sie Regeln, die weniger einschränkenden – beispielsweise eine Regel für alle Ursprung – am Ende der Liste zulassen.

### <a name="example--cors-rules-evaluation"></a>Beispiel – Regeln CORS Auswertung

Im folgenden Beispiel wird eine teilweise Anforderungstexts für einen Vorgang CORS Regeln für die Speicherdienste festlegen. Weitere Informationen zum Erstellen der Anforderung finden Sie unter [Blob-Service-Eigenschaften festlegen](https://msdn.microsoft.com/library/hh452235.aspx), [Warteschlange Diensteigenschaften festlegen](https://msdn.microsoft.com/library/hh452232.aspx)und [Festlegen von Tabelleneigenschaften Dienst](https://msdn.microsoft.com/library/hh452240.aspx) .

    <Cors>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>PUT,HEAD</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>*</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
        </CorsRule>
    </Cors>

Nehmen Sie die folgenden CORS Anfragen an:

Anfordern||| Antwort||
---|---|---|---|---
**Methode** |**Origin** |**Anfordern von Kopfzeilen** |**Vergleich der Regel** |**Ergebnis**
**SETZEN** | http://www.contoso.com |X-ms-Blob-Inhaltstyp | Erste Regel |Erfolg
**Erhalten** | http://www.contoso.com| X-ms-Blob-Inhaltstyp | Zweite Regel |Erfolg
**Erhalten** | http://www.contoso.com| X-ms-Blob-Inhaltstyp | Zweite Regel | Fehler bei der

Die erste Anforderung entspricht die erste Regel – die Domäne Origin entspricht der zulässige Ursprung, die Methode entspricht der zulässigen Methoden und die Kopfzeile der zulässigen Header – entspricht und daher erfolgreich ist.

Die zweite Anforderung entspricht nicht die erste Regel, da die Methode nicht die zulässigen Methoden übereinstimmt. Es, jedoch die zweite Regel entspricht, damit sie erfolgreich ist.

Die dritte Anforderung entspricht die zweite Regel seine Origin Domäne und eine Methode, damit keine weiteren Regeln ausgewertet werden. Die *X-ms-Client-Anfrage-Id-Kopfzeile* ist jedoch durch die zweite Regel nicht zulässig, damit die Anforderung, trotz der Fakultät fehlschlägt, die die Semantik der dritten Regel erfolgreich ist es zugelassen haben möchten.

>[AZURE.NOTE] Obwohl in diesem Beispiel wird eine Regel weniger einschränkende vor eine restriktiverem angezeigt wird, wird im Allgemeinen empfiehlt es sich zuerst aufgeführt, die am meisten einschränkenden Regeln an.

## <a name="understanding-how-the-vary-header-is-set"></a>Grundlegendes zum Festlegen der variieren in Kopfzeile

Die Kopfzeile *variieren* ist ein standard HTTP/1.1-Header, die aus einer Reihe von Anforderung Felder, die über die Kriterien des Browser oder Benutzer-Agents zu benachrichtigen, die vom Server für die Anforderung ausgewählt wurden. Die Kopfzeile *variieren* wird hauptsächlich zum Zwischenspeichern von Proxys, Browsern und CDNs, die sie verwenden, um zu bestimmen, wie die Antwort zwischengespeichert werden soll. Details finden Sie unter der Spezifikation für die [Kopfzeile variieren](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

Wenn im Browser oder einem anderen Benutzer-Agents wird die Antwort einer Anforderung CORS zwischengespeichert, wird die Origin-Domäne als die zulässige Origin zwischengespeichert. Wenn Sie eine zweite Domäne Probleme mit der gleichen Anforderung für eine Speicherressource, während der Cache aktiv ist, ruft der Benutzer-Agents die Cache Origin Domäne aus. Die zweite Domäne entspricht nicht die Domäne Cache, damit die Anforderung schlägt fehl, wenn es sonst erfolgreichen. In bestimmten Fällen legt Azure-Speicher die Kopfzeile variieren **Origin** anweisen der Benutzer-Agents, auf die nächste CORS Anforderung an den Dienst zu senden, wenn die Domäne anfordernde von den Cache Ursprung unterscheidet.

Azure-Speicher: Legt die Kopfzeile *variieren* auf **Origin** für geleistete GET/Leiter Besprechungsanfragen in den folgenden Fällen:

- Wenn die Anfrage Origin stimmt exakt mit zulässigen Ursprung mithilfe einer Regel CORS definiert. Um eine genaue Übereinstimmung sein, die Regel CORS darf keinen Platzhalter ' * ' Zeichen.

- Es gibt keine Regel übereinstimmenden den Ursprung der Anfrage, aber für die Speicherdienst CORS aktiviert ist.

In der Groß-/Kleinschreibung, bei denen eine GET/Leiter Anforderung eine Regel CORS übereinstimmt, die alle Ursprung ermöglicht, gibt die Antwort an, dass alle Ursprung zulässig sind und der Benutzer-Agentcache nachfolgende Anforderungen aus einer beliebigen Domäne Origin ermöglicht während der Cache aktiv ist.

Beachten Sie, dass für Abfragen, die als erste/Leiter Methoden verwenden, die Speicherdienste nicht variieren in die Kopfzeile festgelegt, da die Antworten auf diese Methoden nicht vom Benutzer-Agents zwischengespeichert werden.

Die folgende Tabelle zeigt an, wie Azure-Speicher wird basierend auf den oben erwähnten Fällen GET/Leiter-Besprechungsanfragen beantworten:

Anfordern|Einstellung für das Konto und das Ergebnis der Auswertung|||Antwort|||
---|---|---|---|---|---|---|---|---
**Origin Kopfzeile präsentieren Anforderung** | **Für diesen Dienst angegebene CORS-Regeln** | **Übereinstimmende Regel vorhanden ist, die alle Ursprung ermöglicht (*)** | **Übereinstimmende Regel vorhanden ist, für den Ursprung genaue Übereinstimmung** | **Antwort umfasst variieren Kopfzeile zu Origin festlegen** | **Antwort enthält Access Steuerelement zulässig Origin: "*"** | **Antwort enthält Access Steuerelement verfügbar gemacht Kopfzeilen**
Nein|Nein|Nein|Nein|Nein|Nein|Nein
Nein|Ja|Nein|Nein|Ja|Nein|Nein
Nein|Ja|Ja|Nein|Nein|Ja|Ja
Ja|Nein|Nein|Nein|Nein|Nein|Nein
Ja|Ja|Nein|Ja|Ja|Nein|Ja
Ja|Ja|Nein|Nein|Ja|Nein|Nein
Ja|Ja|Ja|Nein|Nein|Ja|Ja


## <a name="billing-for-cors-requests"></a>Rechnung für CORS Anfragen

Erfolgreiche Preflight-Anfragen werden berechnet, wenn Sie CORS für den Speicherdienste für Ihr Konto aktiviert haben (durch Aufrufen [Blob-Service-Eigenschaften festlegen](https://msdn.microsoft.com/library/hh452235.aspx), [Warteschlange Diensteigenschaften festlegen](https://msdn.microsoft.com/library/hh452232.aspx)oder [Festlegen von Tabelleneigenschaften Service](https://msdn.microsoft.com/library/hh452240.aspx)). Um die Gebühren zu minimieren, berücksichtigen Sie das Element **MaxAgeInSeconds** in CORS Regeln auf einen hohen Wert, damit die Benutzer-Agents die Anforderung zwischengespeichert.

Nicht erfolgreiche Preflight-Anfragen werden nicht berechnet.

## <a name="next-steps"></a>Nächste Schritte

[Festlegen von Eigenschaften für Blob-Dienst](https://msdn.microsoft.com/library/hh452235.aspx)

[Festlegen von Eigenschaften-Dienst](https://msdn.microsoft.com/library/hh452232.aspx)

[Festlegen von Tabelleneigenschaften Service](https://msdn.microsoft.com/library/hh452240.aspx)

[Gemeinsame Nutzung von Spezifikation W3C Cross-Origin-Ressourcen](http://www.w3.org/TR/cors/)
