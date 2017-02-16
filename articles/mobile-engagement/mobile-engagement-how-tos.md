<properties 
   pageTitle="Azure mobilen Engagement Benutzeroberfläche - Reichweite so"
   description="User Interface Overview für Azure mobilen Engagement" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="how-to-get-started-using-and-managing-pushes-to-reach-out-to-your-end-users"></a>Verwenden und Verwalten von Schritte legt zum erreichen den Endbenutzern

Nachdem das SDK vollständig in Ihre app integriert ist, Sie können die ersten Schritte mit den Abschnitt Reichweite der Benutzeroberfläche Pushbenachrichtigungen für die Benutzer der app.  

## <a name="do-your-first-push-notification-campaign"></a>Führen Sie Ihre erste Pushbenachrichtigungen Benachrichtigung für eine Marketingkampagne
-    Bestätigen Sie, dass Ihre Reichweite in Ihre app mit dem SDK integriert ist. 
-    Wählen Sie aus Ihrer Anwendung
 
![First1][1]

-    Wechseln Sie zu dem Abschnitt "Reichweite", und klicken Sie auf "neue Ankündigung"
 
![First2][2]

-    Erstellen Sie einer neuen für eine Marketingkampagne, und nennen Sie es
 
 ![First3][3]

-    Wählen Sie aus, wie die Benachrichtigung übermittelt werden soll, als In-app nur
 
![First4][4]

-    Erstellen Sie eine Nachricht, die Sie übertragen möchten
 
![First5][5]

-    Sie können einen Titel auf die Benachrichtigung (Optional) schreiben.
-    Schreiben Sie Pushbenachrichtigungen Nachrichteninhalt.
-    Sie können ein Bild hochladen. Achten Sie darauf, dass die Größe der Datei 32.768 Byte überschreiten, kann nicht.
-    Sie haben auch die Möglichkeit, weitere Optionen auswählen werden, für den Fokus dieses Lernprogramms, wir jedoch angezeigt, die später.

-    Wählen Sie nur den Inhaltstyp als Benachrichtigung
 
![First6][6]

-    Ihre Pushbenachrichtigungen für eine Marketingkampagne erstellen und es in der Liste für eine Marketingkampagne angezeigt.
 
![First7][7]

## <a name="test-your-push-notification-campaign"></a>Testen Sie Ihrer Pushbenachrichtigungen Benachrichtigung für eine Marketingkampagne
![Test1][8]

-    Registrieren Sie sich Ihrem Gerät.
-    Klicken Sie auf das Kontrollkästchen des Geräts, die Sie verschieben möchten.
-    Klicken Sie auf die Schaltfläche "Testen", die Pushbenachrichtigungen an das Gerät zu senden.
 
![Test2][9]

-    Aktivieren Sie die für eine Marketingkampagne
 
![. 3][10]

-    Jetzt, da Sie Ihre für eine Marketingkampagne erstellt haben, müssen Sie nur aktivieren sie die Benachrichtigung an die Benutzer abgelegt werden.
 
## <a name="send-personalized-pushes"></a>Senden von personalisierten Push-Vorgänge
-    In diesem Beispiel wird eine eingegeben ein benutzerdefinierten Rabatt Code in der Pushbenachrichtigung von Pushbenachrichtigungen erstellt.
 
![Personalize1][11]

Personalisierung funktioniert durch eine Marke von aus einer app Info Tag also ersetzen, müssen Sie sicherstellen, dass der Benutzer den richtigen app Info zuerst definiert wurde. In diesem Beispiel können den Benutzer eine app Info Kategorie mit dem Namen Rebate_code definiert sind.
Wie Sie, über sehen enthält den Inhalt der Pushbenachrichtigungen Benachrichtigung Marker ${Rebate_code} die darauf hinweisen, dass es durch den tatsächlichen Inhalt der app Info-Tags ersetzt werden.

> Warnung: Wenn die Kategorie der app-Informationen für den Benutzer nicht definiert ist, der Benutzer nicht die Pushbenachrichtigungen erhalten.

-    Ergebnis
 
