<properties 
    pageTitle="So autorisieren Entwicklertools Konten OAuth 2.0 in Azure-API Management verwenden" 
    description="Erfahren Sie, wie für Benutzerberechtigungen OAuth 2.0 in Management-API verwenden." 
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

# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>So autorisieren Entwicklertools Konten OAuth 2.0 in Azure-API Management verwenden

Viele APIs unterstützen [OAuth 2.0](http://oauth.net/2/) installieren, um die API secure, und stellen Sie sicher, dass nur gültige Benutzer Zugang und Ressourcen, die sie in Anspruch nehmen können, nur zugreifen können. Azure-API des interaktiven Entwicklertools Verwaltungskonsole mit solche APIs verwenden möchten, können Sie der Dienst so konfigurieren Sie Ihre Service-Instanz aus, um Ihre OAuth 2.0 aktiviert API konzipiert.

## <a name="prerequisites"> </a>Erforderliche Komponenten

In diesem Handbuch wird gezeigt, wie so konfigurieren Sie die Instanz der API Management-Dienst, um die Autorisierung OAuth 2.0 für Entwickler Konten verwenden, jedoch wird nicht gezeigt, wie einen OAuth 2.0-Anbieter konfigurieren. Die Konfiguration für jeden Anbieter OAuth 2.0 unterscheidet, obwohl die beschriebenen Schritte aus, und die erforderlichen Informationen in konfigurieren OAuth 2.0 in Ihrem API Management Service-Instanz verwendet gleich sind. Dieses Thema zeigt Beispiele für die Verwendung von Azure Active Directory als OAuth 2.0 Provider.

>[AZURE.NOTE] Weitere Informationen zum Konfigurieren von OAuth 2.0 Azure Active Directory verwenden finden Sie unter der Stichprobe [WebApp-GraphAPI-DotNet][] .

## <a name="step1"> </a>Konfigurieren einer OAuth 2.0 Autorisierung in Management-API

Um anzufangen, klicken Sie auf **Verwalten** im klassischen Azure-Portal für Ihre API Verwaltungsdienst. Dadurch gelangen Sie Publisher-API Verwaltungsportal.

![Publisher-portal][api-management-management-console]

>[AZURE.NOTE] Wenn Sie eine Instanz der API Management-Dienst noch nicht erstellt haben, finden Sie unter [Erstellen einer API Management Service-Instanz][] , in die [Erste Schritte mit Azure-API Management][] Lernprogramm.

Klicken Sie im Menü " **API Management** " auf der linken Seite auf **Sicherheit** auf **OAuth 2.0**, und klicken Sie dann auf **die Autorisierung Server hinzufügen**.

![OAuth 2.0][api-management-oauth2]

Nach dem Klicken auf **die Autorisierung Server hinzufügen**, wird das neue Autorisierung Server Formular angezeigt.

![Neue server][api-management-oauth2-server-1]

Geben Sie in die Felder **Name** und **Beschreibung** einen Namen und optional eine Beschreibung ein. 

>[AZURE.NOTE] Diese Felder werden verwendet, um den OAuth 2.0 Autorisierung Server in der aktuellen Instanz von API Management Service identifizieren und deren Werte nicht von der OAuth 2.0-Server stammen.

Geben Sie die **URL der Registrierung Client**. Diese Seite ist, in dem Benutzer erstellen und Verwalten von ihren Konten, und richtet sich nach dem OAuth 2.0-Anbieter verwendet. Die **URL der Registrierung Client** verweist auf die Seite, die Benutzer verwenden können, erstellen und konfigurieren ihre eigenen Konten für OAuth 2.0-Anbieter, die Benutzer die Verwaltung von Konten zu unterstützen. Einige Organisationen konfigurieren und verwenden Sie diese Funktion, auch wenn Sie der Anbieter OAuth 2.0 unterstützt nicht. Wenn Sie Ihren Anbieter OAuth 2.0 keinen Benutzer die Verwaltung von Konten, die so konfiguriert ist, geben Sie die Platzhalter URL hier beispielsweise die URL Ihrer Firma oder einer URL wie `https://placeholder.contoso.com`.

Im nächste Abschnitt des Formulars enthält die Einstellungen **Autorisierungscode erteilen Typen**, **Endpunkt-URL Autorisierung**und **Autorisierung Anforderungsmethode** .

![Neue server][api-management-oauth2-server-2]

Geben Sie die **Autorisierungscode erteilen Typen** , indem Sie die gewünschten Typen auschecken. **Autorisierungscode** wird standardmäßig angegeben.

Geben Sie die **Autorisierung Endpunkt-URL**ein. Für Azure Active Directory, diese URL den folgenden URL, ähnlich wie möglich sein, in dem `<client_id>` wird durch die Client-Id, die eine Anwendung auf dem Server OAuth 2.0 identifiziert ersetzt.

    https://login.windows.net/<client_id>/oauth2/authorize

Die **Autorisierung Anforderungsmethode** gibt an, wie die Autorisierung Anforderung auf dem Server OAuth 2.0 gesendet wird. **Erste** ist standardmäßig aktiviert.

Im nächste Abschnitt ist, wobei die **Token Endpunkt-URL**, **Authentifizierungsmethoden Client**, **Access Token Methode senden**und **Standard Bereich** angegeben werden.

![Neue server][api-management-oauth2-server-3]

Für einen Server Azure Active Directory OAuth 2.0, den **Endpunkt-URL Token** haben das folgende Format, wobei `<APPID>` weist das Format der `yourapp.onmicrosoft.com`.

    https://login.windows.net/<APPID>/oauth2/token

Die Standardeinstellung für **Clientauthentifizierungsmethoden** ist **grundlegende**und **Access-Token senden Methode** **Autorisierung Kopfzeile**. Diese Werte werden in diesem Abschnitt des Formulars, zusammen mit den **Bereich Standard**konfiguriert.

Im Abschnitt **Anmeldeinformationen Clients** enthält die **Client-ID** und **Client geheim**, die während der Erstellung und Konfiguration Ihres OAuth 2.0-Servers abgerufen werden. Sobald die **Client-ID** und **Client geheim** festgelegt sind, wird die **Redirect_uri** für die **Autorisierungscode** generiert. So konfigurieren Sie die URL der Antworten in der OAuth 2.0-Server-Konfiguration, wird dieser URI verwendet.

![Neue server][api-management-oauth2-server-4]

Wenn **Autorisierungscode erteilen Typen** **Ressource Besitzer ein Kennwort**festgelegt ist, wird im Abschnitt **Ressource Besitzer Kennwort Anmeldeinformationen** verwendet, um anzugeben, diese Anmeldeinformationen; Andernfalls können Sie es leer lassen.

![Neue server][api-management-oauth2-server-5]

Nachdem Sie das Formular abgeschlossen ist, klicken Sie auf **Speichern** , um die Konfiguration von API Management OAuth 2.0 Autorisierung Server zu speichern. Nachdem Sie die Konfiguration des Servers gespeichert wurde, können Sie APIs zum Verwenden dieser Konfiguration konfigurieren, wie im nächsten Abschnitt dargestellt.

## <a name="step2"> </a>Konfigurieren eine API um OAuth 2.0 Benutzer Autorisierung verwenden

Klicken Sie im Menü " **API Management** " auf der linken Seite auf **APIs** , klicken Sie auf den Namen der gewünschten-API, klicken Sie auf **Sicherheit**, und klicken Sie dann die Kontrollkästchen für **OAuth 2.0**.

![Autorisierung für Benutzer][api-management-user-authorization]

Wählen Sie den gewünschten **Autorisierung Server** in der Dropdown-Liste aus, und klicken Sie auf **Speichern**.

![Autorisierung für Benutzer][api-management-user-authorization-save]

## <a name="step3"> </a>Die Autorisierung OAuth 2.0 Benutzer in der Entwicklerportal testen

Nachdem Sie Ihre OAuth 2.0 Autorisierung Server konfiguriert und so konfiguriert, dass Ihre API, um diesen Server verwenden, können Sie ihn testen, indem Sie die Entwicklerportal und Aufrufen einer API.  Klicken Sie in der oberen rechten Menü auf **Entwicklerportal** .

![Entwicklerportal][api-management-developer-portal-menu]

Klicken Sie im oberen Menü auf **APIs** , und wählen Sie **Echo-API**.

![Echo-API][api-management-apis-echo-api]

>[AZURE.NOTE] Wenn Sie nur eine API für Ihr Konto sichtbar oder konfiguriert haben, wird dann auf APIs direkt an die Aktionen für diese API.

Wählen Sie den Vorgang von **Ressourcen abrufen** , klicken Sie auf **Öffnen Konsole**, und wählen Sie dann aus der Dropdownliste **Autorisierungscode** .

![Geöffnete Konsole][api-management-open-console]

Wenn die **Autorisierungscode** ausgewählt ist, wird ein Popupfenster mit dem Formular Anmeldung des OAuth 2.0-Anbieters angezeigt. In diesem Beispiel wird das Formular Anmeldung von Azure Active Directory bereitgestellt.

>[AZURE.NOTE] Wenn Sie Popups deaktiviert haben werden Sie aufgefordert, die sie vom Browser zu aktivieren. Nachdem Sie sie aktiviert haben, werden erneut select **Autorisierungscode** und die Anmeldung Form angezeigt.

![Anmelden][api-management-oauth2-signin]

Nachdem Sie sich angemeldet haben, werden die **Anfrage-Header** aufgefüllt, mit einer `Authorization : Bearer` Kopfzeile, die die Anforderung autorisiert.

![Kopfzeile Token anfordern][api-management-request-header-token]

Sie können an dieser Stelle konfigurieren die gewünschten Werte für die verbleibenden Parameter, und übermitteln Sie die Anforderung. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von OAuth 2.0 und -API Management finden Sie unter das folgende Video und öffentlichem [Artikel](api-management-howto-protect-backend-with-aad.md).

> [AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Erste Schritte mit Azure-API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Erstellen Sie eine Instanz der API Management-Dienst]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

