<properties
    pageTitle="So konfigurieren Sie die Azure Active Directory-Authentifizierung für Ihre App Services-Anwendung"
    description="Informationen Sie zum Konfigurieren von Azure Active Directory-Authentifizierung für Ihre App Services-Anwendung."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
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

# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>So konfigurieren Sie Ihre App-verwaltungsdienstanwendung Azure Active Directory Login verwenden

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema wird gezeigt, wie Azure-App-Services zur Verwendung von Azure Active Directory als Provider Authentifizierung konfigurieren.

## <a name="express"> </a>Konfigurieren Azure Active Directory mithilfe von express-Einstellungen

13. Navigieren Sie in der [Azure-Portal]an Ihrer Anwendung. Klicken Sie auf **Einstellungen**, und klicken Sie dann auf **Authentifizierung**.

14. Wenn die Authentifizierung / Autorisierungsfeature nicht aktiviert ist, aktivieren Sie die wechseln, **Klicken Sie auf**.

15. Klicken Sie auf **Azure Active Directory**, und klicken Sie auf **Express** unter **Verwaltungsmodus**.

16. Klicken Sie auf **OK** , um die Anwendung in Azure Active Directory zu registrieren. Dadurch wird eine neue Registrierung erstellt. Wenn Sie eine vorhandene Registrierung stattdessen auswählen möchten, klicken Sie auf **eine vorhandene app auswählen** , und suchen Sie nach dem Namen einer zuvor erstellten Registrierung in Ihrem Mandanten.
Klicken Sie auf die Registrierung, um es auszuwählen, und klicken Sie auf **OK**. Klicken Sie auf **OK** , klicken Sie auf das Blade der Azure-Active Directory-Einstellungen.

    ![][0]

    Standardmäßig App-Dienst bietet Authentifizierung, aber nicht den autorisierten Zugriff auf Ihre Websiteinhalt und -APIs. Sie müssen die Benutzer in Ihren app-Code autorisieren.

17. (Optional) Legen Sie zum Einschränken des Zugriffs auf Ihre Website für nur durch Azure Active Directory authentifizierte Benutzer **Aktion ausgeführt werden soll, wenn die Anforderung nicht authentifiziert wird** , **Melden Sie sich mit Azure Active Directory**. Setzt alle Anfragen authentifiziert werden, und alle nicht authentifizierte Anfragen an Azure Active Directory für die Authentifizierung umgeleitet werden.

17. Klicken Sie auf **Speichern**.

Sie können nun Azure Active Directory für die Authentifizierung in Ihrer app zu verwenden.

## <a name="advanced"> </a>(Alternative Methode) manuell konfigurieren Azure Active Directory mit erweiterten Einstellungen
Sie können auch Konfiguration Einstellungen bieten manuell. Dies ist die bevorzugte Lösung ist der AAD Mandanten, die, den Sie verwenden möchten, von den Mandanten mit dem Azure Anmelden bei. Um die Konfiguration abzuschließen, müssen Sie zuerst eine Registrierung in Azure Active Directory erstellen, und klicken Sie dann geben Sie einige der Registrierungsdetails App-Dienst.

### <a name="register"> </a>Registrieren Ihrer Anwendung mit Azure Active Directory

1. Melden Sie sich bei der [Azure-Portal], und navigieren Sie zu Ihrer Anwendung. Kopieren Sie die **URL**ein. Sie verwenden diese so konfigurieren Sie Ihre app Azure Active Directory.

3. Melden Sie sich bei der [Azure klassischen Portal] , und navigieren Sie zu **Active Directory**.

    ![][2]

4. Wählen Sie das Verzeichnis aus, und wählen Sie dann auf die Registerkarte **Applications** oben. Klicken Sie am unteren Rand, erstellen eine neue app-Registrierung auf **Hinzufügen** .

5. Klicken Sie auf **eine Anwendung, die zur Entwicklung von meinem Unternehmen hinzufügen**.

6. Klicken Sie im Assistenten zum Hinzufügen von Anwendung geben Sie einen **Namen** für die Anwendung, und klicken Sie auf den Typ des **Web Anwendung und/oder Web-API** . Klicken Sie dann auf, um den Vorgang fortzusetzen.

7. Fügen Sie im Feld **URL-ein melden Sie sich an** die Anwendung-URL, die Sie zuvor kopiert haben. Geben Sie die gleichen URL im Feld **App-ID-URI** aus. Klicken Sie dann auf, um den Vorgang fortzusetzen.

8. Nachdem Sie die Anwendung hinzugefügt wurde, klicken Sie auf die Registerkarte **Konfigurieren** . Bearbeiten der **Antwort URL** unter **einmaliges Anmelden** auf die URL der Anwendung durch den Pfad, _/.auth/login/aad/callback_angefügt werden soll. Beispielsweise `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Stellen Sie sicher, dass Sie das HTTPS-Schema verwenden.

    ![][3]

9. Klicken Sie auf **Speichern**. Kopieren Sie die **Client-ID** für die app. Sie Konfigurieren Ihrer Anwendung dies später verwenden.

10. Klicken Sie in der unteren Befehlsleiste auf **Ansicht Endpunkte**, und klicken Sie dann kopieren Sie die **Föderation Metadaten Dokument** -URL und herunterladen Sie dieses Dokument oder navigieren sie in einem Browser.

11. Innerhalb des Elements Root **EntityDescriptor** , sollten ein **%EntityID** Attribut des Formulars `https://sts.windows.net/` gefolgt von einer bestimmten GUID in Ihrem Mandanten ("Mandanten-ID" bezeichnet). Kopieren Sie diesen Wert: Es dient als Ihre **URL des Herausgebers**. Sie Konfigurieren Ihrer Anwendung dies später verwenden.

