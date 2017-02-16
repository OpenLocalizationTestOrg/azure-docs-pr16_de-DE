<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für Android eins Bereitstellung"
    description="Informationen Sie zur Verwendung von Azure Mobile Engagement mit Analytics und Pushbenachrichtigungen für Einheit apps bereitstellen für iOS-Geräte."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Erste Schritte mit Azure Mobile Engagement für Android eins Bereitstellung

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird gezeigt, wie Azure Mobile Engagement zu verwenden, um Ihre app Verwendung zu verstehen und so Pushbenachrichtigungen zu senden Benutzern einer Anwendung Einheit aufgeteilt, wenn Sie auf einem Android-Gerät bereitstellen.
In diesem Lernprogramm verwendet die klassische eins einsatzbereit ein Ball Lernprogramm als Ausgangspunkt. Sie sollten die Schritte in diesem [Lernprogramm](mobile-engagement-unity-roll-a-ball.md) , bevor Sie mit der Mobile Engagement Integration folgen, die wir in den folgenden Lernprogramm verdeutlichen. 

In diesem Lernprogramm benötigen Sie Folgendes:

+ [Einheit im Editor](http://unity3d.com/get-unity)
+ [Mobile Engagement Einheit im SDK](https://aka.ms/azmeunitysdk)
+ Google Android SDK

> [AZURE.NOTE] Um dieses Lernprogramms abgeschlossen haben, müssen Sie ein aktives Azure-Konto verfügen. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).

##<a name="a-idsetup-azmeasetup-mobile-engagement-for-your-android-app"></a><a id="setup-azme"></a>Einrichten von mobilen Recherchemöglichkeiten für Ihre app Android

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

1. Öffnen Sie die Skriptdatei **EngagementConfiguration** aus dem Ordner SDK und Aktualisieren der **ANDROID\_Verbindung\_Zeichenfolge** mit der Verbindungszeichenfolge, die Sie zuvor vom Azure-Portal abgerufen.  

    ![][73]

2. Speichern Sie die Datei 

3. Führen Sie **Datei -> Engagement -> Android Manifest generieren**. Dies ist das Plug-in vom Mobile Engagement SDK hinzugefügt und darauf klickt die Einstellungen für Project automatisch aktualisiert wird. 

    ![][74]

> [AZURE.IMPORTANT] Vergewissern Sie sich zu dieser Vorgang ausgeführt werden jedes Mal, wenn Sie die Datei **EngagementConfiguration** andernfalls aktualisieren, die die Änderungen werden nicht in der app wiedergegeben werden. 

###<a name="configure-the-app-for-basic-tracking"></a>Konfigurieren der app für einfache Überwachung

1. Öffnen Sie das **PlayerController** -Skript, die das Objekt Player zur Bearbeitung angefügt. 

2. Fügen Sie die folgende Anweisung verwenden:

        using Microsoft.Azure.Engagement.Unity;

3. Fügen Sie die folgenden Optionen, um die `Start()` Methode
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Bereitstellen und Ausführen der app
Stellen Sie sicher, dass Sie Android SDK, bevor Sie versuchen, diese app Einheit mit Ihrem Gerät bereitstellen auf Ihrem Computer installiert haben. 

1. Herstellen einer Verbindung Ihres Computers mit einem Android-Gerät. 

2. Öffnen von **Datei-Einstellungen erstellen >** 

    ![][40]

3. Wählen Sie **Android** , und klicken Sie dann auf **Switch-Plattform**

    ![][51]

    ![][52]

4. Klicken Sie auf **Einstellungen für die Medienwiedergabe** auf, und geben Sie einen gültigen Paket Bezeichner. 

    ![][53]

5. Klicken Sie abschließend auf **Erstellen und ausführen.**

    ![][54]

6. Möglicherweise aufgefordert, einen Ordnernamen zum Speichern des Pakets Android bereitzustellen. 

7. Wenn alles fein geht, wird das Paket mit Ihrem verbundenen Gerät bereitgestellt werden, und Ihre Einheit im Spiel sollte angezeigt werden, auf Ihrem Smartphone! 

##<a name="a-idmonitoraconnect-app-with-real-time-monitoring"></a><a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a name="a-idintegrate-pushaenable-push-notifications-and-in-app-messaging"></a><a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und messaging innerhalb der app

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

###<a name="update-the-engagementconfiguration"></a>Aktualisieren der EngagementConfiguration

1. Öffnen Sie die Skriptdatei **EngagementConfiguration** aus dem Ordner SDK und Aktualisieren der **ANDROID\_GOOGLE\_Zahl** mit der **Projektnummer Google** einer früheren Version von Google Cloud-Entwicklerportal abgerufen. Dies ist eine Zeichenfolge Wert so stellen Sie sicher, es in doppelte Anführungszeichen setzen. 

    ![][75]

2. Speichern Sie die Datei ein. 

3. Führen Sie **Datei -> Engagement -> Android Manifest generieren**. Dies ist das Plug-in vom Mobile Engagement SDK hinzugefügt und darauf klickt die Einstellungen für Project automatisch aktualisiert wird. 

    ![][74]

###<a name="configure-the-app-to-receive-notifications"></a>Konfigurieren der app zum Empfangen von Benachrichtigungen

1. Öffnen Sie das **PlayerController** -Skript, die das Objekt Player zur Bearbeitung angefügt. 

2. Fügen Sie die folgenden Optionen, um die `Start()` Methode

        EngagementReachAgent.Initialize();

3. Nachdem Sie nun die app aktualisiert wird, Bereitstellen Sie, und führen Sie die app auf einem Gerät pro den Anweisungen unter bereitgestellt. 

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
