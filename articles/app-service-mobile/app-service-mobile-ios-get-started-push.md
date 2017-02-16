<properties
    pageTitle="Hinzufügen von Pushbenachrichtigungen zu iOS-App mit Azure Mobile-Apps"
    description="Informationen Sie zum Azure Mobile-Apps zu verwenden, um Pushbenachrichtigungen zu Ihrer iOS-Anwendung zu senden."
    services="app-service\mobile"
    documentationCenter="ios"
    manager="yochayk"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="yuaxu"/>


# <a name="add-push-notifications-to-your-ios-app"></a>Hinzufügen von Pushbenachrichtigungen zu Ihrer App für iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>(Übersicht)
In diesem Lernprogramm fügen Sie Pushbenachrichtigungen des Projekts [iOS Symbolleiste starten] , damit eine Pushbenachrichtigung an das Gerät gesendet werden, jedes Mal, wenn ein Datensatz eingefügt wird.

Wenn Sie die heruntergeladene Schnellstart Server Project nicht verwenden, benötigen Sie das Pushbenachrichtigungen Benachrichtigung Erweiterungspaket. Weitere Informationen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Mobile-Apps Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

[IOS Simulator Pushbenachrichtigungen nicht unterstützt](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Sie benötigen eine physische iOS-Gerät und eine [Mitgliedschaft Apple Developer-Programm](https://developer.apple.com/programs/ios/).

##<a name="a-nameconfigure-hubaconfigure-notification-hub"></a><a name="configure-hub"></a>Konfigurieren Sie die Benachrichtigung Hub

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="a-idregisteraregister-app-for-push-notifications"></a><a id="register"></a>Register-app für Pushbenachrichtigungen

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Konfigurieren von Azure um Pushbenachrichtigungen zu senden.

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a name="a-idupdate-serveraupdate-backend-to-send-push-notifications"></a><a id="update-server"></a>Aktualisieren Sie die Back-End-, um Pushbenachrichtigungen zu senden.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a name="a-idadd-pushaadd-push-notifications-to-app"></a><a id="add-push"></a>Hinzufügen von Pushbenachrichtigungen zu app

[AZURE.INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a name="a-idtestatest-push-notifications"></a><a id="test"></a>Test-Pushbenachrichtigungen

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

##<a name="a-idmoreamore"></a><a id="more"></a>Weitere

* Vorlagen bieten Ihnen Flexibilität Plattformen schiebt und lokalisierten Push-Vorgänge senden. [So verwenden Sie iOS-Client-Bibliothek für Mobile-Apps Azure](app-service-mobile-ios-how-to-use-client-library.md#templates) wird gezeigt, wie Vorlagen registrieren.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[Schnellstart für iOS]: app-service-mobile-ios-get-started.md
