<properties 
    pageTitle="So autorisieren Entwicklertools Konten mit Azure Active Directory in Azure-API Management" 
    description="Erfahren Sie, wie Benutzer per Azure Active Directory-API Management autorisieren." 
    services="api-management" 
    documentationCenter="API Management" 
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

# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>So autorisieren Entwicklertools Konten mit Azure Active Directory in Azure-API Management


## <a name="overview"></a>(Übersicht)
Dieser Anleitung erfahren Sie, wie Sie Zugriff auf die Entwicklerportal für alle Benutzer in ein oder mehrere Azure Active Verzeichnisse zu aktivieren. Dieses Handbuch zeigt auch zum Verwalten von Benutzergruppen Azure Active Directory durch Hinzufügen von externen Gruppen, die der Benutzer eines Azure Active Directory enthalten.

>Um die Schritte in diesem Handbuch ausführen müssen Sie zuerst eine Azure Active Directory in der Anwendung erstellt haben.

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>So autorisieren Azure Active Directory mithilfe von Entwicklertools Konten

Um anzufangen, klicken Sie auf **Verwalten** im klassischen Azure-Portal für Ihre API Verwaltungsdienst. Dadurch gelangen Sie zum Publisher-API Verwaltungsportal.

![Publisher-portal][api-management-management-console]

>Wenn Sie eine Instanz der API Management-Dienst noch nicht erstellt haben, finden Sie unter [Erstellen einer API Management Service-Instanz][] , in die [Erste Schritte mit Azure-API Management][] Lernprogramm.

Klicken Sie im Menü " **API Management** " auf der linken Seite auf **Sicherheit** , und klicken Sie auf **Externe Identitäten**.

![Externe Identitäten][api-management-security-external-identities]

Klicken Sie auf **Azure-Active Directory**. Notieren Sie den **URL umleiten** und kehren über zu Azure Active Directory im klassischen Azure-Portal.

![Externe Identitäten][api-management-security-aad-new]

Klicken Sie auf die Schaltfläche **Hinzufügen** , um eine neue Azure Active Directory-Anwendung zu erstellen, und wählen Sie **eine Anwendung, die zur Entwicklung von meinem Unternehmen hinzufügen**.

![Fügen Sie neue Azure Active Directory-Anwendung][api-management-new-aad-application-menu]

Geben Sie einen Namen für die Anwendung, wählen Sie **Web-Anwendung und/oder Web-API**aus, und klicken Sie auf die Schaltfläche Weiter.

![Neue Azure Active Directory-Anwendung][api-management-new-aad-application-1]

Geben Sie für **einmaliges Anmelden URL**die URL anmelden, der Ihre Entwicklerportal ein. In diesem Beispiel wird die **Anmeldung URL** `https://aad03.portal.current.int-azure-api.net/signin`. 

Geben Sie für die **App-ID-URL**entweder die Standarddomäne oder eine benutzerdefinierte Domäne für die Azure Active Directory und damit eine eindeutige Zeichenfolge angefügt werden soll. In diesem Beispiel die Standarddomäne der **https://contoso5api.onmicrosoft.com** und des Suffixes **/api/API** angegeben wird.

![Eigenschaften der neuen Azure Active Directory-Anwendung][api-management-new-aad-application-2]

Klicken Sie auf die Schaltfläche Überprüfen, um zu speichern und erstellen die neue Anwendung, und wechseln Sie zur Registerkarte zum Konfigurieren der neuen Anwendungs **Konfigurieren** .

![Neue Azure Active Directory-Anwendung erstellt][api-management-new-aad-app-created]

Wenn mehrere Azure Active Directory abgelegt werden für diese Anwendung verwendet werden soll, klicken Sie auf **Ja** , für die **Anwendung wird mit mehreren Mandanten**. Die Standardeinstellung ist **keine**.

![Anwendung wird mit mehreren Mandanten][api-management-aad-app-multi-tenant]

Kopieren Sie die **URL umleiten** aus dem **Azure Active Directory** -Abschnitt der Registerkarte **Externe Identitäten** im Portal Publisher, und fügen Sie ihn in das Textfeld **URL Antworten** . 

