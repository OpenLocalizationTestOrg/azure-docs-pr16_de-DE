<properties 
    pageTitle="Senden von personalisierten Benachrichtigung mit Azure Mobile Engagement" 
    description="Informationen zum Senden von personalisierter Benachrichtigungen von Benutzerprofildaten in die Benachrichtigungen wie deren Namen einschließlich"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="all" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="personalize-notifications-by-including-user-name"></a>Personalisieren von Benachrichtigungen durch, einschließlich des Benutzernamens

Bei der Suche nach zu Benachrichtigungen für Ihre app-Benutzer ansprechender zu gestalten sollten Sie personalisieren können. Verwenden die Namen der app-Benutzer Selektives behoben werden die Benachrichtigungen, sodass sie weitere persönliche sind umfasst eine leistungsfähige Ansatz. Hier - Vorsicht sollten Sie Benutzernamen hinzufügen, die Benachrichtigungen sorgfältig Ansatz da, wenn Sie diese Strategie viele dann es für einige Benutzer app als gruseligen stoßen konnte auf. Sie sollten auch sicherstellen, dass den Benutzer entscheiden Sie sich in diese persönliche Details auf Sie mit volle Zustimmung in der mobilen app bereit, bevor Sie beginnen, verwenden Sie diese ganz können, sind. 

Technisch, mit Azure Mobile Engagement, Sie erreichen personalisieren können die Benachrichtigungen, indem Sie die nachstehenden Schritte durchführen in der nun das Szenario einschließlich Benutzername in den Benachrichtigungen verwendet wird. Verwenden Sie des Konzepts der App-Info oder Kategorien, deren Werte entweder durch die SDKs übergeben werden könnte, integriert, in der Mobile-App oder über APIs. Diese App-Ereignisinfos hinzu oder Tags könnte dann verwendet werden:

1. Benutzen Sie Benachrichtigungen für bestimmte Benutzer basierend auf den Werten des App-Info oder 
2. als Platzhalter in die Benachrichtigungen das mit Werten, die speziell für Benutzer oder Geräts beim Senden von Benachrichtigungen zu diesem Gerät ersetzt wird. 

> [AZURE.IMPORTANT] Beachten Sie, dass die Geschwindigkeit von Benachrichtigungen senden eine Verringerung aufgrund dieser weitere Verarbeitung von app-Info Werte ersetzt werden mit jeder Benachrichtigungen angezeigt wird. 

##<a name="register-app-info-in-the-mobile-engagement-portal"></a>Registrieren Sie App-Informationen im Portal mobilen Engagement

1) Beginnen Sie mit der App Informationen oder Kategorien im Azure-Portal registrieren. Wechseln Sie zu **Einstellungen** -> **Kategorie (App-Info)** für diesen.  

![][1]  

2) Klicken Sie auf **neue Kategorie (app-Info)** , und geben Sie den Namen oder als *Benutzername* und den Dateityp als *Zeichenfolge* , und klicken Sie auf **Absenden**. 

![][2]

3) Schließlich werden diese app-Info registriert ist, wie die folgende angezeigt:

![][3]

##<a name="send-app-info-from-the-client-sdk"></a>App-Informationen im Desktopclient SDK senden

Stelle im Universal Windows-app-Beispiel verwenden, aber entsprechende Methoden sind für unsere anderen SDKs auch vorhanden. 

Vorausgesetzt, Sie verfügen über eine Methode in der mobilen app die Profilinformationen vom Benutzer wie deren Namen wahrscheinlich Ursprungsort nach der Authentifizierung können, rufen Sie `SendAppInfo` hier Methode, und füllen Sie den Wert von der `user_name` app Informationen, die Sie zuvor in die Mobile Engagement Dienst Back-End-registriert. 

    Dictionary<object, object> appInfo = new Dictionary<object, object>();
    appInfo.Add("user_name", str);
    EngagementAgent.Instance.SendAppInfo(appInfo); 

##<a name="send-personalized-notifications"></a>Senden von personalisierten Benachrichtigungen

Nun können Sie alle festlegen, um mit dieser **Benutzername**Benachrichtigungen senden an. 

1) Wechseln Sie zu Mobile Engagement Portal auf die Registerkarte **erreicht haben** , um eine Benachrichtigung zu erstellen, und Sie dieser Platzhalter im folgenden Format an einer beliebigen Stelle in der Benachrichtigung Titel oder Textkörper verwenden können. 

![][4]  

> [AZURE.NOTE] Alle Benutzer für die Benutzername app Informationen nicht festgelegt ist, erhalten Sie eine Benachrichtigung. Wenn Sie die Benachrichtigung für eine Marketingkampagne im Testmodus ausgeführt und Sie keinen app-Status festlegen, und klicken Sie dann unsere Mitteilungen wird '?' Zeichen, um den Platzhalter zu ersetzen. 

2) Wann wird Mobile Engagement wählen Sie ein Gerät, diese Benachrichtigung zu senden, dann sehen Sie sich diese app-Info wird, und Ersetzen Sie den Wert in den Platzhalter.  
Angenommen, wir festgelegt haben `str = "Scott"` für einen Benutzer als das Gerät Registrierung erhalten die app-Informationen des zugeordnet **Benutzername = STEFAN** für diesen Benutzer und diese Benutzer eine außerhalb der Pushbenachrichtigung von app in folgendem Format angezeigt werden. 

![][5]  

<!-- Images. -->
[1]: ./media/mobile-engagement-send-personalized-notifications/app-info.png
[2]: ./media/mobile-engagement-send-personalized-notifications/create-app-info.png
[3]: ./media/mobile-engagement-send-personalized-notifications/app-info-user-name.png
[4]: ./media/mobile-engagement-send-personalized-notifications/personal-notification.png
[5]: ./media/mobile-engagement-send-personalized-notifications/notification.png

