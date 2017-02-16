<properties 
   pageTitle="Azure mobilen Engagement Problembehandlung Führungslinien" 
   description="Problembehandlung bei der Azure mobilen Engagement" 
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

# <a name="azure-mobile-engagement---troubleshooting-guide"></a>Azure mobilen Engagement - Leitfadens zur Problembehandlung

## <a name="introduction"></a>Einführung
Der folgende zur Problembehandlung Leitfaden hilft Ihnen zu verstehen, dass die Hauptursachen für einige der häufigsten Probleme und wird Sie auf eigene beheben können. 

## <a name="general"></a>Allgemeine

Im Allgemeinen sollten Sie immer Folgendes sicher:

1. Stellen Sie sicher, dass Sie über alle erforderlichen zur Integration in unseren [überfordert Lernprogramme](mobile-engagement-windows-store-dotnet-get-started.md) beschriebenen Schritte überschritten haben
2. Verwenden Sie die neueste Version von die Platform SDKs auszuwählen. 
3. Testen Sie auf einem tatsächlichen Gerät und für Emulatoren, da einige Probleme nur Emulator spezifisch sind. 
4. Sie sind keine Grenzwerte/Drosselungen aus Mobile Engagement der dokumentierten Berührung [hier](../azure-subscription-service-limits.md)
5. Wenn Sie nicht die Mobile Engagement Dienst Back-End-Verbindung können oder anzeigen sind nicht geladene wird kontinuierlich dann Daten sicherzustellen, dass keine laufenden Service-Fälle durch Überprüfen [hier](https://azure.microsoft.com/status/)

## <a name="monitor-issues"></a>Probleme mit 'Überwachen'

### <a name="i-am-not-seeing-my-device-showing-up-on-the-monitor-tab"></a>Ich sehe nicht meinem Gerät auf der Registerkarte Monitor angezeigt
Registerkarte Monitor zeigt Ihre Mobile Engagement Platform in Echtzeit verbundenen Geräte. Wenn Sie auf einem Emulator und Gerät Debuggen, sollte mindestens eine Sitzung hier angezeigt werden. Wenn die app verteilt wurde, dann sehen Sie den Monitor "aktive Sitzungen" wirken sich auf die Geräte aus dem zur Plattform in Echtzeit verbunden sind. 

Wenn Sie Ihr Gerät nicht auf der Registerkarte Monitor angezeigt werden ist es wahrscheinlich ein SDK Integration Problem. Einige häufig angewendete Schritte zur Problembehandlung bei werden wie folgt aus:

1. Stellen Sie sicher, dass Sie die richtige Verbindungszeichenfolge in der mobilen app Verwenden von Abschnitt Tasten SDK und nicht Abschnitt Tasten API ist. Die Verbindungszeichenfolge herstellt mobile-app auf die Instanz der Engagement Mobile-app in der Sie Ihr Gerät auf der Registerkarte Monitor angezeigt werden. 
2. Für Windows-Plattform – Wenn die Seite überschreibt die `OnNavigatedTo` Methode, müssen Sie aufrufen `base.OnNavigatedTo(e)`.
3. Sie sind Mobile Engagement in einer vorhandenen mobile-app zu integrieren, und Sie können sicherstellen, dass Sie keine Schritte fehlen, indem Sie die Schritte für die erweiterte Integration [hier](mobile-engagement-windows-store-integrate-engagement.md)
4. Stellen Sie sicher, dass Sie mindestens eine Bildschirm-Aktivität senden möchten, indem Sie die Seite mit EngagementActivity je nach Plattform, die Sie beim Arbeiten in die [Erste Schritte-Lernprogramme](mobile-engagement-windows-store-dotnet-get-started.md)beschriebenen überschreiben.

### <a name="i-am-seeing-the-monitor-tab-showing-a-session-even-when-i-have-disconnected-or-closed-my-app-emulator"></a>Ich sehe die Registerkarte Monitor mit einer Sitzung auch wenn ich haben getrennt oder geschlossen Meine app / Emulator. 
Wenn Sie der einzige zu diesem Zeitpunkt zur Plattform verbunden sind, und Sie einen Emulator verwenden werden, um die app zu öffnen und ist dies wahrscheinlich ein Emulator Quirks. Im Allgemeinen müssen Sie sicherstellen, dass Sie wieder zum Startbildschirm auf dem Emulator für die app-Sitzung erfolgreich trennen gelangen. Darüber hinaus auf Windows-Plattform müssen beim Debuggen mit Visual Studio, Sie sicherstellen, dass in Visual Studio, wechseln Sie zu der Menüleiste **Lebenszyklusereignisse** , und klicken Sie auf **Anhalten** in die Sitzung wirklich schließen. Details finden Sie unter [Windows-Lernprogramm](mobile-engagement-windows-store-dotnet-get-started.md) . 

## <a name="analytics-issues"></a>'Analytics' Probleme

### <a name="i-am-not-seeing-any-data-refreshed-data-on-analytics-tab"></a>Ich sehe keine Daten / Daten auf der Registerkarte Analytics aktualisiert 
Analytics-Daten in regelmäßigen Abständen neu berechnet, und es kann bis zu 24 Stunden für diese Aktualisierung dauern. Diese Daten ist nicht in Echtzeit, und sehen Sie, dass es in diesem 24-Stunden-Zeitraum aktualisiert.
Gehen Sie wie folgt überprüfen Sie jedoch, dass Sie mindestens eine Bildschirmseite oder Aktivität an die Back-End-Plattform-um entweder überschreiben mindestens eine Seite mit senden, `EngagementActivity` oder Aufrufen `SendActivity` explizit. 

### <a name="i-am-seeing-incorrectly-captured-datetime-for-a-device-on-the-analytics-tab"></a>Ich sehe auf der Registerkarte Analytics falsch aufgenommene Datum/Uhrzeit für ein Gerät
Der Zeitraum für Analytics basiert auf das Datum aus der Benutzer des Audiogeräts. So stellen Sie sicher, dass das Gerät das Datum richtig eingestellt ist. 

## <a name="segment-issues"></a>'Segment' Probleme

### <a name="i-created-a-segment-and-it-is-showing-up-as-greyed-out-or-not-showing-any-data"></a>Ich habe ein Segment erstellt und angezeigt, wie abgeblendet oder keine Daten angezeigt wird
Segment Erstellung in Echtzeit im Moment nicht zur Verfügung. Es wird zur gleichen Zeit berechnet, und wie die Daten Analytics aggregiert werden, sodass es bis zu 24 Stunden dauern konnte. Sie sollten später erneut, aber in der Zwischenzeit Sie sollten auch sicherstellen, dass Ihre mobile apps tatsächlich um die Daten senden möchten, auf denen Grundlage Sie die Segmente bilden. Z. B. Wenn ein Ereignis sagen Sie "Foo" ist nicht gesendet werden, indem allen mobilen Geräten, und klicken Sie dann es wäre Segmentdaten für ein Segment mit EventName erstellte nicht = Foo als das Kriterium. Sie sollten auch überprüfen, dass Ihre Integration SDK, um sicherzustellen, dass Ihre mobile-app Daten ordnungsgemäß senden ist. 

## <a name="reach-or-push-notifications-issues"></a>'Erreichen' oder Pushbenachrichtigungen Probleme

### <a name="my-push-messages-are-not-being-delivered"></a>Meine Pushbenachrichtigungen Nachrichten werden nicht übermittelt 

1. Versuchen Sie Benachrichtigungen an ein Testgerät zuerst, um sicherzustellen, dass alle Komponenten - mobile-app, SDK und der Dienst ordnungsgemäß verbunden und kann Pushbenachrichtigungen vorführen werden. 
2. Die einfachste '-von-app-Benachrichtigung immer zuerst über eine für eine Marketingkampagne die nicht geplant ist senden und noch ein Publikum Kriterium angegeben haben. Dies ist wieder um nachzuweisen, dass die Benachrichtigung Connectivity ordnungsgemäß funktioniert. 
3. Wenn Sie Probleme bei der Bereitstellung von Benachrichtigungen in-app ist ebenfalls es ein guter erster Schritt, versuchen zuerst eine Benachrichtigung Out-von-app zu senden. 
4. Stellen Sie sicher, dass die 'Native Pushbenachrichtigungen' für mobile-app richtig konfiguriert ist. Je nach Plattform wird es entweder Schlüssel (Android, Windows) oder Zertifikate (iOS) umfassen. Finden Sie unter [Benutzeroberfläche - Einstellungen](mobile-engagement-user-interface-settings.md)
5. Stellen Sie sicher, dass dies nicht der Fall ist, von der app, die Benachrichtigungen auch durch den Benutzer nicht über die mobile OS daher blockiert werden konnte. 
6. Stellen Sie sicher, dass Sie nicht die Option *ignorieren Zielgruppe wird Pushbenachrichtigungen für Benutzer über die-API gesendet* im Abschnitt **für eine Marketingkampagne** eine Zeit für eine Marketingkampagne festlegen, da dies sorgt dafür, dass Pushbenachrichtigungen nur über APIs gesendet werden konnten. 
7. Stellen Sie sicher, dass Sie Ihre Pushbenachrichtigungen für eine Marketingkampagne mit beide ein Gerät wider, die über WIFI und Telefon Operator Netzwerk die Verbindung als mögliche Ursache von Problemen zu unterdrücken testen.
8. Stellen Sie sicher, dass die System Datum/Uhrzeit auf Ihrem Geräteemulator korrekt ist, da auch jedem Gerät nicht synchron mit dem Pushbenachrichtigungen Benachrichtigungsdienst des Möglichkeit Benachrichtigungen vorführen beeinträchtigt. 

Weitere Plattform bestimmte Problembehandlung aufgeführten Schritte ausführen:

1. **iOS** 

    - Stellen Sie sicher, dass die Zertifikate gültig und nicht für iOS Pushbenachrichtigungen abgelaufene sind. 
    - Stellen Sie sicher, dass Sie ein Zertifikat für die *Herstellung* ordnungsgemäß in Ihrer app Mobile Engagement konfigurieren. 
    - Stellen Sie sicher, dass Sie auf Testen einer *real, physischen Gerät.* Der Simulator iOS kann nicht Pushbenachrichtigungen Nachrichten verarbeitet werden.
    - Stellen Sie sicher, dass die Paket-ID in der mobilen app richtig konfiguriert ist. Finden Sie [hier](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)
    - Beim Testen, verwenden Sie "Ad-hoc-" Verteilung in Ihrem mobilen provisioning Profil ein. Sie werden nicht aufzunehmen, wenn Ihre app kompiliert wurde mithilfe von "Debuggen"

2. **Android**

    - Stellen Sie sicher, dass Sie die richtige Anzahl von Project in der mobilen app AndroidManifest.xml Datei angegeben haben gefolgt von \n Zeichen. 
    
            <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
        
    - Stellen Sie sicher, dass Sie nicht fehlende oder falsch alle Berechtigungen in der Android Manifestdatei konfiguriert 
    - Stellen Sie sicher, dass die Projektnummer der hinzuzufügende zu Ihrer Clientanwendung aus dem Konto ist, haben Sie die GCM Server-Taste. Jede Abweichung zwischen den beiden wird verhindert, dass Ihre schiebt vertraut ab. 
    - Wenn System Benachrichtigungen, aber nicht in der app dann überprüfen [angeben ein Symbol für Benachrichtigungen Abschnitt](mobile-engagement-android-get-started.md) als wahrscheinlich erhält fest Sie nicht das richtige Symbol in die Android-Manifestdatei. 
    - Werden durch das Senden einer Benachrichtigung BigPicture, und stellen Sie sicher, dass bei externen Bilds Servern stehen Ihnen dann Support HTTP können benötigten "Abrufen" und "Kopf".

3. **Windows**
    
    - Stellen Sie sicher, dass Sie eine gültige Windows Store-app die app zugeordnet haben. In Visual Studio - müssen Sie für klicken Sie mit der rechten Maustaste auf das Projekt, wählen Sie die Option "App mit Store zuordnen", und wählen die erstellten im Windows Store-app. Diese Windows Store-app sollten die gleiche aus, in dem Sie die native Pushbenachrichtigungen Anmeldeinformationen so konfigurieren Sie die Mobile Engagement-Portal haben.
    - Empfangen von Pushbenachrichtigungen Out-von-app, aber nicht innerhalb der app-Benachrichtigungen mit `EngagementOverlay` Integration dann sicher, dass ein Element mit Raster Stamm ist, auf der Seite. EngagementOverlay verwendet, die sie in die XAML-Datei zum Hinzufügen von zwei Web findet das erste "Raster"-Element auf der Seite Ansichten. Wenn Sie festgelegtem Web-Ansichten suchen möchten, können Sie definieren, einem Raster, mit dem Namen "EngagementGrid", wie dies jedoch müssen Sie sicherstellen, dass es ist ausreichend Höhe und Breite für das Web mit zwei nachfolgenden der Ansichten die Benachrichtigung und der folgenden Ankündigung als innerhalb der app-Benachrichtigung angezeigt wird:
        
            <Grid x:Name="EngagementGrid"></Grid>

### <a name="i-created-a-push-notificationannouncement-campaign-and-even-after-it-sent-me-the-notification-it-is-showing-as-active-what-does-it-mean"></a>Erstellt eine Benachrichtigung/Ankündigung von Pushbenachrichtigungen/campaign und sogar nachdem es mir die Benachrichtigung gesendet, als "Aktiv" angezeigt. Was bedeutet es? 
Die **für eine Marketingkampagne** , die Sie in Mobile Engagement erstellt heißt, da es eine zeitintensive Pushbenachrichtigungen Benachrichtigung Bedeutung ist, wie Ihre mobile Engagement Plattform neue Geräte Verbindung, diese automatisch der Benachrichtigung, die Sie hier konfigurieren gesendet, solange sie das Kriterium entsprechen, die, das Sie in der für eine Marketingkampagne festlegen. Dies ist keinen Screenshot einzelne Benachrichtigung. Sie müssen manuell auf die Schaltfläche **Fertig stellen** , die für eine Marketingkampagne zu beenden, damit sie weiteren Benachrichtigungen senden nicht klicken. 

### <a name="i-created-a-push-campaign-and-i-am-receiving-notifications-successfully-however-whenever-i-open-up-the-app-i-get-the-same-notification-even-when-i-had-actioned-it-before"></a>Ich habe eine Pushbenachrichtigungen für eine Marketingkampagne erstellt und ich erhalte Benachrichtigungen erfolgreich jedoch immer, wenn ich die App öffnen, die denselben Benachrichtigung ich erhalte auch, wenn ich hatte verarbeitet es vor? 
Testen dies wahrscheinlich geschehen, während des Testens und Verwendung von Emulatoren oder einige Framework wie TestFlight. Was geschieht hier, dass bei jeder app Instanz ausgeführt wird, ist es beim Abrufen eines neuen Geräte-ID und senden Sie sie an unserem Back-End-die verursacht die Mobile Engagement Plattform als ein neues Gerät, und senden die Benachrichtigung behandelt. 

## <a name="getting-support"></a>Abrufen von Support

Wenn Sie nicht zur Lösung des Problems können können selbst dann Sie:

1. Das Problem in den vorhandenen Threads StackOverflow Forum und [MSDN-Forum](https://social.msdn.microsoft.com/Forums/windows/en-US/home?forum=azuremobileengagement) suchen, und wenn nicht dann eine Frage vorhanden. 
2. Wenn Sie feststellen, dass ein Feature fehlende dann hinzufügen/Stimme für die Anfrage auf unsere [UserVoice-forum](https://feedback.azure.com/forums/285737-mobile-engagement/)
3. Wenn Sie Microsoft unterstützen öffnen einen Support-Fall haben können, indem Sie die folgenden Details: 
    - Azure-Abonnement-ID
    - Plattform (z. B. iOS, Android usw.)
    - App-ID
    - Für eine Marketingkampagne-ID (für Pushbenachrichtigungen Benachrichtigung Probleme)
    - Geräte-ID
    - Mobile Engagement SDK Version (z. B. Android SDK v2.1.0)
    - Fehlerdetails mit genauen Fehlermeldung und Szenario