![Antwort-URL][api-management-aad-reply-url]

Führen Sie einen Bildlauf an das Ende der Registerkarte konfigurieren, wählen Sie aus der Dropdownliste **Berechtigungen der Anwendung** , und aktivieren Sie **die Directory Daten lesen**.

![Von Anwendungsberechtigungen][api-management-aad-app-permissions]

Wählen Sie aus der Dropdownliste **Berechtigungen der Stellvertretung** ein, und aktivieren Sie **Anmelden aktivieren, und Lesen Sie Benutzer Profile**.

![Delegieren von Berechtigungen][api-management-aad-delegated-permissions]

>Weitere Informationen zur Anwendung und delegierte Berechtigungen finden Sie unter [Zugreifen auf das Graph-API][].

Kopieren Sie die **Client-Id** in die Zwischenablage.

![Client-Id][api-management-aad-app-client-id]

Wechseln Sie zurück zu Publisher-Portal an, und fügen Sie in der **Client-Id** aus der Anwendungskonfiguration Azure Active Directory kopiert.

![Client-Id][api-management-client-id]

Wechseln Sie zurück zur Azure-Active Directory-Konfiguration, und klicken Sie auf die **Dauer wählen** Dropdown-Liste im Abschnitt **Tasten** , und geben Sie ein Intervall. In diesem Beispiel wird **1 Jahr** verwendet.

![Schlüssel][api-management-aad-key-before-save]

Klicken Sie auf **Speichern** , um die Konfiguration zu speichern, und zeigen Sie die Taste. Kopieren Sie die Taste in die Zwischenablage.

>Notieren Sie sich dieses Schlüssels. Nachdem Sie das Fenster Azure Active Directory-Konfiguration schließen, kann die Taste erneut angezeigt werden.

![Schlüssel][api-management-aad-key-after-save]

Wechseln Sie zurück zu Publisher-Portal an, und fügen Sie die Taste in das Textfeld **Client geheim** .

![Geheim Client][api-management-client-secret]

**Zulässige Mandanten** gibt an, welche Verzeichnisse Zugriff auf die APIs der API Management Services-Instanz haben. Geben Sie die Domänen der Instanzen Azure Active Directory, die Sie Zugriff gewähren möchten. Sie können mehrere Domänen mit Zeilenumbrüche, Leerzeichen oder Kommas trennen.

![Zulässige Mandanten][api-management-client-allowed-tenants]

Im Abschnitt **Zulässige Mandanten** können mehrere Domänen angegeben werden muss. Vor dem jeder Benutzer über eine andere Domäne als die ursprüngliche Domäne anmelden kann, in dem die Anwendung registriert wurde, muss ein globaler Administrator der anderen Domäne für die Anwendung zu Access Directory Daten erteilt. Um die Berechtigung erteilen, muss ein globaler Administrator melden Sie sich bei der Anwendung, und klicken Sie auf **annehmen**. Im folgenden Beispiel `miaoaad.onmicrosoft.com` hinzugefügt wurde **Zulässige Mandanten** und ein globaler Administrator aus dieser Domäne ist zum ersten Mal anmelden.

![Berechtigungen][api-management-permissions-form]

>Wenn ein nicht-globalen Administrator versucht, melden Sie sich, bevor Sie von einem globalen Administrator die entsprechende Berechtigung erteilt wird, die Anmeldung schlägt fehl, und ein Fehler wird angezeigt.

Nachdem Sie die gewünschte Konfiguration angegeben ist, klicken Sie auf **Speichern**.

![Speichern][api-management-client-allowed-tenants-save]

Nachdem Sie die Änderungen gespeichert haben, können Benutzer in der angegebenen Azure Active Directory in der Entwicklerportal melden Sie sich anhand der Schritte in [Melden Sie sich bei der Entwicklerportal Azure-Active Directory-Konto verwenden][].

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Zum Hinzufügen eines externen Azure Active Directory-Gruppe

