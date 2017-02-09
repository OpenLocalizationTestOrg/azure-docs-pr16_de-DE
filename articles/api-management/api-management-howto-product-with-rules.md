<properties
    pageTitle="Schützen Ihrer API mit Azure-API Management | Microsoft Azure"
    description="Erfahren Sie, wie Ihre API mit Kontingenten und Richtlinien (Zins eingrenzen) begrenzungsebene schützen."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>Schützen Sie Ihrer API mit Zins Grenzwerte mit Azure-API Management

In diesem Handbuch wird gezeigt, wie einfach es ist, Schutz für Ihre Back-End-API hinzufügen, indem Sie Zins Grenzwert und Kontingent Richtlinien mit Azure-API Management konfigurieren.

In diesem Lernprogramm erstellen Sie ein "Kostenlose Testversion" API-Produkt, mit dem Entwickler bis zu 10 Anrufe pro Minute und bis zu einem Maximum von 200 Anrufen pro Woche an Ihre API vornehmen mithilfe der [Grenzwert Anruf Kostensätze pro Abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) und Richtlinien [einsatzkontingent pro Abonnement festlegen können](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) . Sie dann die-API veröffentlichen und testen die Richtlinie für das Zins.

Erweiterte Drosselung Szenarien, die die Richtlinien [Zins Beschränkung durch Schlüssel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) und [Kontingent-von-Key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) verwenden finden Sie unter [Erweiterte Anforderung mit Azure-API Management begrenzungsebene](api-management-sample-flexible-throttling.md).

## <a name="create-product"> </a>Erstellen eines Produkts

In diesem Schritt erstellen Sie eine kostenlose Testversion, die keine Abonnement Genehmigung erforderlich ist.

>[AZURE.NOTE] Wenn Sie bereits ein Produkt konfiguriert haben und es in diesem Lernprogramm verwenden möchten, können Sie [Konfigurieren][] aufrufen Zins Grenzwert und Kontingent Richtlinien springen und führen Sie das Lernprogramm von dort das Produkt anstelle des Produkts kostenlose Testversion verwenden.

Um anzufangen, klicken Sie auf **Verwalten** in der klassischen Azure für Ihre API Verwaltungsdienst. Dadurch gelangen Sie zum Publisher-API Verwaltungsportal.

![Publisher-portal][api-management-management-console]

>Wenn Sie eine Instanz der API Management-Dienst noch nicht erstellt haben, finden Sie unter [Erstellen einer API Management Service-Instanz][] im Lernprogramm [Verwalten Ihrer ersten API in Azure-API Management][] .

Klicken Sie auf **Products** , im Menü **API Management** auf der linken Seite die Seite **Produkte** angezeigt werden.

![Produkt hinzufügen][api-management-add-product]

Klicken Sie auf **Produkt hinzufügen** , um das Dialogfeld **Neues Produkt hinzufügen** angezeigt wird.

![Hinzufügen von neuen Produkts][api-management-new-product-window]

Geben Sie im Feld **Titel** **Kostenlose Testversion**.

Geben Sie im Feld **Beschreibung** den folgenden Text ein:  **Abonnenten sind in der Lage, Ausführen von 10 Anrufe/Minute bis maximal 200 Anrufe/Woche nach dem Zugriff verweigert.**

Produkte in API Management geschützt werden können, oder öffnen. Geschützte Produkte müssen die abonniert, bevor sie verwendet werden können. Öffnen der Produkte können ohne ein Abonnement verwendet werden. Sicherstellen Sie, dass das **Abonnement erforderlich** ausgewählt ist, um ein geschütztes Produkt erstellen, die ein Abonnement erforderlich sind. Dies ist die Standardeinstellung.

Wenn Sie möchten ein Administrator zu überprüfen und annehmen oder ablehnen Abonnement Versuche, dieses Produkt, wählen Sie **Abonnement Genehmigung erforderlich**. Wenn das Kontrollkästchen nicht ausgewählt ist, werden Versuche Abonnement automatisch genehmigt. In diesem Beispiel werden Abonnements automatisch genehmigt, damit das Feld nicht auswählen.

