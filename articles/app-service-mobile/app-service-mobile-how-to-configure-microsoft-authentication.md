<properties
    pageTitle="So konfigurieren Sie Microsoft Account Authentifizierung für Ihre App Services-Anwendung"
    description="Informationen Sie zum Microsoft Account Authentifizierung für Ihre App Services-Anwendung zu konfigurieren."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>So konfigurieren Sie Ihre App-verwaltungsdienstanwendung Microsoft Account Login verwenden

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema wird gezeigt, wie Azure App-Dienst verwenden Sie Microsoft Account als Provider Authentifizierung konfigurieren. 

## <a name="register-microsoft-account"> </a>Registrieren Ihre app mit Microsoft-Konto

1. Melden Sie sich bei der [Azure-Portal], und navigieren Sie zu Ihrer Anwendung. Kopieren Sie Ihre **URL**, die später Sie verwenden Ihre app mit Microsoft Account konfigurieren.

2. Navigieren Sie zur Seite [Meine Programme] in der Microsoft Account Developer Center, und melden Sie sich mit Ihrem Microsoft-Konto, falls erforderlich.

3. Klicken Sie auf **app hinzufügen**, und geben Sie einen Anwendungsnamen, und klicken Sie auf die **Anwendung erstellen**.

4. Notieren Sie sich die **ID der Anwendung**, da Sie es später benötigen. 

5. Klicken Sie unter "Plattformen", klicken Sie auf **Add Platform** , und wählen Sie "Web" aus.

6. Klicken Sie unter "Umleiten URIs" Geben Sie den Endpunkt für eine Anwendung an, und klicken Sie auf **Speichern**. 
 
    >[AZURE.NOTE]Der URI-Umleitung ist die URL Ihrer Anwendung durch den Pfad, _/.auth/login/microsoftaccount/callback_angefügt. Beispielsweise `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
    >Stellen Sie sicher, dass Sie das HTTPS-Schema verwenden.

7. Klicken Sie unter "Anwendung Kennwörter", klicken Sie auf **Neues Kennwort generieren**. Notieren Sie den Wert, der angezeigt wird. Nachdem Sie die Seite verlassen, wird es nicht wieder angezeigt werden.


    > [AZURE.IMPORTANT] Das Kennwort ist ein wichtiger Sicherheitsanmeldeinformationen. Teilen Sie das Kennwort mit jedem oder verteilen Sie einer Clientanwendung nicht.

## <a name="secrets"> </a>Informationen Ihre App-verwaltungsdienstanwendung Microsoft-Konto hinzufügen

1. Wieder im [Portal Azure], navigieren Sie zu Ihrer Anwendung, klicken Sie auf **Einstellungen** > **Authentifizierung / Autorisierung**.

2. Wenn die Authentifizierung / Autorisierungsfeature nicht aktiviert ist, **Klicken Sie auf**zu wechseln.

3. Klicken Sie auf **Microsoft-Konto**. Fügen Sie die Anwendung-ID und Ihr Kennwort ein Werte, die Sie zuvor abgerufen, und aktivieren Sie alle Bereiche, die die Anwendung erforderlich ist optional. Klicken Sie dann auf **OK**.

    ![][1]

    Standardmäßig App-Dienst bietet Authentifizierung, aber nicht den autorisierten Zugriff auf Ihre Websiteinhalt und -APIs. Sie müssen die Benutzer in Ihren app-Code autorisieren.

4. (Optional) Legen Sie zum Einschränken des Zugriffs auf Ihre Website nur für Benutzer von Microsoft-Konto authentifiziert **Aktion ausgeführt werden soll, wenn die Anforderung nicht authentifiziert ist** auf **Microsoft-Konto**ein. Setzt alle Anfragen authentifiziert werden, und alle nicht authentifizierte Anfragen mit Microsoft-Konto für die Authentifizierung umgeleitet werden.

5. Klicken Sie auf **Speichern**.

Sie können nun Microsoft Account für die Authentifizierung in Ihrer app zu verwenden.

## <a name="related-content"> </a>Verbundener Inhalt

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Meine Programme]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure-portal]: https://portal.azure.com/
