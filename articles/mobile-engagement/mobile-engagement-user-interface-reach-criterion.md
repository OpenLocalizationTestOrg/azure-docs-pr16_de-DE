<properties 
   pageTitle="Azure mobilen Engagement Benutzeroberfläche - Reichweite Kriterium" 
   description="Erfahren Sie, wie mit Zielkriterien Pushbenachrichtigungen massensendungen zu ermitteln an eine ausgewählte Teilmenge mit Azure Mobile Engagement Benutzer senden" 
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


# <a name="how-to-use-targeting-criteria-to-send-push-campaigns-to-a-select-subset-of-your-users"></a>So verwenden Sie Zielkriterien Pushbenachrichtigungen massensendungen zu ermitteln, eine ausgewählte Teilmenge der Benutzer senden

Verwendet das Publikum von bestimmter Kriterien für die Schaltfläche "Neue Kriterien" ist eine der leistungsfähigsten Konzepte in Azure Mobile Engagement, hilft Ihnen relevante gesendeten Textnachricht schieben, dass die Kunden anstelle von Spam jeder zu antworten. Können Sie Ihrem Publikum basierend auf standard Kriterien beschränken und reproduzieren schiebt, um zu bestimmen, wie viele Personen die Benachrichtigung erhalten sollen.

**Siehe auch:**

- [Benutzeroberfläche Dokumentation - Reichweite - neue Pushbenachrichtigungen für eine Marketingkampagne][Link 27]

## <a name="audience-criteria-can-include"></a>Zielgruppe Kriterien können einbeziehen:
- **Technicals:** Sie können Zielgruppen basierend auf den gleichen technische Informationen Sie in den Abschnitten Analytics und überwachen sehen. **Siehe auch:** [Benutzeroberfläche Dokumentation - Analytics] [ Link 15], [Benutzeroberfläche Dokumentation - Monitor][Link 16]
- **Speicherort:** Verwenden "Echtzeit Speicherort reporting" mit Geo-Zäune, Geo-Speicherort als Kriterium können Zielgruppe ab der GPS-Position werden sollen. "Aus-Bereich Speicherort Berichterstattung" Anruf auch verwendet werden, um einer Zielgruppe ab der Position Handy aufgenommene adressieren ("Echtzeit Speicherort Berichterstattung" und "Aus-Bereich Speicherort Reporting" muss aktiviert sein, aus dem SDK). **Siehe auch:** [SDK-Dokumentation - iOS - Integration] [ Link 5], [SDK Dokumentation - Android - Integration][Link 5]
- **Feedback zu erreichen:** Sie können Ihr Publikum basierend auf deren Feedback von vorherigen Reichweite Benachrichtigung durch Reichweite Feedback von Ankündigungen, Umfragen und legt Daten Zielgruppen ausrichten. Auf diese Weise können Sie besser ausrichten des Publikums nach zwei oder drei erreichen massensendungen zu ermitteln, als Sie das erste Mal konnte. Sie können auch Benutzer herausgefiltert werden, die bereits eine Benachrichtigung mit ähnlichen Inhalten, erhalten durch Festlegen eines Campaign nicht an Benutzer gesendet werden, die bereits eine bestimmte vorherigen Campaign erhalten verwendet werden. Benutzer können auch ausgeschlossen werden, die im Lieferumfang von einer bestimmten für eine Marketingkampagne Empfang von neuen schiebt noch aktiv ist. **Siehe auch:** [Benutzeroberfläche Dokumentation - erreichen – Pushbenachrichtigungen Inhalt][Link 29]
- **Verlauf installieren:** Sie können Informationen basierend auf die Stelle, an der die Benutzer Ihre App installiert verfolgen. **Siehe auch:** [Benutzeroberfläche Dokumentation - Einstellungen][Link 20]
- **Benutzerprofil:** Sie können basierend auf Standard Zielgruppen Benutzerinformationen und Sie können Ziel basierend auf den benutzerdefinierten app Informationen, die Sie erstellt haben. Dies umfasst Benutzer, die aktuell angemeldet sind und die Benutzer, die bestimmte Fragen beantwortet haben, die Sie gebeten haben, um festzulegen, in der app einfach wie diese vorherigen sicherstellen, dass sich geantwortet haben. Alle Ihre App-Informationen definiert für Ihre app Anzeigen von in dieser Liste.
- Segmente: Sie können auch Ziel basierend auf Segmente, die Sie erstellt haben basierend auf bestimmten Benutzerverhalten, die mehreren Kriterien enthält. Alle Ihre Segmente definiert für Ihre app in dieser Liste angezeigt. **Siehe auch:** [Benutzeroberfläche Dokumentation - Segmente][Link 18]
- **App Informationen:** Benutzerdefinierte Tags der App-Informationen können in "Einstellungen", um Benutzerverhalten zu verfolgen erstellt werden. **Siehe auch:** [Benutzeroberfläche Dokumentation - Einstellungen][Link 20]

