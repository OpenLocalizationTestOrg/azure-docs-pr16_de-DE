<properties
   pageTitle="Azure mobilen Engagement Benutzeroberfläche - Analytics"
   description="Erfahren Sie, wie Sie zu Ihrer Anwendung mit Azure Mobile Engagement zurückliegenden Datenanalyse"
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

# <a name="how-to-analyze-historical-data-about-your-application"></a>So zurückliegende Daten zu Ihrer Anwendung zu analysieren

In diesem Artikel werden die Registerkarte **ANALYTICS** des Portals **Mobile Engagement** . Sie verwenden das **Mobile Engagement** Portal zur Überwachung und Verwaltung Ihrer mobilen apps aus. Beachten Sie, dass zum Verwenden des Portals Sie zunächst ein Konto **Azure Mobile Engagement** zu erstellen müssen.


Im Abschnitt Analytics der Benutzeroberfläche bietet aggregierten Informationen zu Ihrer Anwendung basierend auf Archiv Ihrer Daten, die alle 24 Stunden aktualisiert wird. Die Informationen werden auf anderen Dashboards erstellter Linie/Balken-aus-Kreis-Diagramme, Raster und Karten angezeigt. Die Daten können auch als CSV-Dateien heruntergeladen werden. Die meisten dieser Daten in Echtzeit im Abschnitt Überwachen der Benutzeroberfläche verfügbar ist, und auch aus der Analytics-API zugegriffen werden kann.

>[AZURE.NOTE] Viele Abschnitte des Portals **Mobile Engagement** Benutzeroberfläche enthalten die Schaltfläche **Hilfe anzeigen** . Drücken Sie diese Schaltfläche, um weitere kontextbezogene Informationen zu einem bestimmten Bereich zu erhalten.

## <a name="standard-and-custom-analytics"></a>Standard- und benutzerdefinierte Analytics

Azure Mobile Engagement bietet eine Reihe von grundlegende, standard analytischen Informationen zu Ihrer Applications, die grafisch dargestellt werden können, sobald Sie Ihre app mit dem SDK integrieren. Azure Mobile Engagement bietet auch die Möglichkeit zum Sammeln zusätzlicher benutzerdefinierter Analytics die gewünschten Informationen über das Verhalten von den Endbenutzern. Sie können dafür durch Erstellen eines Plans Kategorie benutzerdefinierte "Kategorien (app Info)", erstellt in **Einstellungen** , damit diese zusätzlichen Daten zu Azure Mobile Engagement für Sie erfassen können.



## <a name="analytics"></a>Analytics
- Dashboard: Zeigt allgemeine Informationen zu den neuen und aktiv Benutzern und deren Trends.
- Benutzer: Benutzer durch ihre Geräte-ID. gekennzeichnet: Dieser Bezeichner für jedes Gerät eindeutig ist (einen neuer Benutzer ist tatsächlich ein neues Gerät). Ein Benutzer ist als neue auf ein angegebenes Zeitintervall betrachtet, wenn er seine ersten Sitzung während dieses Zeitraums durchgeführt hat. Wie beibehalten, wenn er mindestens eine Sitzung während der letzten 7 Tage durchgeführt hat, wird ein Benutzer betrachtet. Aktive Benutzer sind Benutzer, die mindestens eine Sitzung in einem bestimmten Zeitraum vorgenommen werden. Sie können nach, monatlich, wöchentlich, täglich oder stündlich Zeiträume sortieren. Alle Diagramme sind vergleichbar, allerdings können Sie zum Filtern nach anderen Features wie die Version der Anwendung, und klicken Sie dann auf Sortieren nach einer bestimmten Zeitspanne. Standard durch die Integration von SDK gesammelte Informationen umfassen Folgendes: aktive Benutzer, neuer Benutzer, Anzahl Sitzungen, die Länge der einzelnen Sitzung, technische Informationen zu Land, lokal, Speicherort, Carrier Sprache, Geräte, Firmware, Netzwerk (WIFI), Versionen der app und SDK, indem Sie Kunden verwendet. Diese Informationen kann in Echtzeit aus dem Abschnitt Monitor angezeigt werden.

> Hinweis: Der Zeitraum basiert auf das Datum aus der Benutzer des Audiogeräts, damit ein Benutzer, dessen Telefon des Datums nicht ordnungsgemäß festgelegt wurde, konnte in der falschen Zeitraum angezeigt.

