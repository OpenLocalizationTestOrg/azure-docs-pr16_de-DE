<properties
    pageTitle="So konfigurieren Sie die Google-Authentifizierung für Ihre App Services-Anwendung"
    description="Informationen Sie zum Konfigurieren von Google-Authentifizierung für Ihre App Services-Anwendung."
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

# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>So konfigurieren Sie Ihre App-verwaltungsdienstanwendung Google Login verwenden

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema wird gezeigt, wie Azure App-Dienstes zur Verwendung von Google als Provider Authentifizierung konfigurieren.

Um das Verfahren in diesem Thema ausführen zu können, müssen Sie Gmail-Konto verfügen, die eine überprüft e-Mail-Adresse enthält. Wechseln Sie zum Erstellen eines neuen Gmail-Kontos zu [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"> </a>Google Registrieren Ihrer Anwendung

1. Melden Sie sich bei der [Azure-Portal], und navigieren Sie zu Ihrer Anwendung. Kopieren Sie die **URL**, die Sie später so konfigurieren Sie die Google-app verwenden.

2. Navigieren Sie zu der Website von [Google-apis](http://go.microsoft.com/fwlink/p/?LinkId=268303) , melden Sie sich mit Ihrer Gmail-Kontoanmeldeinformationen, klicken Sie auf **Projekt erstellen**, geben Sie einen **Projektnamen ein**, und klicken Sie auf **Erstellen**.

3. Klicken Sie unter **Sozialen APIs** auf **Google + API** und klicken Sie dann auf **Aktivieren**.

4. Im linken Navigationsbereich, **Anmeldeinformationen** > **OAuth Zustimmung Bildschirm**, dann wählen Sie Ihre **e-Mail-Adresse**aus, geben Sie einen **Produktnamen**, und klicken Sie auf **Speichern**.

5. Klicken Sie auf der Registerkarte **Anmeldeinformationen** auf **Anmeldeinformationen erstellen** > **OAuth-Client-ID**, und klicken Sie dann select **Web-Anwendung**.

6. Fügen Sie die App- **URL** Sie zuvor in **JavaScript Ursprung autorisiert**kopiert haben, und fügen Sie Ihre URI-Umleitung in **Umleiten URI berechtigt**. Die Umleitung ist URI die URL Ihrer Anwendung durch den Pfad, _/.auth/login/google/callback_angefügt. Beispielsweise `https://contoso.azurewebsites.net/.auth/login/google/callback`. Stellen Sie sicher, dass Sie das HTTPS-Schema verwenden. Klicken Sie dann auf **Erstellen**.

7. Klicken Sie auf dem nächsten Bildschirm Notieren Sie die Werte der Client-ID und Client geheim.


    > [AZURE.IMPORTANT]
    Der Client-Schlüssel ist eine wichtige Sicherheitsanmeldeinformationen. Teilen Sie diese geheim mit jedem oder verteilen Sie einer Clientanwendung nicht.


## <a name="secrets"> </a>Hinzufügen von Google-Informationen an Ihrer Anwendung

8. Navigieren Sie wieder in der [Azure-Portal]an Ihrer Anwendung. Klicken Sie auf **Einstellungen**und dann **Authentifizierung / Autorisierung**.

9. Wenn die Authentifizierung / Autorisierungsfeature nicht aktiviert ist, aktivieren Sie die wechseln, **Klicken Sie auf**.

10. Klicken Sie auf **Google**. Fügen Sie die App-ID und App geheim Werte, die Sie zuvor abgerufen, und aktivieren Sie alle Bereiche, die die Anwendung erforderlich ist optional. Klicken Sie dann auf **OK**.

    ![][1]

    Standardmäßig App-Dienst bietet Authentifizierung, aber nicht den autorisierten Zugriff auf Ihre Websiteinhalt und -APIs. Sie müssen die Benutzer in Ihren app-Code autorisieren.

17. (Optional) Legen Sie zum Einschränken des Zugriffs auf Ihre Website für nur von Google authentifizierte Benutzer **bei der Anforderung nicht authentifiziert ist auszuführende Aktion** auf **Google**ein. Setzt alle Anfragen authentifiziert werden, und alle nicht authentifizierte Anfragen werden auf Google umgeleitet, für die Authentifizierung.

12. Klicken Sie auf **Speichern**.

Sie können nun zum Verwenden von Google für die Authentifizierung in Ihrer app.

## <a name="related-content"> </a>Verbundener Inhalt

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure-portal]: https://portal.azure.com/