Aktivieren Sie das Kontrollkästchen **mehrere Gleichzeitiges Abonnements zulassen** , Entwicklertools Konten mehrmals auf Neues Produkt abonnieren um zu ermöglichen. In diesem Lernprogramm nicht mehrere Gleichzeitiges Abonnements nutzen, also lassen Sie es deaktiviert.

Nachdem alle Werte eingegeben werden, klicken Sie auf **Speichern** , um das Produkt zu erstellen.

![Produkt hinzugefügt][api-management-product-added]

Standardmäßig sind neue Produkte für Benutzer in der Gruppe **Administratoren** sichtbar. Wir werden die **Entwickler** Gruppe hinzufügen. Klicken Sie auf **Kostenlose Testversion**, und klicken Sie dann auf die Registerkarte **Sichtbarkeit** .

>API-Verwaltung werden Gruppen zum Verwalten der Sichtbarkeit von Produkten für Entwickler. Produkte erteilen Sichtbarkeit von Gruppen und Entwickler anzeigen und abonnieren Sie den Produkten, die sichtbar sind die Gruppen, in denen sie angehören, können. Weitere Informationen finden Sie unter [So erstellen und Verwenden von Gruppen in Azure-API Management][].

![Entwicklergruppe hinzufügen][api-management-add-developers-group]

Aktivieren Sie das Kontrollkästchen **Entwickler** , und klicken Sie dann auf **Speichern**.

## <a name="add-api"> </a>Eine API mit dem Produkt hinzufügen

In diesem Schritt des Lernprogramms werden wir die Echo-API mit dem neuen kostenlose Testversion Produkt hinzufügen.

>Jede Instanz der API Management-Dienst gehört zum vorkonfiguriertes mit einer Echo-API, die mit experimentieren und Informationen zu API Management verwendet werden können. Weitere Informationen finden Sie unter [Verwalten der erste API in Azure-API Management][].

Klicken Sie im Menü " **API Management** " auf der linken Seite auf **Products** , und klicken Sie dann auf **Kostenlose Testversion** , um das Produkt zu konfigurieren.

![Produkt konfigurieren][api-management-configure-product]

Klicken Sie auf **API mit Produkt hinzufügen**.

![Produkt API hinzufügen][api-management-add-api]

Wählen Sie **Echo-API**aus, und klicken Sie dann auf **Speichern**.

![Hinzufügen von Echo-API][api-management-add-echo-api]

## <a name="policies"> </a>Zum Konfigurieren der Anruf Zins Grenzwert und Kontingent Richtlinien

Zins Grenzwerte und Kontingente sind in den Richtlinien-Editor konfiguriert. Klicken Sie im Menü **-API Management** auf der linken Seite auf **Richtlinien** . Klicken Sie in **der Produktliste** auf **Kostenlose Testversion**.

![Produkt-Richtlinie][api-management-product-policy]

Klicken Sie auf die **Richtlinie hinzufügen** importieren Sie die Richtlinienvorlage und erstellen die Rate Grenzwert und Kontingent Richtlinien zu starten.

![Richtlinie hinzufügen][api-management-add-policy]

Positionieren Sie den Cursor in der **eingehenden** oder **ausgehenden** Abschnitt der Richtlinienvorlage, Richtlinien um einzufügen. Zins Grenzwert und Kontingent Richtlinien werden eingehende Richtlinien, also positionieren Sie den Cursor in das eingehende Element.

![Gruppenrichtlinien-editor][api-management-policy-editor-inbound]

Die beiden Richtlinien, die wir in diesem Lernprogramm hinzufügen möchten sind die Richtlinien [Grenzwert Anruf Kostensätze pro Abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) und [einsatzkontingent pro Abonnement festlegen](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) .

![Richtlinie Anweisungen][api-management-limit-policies]

Nachdem sich der Cursor im **eingehenden** Richtlinienelement positioniert ist, klicken Sie auf den Pfeil neben **Beschränkung Anruf Kostensätze pro Abonnement** zum Einfügen von der Richtlinienvorlage.

    <rate-limit calls="number" renewal-period="seconds">
    <api name="name" calls="number">
    <operation name="name" calls="number" />
    </api>
    </rate-limit>