### <a name="secrets"> </a>Hinzufügen Azure Active Directory-Informationen an Ihrer Anwendung

13. Navigieren Sie wieder in der [Azure-Portal]an Ihrer Anwendung. Klicken Sie auf **Einstellungen**, und klicken Sie dann auf **Authentifizierung**.

14. Wenn das Feature Authentifizierung/Autorisierung nicht aktiviert ist, aktivieren Sie zu **auf**wechseln.

15. Klicken Sie auf **Azure Active Directory**, und klicken Sie dann auf **Erweitert** klicken Sie unter **Verwaltungsmodus**. Fügen Sie in der Client-ID und Herausgeber URL-Wert die Sie zuvor abgerufen. Klicken Sie dann auf **OK**.

    ![][1]

    Standardmäßig App-Dienst bietet Authentifizierung, aber nicht den autorisierten Zugriff auf Ihre Websiteinhalt und -APIs. Sie müssen die Benutzer in Ihren app-Code autorisieren.

17. (Optional) Legen Sie zum Einschränken des Zugriffs auf Ihre Website für nur durch Azure Active Directory authentifizierte Benutzer **Aktion ausgeführt werden soll, wenn die Anforderung nicht authentifiziert wird** , **Melden Sie sich mit Azure Active Directory**. Setzt alle Anfragen authentifiziert werden, und alle nicht authentifizierte Anfragen an Azure Active Directory für die Authentifizierung umgeleitet werden.

17. Klicken Sie auf **Speichern**.

Sie können nun Azure Active Directory für die Authentifizierung in Ihrer app zu verwenden.

## <a name="optional-configure-a-native-client-application"></a>(Optional) Konfigurieren einer systemeigenen Clientanwendung

Azure-Active Directory können Sie auch so registrieren systemeigener Clients, die bessere Kontrolle über die Berechtigungen, die Zuordnung bereitstellt. Sie benötigen, wenn Sie mit einer Bibliothek beispielsweise die **Active Directory-Authentifizierungsbibliothek**Benutzernamen durchführen möchten.

1. Navigieren Sie zu **Active Directory** im [Azure klassischen Portal].

2. Wählen Sie das Verzeichnis aus, und wählen Sie dann auf die Registerkarte **Applications** oben. Klicken Sie am unteren Rand, erstellen eine neue app-Registrierung auf **Hinzufügen** .

3. Klicken Sie auf **eine Anwendung, die zur Entwicklung von meinem Unternehmen hinzufügen**.

4. Klicken Sie im Assistenten zum Hinzufügen von Anwendung geben Sie einen **Namen** für die Anwendung, und klicken Sie auf den Typ **Native Client-Anwendung** . Klicken Sie dann auf, um den Vorgang fortzusetzen.

5. Geben Sie im Feld **Umleiten URI** Ihres Standorts _/.auth/login/done_ Endpunkt, mit dem HTTPS-Schema aus. Dieser Wert sollte ähnlich wie _https://contoso.azurewebsites.net/.auth/login/done_sein. Wenn eine Windows-Anwendung zu erstellen, verwenden Sie stattdessen das [Paket SID](app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) als URI an.

6. Sobald die ursprüngliche Anwendung hinzugefügt wurde, klicken Sie auf die Registerkarte **Konfigurieren** . Suchen Sie nach der **Client-ID** , und notieren Sie sich diesen Wert.

7. Einen Bildlauf nach unten bis zum Abschnitt **Berechtigungen für andere Programme** , und klicken Sie auf **Anwendung hinzufügen**.

8. Suchen Sie nach der Anwendung, die Sie zuvor registriert, und klicken Sie auf das Plussymbol. Klicken Sie dann auf das Kontrollkästchen, um das Dialogfeld zu schließen. Wenn die Webanwendung gefunden, navigieren Sie zu der Registrierung, und fügen Sie eine neue Antwort-URL (z. B. die HTTP-Version von Ihrem aktuellen URL) werden kann, klicken Sie auf Speichern, und wiederholen Sie diese Schritte: sollte die Anwendung in der Liste angezeigt.

9. Klicken Sie auf den neuen Eintrag, den Sie soeben hinzugefügt haben, Öffnen der Dropdownliste **Berechtigungen delegiert** , und wählen Sie **Access (AppName)**. Klicken Sie dann auf **Speichern**.

Jetzt haben Sie eine native Client-Anwendung konfiguriert, die Ihre App-verwaltungsdienstanwendung zugreifen können.

## <a name="related-content"> </a>Verbundener Inhalt

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[Azure-portal]: https://portal.azure.com/
[Azure klassischen-portal]: https://manage.windowsazure.com/
[alternative method]:#advanced
