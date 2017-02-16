<properties 
   pageTitle="Azure mobilen Engagement Benutzeroberfläche - Reichweite" 
   description="Informationen Sie zum Erreichen an die Benutzer Ihrer Anwendung mit Azure Mobile Engagement mit Pushbenachrichtigungen" 
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


# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a>So erreichen Sie an die Benutzer Ihrer Anwendung mit Pushbenachrichtigungen

In diesem Artikel werden die Registerkarte **erreichen** des Portals **Mobile Engagement** . Sie verwenden das **Mobile Engagement** Portal zur Überwachung und Verwaltung Ihrer mobilen apps aus. Beachten Sie, dass zum Verwenden des Portals Sie zunächst ein Konto **Azure Mobile Engagement** zu erstellen müssen. Weitere Informationen finden Sie unter [Erstellen eines Kontos Azure Mobile Engagement](mobile-engagement-create.md).

Im Abschnitt Reichweite der Benutzeroberfläche des Pushbenachrichtigungen für eine Marketingkampagne-Verwaltungstools können Sie hier erstellen/bearbeiten/aktivieren/Ende/Monitor ist und erhalten Statistiken über Pushbenachrichtigungen Benachrichtigung massensendungen zu ermitteln und Features, die auch über die API erreicht haben (und einige Elemente mit niedriger Ebene Pushbenachrichtigungen API) zugegriffen werden können. Denken Sie daran, dass, ob Sie die APIs oder die Benutzeroberfläche verwenden, Sie integrieren müssen sowohl Azure Mobile Engagement und Reichweite in Ihrer Anwendung für jede Plattform mit dem SDK, bevor Sie verwenden können, erreicht haben massensendungen zu ermitteln.

>[AZURE.NOTE] Viele Abschnitte des Portals **Mobile Engagement** Benutzeroberfläche enthalten die Schaltfläche **Hilfe anzeigen** . Drücken Sie diese Schaltfläche, um weitere kontextbezogene Informationen zu einem bestimmten Bereich zu erhalten.

## <a name="four-types-of-push-notifications"></a>Vier Arten von Pushbenachrichtigungen
1.    Ankündigungen - ermöglichen Ihnen die Werbung Nachrichten an Benutzer senden, die sie an eine andere Position innerhalb der app umgeleitet oder wahlweise zu einer Webseite oder außerhalb der app Store. 
2.    Umfragen - können Sie Informationen aus den Endbenutzer sammeln, indem sie Fragen.
3.    Daten schiebt - ermöglichen es Ihnen, eine binäre oder base64-Datendatei zu senden. Die Informationen in einer Pushbenachrichtigungen Daten wird an Ihrer Anwendung ändern Ihrer Benutzer für den aktuellen Erfahrung in Ihrer app gesendet. Die Anwendung benötigt werden sollen, die Daten in einer Pushbenachrichtigungen Daten zu verarbeiten.

## <a name="campaign-details"></a>Details für eine Marketingkampagne

Sie können bearbeiten, Klonen, löschen oder massensendungen zu ermitteln, die noch nicht aktiviert haben, indem Sie auf deren Namen Auswahlspalte aktivieren, oder klicken Sie auf, um sie zu öffnen. Sie können massensendungen zu ermitteln, die bereits aktiviert wurde, indem Sie auf deren Namen Auswahlspalte klonen, oder klicken Sie auf, um sie zu öffnen. Sie können jedoch eine für eine Marketingkampagne nicht geändert, nachdem er aktiviert wurde.
 
![Reach1][18]

## <a name="reach-feedback"></a>Feedback zu erreichen

Klicken Sie auf **Statistik** um die Details einer Reichweite für eine Marketingkampagne anzuzeigen. Die **einfache** Ansicht bietet eine visuelle Darstellung in Form einer Spalte Balkendiagramm zu den Vorkommnissen, nachdem eine für eine Marketingkampagne aktiviert wurde. Die Option **Erweiterte** Ansicht bietet eine genauere Details der Pushbenachrichtigungen für eine Marketingkampagne. Diese Details werden nicht zur Verfügung, wenn Sie einen Test für eine Marketingkampagne h. einer Pushbenachrichtigungen an ein Testgerät gesendet senden möchten. Hier ist, wie Sie diese Angaben bei der Interpretation sollten:

1. **Pushed** - gibt die Anzahl der Nachrichten, die mit den Geräten abgelegt. Diese Anzahl hängt von der Zielgruppe, die Sie beim Erstellen der Campaign Pushbenachrichtigungen angegeben haben. Wenn Sie jedes beliebige Zielpublikum nicht angeben, werden dann diese Pushbenachrichtigungen an alle registrierten Geräte gesendet ab. Wie alle anderen Pushbenachrichtigungen Dienste wir nicht direkt mit den Geräten Pushbenachrichtigungen aber drücken Sie sie stattdessen die entsprechenden Platform bestimmte Pushbenachrichtigungen Benachrichtigung Services (PNS - APNS/GCM/WNS), damit sie die Geräte die Benachrichtigungen vorführen können. 

