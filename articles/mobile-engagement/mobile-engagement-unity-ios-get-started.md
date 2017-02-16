<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für Einheit iOS-Bereitstellung"
    description="Informationen Sie zur Verwendung von Azure Mobile Engagement mit Analytics und Pushbenachrichtigungen für Einheit apps bereitstellen für iOS-Geräte."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a>Erste Schritte mit Azure Mobile Engagement für Einheit iOS-Bereitstellung

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird gezeigt, wie Azure Mobile Engagement zu verwenden, um Ihre app Verwendung zu verstehen und so Pushbenachrichtigungen zu senden Benutzern einer Anwendung Einheit aufgeteilt, wenn Sie auf einem iOS-Gerät bereitstellen.
In diesem Lernprogramm verwendet die klassische eins einsatzbereit ein Ball Lernprogramm als Ausgangspunkt. Sie sollten die Schritte in diesem [Lernprogramm](mobile-engagement-unity-roll-a-ball.md) , bevor Sie mit der Mobile Engagement Integration folgen, die wir in den folgenden Lernprogramm verdeutlichen. 

In diesem Lernprogramm benötigen Sie Folgendes:

+ [Einheit im Editor](http://unity3d.com/get-unity)
+ [Mobile Engagement Einheit im SDK](https://aka.ms/azmeunitysdk)
+ XCode-Editor

> [AZURE.NOTE] Um dieses Lernprogramms abgeschlossen haben, müssen Sie ein aktives Azure-Konto verfügen. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).

##<a name="a-idsetup-azmeasetup-mobile-engagement-for-your-ios-app"></a><a id="setup-azme"></a>Setup Mobile Engagement für Ihre app für iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a name="a-idconnecting-appaconnect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Herstellen einer Verbindung die Mobile Engagement Back-End-mit der app

###<a name="import-the-unity-package"></a>Importieren Sie das Einheit im Paket

1. Herunterladen Sie das [Mobile Engagement Einheit im Paket](https://aka.ms/azmeunitysdk) , und speichern Sie es auf Ihrem lokalen Computer. 

2. Wechseln Sie zu **Posten-Paket importieren > -> benutzerdefinierte Paket** , und wählen Sie in den vorstehenden Schritt das Paket, die Sie heruntergeladen haben. 

    ![][70] 

3. Vergewissern Sie sich, alle Dateien ausgewählt sind, und klicken Sie auf die Schaltfläche **Importieren** . 

    ![][71] 

4. Nach dem Import erfolgreich ist, wird die importierten SDK-Dateien in Ihrem Projekt angezeigt.  

    ![][72] 

###<a name="update-the-engagementconfiguration"></a>Aktualisieren der EngagementConfiguration

1. Öffnen Sie die Skriptdatei **EngagementConfiguration** aus dem Ordner SDK und Aktualisieren der **IOS\_Verbindung\_Zeichenfolge** mit der Verbindungszeichenfolge, die Sie zuvor vom Azure-Portal abgerufen.  

    ![][73]

2. Speichern Sie die Datei ein. 

###<a name="configure-the-app-for-basic-tracking"></a>Konfigurieren der app für einfache Überwachung

1. Öffnen Sie das **PlayerController** -Skript, die das Objekt Player zur Bearbeitung angefügt. 

2. Fügen Sie die folgende Anweisung verwenden:

        using Microsoft.Azure.Engagement.Unity;

3. Fügen Sie die folgenden Optionen, um die `Start()` Methode
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Bereitstellen und Ausführen der app

1. Herstellen einer Verbindung Ihres Computers mit einem iOS-Gerät. 

2. Öffnen von **Datei-Einstellungen erstellen >** 

    ![][40]

3. Wählen Sie **iOS** , und klicken Sie dann auf **Switch-Plattform**

    ![][41]

    ![][42]

4. Klicken Sie auf **Einstellungen für die Medienwiedergabe** auf, und geben Sie einen gültigen Paket Bezeichner. 

    ![][53]

5. Klicken Sie abschließend auf **Erstellen und ausführen.**

    ![][54]

6. Möglicherweise aufgefordert, einen Ordnernamen zum Speichern des Pakets iOS bereitzustellen. 

    ![][43]

7. Wenn alles fein geht, Kompilieren des Projekts, und Sie sollten es auf Ihrer Anwendung XCode öffnen. 

8. Stellen Sie sicher, dass die **zu bündeln Bezeichner** im Projekt korrekt ist.  

    ![][75]

10. Führen Sie die app jetzt in XCode, damit das Paket mit Ihrem verbundenen Gerät bereitgestellt wird und das Einheit im Spiel werden, auf Ihrem Smartphone angezeigt sollte! 

##<a name="a-idmonitoraconnect-app-with-real-time-monitoring"></a><a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a name="a-idintegrate-pushaenable-push-notifications-and-in-app-messaging"></a><a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und messaging innerhalb der app

Mobile Engagement ermöglicht es Ihnen, die für Ihre Benutzer interagieren und erreichen mit Pushbenachrichtigungen und app messaging im Kontext massensendungen zu ermitteln. In diesem Modul heißt REICHWEITE im Portal Mobile Engagement.
Verfügen Sie nicht über zusätzliche Konfiguration in Ihrer app-Benachrichtigungen erhalten möchten, und es ist bereits für sie einrichten.

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png
