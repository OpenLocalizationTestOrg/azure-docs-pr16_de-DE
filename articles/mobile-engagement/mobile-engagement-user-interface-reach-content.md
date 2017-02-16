<properties 
   pageTitle="Azure mobilen Engagement Benutzeroberfläche - Reichweite Inhalt" 
   description="Erfahren Sie, wie die verschiedenen Arten von Pushbenachrichtigungen Benachrichtigung Aktionen Azure Mobile Engagement eindeutige Inhalte verwalten" 
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

# <a name="how-to-manage-the-unique-content-of-the-different-types-of-push-notification-campaigns"></a>Zum Verwalten des eindeutigen Inhalts die verschiedenen Typen von Pushbenachrichtigungen Benachrichtigung massensendungen zu ermitteln
 
Im Abschnitt Inhalt eine neue Zeit für eine Marketingkampagne können Sie um den Inhalt Ihrer Ankündigungen, Umfragen, legt Daten und Kacheln (nur für Windows Phone) zu ändern. Die Einstellung der Inhalte von Pushbenachrichtigungen massensendungen zu ermitteln ist für den Typ der für eine Marketingkampagne spezifisch. 
 
### <a name="content-types"></a>Inhaltstypen:
- Ankündigungen
- Umfragen
- Daten Push-Vorgänge
- Kacheln (nur für Windows Phone)
 
## <a name="content-of-announcements"></a>Inhalt der Ankündigungen
 ![Reichweite-Content1][30] 

### <a name="choose-the-type-of-your-announcement"></a>Wählen Sie den Typ der Ankündigung aus:
-    Nur Benachrichtigung: Es ist eine einfache standardbenachrichtigung. Was bedeutet, dass, wenn ein Benutzer darauf klickt, wird keine zusätzliche Ansicht angezeigt, aber, nur die Aktion, die sie zugeordnet sind auftreten werden.
-    Text Ankündigung: es eine Benachrichtigung, der die Benutzer erhalten einen Blick auf einen Text anzeigen aktiviert ist.
-    Web Ankündigung: es eine Benachrichtigung, der die Benutzer erhalten einen Blick in einer Webansicht aktiviert ist.

### <a name="see-also"></a>Siehe auch
- [Erreichen – wie Vorgehensweisen - Ankündigungen][Link 3] 

### <a name="about-web-view-announcements"></a>Informationen zum Web Ansicht Ankündigungen:
Vorkommen des Musters "{Geräte-ID}" in den HTML-Code oder JavaScript-Code, die hier eingegebenen werden automatisch durch die ID des Geräts Anzeige der Ankündigung ersetzt. Dies ist eine einfache Möglichkeit zum Abrufen von Azure Mobile Engagement Gerätebezeichner in einem externen Webdienst auf Ihr Büro gehostet wird.
Wenn Sie eine Vollbildansicht "Web" (ohne den standardmäßigen Aktion beenden Schaltflächen und wir erläutern die notwendigen) erstellen möchten können Sie die folgenden Funktionen aus Ihrem Web Ansicht Ankündigung JavaScript-Code verwenden: 

-    Ausführen der Aktion Ankündigung: ReachContent.actionContent()
-    Beenden Sie die Ankündigung: ReachContent.exitContent()
 
### <a name="choose-your-action"></a>Wählen Sie die Aktion aus:

### <a name="about-action-urls"></a>Informationen zu Action URLs:
URLs ein, die vom Betriebssystem des Geräts eine gezielte interpretiert werden kann, kann eine URL-Aktion verwendet werden.
Eine dedizierte URL, die Ihrer Anwendung unterstützen (z. B. Benutzer wechseln zu einem bestimmten Bildschirm vornehmen) auch als eine Aktions-URL verwendet werden können.
Jede Wiederholung des Musters {Geräte-ID} wird automatisch durch die ID des Geräts durchführen der Aktion ersetzt. Dies kann zum Abrufen von Azure Mobile Engagement Gerätebezeichner über einen externen Webdienst gehostet wird, klicken Sie auf Ihr Büro einfach verwendet werden.

