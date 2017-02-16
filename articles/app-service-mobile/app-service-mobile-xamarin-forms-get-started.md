<properties
    pageTitle="Erste Schritte mit Mobile-Apps mit Xamarin.Forms"
    description="Führen Sie dieses Lernprogramm den Einstieg Azure Mobile-Apps für die Entwicklung von Xamarin.Forms"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-xamarinforms-app"></a>Erstellen Sie eine app Xamarin.Forms

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>(Übersicht)

In diesem Lernprogramm erfahren Sie, wie Sie eine Xamarin.Forms mobile-app mit einer Azure Mobile-App Back-End-einen cloudbasierten Back-End-Dienst hinzufügen. Erstellen Sie eine neue Mobile-App Back-End-, und eine einfache _Aufgabenliste_ Xamarin.Forms-app, die app-Daten in Azure gespeichert sind.

Durchführen dieses Lernprogramms ist eine Vorbedingung für andere Mobile-Apps Lernprogramme für Xamarin.Forms.

##<a name="prerequisites"></a>Erforderliche Komponenten

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Wenn Sie ein Konto besitzen, können Sie für eine Testversion Azure anmelden und bis zu erhalten kostenfreie 10 Mobile-Apps, die Sie auch nach dem Ende Ihrer Testversion weiterhin dazu verwenden können. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio mit Xamarin. Anweisungen finden Sie unter [einrichten und installieren Sie für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) . 

* Einen Mac mit Xcode Version 7.0 oder höher und Xamarin Studio Community installiert. Finden Sie unter [einrichten und für Visual Studio und Xamarin installieren](https://msdn.microsoft.com/library/mt613162.aspx) und [einrichten, installieren, und Überprüfungen für Mac-Benutzer](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).
 
>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](https://tryappservice.azure.com/?appServiceName=mobile), in dem Sie eine kurzlebige Starter Mobile-App sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="create-a-new-azure-mobile-app-backend"></a>Erstellen einer neuen Azure Mobile-App Back-End-

Wie folgt vor, um eine neue Mobile-App Back-End-erstellen.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]


Sie haben nun eine Azure Mobile-App Back-End-bereitgestellt, die von der mobilen Clientanwendungen verwendet werden kann. Sie werden als Nächstes Herunterladen einer Project Server für eine einfache "Aufgabenliste" Back-End- und Azure veröffentlichen.

## <a name="configure-the-server-project"></a>Konfigurieren der Project server

Folgen Sie den Schritten unter Serverprojekt zum Verwenden der Node.js oder .NET Back-End-zu konfigurieren.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-xamarinforms-solution"></a>Herunterladen und Ausführen der Lösung Xamarin.Forms

