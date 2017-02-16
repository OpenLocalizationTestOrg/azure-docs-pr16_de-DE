<properties 
   pageTitle="Azure mobilen Engagement Benutzeroberfläche - Reichweite für eine Marketingkampagne" 
   description="Laern wie erstellen und Verwalten der Pushbenachrichtigung Kampagnen mit Azure Mobile Engagement" 
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


# <a name="how-to-create-and-manage-push-notification-campaigns"></a>So erstellen und verwalten Pushbenachrichtigungen Benachrichtigung
Im Abschnitt Reichweite der Benutzeroberfläche können zum Erstellen einer neuen Pushbenachrichtigungen für eine Marketingkampagne mit einer komplexen Formel können, indem Sie die Informationen, die Sie zum Senden einer Benachrichtigung Pushbenachrichtigungen müssen. Die Optionen für eine Pushbenachrichtigungen für eine Marketingkampagne sind ein wenig anders je nach den vier Campaign: Ankündigungen, Umfragen, legt Daten und Kacheln (nur für Windows Phone).

### <a name="option-applies-to"></a>Option gilt für:
- Sprachen: Alle (Ankündigungen, Umfragen, Daten schiebt, Kacheln)
- Für eine Marketingkampagne: Alle (Ankündigungen, Umfragen, Daten schiebt, Kacheln)
- Benachrichtigung: Ankündigungen, Umfragen
- Content: Eindeutige für jeden für eine Marketingkampagne
- Zielgruppe: Alle (Ankündigungen, Umfragen, Daten schiebt, Kacheln)
- Zeitrahmen: Ankündigungen, Umfragen, Kacheln
- Test: Alle (Ankündigungen, Umfragen, Daten schiebt, Kacheln)
 
![Reichweite-Campaign1][20]

## <a name="languages"></a>Sprachen
Das Dropdownmenü Sprachen können Sie um eine andere Version von Ihrem Pushbenachrichtigungen an Geräte zu senden, die mit verschiedenen Sprachen festgelegt werden. Standardmäßig erhalten alle Geräte die gleiche Pushbenachrichtigungen unabhängig von der Sprache, die sie verwenden festgelegt sind. Benutzer mit ihrem Gerät zu einer anderen Sprache festlegen, die standardmäßig Sprachversion von der Pushbenachrichtigungen erhalten. Viele der Optionen für eine Marketingkampagne Pushbenachrichtigungen können Sie alternative Inhalt für alle zusätzlichen Sprachen angeben, die Sie auswählen. 
 
![Reichweite-Campaign2][21]

### <a name="language-differences-apply-to"></a>Sprachunterschiede gelten für:
- Sprachen: Eindeutige Sprachen möglicherweise über die Standardsprache ausgewählt werden
- Für eine Marketingkampagne: Dasselbe gilt für alle Sprachen
- Benachrichtigung: Eindeutige für die einzelnen Sprachen sowie die Standardsprache
- Content: Eindeutige für die einzelnen Sprachen sowie die Standardsprache
- Zielgruppe: Möglicherweise nach einem Kriterium separaten Sprache gefiltert werden
- Zeitrahmen: gleichen für alle Sprachen
- Test: Möglicherweise einzelnen Sprachen jeweils gesendet werden
 
### <a name="supported-languages"></a>Unterstützte Sprachen:
- Arabisch (Ar) 
- Bulgarisch (bg) 
- Katalanisch (ca) 
- Chinesisch (Zh) 
- Kroatisch (h) 
- Czech (cs) 
- Dänisch (da) 
- Niederländisch (nl) 
- Englisch (En) 
- Finnisch (Fi) 
- Französisch (Französisch) 
- Deutsch (de) 
- Griechisch (el) 
- Hebräisch (he) 
- Hindi (Hallo) 
- Ungarisch (Hu) 
- Indonesisch (Id) 
- Italienisch (it) 
- Japanischen (ja) 
- Koreanisch (Ko) 
- Lettisch (lv) 
- Litauisch (Lt) 
- Malaiisch (Macrolanguage) (ms) 
- Norwegisch (Bokmål) (Hinweis) 
- Polnisch (pl) 
- Portugiesisch (pt) 
- Rumänisch (Ro) 
- Russisch (ru) 
- Serbisch (sr) 
- Slowakisch (sk) 
- Slowenisch (sl) 
- Spanisch (es) 
- Schwedisch (Pa) 
- Tagalog (tl) 
- Thailändisch (ten) 
- Türkisch (tr) 
- Ukrainisch (Großbritannien) 
- Vietnamesisch (vi) 
 
