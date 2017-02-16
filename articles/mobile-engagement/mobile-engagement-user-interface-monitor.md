<properties
   pageTitle="Azure mobilen Engagement Benutzeroberfläche - Monitor"
   description="Informationen Sie zum Überwachen von Echtzeitdaten über die Anwendung mit Azure Mobile Engagement"
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

# <a name="how-to-monitor-real-time-data-about-your-application"></a>Überwachen der Echtzeitdaten zu Ihrer Anwendung

In diesem Artikel werden die Registerkarte **MONITOR** des Portals **Mobile Engagement** . Sie verwenden das **Mobile Engagement** Portal zur Überwachung und Verwaltung Ihrer mobilen apps aus. Beachten Sie, dass zum Verwenden des Portals Sie zunächst ein Konto **Azure Mobile Engagement** zu erstellen müssen. 


Im Abschnitt Überwachen der Benutzeroberfläche in Echtzeit Analytics stellt und ermöglicht Ihnen, Benachrichtigungen festgelegt werden, wenn Schwellenwerte für die meisten denselben Informationen erreicht werden, die in der Vergangenheit im Abschnitt [ANALYTICS](mobile-engagement-user-interface-analytics.md) der Benutzeroberfläche zur Verfügung. Finden Sie unter Abschnitt **Glossar** unter dem Thema [Konzepte](http://go.microsoft.com/fwlink/?LinkId=525555) Definitionen Begriffe und Abkürzungen in Analytics und Überwachung (wie die folgende: aktiven Benutzer, neuer Benutzer, Benutzer beibehalten, Sitzung, Benutzer Pfad Graph, Benutzer Karte, Überwachung URLs, Trends, Aktivität, Ereignis, Position, Fehler, zusätzliche Informationen, Absturz und App-Info).

>[AZURE.NOTE] Viele Abschnitte des Portals **Mobile Engagement** Benutzeroberfläche enthalten die Schaltfläche **Hilfe anzeigen** . Drücken Sie diese Schaltfläche, um weitere Kontextinformationen zu einem bestimmten Bereich zu erhalten.

## <a name="monitor---sessions-jobs-events-errors-and-crashes"></a>Monitor - Sitzungen, Aufträge, Ereignisse, Fehlern und Abstürzen

Sie können sehen, wie viele Benutzer befinden, in der Sitzung und auf bestimmte Seiten und bestimmte Aktionen ausführen. Sie können die Benutzeraktivitäten geteilt durch Sitzungen, Aufträge, Ereignisse, Fehler und Absturz anzeigen. Sie können finden Sie unter aktuelle Informationen und die Informationen aus der letzten Stunde, Tag oder Woche anzeigen. Sie können die Informationen in jeder Kategorie oder sortieren nach den bestimmten Sitzung, Position, Ereignis, Fehler und Absturz angezeigt.  Live Überwachung ist hilfreich, die bei Fehlern eine Pushbenachrichtigungen für eine Marketingkampagne verwenden, um festzustellen, ob es eine Kurssteigerung in Aktion ist nach dem Senden der Pushbenachrichtigung.

![Monitor1][14]  

## <a name="troubleshooting-with-monitor---events---details"></a>Behandlung von Problemen mit - Ereignisse - Monitordetails

Auslösen eines Ereignisses in der Anwendung von Ihrem Testgerät, und suchen sie im - Ereignisse - Monitordetails ist eine der einfachsten Methoden, um Ihre Geräte-ID für Ihr Testgerät finden und zum Bestätigen dieser Azure Mobile Engagement Integration von Analytics, Überwachung und Segmente aus Ihrer Anwendung arbeiten. Nachdem Sie die Geräte-ID des Geräts Test haben, können Sie Ihren Test-Geräten in "Mein Konto – Geräte" hinzufügen. Wenn Sie ein Ereignis generiert werden können, stellen Sie sicher, dass Azure Mobile Engagement ordnungsgemäß in Ihrer Android/iOS/Web/Windows/Windows Phone-app mit dem SDK integriert ist.

Weitere Informationen finden Sie unter: [SDK-Dokumentation][Link 5]

![Monitor2][15]  

## <a name="troubleshooting-with-monitor---crashes---details"></a>Behandlung von Problemen mit Monitor - stürzt ab - Details

Sie können überprüfen Absturzinformationen über Ihre app - Absturz - Monitordetails, um zu bestimmen, warum die app abstürzt. Sie sollten auch bekannte Probleme mit jeder Version des SDK in die Versionsinformationen für jede Version des SDK für Android/iOS/Web/Windows/Windows Phone nachschlagen.

Weitere Informationen finden Sie unter: [SDK-Dokumentation - Versionsinformationen][Link 5]

![Monitor3][16]

## <a name="monitor---alerts"></a>Monitor - Benachrichtigungen
Sie können auch Bedingungen für Benachrichtigungen angeben, die automatisch per E-mail oder Sofortnachricht an Sie gesendet werden. (Alle XMPP-kompatible Dienste wie Google GTalk oder Apple iChat werden unterstützt). Benachrichtigungen basieren auf eine vordefinierte Erkennung Schwellenwert größer als (>) oder kleiner als (<) eine bestimmte Anzahl von Sitzungen, Aufträge, Ereignisse, Fehlern oder abstürzen pro Stunde, Minute oder Sekunde. Benachrichtigungen können alle Aktivitäten eines bestimmten Typs oder nur überwachen eine bestimmte Position, Ereignis oder Fehler Aktivität. 

Sie können auch einen minimalen Erkennung Satz angeben, also den minimal erforderlichen Minuten, die getrennt werden sollen zwei Benachrichtigungen für die gleichen Warnung, um sicherzustellen, dass beim Auslösen der Warnung, nie mehr als 1 Benachrichtigung pro angegebenen Intervall erhalten sollen.

![Monitor4][17]


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
