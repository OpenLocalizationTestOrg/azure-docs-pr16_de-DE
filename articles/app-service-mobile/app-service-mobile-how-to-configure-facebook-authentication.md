<properties
    pageTitle="So konfigurieren Sie die Facebook-Authentifizierung für Ihre App Services-Anwendung"
    description="Informationen Sie zum Konfigurieren der Facebook-Authentifizierung für Ihre App Services-Anwendung."
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>So konfigurieren Sie Ihre App-verwaltungsdienstanwendung Facebook Login verwenden

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema wird gezeigt, wie Facebook als eine Authentifizierungsanbieter verwenden Azure App Dienst konfigurieren.

Um das Verfahren in diesem Thema ausführen zu können, müssen Sie Facebook-Konto verfügen, die eine überprüft e-Mail-Adresse und eine Mobiltelefonnummer enthält. Wechseln Sie zum Erstellen eines neuen Facebook-Kontos zu ["Facebook.com"].

## <a name="register"> </a>Registrieren Ihrer Anwendung mit Facebook

1. Melden Sie sich bei der [Azure-Portal], und navigieren Sie zu Ihrer Anwendung. Kopieren Sie die **URL**ein. Sie verwenden diese so konfigurieren Sie die Facebook-app.

2. Navigieren Sie in einem anderen Browserfenster zu der Website [Facebook-Entwickler] und mit Ihren Anmeldeinformationen der Facebook-Konto anmelden.

3. (Optional) Wenn Sie nicht bereits registriert haben, klicken Sie auf **Apps** > **als Entwickler registrieren**, klicken Sie dann die Richtlinie zu übernehmen, und führen Sie die Schritte zur Registrierung.

4. Klicken Sie auf **Meine Apps** > **Hinzufügen einer neue App** > **Website** > **überspringen und App-ID erstellen**. 

5. **Anzeigenamen ein**Geben Sie einen eindeutigen Namen für Ihre app Geben Sie Ihre **Kontakt-e-Mail**, wählen Sie eine **Kategorie** für Ihre app, und klicken Sie dann auf **App-ID erstellen** und führen Sie das Kontrollkästchen Sicherheit. Dadurch gelangen Sie zum Dashboard Entwicklertools für Ihre neue Facebook-app.

6. Klicken Sie unter "Facebook Anmeldung" klicken Sie auf **Erste Schritte**. Fügen Sie Ihrer Anwendung **URI umleiten** **gültige OAuth umleiten URIs hinzu**und dann klicken Sie auf **Save Changes**. 

    > [AZURE.NOTE] Der URI-Umleitung ist die URL Ihrer Anwendung durch den Pfad, _/.auth/login/facebook/callback_angefügt. Beispielsweise `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Stellen Sie sicher, dass Sie das HTTPS-Schema verwenden.

6. Klicken Sie im linken Navigationsbereich auf **Einstellungen**. Klicken Sie auf das Feld **App geheim** klicken Sie auf **Anzeigen**, geben Sie Ihr Kennwort ein, angefordert, und notieren Sie die Werte der **App-ID** und **App geheim**. Sie verwenden diese später zum Konfigurieren Ihrer Anwendungs in Azure.

    > [AZURE.IMPORTANT] Die app geheim ist eine wichtige Sicherheitsanmeldeinformationen. Teilen Sie diese geheim mit jedem oder verteilen Sie einer Clientanwendung nicht.

7. Das Facebook-Konto die verwendet wurde, um die Anwendung zu registrieren ist ein Administrator der app. Nur Administratoren können zu diesem Zeitpunkt in dieser Anwendung anmelden. Um anderen Facebook-Konten authentifizieren, klicken Sie auf **App überprüfen** , und aktivieren Sie **veröffentlichen < Ihr app-Name >** allgemeinen öffentlichen Zugriff mithilfe der Facebook-Authentifizierung aktivieren.

## <a name="secrets"> </a>Hinzufügen von Facebook-Informationen an Ihrer Anwendung

1. Navigieren Sie wieder in der [Azure-Portal]an Ihrer Anwendung. Klicken Sie auf **Einstellungen** > **Authentifizierung / Autorisierung**, und stellen Sie sicher, dass **App-Authentifizierung** **aktiviert**ist.

2. Klicken Sie auf **Facebook**, fügen Sie die App-ID und App geheim Werte, die Sie zuvor abgerufen, optional aktivieren Sie alle Bereiche, die von Ihrer Anwendung benötigt und dann auf **OK**.

    ![][0]

    Standardmäßig App-Dienst bietet Authentifizierung, aber nicht den autorisierten Zugriff auf Ihre Websiteinhalt und -APIs. Sie müssen die Benutzer in Ihren app-Code autorisieren.

3. (Optional) Legen Sie zum Einschränken des Zugriffs auf Ihre Website nur für Benutzer von Facebook authentifiziert **Aktion ausgeführt werden soll, wenn die Anforderung nicht authentifiziert wird** mit **Facebook**. Setzt alle Anfragen authentifiziert werden, und alle nicht authentifizierte Anfragen mit Facebook für die Authentifizierung umgeleitet werden.

4. Anschließend konfigurieren von Authentifizierung, klicken Sie auf **Speichern**.

Sie können nun Facebook für Authentifizierung in Ihrer app verwendet werden soll.

## <a name="related-content"> </a>Verbundener Inhalt

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook-Entwickler]: http://go.microsoft.com/fwlink/p/?LinkId=268286
["Facebook.com"]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure-portal]: https://portal.azure.com/
