<properties
    pageTitle="Hinzufügen von Authentifizierung unter iOS mit Azure Mobile-Apps"
    description="Erfahren Sie, wie der app iOS mithilfe einer Vielzahl von Identitätsanbieter, einschließlich AAD, Google, Facebook, Twitter und Microsoft Benutzerauthentifizierung mit Azure Mobile-Apps."
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-ios-app"></a>Authentifizierung für Ihre app für iOS hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In diesem Lernprogramm hinzufügen Sie Authentifizierung für [iOS Symbolleiste starten] Projekt mithilfe eines unterstützten Identitätsanbieter. In diesem Lernprogramm wird das Lernprogramm [Starten iOS-Symbolleiste] anhand der zuerst ausgeführt werden müssen.

##<a name="a-nameregisteraregister-your-app-for-authentication-and-configure-the-app-service"></a><a name="register"></a>Registrieren Sie Ihre app für die Authentifizierung und konfigurieren Sie der App-Verwaltungsdienst

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="a-namepermissionsarestrict-permissions-to-authenticated-users"></a><a name="permissions"></a>Einschränken der Berechtigungen für authentifizierte Benutzer

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Drücken Sie in Xcode, **Ausführen** , um die app zu starten. Eine Ausnahme wird ausgelöst, da die app versucht wird, auf die Back-End-als nicht authentifizierte Benutzer zuzugreifen, aber _TodoItem_ Tabelle jetzt erfordert Authentifizierung eine.

##<a name="a-nameadd-authenticationaadd-authentication-to-app"></a><a name="add-authentication"></a>Authentifizierung für app hinzufügen

[AZURE.INCLUDE [app-service-mobile-ios-authenticate-app](../../includes/app-service-mobile-ios-authenticate-app.md)]


<!-- URLs. -->

[Schnellstart für iOS]: app-service-mobile-ios-get-started.md

[Azure portal]: https://portal.azure.com