![Personalize2][12]

### <a name="you-can-further-personalize-the-text-your-notification"></a>Sie können dem Text die Benachrichtigung persönlicher gestalten
![Personalize3][13]

-    Einschließlich den Titel der Benachrichtigung,
-    Und dem Inhalt der Nachricht.
-    Wählen Sie den Typ der Ankündigung (Text oder an)
 
![Personalize4][14]

### <a name="the-body-of-an-announcement-may-also-be-personalized-with"></a>Textkörper einer Ankündigung möglicherweise auch mit individuellen werden:
-    Die Aktions-URL möchten, sollten Sie die Startseite anpassen
-    Titel
-    Hauptteil der Nachricht.
 
 
## <a name="differentiate-your-push-notification-in-or-out-of-app"></a>Ihr Pushbenachrichtigungen Benachrichtigung zu unterscheiden, (in oder aus dem app)
-    Wählen Sie die Art der Benachrichtigung, die Sie Pushbenachrichtigungen, wählen Sie die Anwendung, wechseln Sie zum Abschnitt "Erreichen", wählen Sie aus oder Erstellen einer Pushbenachrichtigungen für eine Marketingkampagne und wechseln Sie zum Abschnitt "Benachrichtigung".
 
-    Klicken Sie auf den Modus"Übermittlung" werden sollen.
-    Klicken Sie auf das Kontrollkästchen "Aktivitäten einschränken", wenn Sie möchten, dass die Benachrichtigung tritt auf bestimmten Aktivitäten (Bildschirme).

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a>"Nicht genügend App" Übermittlung Modus
![Differentiate2][16]

"Nicht genügend App" Übermittlung Modus bietet Pushbenachrichtigung an, wenn die Anwendung geschlossen wird. Dies ist der Pushbenachrichtigung von standard.
Wenn Sie "nicht genügend app" auswählen, muss bereits die Zertifikate von der Plattform eingegebenen, die Erstellen der Anwendung auf (APNS oder GCM).

