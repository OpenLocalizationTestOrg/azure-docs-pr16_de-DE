<properties
    pageTitle="Authentifizierung auf Android-Gerät mit Mobile-Apps hinzufügen | Azure App-Verwaltungsdienst"
    description="Erfahren Sie, wie Mobile-Apps in Azure-App-Dienst verwenden, für die Benutzerauthentifizierung der Android app mithilfe einer Vielzahl von Identitätsanbieter, einschließlich Google, Facebook, Twitter und Microsoft."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-android-app"></a>Authentifizierung für Ihre app Android hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm hinzufügen Sie Authentifizierung für Android mit einer unterstützten Identitätsanbieter Projekt Schnellstart Aufgabenliste. In diesem Lernprogramm basiert auf das [Erste Schritte mit Mobile-Apps] Lernprogramm, die zuerst ausgeführt werden müssen.

##<a name="a-nameregisteraregister-your-app-for-authentication-and-configure-the-app-service"></a><a name="register"></a>Registrieren Sie Ihre app für die Authentifizierung und konfigurieren Sie der App-Verwaltungsdienst

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="a-namepermissionsarestrict-permissions-to-authenticated-users"></a><a name="permissions"></a>Einschränken der Berechtigungen für authentifizierte Benutzer

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

+ Öffnen Sie das Projekt gestellten geplante in Android Studio mit des Lernprogramms [Erste Schritte mit Mobile-Apps]. Klicken Sie im Menü **Ausführen** auf **app ausführen** , und stellen Sie sicher, dass Ausnahmefehler mit einem Statuscode von 401 (nicht autorisiert) ausgelöst wird, nachdem die app gestartet wird.

     Diese Ausnahme liegt daran, dass die app versucht wird, auf die Back-End-als nicht authentifizierte Benutzer zuzugreifen, aber die _TodoItem_ Tabelle jetzt erfordert Authentifizierung eine.

Danach aktualisieren Sie die app zum Authentifizieren von Benutzern vor dem Anfordern von Ressourcen aus dem Mobile-App Back-End.

## <a name="add-authentication-to-the-app"></a>Authentifizierung für die app hinzufügen

[AZURE.INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="a-namecache-tokensacache-authentication-tokens-on-the-client"></a><a name="cache-tokens"></a>Cache Authentifizierungstoken auf dem client

[AZURE.INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie in diesem Lernprogramm Standardauthentifizierung abgeschlossen haben, sollten Sie Sie mit einer der folgenden Lernprogramme fortfahren:

+ [Hinzufügen von Pushbenachrichtigungen zu Ihrer Android-Anwendung](app-service-mobile-android-get-started-push.md) Informationen Sie zum Konfigurieren Ihrer Mobile-App Back-End-um Azure Benachrichtigung Hubs verwenden, um Pushbenachrichtigungen zu senden.

+ [Offlinesynchronisierung für Ihre app Android aktivieren](app-service-mobile-android-get-started-offline-data.md) Erfahren Sie, wie offlinesupport Ihre mithilfe einer Back-End-Mobile-App-app hinzufügen. Offlinesynchronisierung ermöglicht es Endbenutzern zu interagieren mit einer mobile-app&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;auch, wenn keine Verbindung zum Netzwerk vorhanden ist.



<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Erste Schritte mit Mobile-Apps]: app-service-mobile-android-get-started.md
