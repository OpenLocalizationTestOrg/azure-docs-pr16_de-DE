<properties
    pageTitle="Erstellen eine app Cordova auf mobilen Apps Azure App-Verwaltungsdienst | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm zu ersten Schritten mit Azure mobile-app Downloadzeit für Apache Cordova Entwicklung"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""
    tags=""
    keywords="Cordova, JavaScript-, Mobilclient" />

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-an-apache-cordova-app"></a>Erstellen eines Apache Cordova-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>(Übersicht)

In diesem Lernprogramm erfahren Sie, wie ein Apache Cordova mobile-app mithilfe einer Azure mobile-app Back-End-einen cloudbasierten Back-End-Dienst hinzugefügt.  Erstellen Sie eine neue mobile-app Back-End-, und eine einfache _Aufgabenliste_ Apache Cordova-app, die app-Daten in Azure gespeichert sind.

In diesem Lernprogramm durchführen ist eine Voraussetzung für alle anderen Apache Cordova Lernprogramme zu mithilfe des Mobile-Apps in Azure-App-Verwaltungsdienst.

## <a name="prerequisites"></a>Erforderliche Komponenten

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

* PC mit [Visual Studio Community 2015] oder höher.
* [Visual Studio-Tools für Apache Cordova].
* Ein [Aktives Azure-Konto](https://azure.microsoft.com/pricing/free-trial/).

Wählen Sie auch Visual Studio umgehen und die Befehlszeile Apache Cordova direkt verwenden.  Dies ist hilfreich beim Abschließen des Lernprogramms auf einem Mac.  Apache Cordova Clientanwendungen über die Befehlszeile kompilieren ist nicht durch dieses Lernprogramms abgedeckt.

## <a name="create-a-new-azure-mobile-app-backend"></a>Erstellen einer neuen Azure mobile-app Back-End-

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Zeigen Sie ein Video mit ähnlichen Schritten](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>Konfigurieren der Project server

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Herunterladen Sie, und führen Sie die app Apache Cordova

[AZURE.INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie in diesem Lernprogramm Schnellstart abgeschlossen haben, fahren Sie mit einer der folgenden Lernprogramme:

* [Authentifizierung hinzufügen] , zu Ihrer Apache Cordova app.
* [Hinzufügen von Pushbenachrichtigungen] , zu Ihrer Apache Cordova app.

Weitere Informationen zu Konzepte mit Azure-App-Verwaltungsdienst.

* [Authentifizierung]
* [Pushbenachrichtigungen]

Erfahren Sie, wie die SDKs verwenden.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio-Community 2015]: http://www.visualstudio.com/
[Visual Studio-Tools für Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Authentifizierung hinzufügen]: app-service-mobile-cordova-get-started-users.md
[Hinzufügen von Pushbenachrichtigungen]: app-service-mobile-cordova-get-started-push.md
[Authentifizierung]: app-service-mobile-auth.md
[Pushbenachrichtigungen]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