### <a name="see-also"></a>Siehe auch
-  [Apple-Pushbenachrichtigungsdienst – Zertifikate](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), Google Cloud Messaging – Zertifikat] (http://developer.android.com/google/gcm/index.html) 

### <a name="in-app-only-delivery-mode"></a>"in-App nur" Übermittlung Modus
![Differentiate3][17]

"Innerhalb der App nur" Übermittlung Modus bietet Pushbenachrichtigung an, wenn die Anwendung ausgeführt wird.
Für diese Benachrichtigung müssen Sie nicht das System APNS und GCM wechseln.
System Übermittlung in-app können Ihre Endbenutzer erreicht haben.
Vollständig können Sie die Benachrichtigung anpassen und entscheiden, in der Aktivität (Bildschirmgröße) die Benachrichtigung angezeigt werden.

### <a name="anytime-delivery-mode"></a>"Jederzeit" Übermittlung Modus
Sie können einen "Jederzeit" Übermittlung Modus auswählen, wird gewährleistet, dass Ihre Endbenutzer erreichen, ob die Anwendung oder nicht ausgeführt wird.
Wenn Sie "Jederzeit" auswählen, muss bereits die Zertifikate von der Plattform eingegebenen, die Erstellen der Anwendung nach (APNS oder GCM). 
 
## <a name="schedule-a-push-campaign"></a>Planen einer Pushbenachrichtigungen für eine Marketingkampagne
### <a name="plan-to-start-a-campaign"></a>Planen einer für eine Marketingkampagne starten
![Shedule1][18]

Es ist der 21. März und Sie haben eine Ankündigung vornehmen und für die 22. März Mitternacht geplanter. Sie müssen nicht vor der Benutzeroberfläche einer Pushbenachrichtigungen tun bleiben! Sie können im voraus die genauen Minute planen, die Benachrichtigungen gesendet werden sollen.
-    Heben Sie die Markierung "Keine" das Kontrollkästchen, und wählen Sie eine Anfangszeit 
-    Wählen Sie das Datum und die Uhrzeit der Pushbenachrichtigungen für eine Marketingkampagne beginnen soll.

### <a name="plan-to-end-a-campaign"></a>Planen der Enden eines für eine Marketingkampagne
![Shedule2][19]

Ihre Campaign auf die 25 % März zur 15:00 bis 16 beendet werden soll, aber Sie wissen, dass es können Sie nicht zu tun.
Sie müssen nicht vor der Benutzeroberfläche zum Pushbenachrichtigungen bleiben! Sie können im voraus die genauen Minute planen, die Ihre Campaign beendet werden soll.
-    Klicken Sie auf "Keine" auf das Kontrollkästchen oder wählen Sie eine Endzeit aus.
-    Wählen Sie das Datum und die Uhrzeit, die Sie der Pushbenachrichtigungen für eine Marketingkampagne Fertig stellen möchten.

### <a name="end-a-campaign-manually"></a>Eine für eine Marketingkampagne manuell zu beenden.
![Shedule3][20]

Standardmäßig die "keine"-Kontrollkästchen aktiviert sind.
Dies bedeutet, dass die für eine Marketingkampagne gestartet wird, sobald Sie sie im Abschnitt Reichweite aktivieren und endet, wenn Sie sie in dem Abschnitt Reichweite angehalten werden.
 
> Hinweis: Massensendungen zu ermitteln, ohne ein Enddatum erstellt die Pushbenachrichtigungen lokal auf dem Gerät speichern und Anzeigen der Differenz das nächste Mal, wenn, das die app geöffnet wird, auch wenn die für eine Marketingkampagne manuell eingestellt ist.

## <a name="enhance-a-push-notification-with-a-text-view"></a>Optimieren eines Pushbenachrichtigung mit einem Text-Ansicht
### <a name="what-is-a-text-view"></a>Was ist eine Ansicht Text?
![TextView1][21]

Eine Textansicht handelt es sich um ein Popupfenster mit Textinhalt. Diesem Popupfenster wird angezeigt, nachdem der Endbenutzer auf der Pushbenachrichtigung geklickt wurde.
Eine Textansicht können Sie weitere Inhalte an den Endbenutzer präsentieren. Dies ist auch die Möglichkeit, einen Anruf an Aktion z. B. springen zu einer Seite der app, eine Store, öffnen eine Webseite anzuzeigen, Senden einer e-Mail-Anwendung starten einer Suche Geo lokalisiert umleiten präsentieren usw....

### <a name="example-text-view"></a>Beispiel: Text anzeigen
-    Erstellen Sie Ihre Pushbenachrichtigungen Benachrichtigung für eine Marketingkampagne im Abschnitt "Reichweite", und benennen Sie Ihre für eine Marketingkampagne
 
![TextView2][22]

-    Schreiben Sie die Nachricht, die angezeigt werden, wird die Benachrichtigung.
-    Wählen Sie die Ankündigung Inhaltstyp "Text"
 
![TextView3][23]

> Hinweis: Wenn Sie eine Textansicht drücken, im Lieferumfang es immer einer Benachrichtigung zuerst. 

- Definieren Sie den Text (nach dem Auswählen des Textinhalt Ankündigung untergeordnete Abschnitt angezeigt wird, ermöglicht es Ihnen, den Text zu definieren, die angezeigt werden sollen.)
 
![TextView4][24]

-    Schreiben Sie den Titel, der am oberen Rand der Nachricht angezeigt wird.
-    Schreiben Sie den Hauptinhalt der Textansicht ein.
-    Schreiben Sie der Inhalts, die angezeigt wird, klicken Sie auf die interaktive Schaltfläche (eine interaktiven Schaltfläche ermöglicht der Anwendung, stellen Sie eine bestimmte Aktion wie das Öffnen einer Seite der Anwendung, Umleiten eine App Store oder alle Arten von Datenquellen, die Sie bereitstellen können).
-    Schreiben die Inhalte, die angezeigt wird, klicken Sie auf die Schaltfläche Beenden (, indem Sie auf die Schaltfläche Beenden, die Ansicht für den wird nicht mehr angezeigt.)
 
-    Erstellen Ihrer Pushbenachrichtigungen Benachrichtigung für eine Marketingkampagne, und es wird in der Liste für eine Marketingkampagne angezeigt.
 
![TextView5][25]

-    Aktivieren Sie Ihre Campaign Pushbenachrichtigungen Benachrichtigung, um die Ansicht für den an Ihre Benutzer zu senden.
 
![TextView6][26]

-    Ergebnis
 
![TextView7][27]

-    Der Benutzer erhält die Benachrichtigung und klicken Sie darauf.
-    Die Ansicht für den wird als dem der Benutzer zur Interaktion mit Popup angezeigt.

## <a name="enhance-a-push-notification-with-a-web-view"></a>Optimieren eines Pushbenachrichtigung mit einer Webansicht
### <a name="what-is-a-web-view"></a>Was ist eine Web-Ansicht?
![WebView1][28]

Eine Webansicht ist ein Popupfenster mit Webinhalten. Diesem Popupfenster angezeigt wird, wenn der Endbenutzer auf der Pushbenachrichtigung geklickt hat.
Eine Webansicht können Sie weitere Interaktion mit den Endbenutzer haben.
Dies ist auch die Möglichkeit, einen Anruf an Aktion z. B. Umleitung zum App Store, präsentieren usw. Öffnen einer Webseite, eine e-Mail-Nachricht senden, eine Suche Geo lokalisiert gestartet,...

### <a name="example-web-view"></a>Beispiel: Webansicht
-    Erstellen Sie Ihre Pushbenachrichtigungen für eine Marketingkampagne im Abschnitt "Reichweite", und benennen Sie Ihre für eine Marketingkampagne.
 
![WebView2][29]

-    Schreiben Sie die Nachricht, die angezeigt werden, wird die Benachrichtigung.
-    Wählen Sie den Inhaltstyp Ankündigung als "Web" aus.
 
![WebView3][30]

### <a name="about-announcement-types"></a>Informationen zum Abitur Arten:
- Nur Benachrichtigung: Es ist eine einfache standardbenachrichtigung. Was bedeutet, dass, wenn ein Benutzer darauf klickt, wird keine zusätzliche Ansicht angezeigt, aber, nur die Aktion, die sie zugeordnet sind auftreten werden.
- Text Ankündigung: es eine Benachrichtigung, der die Benutzer erhalten einen Blick auf einen Text anzeigen aktiviert ist.
- Web Ankündigung: es eine Benachrichtigung, der die Benutzer erhalten einen Blick in einer Webansicht aktiviert ist.
Wählen Sie den Inhalt "Web Ankündigung" aus.

> Hinweis: Wenn Sie eine Webansicht drücken, im Lieferumfang es immer einer Benachrichtigung zuerst.

- Definieren Sie den Webinhalt (nach dem auswählen Webinhalte Ankündigung Kapitel wird angezeigt, dass Sie die Webinhalte definieren, die angezeigt werden soll.)

 
![WebView4][31]

-    Schreiben Sie den Titel, der am oberen Rand der Nachricht (optional) angezeigt wird.
-    Fügen Sie hier HTML-Code ein.
-    Klicken Sie auf die Quelle bearbeiten Modusschaltfläche, um die Edition wechseln und sehen, wie es aussieht.
-    Schreiben Sie der Inhalts, die angezeigt wird, klicken Sie auf die interaktive Schaltfläche (eine interaktiven Schaltfläche ermöglicht der Anwendung, stellen Sie eine bestimmte Aktion wie das Öffnen einer Seite der Anwendung, Umleiten von Store oder alle Arten von Datenquellen, die Sie bereitstellen können).
-    Schreiben die Inhalte, die angezeigt wird, klicken Sie auf die Schaltfläche Beenden (, indem Sie auf die Schaltfläche Beenden, die Webansicht wird nicht mehr angezeigt).
 
-    Ergebnis
 
![WebView5][32]

-    Der Benutzer darüber informiert, und klicken Sie darauf.
-    Die Ansicht für den wird als dem der Benutzer zur Interaktion mit Popup angezeigt.

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 
