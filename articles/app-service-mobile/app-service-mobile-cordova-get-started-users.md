<properties
    pageTitle="Hinzufügen von Authentifizierung auf Apache Cordova mit Mobile-Apps | Azure App-Verwaltungsdienst"
    description="Erfahren Sie, wie Mobile-Apps in Azure-App-Dienst verwenden, für die Benutzerauthentifizierung der app Apache Cordova mithilfe einer Vielzahl von Identitätsanbieter, einschließlich Google, Facebook, Twitter und Microsoft."
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-apache-cordova-app"></a>Authentifizierung für Ihre app Apache Cordova hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
    
## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wird Authentifizierung zum Schnellstart Aufgabenliste auf Apache Cordova mithilfe einer unterstützten Identitätsanbieter Projekt hinzufügen. In diesem Lernprogramm basiert auf das [Erste Schritte mit Mobile-Apps] Lernprogramm, die zuerst ausgeführt werden müssen.

##<a name="a-nameregisteraregister-your-app-for-authentication-and-configure-the-app-service"></a><a name="register"></a>Registrieren Sie Ihre app für die Authentifizierung und konfigurieren Sie der App-Verwaltungsdienst

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Zeigen Sie ein Video mit ähnlichen Schritten](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

##<a name="a-namepermissionsarestrict-permissions-to-authenticated-users"></a><a name="permissions"></a>Einschränken der Berechtigungen für authentifizierte Benutzer

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nun können Sie überprüfen, dass anonymer Zugriff auf Ihre Back-End-deaktiviert wurde. Öffnen Sie in Visual Studio das Projekt, das Sie erstellt haben, wenn Sie das [Erste Schritte mit Mobile-Apps], Lernprogramm abgeschlossen und dann führen Sie die Anwendung im **Google Android Emulator** , und stellen Sie sicher, dass ein unerwarteter Fehler bei der Verbindung angezeigt wird, nachdem die app gestartet.

Aktualisieren Sie als Nächstes die Authentifizierung von Benutzern vor dem Anfordern von Ressourcen aus dem Mobile-App Back-End-app.

##<a name="a-nameadd-authenticationaadd-authentication-to-the-app"></a><a name="add-authentication"></a>Authentifizierung für die app hinzufügen

1. Öffnen Sie das Projekt in **Visual Studio**, und öffnen Sie dann die `www/index.html` Datei zur Bearbeitung.

2. Suchen Sie nach der `Content-Security-Policy` Metatag im Abschnitt am.  Sie müssen den OAuth Host zur Liste der zulässigen Quellen hinzufügen.

  	| Anbieter               | Name des SDK-Anbieters | OAuth Host                  |
  	| :--------------------- | :---------------- | :-------------------------- |
  	| Azure-Active Directory | AAD               | https://Login.Windows.NET   |
  	| Facebook               | Facebook          | https://www.Facebook.com    |
  	| Google                 | Google            | https://Accounts.Google.com |
  	| Microsoft              | MicrosoftAccount  | https://Login.Live.com      |
  	| Twitter                | Twitter           | https://API.twitter.com     |

    Beispiel für Inhalte--Sicherheitsrichtlinie (für Azure Active Directory implementiert) sieht wie folgt aus:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.windows.net https://yourapp.azurewebsites.net; style-src 'self'">

    Ersetzen Sie `https://login.windows.net` mit dem OAuth Host aus der obigen Tabelle.  Weitere Informationen zu diesem Metatag finden Sie in der [Dokumentation Inhalt Sicherheitsrichtlinie] .

    Beachten Sie, dass einige Authentifizierungsanbieter nicht, dass Inhalt Sicherheitsrichtlinie ändert erforderlich ist, wenn auf entsprechenden mobilen Geräten verwendet.  Beispielsweise werden keine Inhalte Sicherheitsrichtlinie Änderungen erforderlich bei Verwendung von Google-Authentifizierung auf einem Android-Gerät.

3. Öffnen der `www/js/index.js` Datei zur Bearbeitung, suchen Sie nach der `onDeviceReady()` Methode, und fügen Sie unter der Erstellung von Client Code Folgendes:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Beachten Sie, dass dieser Code den vorhandenen Code ersetzen, der den Tabellenbezug erstellt und die Benutzeroberfläche aktualisiert.

    Die Methode login() beginnt Authentifizierung mit den Anbieter. Die Methode login() ist eine asynchrone-Funktion, die eine Zusicherung JavaScript zurückgibt.  Der Rest der Initialisierung ist innerhalb der Antwort Versprechen platziert, sodass sie bis zum Abschluss der Methode login() nicht ausgeführt wird.

4. Ersetzen Sie den Code, die Sie soeben hinzugefügt haben, `SDK_Provider_Name` mit dem Namen von Ihrem Anbieter Login. Verwenden Sie für Azure Active Directory, z. B. `client.login('aad')`.

4. Führen Sie Ihr Projekt.  Wenn das Projekt Initialisierung abgeschlossen hat, wird eine Anwendung OAuth Anmeldeseite für den ausgewählten Authentifizierungsanbieter angezeigt.

##<a name="a-namenext-stepsanext-steps"></a><a name="next-steps"></a>Nächste Schritte

* Erfahren Sie weitere [Informationen zur Authentifizierung] mit Azure-App-Verwaltungsdienst.
* Fortsetzen des Lernprogramms durch Hinzufügen von [Pushbenachrichtigungen] zu Ihrer Anwendung Apache Cordova an.

Erfahren Sie, wie die SDKs verwenden.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- URLs. -->
[Erste Schritte mit Mobile-Apps]: app-service-mobile-cordova-get-started.md
[Inhalt der Sicherheitsrichtlinie Dokumentation]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Pushbenachrichtigungen]: app-service-mobile-cordova-get-started-push.md
[Informationen zur Authentifizierung]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md 
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
