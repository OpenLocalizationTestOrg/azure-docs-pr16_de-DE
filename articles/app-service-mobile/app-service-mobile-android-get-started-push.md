<properties
    pageTitle="Hinzufügen von Pushbenachrichtigungen zu Android-App mit Azure Mobile-Apps"
    description="Informationen Sie zum Azure Mobile-Apps zu verwenden, um Pushbenachrichtigungen zu Ihrer Android-Anwendung zu senden."
    services="app-service\mobile"
    documentationCenter="android"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-android-app"></a>Hinzufügen von Pushbenachrichtigungen zu Ihrem Android-App

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>(Übersicht)
In diesem Lernprogramm fügen Sie Pushbenachrichtigungen des Projekts [Android Schnellstart] , sodass eine Pushbenachrichtigung an das Gerät gesendet werden, jedes Mal, wenn ein Datensatz eingefügt wird.

Wenn Sie die heruntergeladene Schnellstart Server Project nicht verwenden, benötigen Sie das Pushbenachrichtigungen Benachrichtigung Erweiterungspaket aus. Weitere Informationen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Mobile-Apps Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen Folgendes:

* Eine IDE je nach Ihres Projekts Back-End:

    * [Android Studio](https://developer.android.com/sdk/index.html) Wenn diese app einer Node.js Back-End-aufweist.

    * [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) oder höher, wenn diese app verfügt über eine .net Back-End-aus.

* Android 2.3 oder höher, Google Repository Überarbeitung 27 oder höher und Google Play Services 9.0.2 oder höher für Firebase Cloud Messaging.

* Führen Sie die [Android Schnellstart].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Erstellen eines Projekts, das die Firebase Cloud Messaging unterstützt

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Konfigurieren eines Benachrichtigung Hubs

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Konfigurieren von Azure um Pushbenachrichtigungen zu senden.

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Aktivieren von Pushbenachrichtigungen für das Serverprojekt

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Hinzufügen von Pushbenachrichtigungen zu Ihrer Anwendung

In diesem Abschnitt Aktualisieren Sie Ihre app Client Android, um Pushbenachrichtigungen zu behandeln.

### <a name="verify-android-sdk-version"></a>Überprüfen Sie, Android SDK Version

[AZURE.INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Im nächste Schritt wird, Google Play Services zu installieren. Google Cloud Messaging enthält einige API Ebenen Mindestanforderungen zum Entwickeln und testen, die die Eigenschaft **MinSdkVersion** im Manifest entsprechen muss.

Wenn Sie mit einem älteren Gerät testen, lesen Sie dann die [Festlegen von Google wiedergeben Services SDK] , um zu ermitteln, wie Niedrig Sie diesen Wert festlegen, und legen Sie ihn ordnungsgemäß können.

### <a name="add-google-play-services-to-the-project"></a>Hinzufügen von wiedergeben Google-Diensten zum Projekt

[AZURE.INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Hinzufügen von code

[AZURE.INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Testen der app mit der veröffentlichten mobile service

Sie können die app durch Anfügen direkt auf einem Android-Smartphone mit einem USB-Kabel oder mithilfe eines virtuellen Geräts im Emulator testen.

## <a name="more"></a>Weitere

<!-- URLs -->
[Android Schnellstart]: app-service-mobile-android-get-started.md

[Einrichten von Google Play Services SDK]:https://developers.google.com/android/guides/setup