2.  **Übermittelte** - gibt die Anzahl der Nachrichten, die erfolgreich von der PNS mit dem Gerät gelieferten und bestätigt als durch Mobile Engagement SDK erhalten. 
        
    *Gründe für das übermittelte zählen kleiner als abgelegten zählen wird:*
    
    1. Wenn der Benutzer hat die app vom Gerät deinstalliert, aber die PNS nicht darüber wissen, zu dem Zeitpunkt, die wir die Pushbenachrichtigungen an die PNS senden, wird die Nachricht gelöscht.
    2. Wenn das Gerät die app hat, aber die Geräte selbst für längere Zeit offline wurden, tritt die PNS die Nachricht an das Gerät zu übermitteln. 
    3. Wenn die Nachricht mit dem Gerät übermittelt erhalten, aber die Mobile Engagement SDK in der app nicht den Inhalt der Nachricht erkennt, löscht sie diese Nachricht. Dies kann geschehen, wenn die Anpassung der Benachrichtigung in der app generiert eine Ausnahme die fangen wir SDK und legen Sie die Nachricht. Dies kann auch auftreten, wenn die app auf dem Gerät ist eine Version des Mobile Engagement SDK, nicht die neuere Version der von der Plattform gesendeten Nachricht Pushbenachrichtigungen zu verstehen ist, aber dies ist nur bei die app aktualisiert wurde, nachdem die Benachrichtigung über die Serviceplattform verteilt wurde. Die Registerkarte **Erweitert** wird angezeigt, wie viele Nachrichten gelöscht wurden. 
    4. Auf iOS-Geräte Nachrichten manchmal Nachrichten nicht gesendet entweder das Gerät ist auf Niedrig oder ob die app viel Power in beim remote-Benachrichtigungen zu verarbeiten Anspruch nimmt. Dies ist eine Einschränkung des iOS-Geräte.   

3.  **Wird angezeigt** – Dies gibt die Anzahl von Nachrichten, die erfolgreich an dem app-Benutzer auf dem Gerät in Form von eine Benachrichtigung in der Mitte der Benachrichtigung des Systems Pushbenachrichtigungen/Out-von-app oder eine Benachrichtigung in der app in der mobilen app angezeigt werden.  Die Registerkarte **Erweitert** erfahren Sie, wie viele System Benachrichtigungen wurden und wie viele innerhalb der app-Benachrichtigungen wurden. 
    
    *Gründe für die angezeigte zählen wird kleiner als übermittelte Count (warten angezeigt werden)*
    
    1. Wenn Sie die Benachrichtigung für eine Marketingkampagne ein Enddatum geführt ist es möglich, dass die Benachrichtigung wurde übermittelt, aber wenn die Uhrzeit gesucht haben, öffnen und auf der app-Benutzer anzeigen können, wurde bereits abgelaufen, damit es nie angezeigt wurde.   
    2. Wenn die Benachrichtigung eine Benachrichtigung in der app ist, und klicken Sie dann die Benachrichtigung nur angezeigt wird, wenn der app-Benutzer die app wird geöffnet. In Fällen, wo der app-Benutzer die app geöffnet noch nicht, meldet das SDK, dass die Benachrichtigung wurde übermittelt, aber noch nicht angezeigt, bis die app geöffnet wird. 
    2. Wenn die Benachrichtigung eine Benachrichtigung, die innerhalb der app ist und so konfiguriert ist, wenn Sie auf eine bestimmte Aktivität/Bildschirm angezeigt werden soll, und klicken Sie dann als auch die Benachrichtigung gemeldet wird übermittelt, aber noch nicht übermittelt bis wird der Benutzer die app auf einem bestimmten Bildschirm geöffnet. 
    
4.  **Benutzerinteraktionen** – Dies gibt die Anzahl von Nachrichten, die mit dem der app-Benutzer interagiert hat und Nachrichten, die entweder verarbeitet oder beendete werden berücksichtigt. 

    - *Der app-Benutzer können die Aktion einer Benachrichtigung in eine der folgenden Methoden:*
            
        1. Wenn die Benachrichtigung ist eine Benachrichtigung System/Out-von-app oder eine innerhalb der app-Benachrichtigung, die nur für den Benachrichtigung gesendet, und klicken Sie dann der app-Benutzer auf die Benachrichtigung klickt.
        2. Die Benachrichtigung ist eine Benachrichtigung in der app mit einem Text oder Web-Ansicht oder Umfragen klickt der app-Benutzer auf die interaktive Schaltfläche in der Benachrichtigung auf.
        3. Wenn die Benachrichtigung eine Benachrichtigung in der app mit einem Web-Ansicht ist, und klicken Sie dann der app-Benutzer eine URL in der Webansicht [nur Android] klickt
    
    - *Der app-Benutzer kann eine Benachrichtigung in eine der folgenden Methoden beenden:*
    
        1. Wenn Sie direkt auf die Schaltfläche Schließen, klicken Sie auf die Benachrichtigung. 
        2. Abwesend Streifen oder löschen die Benachrichtigung. 
        3. Innerhalb der app-Benachrichtigungen mit Text/Webinhalte und Umfragen werden dem Benutzer app in zwei Schritten in der Regel angezeigt. Zunächst eine Benachrichtigung angezeigt, und wenn sie darauf klicken, den Inhalt der nachfolgenden Text/Web/Umfrage angezeigt. Der app-Benutzer kann verlassen, eine Benachrichtigung in einen der folgenden Schritte aus, und die Details in der erweiterten Ansicht erfasst werden diese. 

5.  **Actioned** - gibt die Anzahl der Nachrichten, die vom Benutzer app explizit verarbeitet wurden. Dies ist die Anzahl der interessantesten wie dies weist durch die Nachricht, die Sie in der Benachrichtigung abgelegt, wie viele app-Benutzer interessiert wurden. 
 
> [AZURE.NOTE] Klicken Sie auf iOS und Windows Plattformen, wenn der Benutzer die app hat öffnen und die für eine Marketingkampagne wurde ein Campaign "Jederzeit" und dann es ist möglich, dass beide von app und in der app-Benachrichtigungen gleichzeitig angezeigt werden. Dadurch kann als das übermittelte höhere Anzahl wird angezeigt. Wenn der Benutzer interagiert oder Aktionen der Benachrichtigung, und klicken Sie dann auf auch die Anzahl der Benutzer Interaktionen/Actioned größer als übermittelte könnte. 


![Reach2][19]

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
