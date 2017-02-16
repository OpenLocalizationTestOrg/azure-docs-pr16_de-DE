<properties
    pageTitle="Erste Schritte mit Azure App Dienst Mobile-Apps für apps Xamarin.iOS | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm zu ersten Schritten mit Mobile-Apps für die Entwicklung von Xamarin.iOS."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>


#<a name="create-a-xamarinios-app"></a>Erstellen Sie eine app Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>(Übersicht)

In diesem Lernprogramm erfahren Sie, wie eine Xamarin.iOS mobile-app mithilfe einer Azure mobile-app Back-End-einen cloudbasierten Back-End-Dienst hinzugefügt.  Erstellen Sie sowohl eine neue mobile-app Back-End- und eine einfache _Aufgabenliste_ Xamarin.iOS-app, die app-Daten in Azure gespeichert sind.

In diesem Lernprogramm durchführen ist eine Voraussetzung für alle anderen Xamarin.iOS Lernprogramme zu mithilfe des Mobile-Apps in Azure-App-Verwaltungsdienst.

##<a name="prerequisites"></a>Erforderliche Komponenten

Damit dieses Lernprogramm abgeschlossen, benötigen Sie die folgenden Komponenten:

* Ein aktives Azure-Konto. Wenn Sie kein Konto haben, melden Sie sich bei Azure-Testversion und erhalten Sie bis zu 10 kostenlosen mobile-apps, die Sie auch nach dem Ende Ihrer Testversion weiterhin dazu verwenden können. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio mit Xamarin. Anweisungen finden Sie unter [einrichten und installieren Sie für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .

* Einen Mac mit Xcode Version 7.0 oder höher und Xamarin Studio Community installiert. Finden Sie unter [einrichten und für Visual Studio und Xamarin installieren](https://msdn.microsoft.com/library/mt613162.aspx) und [einrichten, installieren, und Überprüfungen für Mac-Benutzer](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

>[AZURE.NOTE]Wenn Sie mit Azure-App-Verwaltungsdienst anzufangen, bevor Sie für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](https://tryappservice.azure.com/?appServiceName=mobile). Sie können sofort eine kurzlebige Starter mobile-app erstellen, in der App-Dienst – keine Kreditkarte erforderlich, und keine Zusagen.

## <a name="create-an-azure-mobile-app-backend"></a>Erstellen einer Azure Mobile-App Back-End-

Wie folgt vor, um eine Mobile-App Back-End-erstellen.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Konfigurieren der Project server

Sie haben nun eine Azure Mobile-App Back-End-bereitgestellt, die von der mobilen Clientanwendungen verwendet werden kann. Als Nächstes Herunterladen einer Project Server für eine einfache "Aufgabenliste" Back-End- und Azure veröffentlichen.

Führen Sie die folgenden Schritte aus, um das Serverprojekt zum Verwenden der Node.js oder .NET Back-End-zu konfigurieren.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>Herunterladen und Ausführen der Xamarin.iOS-app

1. Öffnen Sie das [Azure-Portal] in einem Browserfenster angezeigt.

2. In den Einstellungen Blade für Ihre Mobile-App, klicken Sie auf **Erste Schritte** > **Xamarin.iOS**. Klicken Sie auf **Erstellen einer neuen app** , klicken Sie unter Schritt 3 Wenn er noch nicht ausgewählt ist.  Klicken Sie anschließend auf die Schaltfläche **herunterladen** .

    Eine Clientanwendung, die mit Ihrem mobilen Back-End-verbindet heruntergeladen werden sollen. Speichern Sie die komprimierte Datei auf den lokalen Computer, und notieren Sie, wo Sie es speichern.

3. Extrahieren Sie das Projekt, das Sie heruntergeladen haben, und öffnen Sie Sie dann im Xamarin Studio (oder Visual Studio).

    ![][9]

    ![][8]

4. Drücken Sie F5, um das Projekt erstellen, und starten Sie die app in den iPhone-Emulator aus.

5. Geben Sie in der app aussagekräftigen Text, wie etwa _Xamarin erfahren Sie_, und klicken Sie dann auf die **+** Schaltfläche.

    ![][10]

    Daten aus der Anforderung werden in der Tabelle TodoItem eingefügt. Elemente in der Tabelle gespeichert werden von der mobilen app Back-End-zurückgegeben, und die Daten in der Liste angezeigt werden.

>[AZURE.NOTE]Überprüfen Sie den Code, der Zugriff auf Ihre mobile-app Back-End-Abfrage und Einfügen von Daten in der QSTodoService.cs C#-Datei.

##<a name="next-steps"></a>Nächste Schritte

* [Hinzufügen von Offlinesynchronisierung zu Ihrer Anwendung](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Authentifizierung für Ihre app hinzufügen](app-service-mobile-xamarin-ios-get-started-users.md)
* [Hinzufügen von Pushbenachrichtigungen zu Ihrer Anwendung Xamarin.Android](app-service-mobile-xamarin-ios-get-started-push.md)
* [Verwendung von verwalteten Client für Azure Mobile-Apps](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure-portal]: https://portal.azure.com/