- Aufbewahrung: Ein Benutzer betrachtet wie auf ein angegebenes Zeitintervall beibehalten werden soll, wenn er seine ersten Sitzung während dieses Zeitraums durchgeführt hat. Sie können die Zeitintervalle während, die Benutzer beibehalten (und neue Benutzer) gezählt werden in Stunden, Tagen, Wochen oder Monaten ändern. Der Benutzer Aufbewahrung Analytics basiert auf Kohorten. Eine Kohorte ist eine Reihe von alle neuen Benutzer, die für einen bestimmten Zeitraum erkannt (d. h., die Gruppe der Benutzer, die Durchführung ihrer ersten Sitzung während dieses Zeitraums). Wir verwenden Kohorten von 1 Tag, 2 Tage, 4-Tag, 7 Tage oder 1 Monat. Ausgehend von einer Kohorte, 1-täglich, 2 Tage, 4-Tag, 7 Tage, oder 1-Monat, Azure mobilen Engagement Festlegen aller Benutzer berechnet, die die sind und Kohorte angehören weiterhin aktiv (d. h., die Gruppe der Benutzer, die mindestens eine Sitzung während des Zeitraums durchgeführt). Diese Gruppe von Benutzern heißt Versionsverlaufs Kohorte. (Azure Mobile Engagement können sehen, wie viele Benutzer weiterhin Ihre app verwenden möchten, aber nur der Plattform Store kann Ihnen mitteilen, wie viele Benutzer Ihre app – beispielsweise GooglePlay, iTunes, Windows Store deinstalliert usw..).
- Sitzungen: Eine Verwendung der Anwendung von einem Benutzer. Sitzungen aus generiert wird, die Reihenfolge der Aktivitäten, die von Benutzern durchgeführt werden (eine Aktivität in der Regel, die Verwendung von einen Bildschirm der Anwendung zugeordnet ist, aber dies kann variieren je nachdem, wie das SDK wurde in der Anwendung integriert). Ein Benutzer kann nur eine Aktivität nacheinander ausführen: eine Sitzung gestartet, sobald der Benutzer seinen ersten Aktivität startet und wird beendet, wenn er seine letzte Aktivität fertig gestellt wurde. Wenn ein Benutzer mehr als ein paar Sekunden bleibt ohne Aktivitäten, ist seine Abfolge von Aktivitäten in zwei unterschiedlichen Sitzungen teilen.
- Aktivitäten: Die Namen der einzelnen Bildschirm in Ihrer Anwendung und die Länge der Zeit Benutzer verbringen in jedem Bildschirm. Aktivitäten sind einer benutzerdefinierten analytisches Option, die die "app Info" Tags entsprechen wird, die Sie für Ihre eigenen app eingerichtet haben:
- Benutzerpfad: Zeigt an, wie Ihre Benutzer Ihrer Anwendung Aktivitäten (Bildschirme) navigieren. Verschieben Sie den Schieberegler, um die Ebene des Details anzupassen. Blaue Knoten darstellen Ihrer Anwendung Aktivitäten. Ihre Größe ist proportional auf den Zeitpunkt Benutzer darin aufgewendet. Weiße Knoten darstellen Sitzung starten und beenden. Rote Knoten darstellen stürzt ab. Links darstellen Übergänge zwischen Ihrer Anwendung Aktivitäten (oder Aktivitäten und Absturz). Klicken Sie auf einen Knoten oder einen Link zu eine QuickInfo mit weiteren Informationen über Ihre Daten angezeigt werden: die verstrichene Zeit in einem bestimmten Bildschirm die Anzahl von Übergängen und den Prozentsatz der Übergänge von die Aktivität Quelle auf das Zielaktivität. (Eine---60 %---> B bedeutet, die Benutzer auf Aktivität A wird auf Aktivität B wechselt 60 % der Zeit.) Sie können das Diagramm neu anordnen, wie sie verdeutlichen möchten möchten; jedes Mal, wenn Sie eine Änderung vorgenommen haben, wird seine Position gespeichert. Sie können ein- oder Ausblenden der stürzt ab, um das Diagramm aufzuhellen.
- Ereignisse: Bestimmte Aktionen vom Benutzer in der Anwendung geöffnet. Die Verteilung der Ereignisse wird als die Anzahl von Ereignissen pro Benutzer pro Sitzung angezeigt. Ein Ereignis repräsentiert Chat, beispielsweise ein Klick auf eine Schaltfläche oder den Empfang einer Benachrichtigung. (Die Bedeutung von Ereignissen hängt wie das SDK in die Anwendung integriert wurde.) Ein Ereignis kann auftreten, während eine Sitzung oder eines Auftrags oder eigenständige handeln.
- Aufträge: Ähnliche Ereignisse außer diese auf die Länge der Aktion konzentrieren. Beispielsweise könnten Sie Aufträge mitteilen, technische Informationen, wie Inhalt zu laden oder eines Anrufs an Webdienst lange. Es kann auch angezeigt, wie lange dauert einen Benutzer ein Formular ausfüllen, erstellen Sie ein Konto oder Bestellung. Einer die Dauer eines Vorgangs darstellt, beispielsweise die Dauer eines Vorgangs herunterladen oder die Uhrzeit ein Banner wird auf dem Bildschirm angezeigt. (Die Bedeutung der Einzelvorgänge hängt wie das SDK in die Anwendung integriert wurde.) Aufträge werden in der Regel Hintergrundaufgaben, die außerhalb des Gültigkeitsbereichs von einer Sitzung (d. h., ohne alle Benutzeraktivitäten) ausgeführt werden zugeordnet.
- Technicals: Technische Informationen zu den Geräten der Benutzer der app, die Sie nachverfolgen möchten, wie etwa das Gebietsschema, Carrier, Netzwerk, Gerät, Firmware, und Bildschirm Größe des Benutzers Geräte und die Version der App und die SDK-Version, die in der app verwendet werden können.
- Fehler: Informationen zu technischen Fehlern innerhalb der Anwendung, die keine zum Absturz die Anwendung verursachen. Ein Fehler handelt es sich um eine instant Problem, beispielsweise ein Fehler bei der oder eine fehlerhafte Manipulation. (Die Bedeutung von Ereignissen hängt wie das SDK in die Anwendung integriert wurde.) Ein Fehler auftreten kann, während einer Sitzung oder eines Auftrags oder eigenständige handeln.
- Absturz: Informationen zu Fehlern, die zu Ihrer Anwendung zu einem Absturz führen. Ein Absturz ist ein unerwarteter Zustand, in dem die Anwendung stoppt Durchführung erwarteten Funktionen und muss beendet werden. Ein Absturz ist in der Regel auf einen Fehler in der Anwendung.