**Grenzwert Anruf Kostensätze pro Abonnement** auf der Produktebene verwendet werden können und können auch am API und einzelne Vorgang Namen Ebenen verwendet werden. In diesem Lernprogramm nur Produkt Ebene Richtlinien verwendet werden, löschen Sie die **api** und **Vorgang** Elemente aus dem Element **Zins-Limit** , damit nur die äußere **Zins-Limit** Element bleibt, wie im folgenden Beispiel dargestellt.

    <rate-limit calls="number" renewal-period="seconds">
    </rate-limit>

In das Produkt kostenlose Testversion der maximal zulässigen Anruf Zins ist 10 Anrufe pro Minute, geben Sie also **10** als der Wert für das Attribut **Anrufe** und **60** für das Attribut **Erneuerung der Periode** .

    <rate-limit calls="10" renewal-period="60">
    </rate-limit>

Um die Richtlinie **festlegen einsatzkontingent pro Abonnement** zu konfigurieren, positionieren Sie den Cursor direkt unterhalb der neu hinzugefügten **Zins-Limit** Element innerhalb des **eingehenden** Elements, und klicken Sie dann auf den Pfeil nach links vom **einsatzkontingent pro Abonnement festlegen**.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="name" calls="number" bandwidth="kilobytes">
    <operation name="name" calls="number" bandwidth="kilobytes" />
    </api>
    </quota>

Da dieser Richtlinie auch auf der Produktebene sein soll, Löschen der **api** und **Vorgang** Namen Elemente wie im folgenden Beispiel gezeigt.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    </quota>

Kontingente können die Anzahl von Anrufen pro Intervall und/oder Bandbreite basieren. In diesem Lernprogramm wir sind nicht einschränken der basierend auf Bandbreite, also das Attribut **Bandbreite** löschen.

    <quota calls="number" renewal-period="seconds">
    </quota>

In das Produkt kostenlose Testversion ist das Kontingent 200 Anrufe pro Woche. Geben Sie **200** als des Werts für das Attribut **Anrufe** an, und geben Sie dann als Wert für das Attribut **der Periode Erneuerung** **604800** .

    <quota calls="200" renewal-period="604800">
    </quota>

>Richtlinienintervalle werden in Sekunden angegeben. Sie können die Anzahl der Tage (7) durch die Anzahl der Stunden in einem Tag (24) durch die Anzahl der Minuten in eine Stunde (60) durch die Anzahl von Sekunden in eine Minute um (60) multiplizieren, um das Intervall für eine Woche zu berechnen: 7 *24* 60 * 60 = 604800.

Wenn Sie die Richtlinie konfigurieren abgeschlossen haben, sollten sie im folgende Beispiel entsprechen.

    <policies>
        <inbound>
            <rate-limit calls="10" renewal-period="60">
            </rate-limit>
            <quota calls="200" renewal-period="604800">
            </quota>
            <base />

    </inbound>
    <outbound>

        <base />

        </outbound>
    </policies>

Nachdem Sie die gewünschten Richtlinien konfiguriert haben, klicken Sie auf **Speichern**.

![Speichern Sie die Richtlinie][api-management-policy-save]

## <a name="publish-product"></a> Das Produkt veröffentlichen

Nachdem Sie nun die die APIs hinzugefügt werden und die Richtlinien konfiguriert sind, muss das Produkt veröffentlicht werden, damit es von Entwicklern verwendet werden kann. Klicken Sie im Menü " **API Management** " auf der linken Seite auf **Products** , und klicken Sie dann auf **Kostenlose Testversion** , um das Produkt zu konfigurieren.

![Produkt konfigurieren][api-management-configure-product]

Klicken Sie auf **Veröffentlichen**, und klicken Sie dann auf **Ja, veröffentlichen** , um zu bestätigen.

![Produkt veröffentlichen][api-management-publish-product]

## <a name="subscribe-account"> </a>Ein Entwicklerkonto, um das Produkt abonnieren

Jetzt, da das Produkt veröffentlicht wird, kann es abonniert und von Entwicklern verwendet werden.

>Administratoren einer API Management-Instanz werden automatisch mit jedem Produkt abonniert. In diesem Lernprogramm Schritt werden wir eine Firma Entwicklertools nicht-Administrator, um das Produkt kostenlose Testversion abonnieren. Wenn Ihr Konto Entwicklertools Administratorrolle gehört, können Sie zusammen mit diesem Schritt befolgen, obwohl Sie bereits abonniert haben.