Nach dem Aktivieren des Zugriffs für Benutzer in einer Azure-Active Directory, können Sie in API Management vereinfacht die Zuordnung der Entwickler in der Gruppe mit der gewünschten Produkte verwalten Azure-Active Directory-Gruppen hinzufügen.

> Um eine externe Azure Active Directory-Gruppe konfigurieren zu können, muss Azure Active Directory zuerst auf der Registerkarte Identitäten konfiguriert werden nach dem Verfahren aus dem vorherigen Abschnitt. 

Externe Azure-Active Directory-Gruppen werden auf der Registerkarte **Sichtbarkeit** des Produkts hinzugefügt, für die Sie in der Gruppe Zugriff gewähren möchten. Klicken Sie auf **Products**, und klicken Sie dann auf den Namen des gewünschten Produkts.

![Produkt konfigurieren][api-management-configure-product]

Wechseln Sie zur Registerkarte **Sichtbarkeit** , und klicken Sie auf **Gruppen aus Azure Active Directory hinzufügen**.

![Hinzufügen von Gruppen][api-management-add-groups]

Wählen Sie den **Azure-Active Directory-Mandanten** aus der Dropdownliste aus, und geben Sie den Namen der gewünschten Gruppe in den **Gruppen** Textfeld hinzugefügt werden soll.

![Wählen Sie Gruppe aus.][api-management-select-group]

Dieser Gruppenname kann in der Liste **Gruppen** für Ihre Azure Active Directory gefunden werden, wie im folgenden Beispiel dargestellt.

![Liste der Azure-Active Directory-Gruppen][api-management-aad-groups-list]

Klicken Sie auf **Hinzufügen** , um überprüfen Sie den Namen der Gruppe, und fügen Sie die Gruppe. In diesem Beispiel die **Contoso 5 Entwickler** wird externen Gruppe hinzugefügt. 

![Gruppe hinzugefügt][api-management-aad-group-added]

Klicken Sie auf **Speichern** , um die neue Gruppenauswahl zu speichern.

Nachdem eine Azure Azure Active Directory-Gruppe aus einem Produkt konfiguriert wurde, steht auf der Registerkarte **Sichtbarkeit** für andere Produkte in die Instanz des Management-API geprüft werden soll.

Um zu überprüfen, und konfigurieren Sie die Eigenschaften für externe Gruppen ein, sobald diese hinzugefügt wurden, klicken Sie auf den Namen der Gruppe auf der Registerkarte **Gruppen** .

![Verwalten von Gruppen][api-management-groups]

Hier können Sie den **Namen** und die **Beschreibung** der Gruppe bearbeiten.

![Bearbeiten der Gruppe][api-management-edit-group]

Benutzer aus konfigurierten Azure Active Directory können melden Sie sich bei der Entwicklerportal und der Ansicht und alle Gruppen für die Sichtbarkeit angezeigt anhand der Anweisungen im folgenden Abschnitt abonnieren.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Zum Anmelden bei der Entwicklerportal mit einem Azure Active Directory-Konto

Wenn Sie sich bei der Entwicklerportal so konfiguriert, dass in den vorherigen Abschnitten Azure-Active Directory-Konto verwenden, öffnen Sie ein neues Browserfenster mit dem **Anmelden URL** aus der Anwendungskonfiguration Active Directory, und klicken Sie auf **Azure Active Directory**.

![Entwicklerportal][api-management-dev-portal-signin]

Geben Sie die Anmeldeinformationen eines der Benutzer in Ihrer Azure-Active Directory, und klicken Sie auf **Anmelden**.

![Anmelden][api-management-aad-signin]

Sie möglicherweise mit einem Formular Registrierungsinformationen aufgefordert werden, wenn alle zusätzlichen Informationen erforderlich ist. Füllen Sie das Anmeldeformular aus, und klicken Sie auf **Anmelden**.

![Registrierung][api-management-complete-registration]

Ihre Benutzer wird jetzt in der Entwicklerportal für Ihre API Management Service-Instanz protokolliert.

![Registrierung abgeschlossen][api-management-registration-complete]



[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

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
[Zugreifen auf das Diagramm-API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Melden Sie sich bei der Entwicklerportal mit einem Azure Active Directory-Konto]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