![Analytics2][11]

## <a name="accessing-the-retention-overview"></a>Zugreifen auf die Aufbewahrung (Übersicht)
![Analytics3][12]

Übersicht über die Aufbewahrung wird in der Mitte in mehrere Karten, jede mit Übersicht über die für einen bestimmten Aufbewahrungszeitraum aufgeschlüsselt. Im Beispiel wird der Aufbewahrungszeitraum 2 Tage angezeigt. Die anderen Karten zeigen die 4-Tag und 7 Tage Aufbewahrungszeiträume.

## <a name="understanding-the-retention-overview-cards"></a>Grundlegendes zu Karten Aufbewahrung (Übersicht)
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Jede Karte besteht 3 Hauptteilen:
1. 1: Kohorte und den Zeitraum eingestuft
2. 2 bis 4: die Aufbewahrung für den aktuellen Zeitraum
3. 5: eine Sparkline mit der aufgezeichneten

### <a name="here-is-detailed-information-about-each-element"></a>So sieht detaillierte Informationen über die einzelnen Elemente aus:
1.    Kohorte und Periode: diese Überschrift bietet den Typ der Kohorte. Hier bedeutet "2 Tage lang" an, dass wir uns anschauen das Verhalten der Benutzer über 2 Tage, die Benutzer, die über einen Zeitraum von 2 Tage und ob eingegangen diese in den folgenden Blöcke von 2 Tagen verbinden. Im oben genannten Beispiel berücksichtigt die Aktivitäten von Benutzern zwischen dem 21. und 22. November.
2.    Dadurch werden die Aufbewahrung Rate über die 21 und 22 November für die Benutzer in 19 und 20 November eingeht. Hier haben wir 1 aktiven Benutzer zwischen dem 21. und 22., über die 3, die neuen Benutzer zwischen dem 19. und 20. wurden.
3.    Dieses visuelle Indikator bietet dieselbe Informationen wie oben grafisch dargestellt. (Die dritte des Kreises ist von der Zahl 33 %). Die Farbe bietet zusätzliche Informationen: Grün zeigt an, diese Nummer aus der vorherigen Berechnung wächst. Gelb bedeutet unveränderliche und Rot bedeutet unbefriedigend.
4.    Dies weist darauf hin, die Werte für die Berechnung verwendet.
5.    Hierbei handelt es sich um eine Sparkline mit der aufgezeichneten Werte Aufbewahrungsrichtlinien. Können Sie die Werte in der Vergangenheit, dass eine umfassende Ansicht wie es entwickeltes angezeigt.


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