Klicken Sie im Menü **-API Management** auf der linken Seite auf **Benutzer** , und klicken Sie dann auf den Namen Ihres Kontos Entwicklertools. In diesem Beispiel werden wir das **Clayton Gragg** Entwickler-Konto verwendet.

![Konfigurieren von Entwicklertools][api-management-configure-developer]

Klicken Sie auf **Abonnement hinzufügen**.

![Hinzufügen von Abonnements][api-management-add-subscription-menu]

Wählen Sie **Kostenlose Testversion**aus, und klicken Sie dann auf **Abonnieren**.

![Hinzufügen von Abonnements][api-management-add-subscription]

>[AZURE.NOTE] In diesem Lernprogramm werden mehrere Gleichzeitiges Abonnements für die kostenlose Testversion nicht aktiviert. Wären sie würden Sie aufgefordert, benennen Sie das Abonnement, wie im folgenden Beispiel dargestellt.

![Hinzufügen von Abonnements][api-management-add-subscription-multiple]

Nach dem Klicken auf **Abonnieren**, wird das Produkt in der Liste **Abonnement** für den Benutzer aus.

![Abonnement hinzugefügt][api-management-subscription-added]

## <a name="test-rate-limit"> </a>Zum Aufrufen eines Vorgangs und testen die Beschränkung Zins

Jetzt, da das Produkt kostenlose Testversion konfiguriert und veröffentlicht wurde, können wir einige Vorgänge anrufen und testen die Richtlinie für das Zins.
Wechseln Sie zum Entwicklerportal, indem Sie in der oberen rechten Menü **Entwicklerportal** auf ein.

![-Entwicklerportal][api-management-developer-portal-menu]

Klicken Sie im oberen Menü auf **APIs** , und klicken Sie dann auf **Echo-API**.

![-Entwicklerportal][api-management-developer-portal-api-menu]

Klicken Sie auf **Ressource abrufen**, und klicken Sie dann auf **Probieren Sie es**.

![Geöffnete Konsole][api-management-open-console]

Übernehmen Sie den Standardnamen Parameterwerte, und wählen Sie dann Ihr Abonnementschlüssel für die kostenlose Testversion.

![Abonnement-Taste][api-management-select-key]

>[AZURE.NOTE] Wenn Sie mehrere Abonnements haben, achten Sie darauf, um die Taste für **Kostenlose Testversion**auszuwählen, da sonst die Richtlinien, die in den vorherigen Schritten konfiguriert wurden nicht wirksam werden.

Klicken Sie auf **Senden**, und zeigen Sie dann die Antwort. Beachten Sie den **Status der Antwort** des **200 OK**.

![Der Ergebnisse][api-management-http-get-results]

Klicken Sie auf **Senden** , um einen Satz größer als die Richtlinie für das Zins von 10 Anrufen pro Minute. Nachdem die Richtlinie Zins Grenzwert überschritten wird, wird ein Antwortstatus **429 zu viele Anfragen** zurückgegeben.

![Der Ergebnisse][api-management-http-get-429]

Den **Inhalt der Antwort** zeigt das restliche Intervall an, bevor Wiederholungsversuche hergestellt werden können.

Wenn die Richtlinie für das Zins von 10 Anrufen pro Minute aktiv ist, nachfolgende Anrufen kann erst nach Ablauf von 60 Sekunden vom ersten von 10 erfolgreich Anrufe mit dem Produkt, bevor die Rate überschritten wurde. In diesem Beispiel wird das restliche Intervall 54 Sekunden an.

## <a name="next-steps"> </a>Nächste Schritte

-   Schauen Sie sich eine Demo zu Rate Grenzwerte und Kontingente in das folgende Video festlegen.

> [AZURE.VIDEO rate-limits-and-quotas]


[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Verwalten Sie Ihre erste API in Azure-API Management]: api-management-get-started.md
[So erstellen und Verwenden von Gruppen in Azure-API Management]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management-Dienst]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Konfigurieren der Anruf Zins Grenzwert und Kontingent Richtlinien]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
