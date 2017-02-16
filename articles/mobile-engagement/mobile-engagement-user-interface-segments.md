<properties 
   pageTitle="Azure mobilen Engagement Benutzeroberfläche - Segmente" 
   description="Informationen Sie zum Erstellen und Verwalten von Segmente des Benutzer Verwendungsmuster mit Azure Mobile Engagement zu identifizieren" 
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

# <a name="how-to-create-and-manage-segments-of-users-to-identify-usage-patterns"></a>So erstellen und Verwalten von Segmente des Benutzer Verwendungsmuster zu identifizieren

In diesem Artikel werden die Registerkarte **Segmente** des Portals **Mobile Engagement** . Sie verwenden das **Mobile Engagement** Portal zur Überwachung und Verwaltung Ihrer mobilen apps aus.

Im Abschnitt Segmente der Benutzeroberfläche können Sie Ihre Benutzer basierend auf dem anderen Verhalten und Analytics, die Sie können aus der Anwendung erhalten und können auch über die Segmente-API zugreifen segmentieren arbeiten. Segmente werden zuerst 24 Stunden, nachdem sie erstellt werden, und sie neu, alle 24 Stunden berechnet werden, basierend auf den neuesten Analytics Informationen berechnet. Nachdem ein Segment berechnet wird, es "Tag zu Tag Verlauf" zeigt ein Diagramm an jeden Tag.


>[AZURE.NOTE] Viele Abschnitte des Portals **Mobile Engagement** Benutzeroberfläche enthalten die Schaltfläche **Hilfe anzeigen** . Drücken Sie diese Schaltfläche, um weitere kontextbezogene Informationen zu einem bestimmten Bereich zu erhalten.

## <a name="create-segments"></a>Erstellen von Segmenten
Sie können ein Segment basierend auf bis zu 10 Kriterien auf einen bestimmten Zeitraum von 60 Tage in der Vergangenheit aus dem Abschnitt Analytics erstellen. Beispielsweise können Sie ein Segment basierend auf Personen, die über bestimmte Seiten angezeigt oder für bestimmte Inhalte innerhalb der app innerhalb der letzten 10 Tage durchsucht erstellen. Diese Informationen sind im Abschnitt Analytics verfügbar. Ja, können Sie es zum Erstellen eines Segments, und richten Sie dann eine Pushbenachrichtigung diese Teilmenge der Benutzer als Ziel ein, um die im Zusammenhang mit der Anwendung wieder abrufen. 
 
> Hinweis: Sobald ein Segment berechnet wurde, darf Sie nicht sein bearbeitet. Sie können nur geklont (kopierten) oder gelöscht (gelöscht). Ein Segment kann innerhalb derselben Anwendung (mit den gleichen AppID) kopiert werden, und es kann auch in anderen Programmen (mit einer anderen AppID) kopiert werden. 
 
 ![segments1][35] 

## <a name="examples-segments"></a>Beispiele für Segmente
 ![segments2][36]

Segmente können Sie die Endbenutzer der Anwendung segmentieren.
Ihre Benutzer segmentieren ist eine wichtige marketing-Strategie. Azure Mobile Engagement ermöglicht das Archiv Ihrer Daten abrufen und Erstellen von benutzerdefinierten Segmenten. Dieses leistungsstarke Tool können Sie Informationen zu Ihrer Kunden berichtet in Ihrer Anwendung. Sie können einfach Ihre Segmente analysieren und Ihre Segmente als Pushbenachrichtigungen Ziele.
Allgemeine Anwendungsfall-ist, dass Sie eine Pushbenachrichtigungen eine Benachrichtigung, die Endbenutzer So bewerten Sie eine Anwendung im Speicher zu fördern senden möchten. Statt eine Benachrichtigung an alle Ihre Endbenutzer senden, können Sie ein Segment erstellen, die nur Benutzer angeben möchten, die eine Anwendung jeden Tag für den letzten Monat verwendet haben und hatten einen herausragender Benutzer auftreten. Wenn Sie weniger, aber sehr zielgerichteten Pushbenachrichtigungen senden, erhalten Sie eine bessere Rendite aus.
 
 ![segments3][37]

