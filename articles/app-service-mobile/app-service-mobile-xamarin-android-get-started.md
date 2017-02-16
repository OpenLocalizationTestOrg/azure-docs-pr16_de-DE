<properties
    pageTitle="Erste Schritte mit Azure Mobile-Apps für Xamarin.Android-apps"
    description="Führen Sie dieses Lernprogramm den Einstieg Azure Mobile-Apps für die Entwicklung von Xamarin Android"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha" />

#<a name="create-a-xamarinandroid-app"></a>Erstellen Sie eine App Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>(Übersicht)

In diesem Lernprogramm erfahren Sie, wie Sie eine app Xamarin.Android einen cloudbasierten Back-End-Dienst hinzufügen. Weitere Informationen finden Sie unter [Was sind Mobile-Apps](app-service-mobile-value-prop.md).

Ein Screenshot mit der fertigen app lautet wie folgt:

![][0]

In diesem Lernprogramm durchführen ist eine Voraussetzung für alle anderen Mobile-Apps Lernprogramme für Xamarin.Android-apps.

##<a name="prerequisites"></a>Erforderliche Komponenten

Damit dieses Lernprogramm abgeschlossen, benötigen Sie die folgenden Komponenten:

* Ein aktives Azure-Konto. Wenn Sie kein Konto haben, melden Sie sich bei einem Testabonnement Azure und erhalten Sie bis zu 10 Mobile-Apps kostenlos. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio mit Xamarin. Anweisungen finden Sie unter [einrichten und installieren Sie für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .

>[AZURE.NOTE]Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](https://tryappservice.azure.com/?appServiceName=mobile).  Sie können sofort eine kurzlebige Starter Mobile-App im App-Dienst erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="create-an-azure-mobile-app-backend"></a>Erstellen einer Azure Mobile-App Back-End-

Wie folgt vor, um eine Mobile-App Back-End-erstellen.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Sie haben nun eine Azure Mobile-App Back-End-bereitgestellt, die von der mobilen Clientanwendungen verwendet werden kann. Als Nächstes Herunterladen einer Project Server für eine einfache "Aufgabenliste" Back-End- und Azure veröffentlichen.

## <a name="configure-the-server-project"></a>Konfigurieren der Project server

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>Herunterladen und Ausführen der Xamarin.Android-app

1. Klicken Sie unter **herunterladen, und führen Sie Ihr Projekt Xamarin.Android**klicken Sie auf die Schaltfläche **herunterladen** .

    Speichern Sie die komprimierte Datei auf den lokalen Computer, und notieren Sie, wo Sie es speichern.

2. Drücken Sie **F5** , um das Projekt erstellen, und starten Sie die app aus.

3. Geben Sie in der app einen aussagekräftigen Text, z. B. _vollständig des Lernprogramms_ ein, und klicken Sie dann auf die Schaltfläche **Hinzufügen** .

    ![][10]

    Daten aus der Anforderung werden in der Tabelle TodoItem eingefügt. Elemente in der Tabelle gespeichert, die von der mobilen app Back-End-zurückgegeben werden, und die Daten, die in der Liste angezeigt wird.

    > [AZURE.NOTE] Überprüfen Sie den Code, der Zugriff auf Ihre mobile-app Back-End-Abfrage und Einfügen von Daten, die sich in der ToDoActivity.cs C#-Datei befindet.

##<a name="next-steps"></a>Nächste Schritte

* [Hinzufügen von Offlinesynchronisierung zu Ihrer Anwendung](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Authentifizierung für Ihre app hinzufügen](app-service-mobile-xamarin-android-get-started-users.md)
* [Hinzufügen von Pushbenachrichtigungen zu Ihrer Anwendung Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md)
* [Verwendung von verwalteten Client für Azure Mobile-Apps](app-service-mobile-dotnet-how-to-use-client-library.md)


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
