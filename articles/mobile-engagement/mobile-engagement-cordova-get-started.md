<properties
    pageTitle="Erste Schritte mit Azure mobilen Engagement für Cordova/Phonegap"
    description="Informationen Sie zur Verwendung von Azure Mobile Engagement mit Analytics und Pushbenachrichtigungen für Cordova/Phonegap-apps."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-phonegap"
    ms.devlang="js"
    ms.topic="hero-article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a>Erste Schritte mit Azure mobilen Engagement für Cordova/Phonegap

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird gezeigt, wie Azure Mobile Engagement grundlegende Informationen zu Ihrer Verwendung von app, und senden Sie Pushbenachrichtigungen segmentierter Benutzern für eine mobile Anwendung entwickelt mit Cordova verwendet werden kann.

In diesem Lernprogramm erstellen Sie eine leere Cordova-app mit Mac und integrieren klicken Sie dann auf Mobile Engagement SDK. Es sammelt grundlegende Analytics-Daten und empfängt Pushbenachrichtigungen Apple Pushbenachrichtigungen Benachrichtigung System (APNS) für iOS und Google Cloud Messaging (GCM) für Android verwenden. Wir werden diese mit einer iOS oder Android-Gerät zum Testen bereitstellen. 

> [AZURE.NOTE] Um dieses Lernprogramms abgeschlossen haben, müssen Sie ein aktives Azure-Konto verfügen. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).

In diesem Lernprogramm benötigen Sie Folgendes:

