<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für Web Apps | Microsoft Azure"
    description="Informationen Sie zur Verwendung von Azure Mobile Engagement mit Analytics und Pushbenachrichtigungen Benachrichtigungen für Web Apps."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="js"
    ms.topic="hero-article"
    ms.date="06/01/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Erste Schritte mit Azure Mobile Engagement für Web Apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird gezeigt, wie Azure Mobile Engagement um zu verstehen Ihrer Verwendung von Web App verwendet werden können.

In diesem Lernprogramm benötigen Sie Folgendes:

+ Visual Studio 2015 oder einer beliebigen anderen Text-Editor Ihrer Wahl
+ [Web SDK](http://aka.ms/P7b453) 

Dieses Web SDK befindet sich in der Vorschau und nur Analytics im Moment unterstützt und senden Browser oder in der app Pushbenachrichtigungen noch unterstützt keine. 

> [AZURE.NOTE] Um dieses Lernprogramms abgeschlossen haben, müssen Sie ein aktives Azure-Konto verfügen. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).

##<a name="setup-mobile-engagement-for-your-web-app"></a>Einrichten von mobilen Engagement für Ihre Web app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a name="a-idconnecting-appaconnect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Herstellen einer Verbindung die Mobile Engagement Back-End-mit der app

In diesem Lernprogramm wird eine "grundlegende Integration," Was den minimalen Satz zum Sammeln von Daten erforderlich ist.

Wir erstellt eine einfache Web app mit Visual Studio für die Integration zu veranschaulichen, obwohl Sie die Schritte für jede Web-Anwendung, die auch außerhalb von Visual Studio erstellt ausführen können. 

###<a name="create-a-new-web-app"></a>Erstellen einer neuen Web App

Die folgenden Schritte durch die Verwendung von Visual Studio 2015 wird davon ausgegangen, obwohl die Schritte in früheren Versionen von Visual Studio ähneln. 

1. Starten Sie Visual Studio, und wählen Sie in **der Startseite** **Neues Projekt**aus.

2. Wählen Sie im Popupfenster, **Web** -> **ASP.Net Web-Anwendung**. Füllen Sie die app- **Name**, **Speicherort** und den **Namen der Lösung**, und klicken Sie dann auf **OK**.

3. Wählen Sie im Popup **Wählen Sie eine Vorlage aus** **leere** unter **ASP.Net 4.5 Vorlagen** aus, und klicken Sie auf **OK**. 

Jetzt haben Sie ein neues leeres Projekt im Web App erstellt, in dem wir Azure Mobile Engagement Web SDK integrieren wird.

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Herstellen einer Verbindung Mobile Engagement Back-End-mit der app

1. Erstellen Sie einen neuen Ordner namens **Javascript** in Ihre Lösung und fügen Sie der Web SDK JS-Datei **Azure-engagement.js** hinein hinzu. 

2. Fügen Sie eine neue Datei mit **main.js** in diesem Ordner Javascript mit den folgenden Code ein. Vergewissern Sie sich die Verbindungszeichenfolge aktualisieren. Dies `azureEngagement` Web SDK Methoden Zugriff auf Objekt verwendet werden. 

        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };

    ![Visual Studio mit Js-Dateien][1]

##<a name="enable-real-time-monitoring"></a>Aktivieren Sie die Überwachung in Echtzeit

Damit beginnen, Daten zu senden und um sicherzustellen, dass die Benutzer aktiv sein sollen, müssen Sie mindestens eine Aktivität an die Mobile Engagement Back-End-senden. Eine Aktivität im Zusammenhang mit einem Web-app ist eine Webseite anzuzeigen. 

1. Erstellen einer neuen Seite aufgerufen **home.html** in Ihre Lösung, und legen Sie es als die Startseite für Ihre Web app. 
2. Enthalten Sie die zwei JavaScript, die wir auf dieser Seite bereits hinzugefügt haben, indem Sie Folgendes im Textkörper Tag hinzufügen. 

        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>

3. Aktualisieren die Textkörper Kategorie zum Aufrufen des EngagementAgent `startActivity` Methode
        
        <body onload="engagement.agent.startActivity('Home')">

4. Hier finden Sie Ihre **home.html** aussehen
        
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

##<a name="connect-app-with-real-time-monitoring"></a>Verbinden von app durch Echtzeit-Überwachung

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

![][2]

##<a name="extend-analytics"></a>Erweitern Sie analytics

Hier sind alle Methoden mit Web SDK, mit denen Sie für Analytics, derzeit verfügbar:

1. Aktivitäten/Webseiten:

        engagement.agent.startActivity(name);
        engagement.agent.endActivity();

2. Ereignisse
        
        engagement.agent.sendEvent(name, extras);

3. Fehler

        engagement.agent.sendError(name, extras);

4. Aufträge

        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

