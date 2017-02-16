<properties
    pageTitle="Hinzufügen zur Verbesserung der Leistung bei Azure-API Verwaltung Zwischenspeichern | Microsoft Azure"
    description="Erfahren Sie, wie Sie zur Verbesserung der Wartezeit, Bandbreite und Webdienst für API Management Serviceanrufe zu laden."
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

# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>Fügen Sie zum Verbessern der Leistung bei Azure-API Verwaltung Zwischenspeichern hinzu

Operationen in API Management können zum Zwischenspeichern von Antwort konfiguriert werden. Antwort zwischenspeichern kann erheblich verringern API Wartezeit, Bandbreite Ernährung und web-Dienst Laden für Daten, die nicht häufig geändert werden.

Dieser Anleitung erfahren Sie, wie Antwort Zwischenspeichern für Ihre API hinzufügen und Konfigurieren von Richtlinien für die Stichprobe Echo API Vorgänge. Sie können dann den Vorgang aus der Entwicklerportal zur Überprüfung Zwischenspeichern in Aktion aufrufen.

>[AZURE.NOTE] Informationen zum Zwischenspeichern von Elementen durch Verwenden von Ausdrücken Richtlinie Schlüssel finden Sie unter [Benutzerdefiniert in Azure-API Management Zwischenspeichern](api-management-sample-cache-by-key.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie die Schritte in diesem Handbuch, müssen Sie eine Instanz des-API Management-Dienst mit einer API und ein Produkt konfiguriert. Wenn Sie eine Instanz der API Management-Dienst noch nicht erstellt haben, finden Sie unter [Erstellen einer API Management Service-Instanz][] , in die [Erste Schritte mit Azure-API Management][] Lernprogramm.

## <a name="configure-caching"> </a>Konfigurieren ein Vorgangs zum Zwischenspeichern

In diesem Schritt überprüfen Sie die Zwischenspeichern Einstellungen des Vorgangs der Stichprobe Echo API **Erste Ressource (zwischengespeichert)** .

>[AZURE.NOTE] Jede Instanz der API Management Service vorkonfiguriert mit einer Echo-API, die mit experimentieren und Informationen zu API Management verwendet werden können. Weitere Informationen finden Sie unter [Erste Schritte mit Azure-API Management][].

Um anzufangen, klicken Sie auf **Verwalten** im klassischen Azure-Portal für Ihre API Verwaltungsdienst. Dadurch gelangen Sie Publisher-API Verwaltungsportal.

![Publisher-portal][api-management-management-console]

Klicken Sie im Menü " **API Management** " auf der linken Seite auf **APIs** , und klicken Sie dann auf **Echo-API**.

![Echo-API][api-management-echo-api]

Klicken Sie auf die Registerkarte **Vorgänge** aus, und klicken Sie dann auf die **Erste Ressource (zwischengespeichert)** Vorgang aus der Liste der **Vorgänge** .

![Echo-API Vorgänge][api-management-echo-api-operations]

Klicken Sie auf der Registerkarte **Zwischenspeichern** , um die Zwischenspeichern Einstellungen für diesen Vorgang anzuzeigen.

![Registerkarte Zwischenspeichern][api-management-caching-tab]

Aktivieren Sie das Kontrollkästchen **Aktivieren** , um Zwischenspeichern für einen Vorgang zu aktivieren. In diesem Beispiel ist das Zwischenspeichern aktiviert.

Wird mit jeder Antwort auf einen Vorgang Schlüsseln basierend auf den Werten in den Feldern **Unterscheidung nach Parameter der Abfragezeichenfolge** und **Unterscheidung nach Überschriften** . Wenn Sie mehrere Antworten basierend auf Parameter der Abfragezeichenfolge oder Fußzeilen zwischenspeichern möchten, können Sie sie in den folgenden beiden Feldern konfigurieren.

**Dauer** gibt an, der das zurückzugebende Ablauf die zwischengespeicherten Antworten. In diesem Beispiel ist das Intervall **3600** Sekunden, die eine Stunde entspricht.

Verwenden in diesem Beispiel die Konfiguration zwischenspeichern, gibt die erste Anforderung an die **Erste Ressource (zwischengespeichert)** Vorgang eine Antwort aus der Back-End-Dienst an. Diese Antwort wird sortiert nach den angegebenen Kopf- und Parameter der Abfragezeichenfolge zwischengespeichert werden. Nachfolgende anrufen, um den Vorgang, mit übereinstimmenden Parametern haben die zwischengespeicherte Antwort zurückgegeben, bis das Cache Dauer Intervall abgelaufen ist.

## <a name="caching-policies"> </a>Überprüfen Sie die Zwischenspeichern Richtlinien

In diesem Schritt überprüfen Sie die Zwischenspeichern Einstellungen für den Vorgang **Erste Ressource (Cache)** des Beispiels Echo-API.

Wenn Zwischenspeichern Einstellungen für einen Vorgang auf der Registerkarte **Zwischenspeichern** konfiguriert sind, werden Zwischenspeichern Richtlinien für den Vorgang hinzugefügt. Diese Richtlinien können angezeigt und bearbeitet wird in den Richtlinien-Editor.

Klicken Sie im Menü " **API Management** " auf der linken Seite auf **Richtlinien** , und wählen Sie dann **Echo API / erste Ressource (zwischengespeichert)** aus der Dropdownliste **Vorgang** .

![Richtlinie Umfang Vorgang][api-management-operation-dropdown]

Dadurch werden die Richtlinien für diesen Vorgang in den Richtlinien-Editor.

![Projektmanagement-API Richtlinie-editor][api-management-policy-editor]

Die Richtliniendefinition für diesen Vorgang umfasst die Richtlinien, die die Zwischenspeichern Konfiguration definieren, die mit der Registerkarte **Zwischenspeichern** im vorherigen Schritt überprüft wurden.

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

>[AZURE.NOTE] Den Zwischenspeichern Richtlinien in den Richtlinien-Editor vorgenommene Änderungen werden auf der Registerkarte **Zwischenspeichern** eines Vorgangs und umgekehrt erkennbar.

## <a name="test-operation"> </a>Anrufen ein Vorgangs und das Zwischenspeichern testen

Um das Zwischenspeichern in Aktion sehen, können wir den Vorgang aus der Entwicklerportal aufrufen. Klicken Sie in der oberen rechten Menü auf **Entwicklerportal** .

![Entwicklerportal][api-management-developer-portal-menu]

Klicken Sie im oberen Menü auf **APIs** , und wählen Sie **Echo-API**.

![Echo-API][api-management-apis-echo-api]

>Wenn Sie nur eine API für Ihr Konto sichtbar oder konfiguriert haben, wird dann auf APIs direkt an die Aktionen für diese API.

Wählen Sie den Vorgang für die **Erste Ressource (zwischengespeichert)** , und klicken Sie dann auf **Öffnen Console**.

![Geöffnete Konsole][api-management-open-console]

Die Konsole können Sie Vorgänge direkt aus der Entwicklerportal aufrufen.

![Konsole][api-management-console]

Die Standardwerte für **param1** und **param2**beibehalten.

Wählen Sie den gewünschten Schlüssel aus der Dropdownliste **Abonnement-Taste** . Wenn Ihr Konto nur ein Abonnement enthält, ist es bereits ausgewählt.

Geben Sie in das Textfeld **Header für die Anforderung** **Sampleheader:value1** .

Klicken Sie auf **HTTP-Get** , und notieren Sie die Antwort Überschriften.

Geben Sie in das Textfeld **Header für die Anforderung** **Sampleheader:value2** ein, und klicken Sie dann auf **HTTP-Get**.

Beachten Sie, dass der Wert von **Sampleheader** weiterhin **Wert1** in der Antwort ein. Beachten Sie, dass die zwischengespeicherte Antwort aus dem ersten Anruf zurückgegeben wird, und versuchen Sie einige andere Werte.

Geben Sie in das Feld **param2** **25** , und klicken Sie dann auf **HTTP-Get**.

Beachten Sie, dass der Wert von **Sampleheader** in der Antwort jetzt **Wert2**ist. Da die Vorgangsergebnisse nach Abfragezeichenfolge zu sortieren sind, wurde die vorherige zwischengespeicherte Antwort nicht zurückgegeben.

## <a name="next-steps"> </a>Nächste Schritte

-   Weitere Informationen zu Zwischenspeichern Richtlinien finden Sie unter [Richtlinien Zwischenspeichern][] [-API Management Richtlinie Bezug][].
-   Informationen zum Zwischenspeichern von Elementen durch Verwenden von Ausdrücken Richtlinie Schlüssel finden Sie unter [Benutzerdefiniert in Azure-API Management Zwischenspeichern](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Erste Schritte mit Azure-API Management]: api-management-get-started.md

[Projektmanagement-API Richtlinie Bezug]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Zwischenspeichern von Richtlinien]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Erstellen Sie eine Instanz der API Management-Dienst]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps
