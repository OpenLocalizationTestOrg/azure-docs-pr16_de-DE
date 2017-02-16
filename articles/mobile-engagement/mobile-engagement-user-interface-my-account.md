<properties 
   pageTitle="Azure mobilen Engagement Benutzeroberfläche – Mein Konto" 
   description="Erfahren Sie, wie Ihr Konto Profil- und Testen Geräte mit Azure Mobile Engagement verwalten" 
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

# <a name="how-to-manage-your-account-profile-and-test-devices"></a>Zum Verwalten Ihrer Konto Profil- und Test-Geräte
 
In diesem Artikel werden **die Startseite des Portals **Mobile Engagement** ** . Sie verwenden das **Mobile Engagement** Portal zur Überwachung und Verwaltung Ihrer mobilen apps aus. 
 
Um zur Seite **Mein Konto** zu gelangen, klicken Sie auf Ihr Konto am oberen Rand der Seite.

Abschnitt "Mein Konto" der Benutzeroberfläche ist, wo Sie können anzeigen und ändern die Einstellungen für Ihr Konto, einschließlich für Ihr Profil und Geräte-IDs zu testen. Diese Einstellungen enthalten Elemente, die auch über das Gerät-API zugegriffen werden können.

![MyAccount1][7]  

## <a name="profile"></a>Profil:
Sie können anzeigen oder ändern Sie die nachfolgend aufgeführten Einstellungen für Ihr Konto. Sie können auch eine andere über die Benutzerberechtigung zum Verwenden Ihrer Anwendungs anhand ihrer e-Mail-Adresse aus den [Start](mobile-engagement-user-interface-home.md)verleihen.

![MyAccount2][8]  

## <a name="devices"></a>Geräte:
Sie können anzeigen, hinzufügen oder Entfernen von testen Gerät IDs Test Gerät, das Sie verwenden können, testen Sie Ihre **erreicht haben** oder **Pushbenachrichtigungen** massensendungen zu ermitteln. Kontextbezogene Anleitungen zum Suchen der Geräte-ID des Geräte für jede Plattform (iOS, Android, Windows Phone, usw.) werden angezeigt, wenn Sie "Neues Gerät" klicken. 
 
![MyAccount3][9]  
 
Verwenden Sie Pushbenachrichtigungen API oder Gerät-API, die Sie Ihrer Benutzer eindeutige Geräte-ID. (der Geräte-ID-Parameter) wissen müssen. Es gibt mehrere Methoden zum Abrufen dieser Daten ein:
 
1. Aus der Back-End können Sie das Feature "Get" der Gerät-API verwenden, können Sie um die vollständige Liste mit Bezeichnern Gerät zu gelangen.
2. Aus der app können Sie das SDK Bezugsarten. (Auf Android, rufen Sie die Funktion getDeviceID() der Klasse Agent, und klicken Sie unter iOS, lesen Sie die Geräte-ID-Eigenschaft der Klasse Agent.)
3. Aus einer Reichweite Ankündigung Wenn die Ankündigung zugeordnete Aktion URL des Musters {Geräte-ID} enthält wird es automatisch durch die ID des Geräts Auslösen der Aktion ersetzt.
http://<example>.com/Registeruser?deviceid = {Geräte-ID} & Otherparam = Myparamdata Fassung: http://<example>.com/Registeruser?deviceid = XXXXXXXXXXXXXXXX & Otherparam = Myparamdata 
4. Aus einer Reichweite Web Ankündigung Wenn Sie der HTML-Code der Ankündigung des Musters {Geräte-ID} enthält wird es automatisch durch die ID des Geräts Anzeige im Web Ankündigung ersetzt.
Hier ist meine Geräte-ID.: {Geräte-ID} Fassung: Hier ist meine Geräte-ID.: XXXXXXXXXXXXXXXX
5.  Öffnen Sie die Anwendung auf Ihrem Gerät und Durchführen eines Ereignisses in Ihrer app die kategorisiert hat.
Anhand der "UI - Ihre app - Monitor - Ereignisse - Details" finden Sie das Ereignis, das Sie in der Liste durchgeführt.
Klicken Sie auf das Ereignis im Monitor.
Finden Sie Ihre Geräte-ID in der Liste der Geräte, die das Ereignis ausgeführt haben.
Klicken Sie dann können Sie diese Geräte-ID kopieren und in der "UI - Mein Konto - Geräte - neues Gerät - Geräteplattform wählen" registrieren.
>(Beachten Sie, dass beim IDFA für iOS deaktiviert ist, die Geräte-ID im Laufe der Zeit ändern können, wenn Sie deinstallieren Sie Ihre app.)

##<a name="troubleshooting-guide"></a>Leitfadens zur Problembehandlung
-  [Problembehandlungsleitfadens - Dienst][Link 24]

## <a name="see-also"></a>Siehe auch
-  [Benutzeroberfläche Dokumentation - Start][Link 13]


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


 
 