+ XCode, das Sie von Mac App Store (für die Bereitstellung von zu iOS) installieren können
+ [Android SDK und Emulator](http://developer.android.com/sdk/installing/index.html) (für die Bereitstellung von zu Android)
+ Drücken Sie die Benachrichtigung Zertifikat (p12), die Sie von Apple Developer Center für APNS erhalten können
+ GCM Projektnummer, die Sie von Ihrem Google Entwicklertools Konsole für GCM erhalten können
+ [Mobile Engagement Cordova-Plug-Ins](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [AZURE.NOTE] Der Quellcode und die Infodatei finden für das Plug-in Cordova auf [Github](https://github.com/Azure/azure-mobile-engagement-cordova) Sie

##<a name="a-idsetup-azmeasetup-mobile-engagement-for-your-cordova-app"></a><a id="setup-azme"></a>Einrichten von mobilen Recherchemöglichkeiten für Ihre app Cordova

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a name="a-idconnecting-appaconnecting-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Ihre app Verbindung auf die Mobile Engagement Back-End-

In diesem Lernprogramm wird eine einfache "Integration" also den minimalen festlegen zum Sammeln von Daten und Senden einer Benachrichtigung Pushbenachrichtigungen erforderlich. 

Mit Cordova, um zu veranschaulichen, die Integration erstellen wir eine einfache app:

###<a name="create-a-new-cordova-project"></a>Erstellen eines neuen Projekts von Cordova

1. Starten Sie *Terminal* -Fenster auf Ihrem Mac-Computer, und geben Sie Folgendes ein, die um ein neues Cordova Projekt aus der Standardvorlage erstellen. Stellen Sie sicher, dass das für die Veröffentlichung Profil, das Sie später verwenden, um Ihre iOS-app bereitstellen 'com.mycompany.myapp' wie die App-ID. verwendet wird 

        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova

2. Führen Sie zum Konfigurieren des Projekts für **iOS** und führen Sie es in den iOS Simulator Folgendes ein:

        $ cordova platform add ios 
        $ cordova run ios

3. Führen Sie vor, um Ihr Projekt für **Android** konfigurieren, und führen Sie es im Android Emulator. Stellen Sie sicher, dass Ihre Android SDK Emulatoreinstellungen den Zielwert wie Google-APIs (Google Inc.) mit der CPU / ABI als Google-APIs ARM.  

        $ cordova platform add android
        $ cordova run android

4.  Fügen Sie das Plug-in Cordova Console hinzu. 

        $ cordova plugin add cordova-plugin-console 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Herstellen einer Verbindung Mobile Engagement Back-End-mit der app

1. Installieren Sie das Plug-in Azure Mobile Engagement Cordova während der die Variablenwerten, um das Plug-in zu konfigurieren:

        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
            --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers the app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

*Symbol für Android erreichen* : muss der Namen der Ressource, ohne eine Erweiterung noch zeichenbaren Präfix (ex: Mynotificationicon), und die Symboldatei in Ihrem android-Projekt (Plattformen und Android/Auflösung/Ausgaben möglich) kopiert werden muss

*iOS Symbol erreicht haben* : muss der Namen der Ressource samt Erweiterung (ex: mynotificationicon.png), und die Symboldatei muss mit XCode (mit im Dateien hinzufügen) in Ihr Projekt iOS hinzugefügt werden

##<a name="a-idmonitoraenabling-real-time-monitoring"></a><a id="monitor"></a>Aktivieren der Überwachung in Echtzeit

1. Bearbeiten Sie im Projekt Cordova - **www/js/index.js** zum Hinzufügen des Anrufs an die Mobile Engagement So deklarieren Sie eine neue Aktivität einmal das Ereignis *DeviceReady* empfangen wird ein.

         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }

2. Führen Sie die Anwendung:
        
    - **Für iOS**
    
        In `Terminal` Fenster starten die app in eine neue Instanz eines Simulator durch Ausführen der Folgendes:

            cordova run ios

    - **Für Android**
        
        In `Terminal` Fenster starten die app in einer neuen Emulatorinstanz durch Ausführen der Folgendes:

            cordova run android

3. Sie können im die Protokolle Console Folgendes angezeigt:

        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

##<a name="a-idmonitoraconnect-app-with-real-time-monitoring"></a><a id="monitor"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a name="a-idintegrate-pushaenabling-push-notifications-and-in-app-messaging"></a><a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und messaging innerhalb der app

Mobile Engagement können Sie für Ihre Benutzer mithilfe von Pushbenachrichtigungen und app im Kontext massensendungen zu ermitteln messaging interagieren. In diesem Modul heißt REICHWEITE im Portal Mobile Engagement.
In den folgenden Abschnitten werden für die Einrichtung Ihrer app, um diese zu erhalten.

###<a name="configure-push-credentials-for-mobile-engagement"></a>Konfigurieren von Anmeldeinformationen für Pushbenachrichtigungen für Mobile Engagement

Damit Mobile Engagement Pushbenachrichtigungen in Ihrem Auftrag senden können, müssen Sie ihm Zugriff auf Ihre Apple iOS Zertifikat oder GCM Server API-Schlüssel gewähren. 
    
1. Navigieren Sie zu Ihrem Engagement Mobile-Portal. Stellen Sie sicher, dass Sie für dieses Projekt verwenden, und klicken Sie dann auf die Schaltfläche **mit einbeziehen** , klicken Sie unten in der app sind:
    
    ![][1]
    
2. Auf der Seite landen in Ihrem Engagement Portal. Aus klicken Sie in Abschnitt **Systemeigenen Pushbenachrichtigungen** vorhanden:
    
    ![][2]

3. Konfigurieren von iOS Zertifikat/GCM Server API-Schlüssel

    **[iOS]**

    ein. Wählen Sie Ihre p12, Hochladen Sie, und geben Sie Ihr Kennwort ein:
    
    ![][3]

    **[Android]**

    ein. Klicken Sie auf das Symbol Edit vor- **API-Schlüssel** im Abschnitt Einstellungen GCM und das Popup der angezeigt wird, fügen Sie die GCM Server-Taste, und klicken Sie auf **OK**. 
        
    ![][4]

###<a name="enable-push-notifications-in-the-cordova-app"></a>Aktivieren Sie in der app Cordova Pushbenachrichtigungen

Bearbeiten Sie **www/js/index.js** , um den Anruf an die Mobile Engagement Pushbenachrichtigungen anfordern und deklarieren einen Ereignishandler hinzuzufügen:

     onDeviceReady: function() {
           Engagement.initializeReach(  
                // on OpenUrl  
                function(_url) {   
                alert(_url);   
                });  
            Engagement.startActivity("myPage",{});  
        }

###<a name="run-the-app"></a>Führen Sie die app

**[iOS]**

1. Wir verwenden XCode das Erstellen und Bereitstellen von der app auf dem Gerät Pushbenachrichtigungen zu testen, da iOS nur Pushbenachrichtigungen zu einem tatsächlichen Gerät ermöglicht. Wechseln Sie zu dem Speicherort, wo Ihr Projekt Cordova erstellt wird, und navigieren Sie zu **...\platforms\ios** Speicherort. Öffnen Sie die Datei systemeigener .xcodeproj in XCode. 
    
2. Erstellen und Bereitstellen der app Cordova den iOS-Gerät mit dem Konto, das die Bereitstellung Profil, in dem sich das Zertifikat, dass Sie nur auf das Portal Mobile Engagement und die App-Id hochgeladen wurde welche entspricht den Ihnen beim Erstellen der app Cordova erteilt. Sie können das *Paket Bezeichner* in Auschecken Ihrer **Ressourcen\*-"Info.plist"** Datei in XCode, damit es von entspricht. 

3. Sie sehen das standardmäßige iOS Popup auf Ihrem Gerät angezeigt, die besagt, dass die app über die Berechtigung zum Senden von Benachrichtigungen anfordert. Erteilen der Berechtigung an. 

**[Android]**

Den Emulator können einfach die Android ausführen wie auf dem Android-Emulator GCM Benachrichtigungen unterstützt werden. 

    cordova run android

##<a name="a-idsendasend-a-notification-to-your-app"></a><a id="send"></a>Senden einer Benachrichtigung zu Ihrer Anwendung

Wir werden nun eine einfache Pushbenachrichtigung für eine Marketingkampagne erstellen, das zum Senden einer Pushbenachrichtigungen zu Ihrer Anwendung auf dem Gerät ausgeführt:

1. Navigieren Sie zur Registerkarte **erreicht haben** Ihre Mobile Engagement-Portal

2. Klicken Sie auf **Neue Ankündigung** , um Ihre Pushbenachrichtigungen für eine Marketingkampagne zu erstellen.

    ![][6]

3. Bereitstellen von Eingaben, um Ihre **[Android]** für eine Marketingkampagne zu erstellen.
    
    - Geben Sie einen **Namen** für Ihre für eine Marketingkampagne ein. 
    - Wählen Sie die **Übermittlungstyp** *System Benachrichtigung* *einfache*
    - Wählen Sie den **Übermittlungszeitpunkt** als *"jedes Mal"*
    - Geben Sie einen **Titel** für Ihre Benachrichtigung, die ersten Zeile der Pushbenachrichtigungen verwendet wird.
    - Stellen Sie eine **Nachricht** für Ihre Benachrichtigung, die als Nachrichtentext dienen soll. 

    ![][11]

4. Bereitstellen von Eingaben zum Erstellen der für eine Marketingkampagne **[iOS]**

    - Geben Sie einen **Namen** für Ihre für eine Marketingkampagne ein. 
    - Wählen Sie den **Übermittlungszeitpunkt** als *"nicht genügend app"*
    - Geben Sie einen **Titel** für Ihre Benachrichtigung, die ersten Zeile der Pushbenachrichtigungen verwendet wird.
    - Stellen Sie eine **Nachricht** für Ihre Benachrichtigung, die als Nachrichtentext dienen soll. 
 
    ![][12]

5. Wählen Sie einen Bildlauf nach unten, und klicken Sie im Abschnitt Inhalt **nur Benachrichtigung**

    ![][8]

6. [Optional] Sie können auch eine Aktions-URL bereitstellen. Stellen Sie sicher, dass er eine URL-Schema bereitgestellten beim Konfigurieren des-Plug-in verwendet **AZME\_UMLEITEN\_URL** Variable z. B. *Myapp://test*.  

7. Dies sind die grundlegendsten für eine Marketingkampagne möglich festlegen. Jetzt einen Bildlauf nach unten erneut ein, und klicken Sie auf die Schaltfläche **Erstellen** , um Ihre Campaign zu speichern.

8. Schließlich **Aktivieren** Ihrer für eine Marketingkampagne
    
    ![][10]

9. Einer der Pushbenachrichtigung sollte auf Ihrem Gerät oder Emulator jetzt als Teil dieses Campaign angezeigt werden. 

##<a name="a-idnext-stepsanext-steps"></a><a id="next-steps"></a>Nächste Schritte
[Übersicht über alle Methoden mit Cordova Mobile Engagement SDK verfügbare Funktionen](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