## <a name="campaign"></a>Für eine Marketingkampagne
Im Abschnitt für eine Marketingkampagne können Sie den Namen und die Kategorie der dienen als auch festlegen, als wäre Sie planen, ignorieren im Abschnitt Publikum eine Pushbenachrichtigungen für eine Marketingkampagne und senden diese für eine Marketingkampagne über die API erreicht haben (und einige Elemente mit niedriger Ebene Pushbenachrichtigungen API) stattdessen. Kategorien können mit einer benutzerdefinierten Benachrichtigungsvorlage, basierend auf vordefinierten Einstellungen Steuerelement in der app-Benachrichtigungen verwendet werden. Sie erhalten eine Liste der bestehenden "Kategorien" über die API erreicht haben.

> Warnung: Wenn Sie die Option "Ignorieren Benutzergruppe werden Pushbenachrichtigungen für Benutzer über die-API gesendet" im Abschnitt "Campaign" eine Campaign Reichweite verwenden, die für eine Marketingkampagne wird nicht automatisch senden, müssen Sie sie manuell über die erreichen-API zu senden.
 
![Reichweite-Campaign3][22]
 
### <a name="option-applies-to"></a>Option gilt für:
- Name: alle
- Kategorie: Ankündigungen, Umfragen
- Ignorieren der Zielgruppe, Pushbenachrichtigungen für Benutzer über die-API gesendet: alle
 
## <a name="notification"></a>Benachrichtigung
Sie können im Abschnitt Benachrichtigung grundlegende Einstellungen für Ihre Pushbenachrichtigungen einschließlich festlegen: den Titel der Pushbenachrichtigungen, die Nachricht, ein Bild in der app oder wenn es zwar ist. Viele Benachrichtigungseinstellungen gelten nur für die Plattform des Geräts. Sie können auswählen, ob Ihre Pushbenachrichtigungen "in der app" oder "ausblenden"app gesendet werden oder beides. (Denken Sie daran, dass Benutzer können "Teilnahme" oder "nicht mehr an" von "-app" am: das Betriebssystem von der Ebene auf ihren Geräten legt und Azure Mobile Engagement wird nicht in der Lage, diese Einstellung außer Kraft setzen. Beachten Sie auch, dass das erreichen API "in"app verarbeitet und legt "Out-of-app". Die Pushbenachrichtigungen API kann verwendet werden, "von"-app schiebt zu behandeln.) Schiebt können mit Bildern oder HTML-Inhalt, einschließlich Tiefe Links zum Verknüpfen von außerhalb Ihrer App oder an eine andere Position in der App (Android SDK 2.1.0 oder höher intent Kategorien erforderlich) angepasst werden. Sie können das Symbol oder den iOS-Badge ändern und senden Sie den Text oder Web Inhalt (ein Popupfenster mit HTML-Inhalt, URL-Link an eine andere Position innerhalb oder außerhalb der app). Sie können die vornehmen Android-Geräten anrufen oder Vibrationsalarm durch das drücken. (Denken Sie daran, dass Sie die richtige benötigen SDK Berechtigungen in Ihrem Android-Gerät Manifestdatei zum Anrufen oder ein Gerät Vibrationsalarm.) Es ist zurzeit kein standard für Android "Überblick" Auswahl an Papiergrößen, da Bildschirmgrößen unterscheiden sich auf jedem Gerät, aber 400 x 100 Bilder auf nahezu jedem Bildschirmgröße arbeiten.