## <a name="example"></a>Beispiel: 
Wenn Sie eine Ankündigung nur zum untergeordnete Satz Benutzer zu übertragen, die eine Language Pack für innerhalb der app-Aktion ausgeführt haben möchten.

1. Wechseln Sie zur Seite Einstellungen Anwendung, wählen Sie im Menü "App Info" aus, und wählen Sie "Neue app Info"
2. Registrieren Sie sich einen neue boolesche app Info namens "InAppPurchase"
3. Stellen Sie eine Anwendung einrichten dieser app Informationen auf "true", wenn der Benutzer erfolgreich Kauf ein in-app ausführt (mithilfe der SendAppInfo ("InAppPurchase",...) Funktion)
4. Wenn Sie nicht dies von Ihrer Anwendung durchführen möchten, können Sie dies mit dem Gerät API aus der Back-End-tun)
5. Klicken Sie dann einfach müssen Sie Ihre Ankündigung, mit einem Kriterium beschränken Ihrem Publikum "InAppPurchase" festlegen dass Benutzer erstellen "true")
 
> Hinweis: Verwendet, basierend auf Kriterien als app Info Kategorien Azure Mobile Webcamvideos, um Informationen aus Ihrer Benutzer-Geräte zu sammeln und vor der Pushbenachrichtigungen gesendet werden, damit eine Verzögerung verursachen können erfordert. Verschiebt komplexe Pushbenachrichtigungen-Konfiguration, die Optionen (wie aktualisieren Badges) auch verzögert werden können. Verwenden eine Marketingkampagne "einen Screenshot" die Pushbenachrichtigungen-API ist die absolut schnellste Pushbenachrichtigungen-Methode in Azure Mobile Engagement. Verwenden nur app Info Kategorien als Pushbenachrichtigungen Kriterien für eine Zeit für eine Marketingkampagne (entweder aus der API erreicht haben oder der Benutzeroberfläche) ist der nächsten schnellste Methode, da app Info Kategorien auf dem Server gespeichert werden. Mithilfe von anderen Zielkriterien für eine Pushbenachrichtigungen für eine Marketingkampagne ist die am häufigsten flexible, aber langsamste Pushbenachrichtigungen Methode, da Azure Mobile Engagement weist die Geräte Abfragen, um die für eine Marketingkampagne senden.
 
![Reichweite-Criterion1][29] 

## <a name="criterion-options-apply-to"></a>Kriterium Optionen gelten für:
- **Technicals**     
- Firmware Namen: Firmware Namen
- Firmwareversion: Firmwareversion
- Gerätemodell: Gerätemodell
- Gerätehersteller: Gerätehersteller
- Version der Anwendung: Version der Anwendung
- Carrier Namen: Carrier nicht definierter Namen
- Carrier Land: Carrier Country undefiniert
- Netzwerk-Typ: Netzwerk-Datentyp
- Gebietsschema: Gebietsschema
- Bildschirmgröße: Bildschirmgröße
- **Speicherort**      
- Letzte bekannte Bereich: Land, Region, Ort
- Echtzeit Geo-Zäune: Liste der POIs ("Name", "Aktionen"), kreisförmigen POI (Name, Breite, Länge, Radius in Meter)
- **Feedback zu erreichen**     
- Ankündigung Feedback: Ankündigung, Feedback
- Umfrage unter Feedback: Umfrage, Feedback
- Umfrage unter Antwort Feedback: Antwort Feedback, Frage und Wahlmöglichkeiten Umfrage
- Daten Pushbenachrichtigungen Feedback: Daten Pushbenachrichtigungen, Feedback
- **Installieren Sie Verlauf**     
- Speicher: Speicher, undefiniert
- Quelle: Quelle, undefiniert
- **Benutzerprofil**     
- Geschlecht: Männer oder Frauen, undefiniert
- Geburtsdatum an: Operator, Datum, undefiniert
- Teilnahme: WAHR oder falsch, nicht definiert
- **App-Informationen**      
- Zeichenfolge: Zeichenfolge, die nicht definiert
- Datum: Datum, undefiniert, operator
- Ganzzahl: Operator, Anzahl, undefinierte
- Boolesch: WAHR oder falsch, undefiniert
- **Segment**    
- Name der Segmente (Dropdownliste) Ausschluss (Zielbenutzer, die nicht Bestandteil dieses Segments sind).

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
 