Hier haben Sie zwei Optionen. Sie können die Lösung auf einem Mac herunterladen und in Xamarin Studio öffnen, oder Sie können die Lösung auf einem Windows-Computer herunterladen und öffnen Sie sie in Visual Studio mit einem vernetzten Mac für iOS-app erstellen. Wenn Sie ausführlichere Anweisungen im Setup-Szenarien Xamarin benötigen, finden Sie unter [Setup, und installieren Sie für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .

Fahren Sie uns:

 1. Öffnen Sie auf Ihrem Mac oder auf Ihrem Windows-Computer das [Azure-Portal] in einem Browserfenster angezeigt.
 2. In den Einstellungen Blade für Ihre Mobile-App, klicken Sie auf **Erste Schritte** (unter Mobile) > **Xamarin.Forms**. Klicken Sie auf **Erstellen einer neuen app** , klicken Sie unter Schritt 3 Wenn er noch nicht ausgewählt ist.  Klicken Sie anschließend auf die Schaltfläche **herunterladen** .

    Diese downloads ein Projekts, das eine Clientanwendung enthält, die mit Ihrem mobilen app verbunden ist. Speichern Sie die komprimierte Datei auf den lokalen Computer, und notieren Sie, wo Sie es speichern.

 3. Extrahieren Sie des Projekts, die Sie heruntergeladen haben, und klicken Sie dann in Xamarin Studio oder Visual Studio öffnen.

    ![][9]

    ![][8]


##<a name="optional-run-the-ios-project"></a>(Optional) Führen Sie das Projekt iOS

Dieser Abschnitt ist für die Ausführung des Xamarin iOS-Projekts für iOS-Geräte. Wenn Sie nicht mit iOS-Geräte arbeiten, können Sie diesen Abschnitt überspringen.

####<a name="in-xamarin-studio"></a>In Xamarin Studio

1. Mit der rechten Maustaste in des iOS-Projekts, und klicken Sie dann auf **Als beim Start Project**.
2. Klicken Sie im Menü **Ausführen** auf **Debuggen starten** , um das Projekt erstellen, und starten Sie die app in den iPhone-Emulator.

####<a name="in-visual-studio"></a>In Visual Studio
1. Mit der rechten Maustaste in des iOS-Projekts, und klicken Sie dann auf **als beim Start-Projekt festlegen**.
2. Klicken Sie im Menü **Erstellen** auf **Konfigurations-Manager**.
3. Wählen Sie im Dialogfeld **Konfigurations-Manager** die Kontrollkästchen **Erstellen** und **Bereitstellen** des Projekts iOS aus.
4. Drücken Sie **F5** , um das Projekt erstellen, und starten Sie die app in den iPhone-Emulator aus.

    >[AZURE.NOTE] Wenn Probleme beim Erstellen von unterstützen ausführen NuGet Paket-Manager und aktualisieren, damit die neueste Version von der Xamarin Pakete. Manchmal können die Projekte Schnellstart positiven hinter in der Aktualisierung auf die neueste.    

Geben Sie in der app aussagekräftigen Text, wie etwa _Xamarin erfahren Sie,_ und klicken Sie dann auf die **+** Schaltfläche.

![][10]

Sendet eine POST-Anforderung an die neue mobile-app Back-End-in Azure gehostet wird. Daten aus der Anforderung werden in der Tabelle TodoItem eingefügt. Elemente in der Tabelle gespeichert werden von der mobilen app Back-End-zurückgegeben, und die Daten in der Liste angezeigt werden.

>[AZURE.NOTE]
> Finden des Codes, die Ihre mobile-app Back-End-TodoItemManager.cs C#-Datei des Projekts tragbaren Klasse Bibliothek Ihrer Lösung greift auf.

##<a name="optional-run-the-android-project"></a>(Optional) Führen Sie das Projekt Android

In diesem Abschnitt ist zum Ausführen des Projekts Droid Xamarin für Android. Wenn Sie nicht mit Android-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

####<a name="in-xamarin-studio"></a>In Xamarin Studio

1. Mit der rechten Maustaste Android Projekt, und klicken Sie dann auf **Als beim Start Project**.
2. Klicken Sie im Menü **Ausführen** auf **Debuggen starten** , um das Projekt erstellen, und starten Sie die app in einen Android Emulator.

####<a name="in-visual-studio"></a>In Visual Studio
1. Mit der rechten Maustaste in des Projekts Android (Droid), und klicken Sie dann auf **als beim Start-Projekt festlegen**.
4. Klicken Sie im Menü **Erstellen** auf **Konfigurations-Manager**.
5. Wählen Sie im Dialogfeld **Konfigurations-Manager** die Kontrollkästchen **Erstellen** und **Bereitstellen** des Projekts Android aus.
6. Drücken Sie **F5** , um das Projekt erstellen, und starten Sie die app in einen Android Emulator aus.

    >[AZURE.NOTE] Wenn Probleme beim Erstellen von unterstützen ausführen NuGet Paket-Manager und aktualisieren, damit die neueste Version von der Xamarin Pakete. Manchmal können die Projekte Schnellstart positiven hinter in der Aktualisierung auf die neueste.    


Geben Sie in der app aussagekräftigen Text, wie etwa _Xamarin erfahren Sie,_ und klicken Sie dann auf die **+** Schaltfläche.

![][11]

Sendet eine POST-Anforderung an die neue mobile-app Back-End-in Azure gehostet wird. Daten aus der Anforderung werden in der Tabelle TodoItem eingefügt. Elemente in der Tabelle gespeichert werden von der mobilen app Back-End-zurückgegeben, und die Daten in der Liste angezeigt werden.

> [AZURE.NOTE]
> Finden des Codes, die Ihre mobile-app Back-End-TodoItemManager.cs C#-Datei des Projekts tragbaren Klasse Bibliothek Ihrer Lösung greift auf.


##<a name="optional-run-the-windows-project"></a>(Optional) Führen Sie das Windows-Projekt


Dieser Abschnitt ist für das Projekt Xamarin WinApp für Windows-Geräten ausgeführt wird. Wenn Sie nicht mit Windows-Geräten arbeiten, können Sie diesen Abschnitt überspringen.


####<a name="in-visual-studio"></a>In Visual Studio
1. Mit der rechten Maustaste keines der Windows-Projekte, und klicken Sie dann auf **als beim Start-Projekt festlegen**.
4. Klicken Sie im Menü **Erstellen** auf **Konfigurations-Manager**.
5. Wählen Sie die Kontrollkästchen **Erstellen** und **Bereitstellen** des Windows-Projekts, die Sie ausgewählt haben, klicken Sie im Dialogfeld **Konfigurations-Manager** .
6. Drücken Sie **F5** , um das Projekt erstellen, und starten Sie die app in einem Windows-Emulator aus.

    >[AZURE.NOTE] Wenn Probleme beim Erstellen von unterstützen ausführen NuGet Paket-Manager und aktualisieren, damit die neueste Version von der Xamarin Pakete. Manchmal können die Projekte Schnellstart positiven hinter in der Aktualisierung auf die neueste.    


Geben Sie in der app aussagekräftigen Text, wie etwa _Xamarin erfahren Sie,_ und klicken Sie dann auf die **+** Schaltfläche.

Sendet eine POST-Anforderung an die neue mobile-app Back-End-in Azure gehostet wird. Daten aus der Anforderung werden in der Tabelle TodoItem eingefügt. Elemente in der Tabelle gespeichert werden von der mobilen app Back-End-zurückgegeben, und die Daten in der Liste angezeigt werden.

![][12]

> [AZURE.NOTE]
> Finden des Codes, die Ihre mobile-app Back-End-TodoItemManager.cs C#-Datei des Projekts tragbaren Klasse Bibliothek Ihrer Lösung greift auf.

##<a name="next-steps"></a>Nächste Schritte

* [Authentifizierung für Ihre app hinzufügen](app-service-mobile-xamarin-forms-get-started-users.md)  
Erfahren Sie, wie Benutzer der app mit einem Identitätsanbieter authentifizieren.

* [Hinzufügen von Pushbenachrichtigungen zu Ihrer Anwendung](app-service-mobile-xamarin-forms-get-started-push.md)  
Informationen Sie zum Hinzufügen von Pushbenachrichtigungen Benachrichtigungen zu Ihrer Anwendung zu unterstützen, und konfigurieren Ihre Mobile-App Back-End-um Azure Benachrichtigung Hubs verwenden, um Pushbenachrichtigungen zu senden.

* [Offlinesynchronisierung für Ihre app zu aktivieren](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Erfahren Sie, wie offlinesupport Ihre app ein Mobile-App Back-End hinzufügen. Offlinesynchronisierung ermöglicht es den Endbenutzern Interaktion mit einem mobilen app&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;auch, wenn keine Verbindung zum Netzwerk vorhanden ist.

* [Verwendung von verwalteten Client für Azure Mobile-Apps](app-service-mobile-dotnet-how-to-use-client-library.md)  
Informationen Sie zum Arbeiten mit dem verwalteten Client SDK in Ihrer app Xamarin. 


<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure-Portal]: https://portal.azure.com/