### <a name="delivery-types"></a>Übermittlung Arten:
-    Abmelden bei der app nur: die Benachrichtigung übermittelt werden, wenn der Benutzer die Anwendung nicht verwendet werden.
- Out-of app nur Benachrichtigung erfordert ein Zertifikat von Apple oder Google (APNS oder GCM Zertifikat).
- Innerhalb der app nur: die Benachrichtigung angezeigt wird, wenn die Anwendung ausgeführt wird.
- Die Benachrichtigung verwendet das Capptain Übermittlungssystem, um den Benutzer zu erreichen. Sie können die visuellen Layout/Anzeige von Ihrem Pushbenachrichtigungen vollständig anpassen.
- Jederzeit: Dieser Option wird sichergestellt, dass eine Benachrichtigung zu senden, die entweder die Anwendung oder nicht ausgeführt wird.

 
![Reichweite-Campaign4][23]

### <a name="option-applies-to"></a>Option gilt für:
- Benachrichtigung: Ankündigungen, Umfragen
 
## <a name="content"></a>Inhalt
Inhaltsabschnitt können Sie um den Inhalt Ihrer Ankündigungen, Umfragen, legt Daten und Kacheln (nur für Windows Phone) zu ändern. Die Einstellung der Inhalt von Pushbenachrichtigungen massensendungen zu ermitteln ist für den Typ der für eine Marketingkampagne spezifisch. 

### <a name="see-also"></a>Siehe auch
- [Benutzeroberfläche Dokumentation - erreichen – Inhalt Pushbenachrichtigungen][Link 29]
 
![Reichweite-Campaign5][24]

## <a name="audience"></a>Zielgruppe
Im Abschnitt Benutzergruppe können Sie eine Liste aller Elemente der für eine Marketingkampagne oder Grenzwerte Ihrer Campaign angepasste Kriterien Grundlage zu beschränken definieren. Der Standardsatz von Optionen zum Einschränken der Zielgruppe können Sie entweder die neue oder alte oder nur Benutzern systemeigenen Pushbenachrichtigungen Pushbenachrichtigungen. Sie können auch festlegen, dass ein Kontingent beschränken Sie die Anzahl der Benutzer, die die Pushbenachrichtigungen zu erhalten. Sie können manuell bearbeiten den Ausdruck wie Ihre für eine Marketingkampagne gefiltert werden, wenn Sie eine oder mehrere Kriterium für Zielgruppen einbeziehen möchten. Sie können einen Ausdruck Zielgruppe manuell eingeben. Ein solcher Ausdruck muss explizit das Verhältnis zwischen der Kriterien definieren. Kriterium wird durch einen Bezeichner beschrieben, die muss mit einem Großbuchstaben beginnen und darf keine Leerzeichen enthalten. Die Beziehung zwischen den Kriterien mithilfe von beschrieben 'und', 'oder', 'keine' sowie Operatoren '(',')'. Beispiel: "Criterion1 oder (Criterion1 und nicht Criterion2)".

> Hinweis: Mit ein großes Publikum im Lieferumfang von massensendungen zu ermitteln, kann das serverseitige Scan verwendet werden langsam, insbesondere dann, wenn Sie versuchen, mehrere Kampagnen gleichzeitig zu starten.

- Falls möglich, nur beginnen Sie eine für eine Marketingkampagne nacheinander.
- Das größte, nur beginnen Sie vier massensendungen zu ermitteln jeweils.
- Drücken Sie nur für die aktive Benutzer (nur Benutzer, die mit einer systemeigenen Pushbenachrichtigungen erreicht werden können das Kontrollkästchen "populärer" und "Populärer nur aktive Benutzer") so, dass nur die Benutzer, die noch die App ist installiert, und verwenden sie gescannt werden müssen.
Nachdem Ihr Publikum definiert ist, können Sie die Schaltfläche simulieren, um herauszufinden, wie viele Benutzer diese Pushbenachrichtigungen empfangen werden. Dadurch wird die Anzahl der bekannten Benutzer potenziell Ziel von dieser Zielgruppe (Dies ist eine Schätzung ausgehend von einer Stichprobe von Benutzern) zu berechnen. Beachten Sie, dass Benutzer, die die Anwendung deinstalliert haben, auch Bestandteil dieser Zielgruppe sind, aber nicht erreicht werden kann.

