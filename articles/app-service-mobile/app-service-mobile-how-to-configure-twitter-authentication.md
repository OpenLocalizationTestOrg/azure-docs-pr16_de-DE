<properties
    pageTitle="So konfigurieren Sie die Twitter-Authentifizierung für Ihre App Services-Anwendung"
    description="Informationen Sie zum Konfigurieren der Twitter-Authentifizierung für Ihre App Services-Anwendung."
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

# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>So konfigurieren Sie Ihre App-verwaltungsdienstanwendung Twitter Login verwenden

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema wird gezeigt, wie Azure App-Dienst Twitter als eine Authentifizierungsanbieter verwenden konfigurieren.

Um das Verfahren in diesem Thema ausführen zu können, müssen Sie Twitter-Konto verfügen, die eine überprüft e-Mail-Adresse und Telefon Zahl enthält. Wechseln Sie zum Erstellen einer neuen Twitter-Kontos zu <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"> </a>Twitter Registrieren Ihrer Anwendung


1. Melden Sie sich bei der [Azure-Portal], und navigieren Sie zu Ihrer Anwendung. Kopieren Sie die **URL**ein. Sie verwenden diese so konfigurieren Sie Ihre app Twitter.

2. Navigieren Sie zu der Website [Twitter-Entwickler] , melden Sie sich mit Ihrer Twitter-Konto-Anmeldeinformationen, und klicken Sie auf **Neue App erstellen**.

3. Geben Sie den **Namen** und eine **Beschreibung** für Ihre neue app. Fügen Sie Ihrer Anwendung **URL** für den Wert für die **Website** ein. Fügen Sie dann für den **Rückruf-URL**, die **Rückruf-URL** , die Sie zuvor kopiert haben. Dies ist Ihre Mobile-App Gateway durch den Pfad, _/.auth/login/twitter/callback_angefügt. Beispielsweise `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Stellen Sie sicher, dass Sie das HTTPS-Schema verwenden.

3.  Klicken Sie am Fuß der Seite lesen und annehmen. Klicken Sie dann auf **Erstellen Ihrer Twitter-Anwendung**. Dadurch wird die app-zeigt die Anwendungsdetails registriert.

4. Klicken Sie auf der Registerkarte **Einstellungen** , aktivieren Sie **diese Anwendung verwendet werden soll, melden Sie sich mit Twitter zulassen**, und klicken Sie auf **Update-Einstellungen**.

5. Wählen Sie die Registerkarte **Tasten und Access Token** . Notieren Sie die Werte der **Consumer Schlüssel (API-Schlüssel)** und **Consumer geheim (API geheim)**.

    > [AZURE.NOTE] Die Consumer geheim ist eine wichtige Sicherheitsanmeldeinformationen. Teilen Sie diese geheim mit jedem oder verteilen Sie es mit der app nicht.


## <a name="secrets"> </a>Hinzufügen Twitter-Informationen an Ihrer Anwendung

13. Navigieren Sie wieder in der [Azure-Portal]an Ihrer Anwendung. Klicken Sie auf **Einstellungen**und dann **Authentifizierung / Autorisierung**.

14. Wenn die Authentifizierung / Autorisierungsfeature nicht aktiviert ist, aktivieren Sie die wechseln, **Klicken Sie auf**.

15. Klicken Sie auf **Twitter**. Fügen Sie in der App-ID und App geheim Werte, die Sie zuvor für Ihren Kunden. Klicken Sie dann auf **OK**.

    ![][1]

    Standardmäßig App-Dienst bietet Authentifizierung, aber nicht den autorisierten Zugriff auf Ihre Websiteinhalt und -APIs. Sie müssen die Benutzer in Ihren app-Code autorisieren.

17. (Optional) Legen Sie zum Einschränken des Zugriffs auf Ihre Website für nur durch Twitter authentifizierte Benutzer **bei der Anforderung nicht authentifiziert ist auszuführende Aktion** auf **Twitter-**ein. Setzt alle Anfragen authentifiziert werden, und alle nicht authentifizierte Anfragen werden auf Twitter umgeleitet, für die Authentifizierung.

17. Klicken Sie auf **Speichern**.

Sie können nun Twitter für Authentifizierung in Ihrer app verwendet werden soll.

## <a name="related-content"> </a>Verbundener Inhalt

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter-Entwickler]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure-portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