- **Android + iOS Aktionen**
    - Öffnen einer Webseite
    - http://\[Web-Website-Domäne\] 
    - Beispiel: http://www.azure.com
    - Senden einer e-Mail-Nachricht
    - Mailto:\[e-Mail-Empfänger\]?subject =\[Betreff\]& Textkörper =\[Nachricht\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Senden eines SMS
    - SMS:\[Telefonnummern\] 
    - Beispiel: sms:2125551212
    - Wählen einer Rufnummer
    - Tel:\[Telefonnummern\] 
    - Beispiel: tel:2125551212
- **Nur die Aktionen für Android**
    - Anwendung auf die Play Store herunterladen
    - Market://details?id=\[app-Pakets\] 
    - Beispiel: market://details?id=com.microsoft.office.word
    - Starten einer Suche Geo lokalisiert
    - Geo:0, 0?q =\[Suchabfrage\] 
    - Beispiel: Geo:0, 0? f = Starbucks, Paris
- **nur die Aktionen für iOS**
    - Eine Anwendung auf dem App Store herunterladen
    - http://iTunes.Apple.com/ [Land/Region] /app/ [Name der app] / ID [app-Nr]? Mt = 8 
    - Beispiel: http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8
    - Windows-Aktionen
    - Öffnen einer Webseite
    - http://\[Web-Website-Domäne\] 
    - Beispiel: http://www.azure.com
    - Senden einer e-Mail-Nachricht
    - Mailto:\[e-Mail-Empfänger\]?subject =\[Betreff\]& Textkörper =\[Nachricht\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Senden Sie eine SMS (Skype Store-App erforderlich)
    - SMS:\[Telefonnummern\] 
    - Beispiel: sms:2125551212
    - Wählen einer Rufnummer (Skype Store-App erforderlich)
    - Tel:\[Telefonnummern\] 
    - Beispiel: tel:2125551212
    - Anwendung auf die Play Store herunterladen
    - MS-Windows-Store: PDP?PFN =\[app-Paket-ID\] 
    - Beispiel: ms-Windows-Store: PDP? PFN = 4d91298a-07cb-40fb-Aecc-4cb5615d53c1
    - Bingmaps Suche starten
    - Bingmaps:?q =\[Suchabfrage\] 
    - Beispiel: Bingmaps:? f = Starbucks, Paris
    - Verwenden Sie ein benutzerdefiniertes Schema
    - \[Benutzerdefiniertes Farbschema\]://\[benutzerdefiniertes Farbschema Parameter\] 
    - Beispiel: myCustomProtocol://myCustomParams
    - Verwenden Sie ein Zusammenfassen von Daten (Store-App für weitere Erweiterung erforderlich)
    - \[Ordner\]\[Daten\]. \[Erweiterung\] 
    - Example:myfolderdata.txt
 
### <a name="build-a-tracking-url"></a>Erstellen einer Verlauf URL an:
-    Finden Sie im Abschnitt "Einstellungen" die <UI Documentation> für laut: Erstellen eines Verlauf-URL, die Benutzer eine andere Anwendung herunterladen ermöglichen wird.
 
### <a name="define-the-texts-of-your-announcement"></a>Definieren Sie die Texte Ihrer Ankündigung
Füllen Sie den Titel, Inhalt und Schaltfläche Texte Ihrer Ankündigung aus. Sie können Zielgruppen ausrichten Zielgruppe für eine zukünftige für eine Marketingkampagne basierend auf das Feedback Reichweite von wie Benutzer auf diese für eine Marketingkampagne geantwortet. Benutzergruppenadressierung kann basieren auf das Feedback von gibt an, ob diese für eine Marketingkampagne einfach, beantwortete, verarbeitet oder beendete abgelegt wurde.

### <a name="see-also"></a>Siehe auch
- [Benutzeroberfläche Dokumentation - Reichweite - neue Pushbenachrichtigungen Kriterium][Link 28]

## <a name="content-of-polls"></a>Inhalt von Umfragen
![Reichweite-Content2][31] füllen Sie den Titel, Beschreibung und Schaltfläche Texte Ihrer Ankündigung. Fügen Sie dann die Fragen und Auswahlmöglichkeiten für die Antworten auf Ihre Fragen.
Sie können die Zielgruppe einer zukünftigen für eine Marketingkampagne basierend auf das Feedback Reichweite von wie Benutzer auf diese für eine Marketingkampagne geantwortet adressieren. Benutzergruppenadressierung kann basieren auf gibt an, ob diese für eine Marketingkampagne einfach, beantwortete, verarbeitet oder beendete abgelegt wurde. Benutzergruppenadressierung kann auch Umfrage Antwort Feedback, basierend auf werden, in dem die Frage und Wahlmöglichkeiten Antwort als Kriterien verwendet werden.

### <a name="see-also"></a>Siehe auch
- [Benutzeroberfläche Dokumentation - Reichweite - neue Pushbenachrichtigungen Kriterium][Link 28]
 
## <a name="content-of-data-pushes"></a>Legt Inhalt der Daten
![Reichweite-Content3][32] 

### <a name="choose-the-type-of-your-data"></a>Wählen Sie den Typ der Daten aus:
- Text
- Binäre Daten
- Base64-Daten

### <a name="define-the-content-of-your-data"></a>Definieren Sie den Inhalt Ihrer Daten
- Wenn Sie Pushbenachrichtigungen Textdaten ausgewählt haben, kopieren Sie, und fügen Sie den Text im Feld "Inhalt".
- Wenn Sie Pushbenachrichtigungen entweder Binärzahl oder base64 Daten ausgewählt haben, verwenden Sie die Schaltfläche "Hochladen eine Datei" Sie Ihre Datei hochladen.
- Sie können Zielgruppen ausrichten Zielgruppe für eine zukünftige für eine Marketingkampagne basierend auf das Feedback Reichweite von wie Benutzer auf diese für eine Marketingkampagne geantwortet. Benutzergruppenadressierung kann basieren auf gibt an, ob diese für eine Marketingkampagne einfach, beantwortete, verarbeitet oder beendete abgelegt wurde.

### <a name="see-also"></a>Siehe auch
- [Benutzeroberfläche Dokumentation - Reichweite - neue Pushbenachrichtigungen Kriterium][Link 28]

## <a name="content-of-tiles-windows-phone-only"></a>Inhalt der Kacheln (nur für Windows Phone)
![Reichweite-Content4][33]

### <a name="define-the-content-of-your-tile"></a>Definieren Sie den Inhalt der Kachel
Die Kachel Nutzlast wird der Text in der Kachel der app auf Windows Phone-Geräten angezeigt werden.
Einer Kachel Pushbenachrichtigungen ist die Microsoft Pushbenachrichtigungen Benachrichtigung Service (MPNS) Version einer systemeigenen Pushbenachrichtigungen für Windows Phone. Der Kachel Pushbenachrichtigungen Typ ist der einzige Pushbenachrichtigungen, die nicht über eine Antwort verfügt und damit das Publikum der zukünftigen massensendungen zu ermitteln kann nicht auf den Ergebnissen einer Kachel Pushbenachrichtigungen für eine Marketingkampagne erstellt werden. 

### <a name="see-also"></a>Siehe auch
- [API-Dokumentation - API Reichweite - systemeigenen Pushbenachrichtigungen][Link 4]

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
 