### <a name="see-also"></a>Siehe auch
- [Benutzeroberfläche Dokumentation - Reichweite - neue Pushbenachrichtigungen Kriterium][Link 28]

![Reichweite-Campaign6][25]

### <a name="edit-expression"></a>Ausdruck bearbeiten
![Reichweite-Campaign7][26]
 
### <a name="limit-your-audience-option-applies-to"></a>Grenzwert, die Ihre Zielgruppe Option gilt:
- Nur eine Teilmenge der Benutzer populärer: alle (Ankündigungen, Umfragen, legt Daten, Kacheln)
- Nur alte Benutzer populärer: alle (Ankündigungen, Umfragen, legt Daten, Kacheln)
- Nur neue Benutzer populärer: alle (Ankündigungen, Umfragen, legt Daten, Kacheln)
- Nur Benutzer, die sich im Leerlauf populärer: Ankündigungen, Umfragen, Kacheln
- Nur aktive Benutzer populärer: alle (Ankündigungen, Umfragen, legt Daten, Kacheln)
- Nur Benutzer, die mit einer systemeigenen Pushbenachrichtigungen erreicht werden können populärer: Ankündigungen, Umfragen
 
## <a name="time-frame"></a>Zeitrahmens
Im Abschnitt Zeitrahmens können Sie festlegen, wenn die Pushbenachrichtigungen gesendet werden, oder Sie den Zeitrahmen leer lassen, um die für eine Marketingkampagne sofort zu starten. Beachten Sie, dass mithilfe der Endbenutzer die Zeitzone die für eine Marketingkampagne Tag früher beginnen kann als für Ihre Endbenutzer in Asien erwartet, und senden kleine Stapel Push nacheinander, bis alle Zeitzonen in der Welt der Zeitrahmen für Ihre Campaign festlegen entsprechen. Mithilfe der Endbenutzer Zeitzone kann auch verursachen verspätungen in massensendungen zu ermitteln, da es die Zeit über das Telefon anfordern, bevor Sie beginnen die Pushbenachrichtigungen muss.

> Hinweis: Kampagnen, ohne ein Enddatum Zwischenspeichern kann schiebt lokal und weiterhin angezeigt, nach dem manuell abgeschlossen massensendungen zu ermitteln. Zum Vermeiden des Problems, spezifische eine Endzeit für massensendungen zu ermitteln.

### <a name="see-also"></a>Siehe auch
- [Erreichen – wie Vorgehensweisen – Planung][Link 3] 
 
![Reichweite-Campaign8][27]

### <a name="settings-apply-to"></a>Einstellungen anwenden auf:
- Zeitrahmen: Ankündigungen, Umfragen, Kacheln
 
## <a name="test"></a>Test
Im Abschnitt können dieses Pushbenachrichtigungen vor dem Speichern die für eine Marketingkampagne zu Ihrer eigenen Testgerät zu senden. Wenn Sie alle benutzerdefinierten Sprachen für diese für eine Marketingkampagne konfiguriert haben, können Sie die Pushbenachrichtigungen in jede Sprache testen. Sie können ein Testgerät von "Mein Konto" einrichten.
> Hinweis: Keine serverseitigen, die Daten angemeldet ist, wenn Sie die Schaltfläche mithilfe "testen" legt, Daten werden nur für Pushbenachrichtigungen tatsächliche massensendungen zu ermitteln protokolliert.

### <a name="see-also"></a>Siehe auch
- [Benutzeroberfläche Dokumentation – Mein Konto][Link 14]
 
![Reichweite-Campaign9][28]

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
 