### <a name="segments-you-can-create-based-on-the-major-azure-mobile-engagement-elements"></a>Segmente, die Sie erstellen können, basierend auf die Hauptelemente Azure Mobile Engagement:
- Ereignis: Erstellen Sie ein Segment, dass ein bestimmter Ziele Ereignis der Anwendung, die mehr als zweimal pro Woche passiert ist. 
- Sitzung: Erstellen einer Benutzersegment, die die Anwendung letzte Woche mehr als 5 Mal verwendet haben.
- Aktivität: Erstellen einer Benutzersegment, die eine Seite oder Inhalte mehr oder weniger als 10 Zeit Letzter Monat verwendet haben.
- Auftrag: Erstellen einer Segment Benutzer, die ein Projekt mit mehr als zwei Mal einen halben Tag abgeschlossen haben.
- Absturz: Erstellen einer Segment alle Benutzer, die einen Absturz letzte Woche mehr als 10 Mal hatten. (Sie können dieses Segments mit einer Entschuldigung oder sogar einer Zinsperiode Pushbenachrichtigungen!)
- Fehler: Erstellen eines Segment alle Benutzer, die einen Fehler mehr als 100 Mal letzten 3 Tage aufweisen.
- App-Informationen: Erstellen eines Segments, das auf einer benutzerdefinierten App Informationen verweisen, die während der letzten 25 Tage passiert ist.
 
 ![segments4][38]

Um Segment optimale Weise zu verwenden, müssen Sie eine benutzerdefinierte Integration des SDK in Ihrer Anwendung durch einen Plan tagging "App Info" Kategorien getan haben.
Klicken Sie dann zur Startseite der Schnittstelle wechseln Sie, wählen Sie die Anwendung, die Sie möchten, und klicken Sie im Abschnitt "Segmente" auf.

1. Wählen Sie im Abschnitt "Segmente" aus.
2. Klicken Sie auf das Segment"Neu", um ein neues Segment zu erstellen.

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a>Real Nutzungsdauer-Beispiel: Erstellen einer einfachen Segment basierend auf "Sitzung" Informationen
Erstellen Sie ein Segment alle Endbenutzer, die Ihre app in der letzten Woche mindestens 50 Mal verwendet haben. Hier finden Sie nur den Endbenutzer, die mindestens 30 Sekunden in Ihrer app pro Sitzung aufgewendet haben. Hieraus sehen alle den Endbenutzer, die eine positive Erfahrung in Ihrer app verfügen. Klicken Sie dann könnte das Segment erstellt verwendet werden, um eine Benachrichtigung an diese Endanwender zu bitten Sie diese so bewerten Sie Ihre app im Speicher zu übertragen.
 
 ![segments5][39]

1. Benennen Sie Ihre Segment akzeptieren, um es in der Liste "Segment" schnell zu finden.
2. Klicken Sie auf die Schaltfläche "Erstellen".
 
 ![segments6][40]

Wählen Sie die Sitzung.
 
 ![segments7][41]

1. Wählen Sie den Zeitraum "Letzte Woche" aus.
2. Klicken Sie auf Weiter.
 
 ![segments8][42]

1. Wählen Sie den Operator, die zwischen der Liste relevant sind: =; ≥, ≤.
2. Geben Sie die gewünschte Anzahl.
3. Wählen Sie das gewünschte vorkommen. 
4. Klicken Sie auf Weiter.
In diesem Beispiel wird festgelegt, damit entsprechen, die in der letzten Woche, Benutzer, die mindestens 50 Sitzungen vorgenommen haben.
 
 ![segments9][43]

Für die Sitzung Segmentierung können Sie die Länge pro Sitzung als Kriterium auswählen.

1. Wählen Sie den Operator aus der Liste aus.
2. Geben Sie die Länge pro Sitzung ein.
3. Klicken Sie auf Weiter.
In diesem Beispiel hierhin über alle der Sitzungen, die in dem Abschnitt Vorkommen voneinander getrennt wurde haben, wählen Sie nur die Benutzer, die mehr als 30 Sekunden pro Sitzung aufgewendet haben.
 
 ![segments10][44]

Benennen Sie das Kriterium zu, um ihn in den vollständigen Trichter abrufen, und klicken Sie auf Fertig stellen.
 
 ![segments11][45]

Wenn Sie das Kriterium eingerichtet haben, wird es im Segment Trichter angezeigt.
Da ein Segment auf Analytics-Daten basiert, werden die Segmente einmal pro Tag berechnet.
In diesem Beispiel 47,7 % von der total Endbenutzer abgeglichen das Kriterium. Sie sollten die Benutzer, die eine ansprechende hatten und wahrscheinlich eine höhere Bewertung vorzunehmen, wenn Sie eine Benachrichtigung, die fragt sie so bewerten Sie die app im Speicher Pushbenachrichtigungen werden.


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
 
