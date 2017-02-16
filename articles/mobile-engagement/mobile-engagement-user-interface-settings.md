<properties 
   pageTitle="Azure mobilen Engagement Benutzeroberfläche - Einstellungen" 
   description="Informationen Sie zum Verwalten der globalen Einstellungen der Anwendung mithilfe von Azure Mobile Engagement" 
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

# <a name="how-to-manage-the-global-settings-of-your-application"></a>Zum Verwalten der globalen Einstellungen der Anwendung

Die **Einstellungen** im Menü verfügbaren Optionen für eine Anwendung variieren je nach Plattform der Anwendung und den Berechtigungen, die Sie für die Anwendung gewährt wurde. Fügen Sie die folgenden Einstellungen: Details, Projekte, systemeigenen Pushbenachrichtigungen, Pushbenachrichtigungen Geschwindigkeit, Kategorie (app Info) und professionellen Druck. Die Kategorie (app Info) Menüoption des Abschnitts Einstellungen kann von der Anwendung (mit dem SDK) oder der Back-End-(mit dem Gerät-API) verwaltet werden. 


>[AZURE.NOTE] Viele Abschnitte des Portals **Mobile Engagement** Benutzeroberfläche enthalten die Schaltfläche **Hilfe anzeigen** . Drücken Sie diese Schaltfläche, um weitere kontextbezogene Informationen zu einem bestimmten Bereich zu erhalten.

## <a name="details"></a>Details

Zeigen Sie ermöglicht es Ihnen, ändern den Namen und eine Beschreibung der Anwendung an, die den Besitzer der Anwendung und Ihre Rollenberechtigungen 

Analytics-Konfiguration können Sie anzeigen oder ändern den Tag, Wochen auf starten, und der Uhrzeit der Aufbewahrungsrichtlinien in Tag(en) ab.
 
  ![settings1][46]
 
## <a name="projects"></a>Projekte

Können Sie alle Projekte wählen Sie die Anwendung in angezeigt werden soll. 

Sie können auch für ein Projekt suchen und Anzeigen der Name, Beschreibung, Besitzer und Ihre Rollenberechtigungen eines Projekts, die Ihrer Anwendung gehört.

Weitere Informationen finden Sie unter: [UI-Dokumentation Start][Link 13]
 
  ![settings3][48]

## <a name="native-push"></a>Systemeigene Pushbenachrichtigungen

Können Sie ein neues Zertifikat oder löschen und vorhandenes Zertifikat für die Verwendung mit einer systemeigenen Pushbenachrichtigungen zu registrieren. Systemeigene Pushbenachrichtigungen ermöglicht Azure Mobile Engagement zu Pushbenachrichtigungen an Ihrer Anwendung zu einem beliebigen Zeitpunkt, auch wenn es nicht ausgeführt wird. 

Nach der Bereitstellung von Anmeldeinformationen oder Zertifikate für mindestens ein systemeigener Pushbenachrichtigungen Dienst, können Sie auswählen, "Jederzeit" beim Erstellen der PUSHBENACHRICHTIGUNGEN-API erreicht haben massensendungen zu ermitteln, und verwenden Sie den Parameter "Notifizierer".



### <a name="apple-push-notification-service-apns"></a>Apple-Pushbenachrichtigungsdienst (APNS)

Zum Aktivieren einer systemeigenen Pushbenachrichtigungen mit dem Apple Pushbenachrichtigungen Benachrichtigungsdienst müssen Sie Ihr Zertifikat registrieren. Sie müssen den Typ des Zertifikats als Entwicklung (Entwickler) oder Herstellung (PROD) angeben. Klicken müssen Sie, Ihr Zertifikat und das Kennwort hochgeladen werden.

Weitere Informationen finden Sie unter: [SDK-Dokumentation - iOS - zum Vorbereiten Ihrer Anwendungs für Apple Pushbenachrichtigungen][Link 5]
 
![settings4][49]
 
### <a name="windows-push-notification-service-wpns"></a>Windows-Pushbenachrichtigungsdienst (WPNS)

Klicken Sie zum Aktivieren einer systemeigenen Pushbenachrichtigungen mit Windows-Benachrichtigungsdienst müssen Sie Ihrer Anwendung Anmeldeinformationen bereitstellen. Sie benötigen Ihre Paket Sicherheits-ID (SID) und dem geheimen Schlüssel.
 
![settings5][50]
 
### <a name="google-cloud-messaging-for-android-gcm"></a>Google Cloud Messaging für Android (GCM)

Um GCM mithilfe einer systemeigenen Pushbenachrichtigungen zu aktivieren, müssen Sie die Anweisungen in Google folgen. Und Sie Sie einen einfachen Server-API Key fügen müssen ohne IP-Einschränkungen konfiguriert. Erfordert die Integration mit dem SDK für Android v1.12.0 +.

Weitere Informationen finden Sie unter: 

- [SDK-Dokumentation Android wie GCM integriert werden soll.][Link 5]
- [Google Developer GCM Guide](http://developer.android.com/guide/google/gcm/gs.html)
 
### <a name="amazon-device-messaging-for-android-adm"></a>Amazon-Gerät Messaging für Android (ADM)

Um ADM mithilfe einer systemeigenen Pushbenachrichtigungen zu aktivieren, müssen Sie angeben, Amazon <OAuth credentials> Client-ID-Client geheim (erfordert die Integration mit SDK für Android v2.1.0 +) aus.

Weitere Informationen finden Sie unter: 

- [SDK-Dokumentation Android wie ADM integriert werden soll.][Link 5]
- [Amazon Entwicklertools ADM-Dokumentation](https://developer.amazon.com/sdk/adm/credentials.html#Getting)
 
![settings6][51]

## <a name="push-speed"></a>Pushbenachrichtigungen Geschwindigkeit

Zeigt die aktuelle Pushbenachrichtigungen Geschwindigkeit Ihrer Anwendung, und ermöglicht es Ihnen, die Geschwindigkeit Pushbenachrichtigungen Ihrer Anwendung definieren.
 
  ![settings7][52]

## <a name="tag-app-info"></a>Tag (app Info)

![settings11][56]
  
## <a name="commercial-pressure"></a>Professioneller Druck


![settings12][57]


## <a name="see-also"></a>Siehe auch

- [Konzepte][Link 6]
- [Problembehandlung beim Dienst Leitfaden][Link 24]

 

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

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
 
