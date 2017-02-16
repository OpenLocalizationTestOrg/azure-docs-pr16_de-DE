<properties 
   pageTitle="Azure mobilen Engagement Problembehandlungsleitfadens - SDK" 
   description="Behandeln von Problemen mit SDK Integration in Azure Mobile Engagement" 
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

# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>Problembehandlung bei der SDK-integration

Nachfolgend werden mögliche Probleme mit von Azure Mobile Engagement in Ihrer Anwendung Integration auftreten können.

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>SDK Probleme nach einem Fehler in einem anderen Bereich der Anwendung

### <a name="issue"></a>Problem
- Benutzeroberfläche Daten Websitesammlung Fehler (in Analytics, Überwachung, Segmentierung oder Dashboards).
- Pushbenachrichtigungen Fehlern (Push-Vorgänge in der app aus dem app oder beide funktionieren nicht).
- Erweiterte Features Fehlern (Verlauf, geografischen Standort oder Plattform, die bestimmte schiebt funktionieren nicht).
- API Fehlern (APIs Fail häufig im Hintergrund ohne Fehlermeldungen).
- Service-Fehlern (keiner der Azure Mobile Engagement funktioniert für eine Anwendung).

### <a name="causes"></a>Bewirkt, dass

- Die meisten Probleme, die mit dem Azure Mobile Engagement SDK gelöst werden müssen, werden durch einen Fehler in der Anwendung (beispielsweise eine Auflistung Fehler bei der Benutzeroberfläche Daten, Pushbenachrichtigungen Fehler, erweiterte Funktion Fehler, -API-Fehler, Anwendung stürzt ab oder offensichtlich Dienstausfall) gefunden.  
- Wenn ein bestimmtes Feature eines Auftrags für Mobile Azure nie in Ihrer app vor gearbeitet hat, müssen Sie die Integration abzuschließen. 
- Wenn ein bestimmtes Feature eines Auftrags für Mobile Azure arbeiten wurde und beendet wird, müssen Sie auf die letzte Version mit dem Azure Mobile Engagement SDK aktualisieren. Denken Sie daran, dass es eine andere Version des Azure Mobile Engagement SDK für jede Plattform von Azure Mobile Engagement (Android, iOS, Windows und Windows Phone) unterstützt werden wird.

#### <a name="sdk-integration"></a>SDK-Integration

- Azure Mobile Engagement integriert nicht ordnungsgemäß im SDK (Analytics).
- Erreicht haben Sie, nicht ordnungsgemäß in SDK (In der App und abwesend App legt) integriert.
- Das Zertifikat abgelaufen oder falschen PROD im Vergleich zu Entwickler (nur iOS).
- GCM oder ADM nicht ordnungsgemäß im SDK integriert (Android nur - Dienst bestimmten legt).
- Nachverfolgen nicht ordnungsgemäß integriert im SDK (installieren Store nachverfolgen).
- Aus- oder GPS Pfad integriert nicht ordnungsgemäß im SDK (Zielgruppenadressierung von geografischen Position).


**Siehe auch:**

- [SDK-Dokumentation - Integration von Führungslinien][Link 5] 
- [Problembehandlungsleitfadens - Pushbenachrichtigungen][Link 23]

#### <a name="sdk-upgrade"></a>SDK Upgrade

- Zur Behebung von Problemen mit älteren Versionen von SDK (oft bezieht sich auf neuere Versionen des Geräts OS) SDK umstellen müssen.
- Deinstallieren Sie alle vorherige Versionen der app auf Ihrem Gerät, und installieren Sie der neuesten Version der app, die neu registrieren Ihre Geräte-ID aus der Benutzeroberfläche Azure Mobile Webcamvideos, um zu bestätigen, dass Ihr Gerät die neueste Version der app verwendet wird.

**Siehe auch:**

- [SDK-Dokumentation - Versionsinformationen](http://go.microsoft.com/fwlink/?LinkId= 525554) 
- [SDK-Dokumentation - Upgrade Führungslinien](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>Andere SDK

- Fehler im Anwendungsmanifest "AndroidManifest.xml" können dazu führen, dass Azure Mobile Engagement nicht entwickelt (nur Android).
- Eine allgemeine Probleme mit SDK Integration API Verwendung besteht darin, die SDK-Taste und die Taste API Begriff.

**Siehe auch:**

- [Konzepte - Glossar][Link 6]

## <a name="advanced-coding-issues"></a>Erweiterte Codierung Probleme

### <a name="issue"></a>Problem
-  Bestimmter Plattform-Code nicht direkt im Zusammenhang mit Azure Mobile Engagement kann auf iOS, Android und Windows Phone Probleme verursachen.

### <a name="causes"></a>Bewirkt, dass

- Viele erweiterte Codierung Probleme mit Azure Mobile Engagement werden von bestimmter falsch geschriebene Plattform-Code nicht direkt im Zusammenhang mit Azure Mobile Engagement verursacht. Sie müssen in der Dokumentation spezifisch für die Plattform, die Sie für zusammen mit der Azure Mobile Engagement Dokumentation (Android, iOS, Web, Windows und Windows Phone) entwickeln.
- "Kategorien" nicht ordnungsgemäß konfiguriert haben, wird verhindert, dass von einer Benachrichtigung an eine andere Position innerhalb oder außerhalb der app (nur Android) verknüpfen. 
- Nicht festlegen "UIKit.framework" auf "optional" in Ihrem iOS-Code zeigt eine "Symbol Fehler nicht gefunden" und/oder auf ältere iOS-Geräte (nur iOS) stürzt ab.
- Abgelaufene Zertifikate oder nicht ordnungsgemäß mit der Entwicklung oder Prod Version von das Zertifikat, Ursachen Pushbenachrichtigungen Probleme (nur iOS).
- Es bestehen einige Einschränkungen gehörende auf eine Plattform, die nicht (wie das System Center von app Funktionsweise für Android und iOS legt) Azure Mobile Engagement steuern können.
- Azure Mobile Engagement veröffentlicht eine vollständige Liste der internen Pakete von Azure Mobile Engagement für iOS und Android als Referenz verwendet. Beachten Sie, dass einige Features von Azure Mobile Engagement spezifisch für die Plattform (Android, iOS, Web, Windows und Windows Phone) werden beibehalten.

### <a name="see-also"></a>Siehe auch

 - [Problembehandlungsleitfadens - Pushbenachrichtigungen][Link 23] 
 - [SDK-Dokumentation - Versionsinformationen][Link 5]
 - [SDK-Dokumentation - Upgrade Führungslinien][Link 5]

## <a name="application-crashes"></a>Anwendungsabstürzen

### <a name="issue"></a>Problem
- Die Anwendung stürzt ab, auf die Endbenutzer-Gerät.

### <a name="causes"></a>Bewirkt, dass

- Absturzinformationen kann in der *Analytics-Benutzeroberfläche* oder die *Analytics-API* angezeigt werden
- Finden die Geräte-ID des Geräts testen und führen Sie die gleiche Aktion, die eine Anwendung zum Absturz für den Endbenutzer helfen, die Ursache des dem Absturz verursacht.
- Bekannte Probleme mit dem Azure Mobile Engagement SDK, die Applikationen zu einem Absturz führen werden manchmal durch ein Upgrade auf die neueste Version des SDK aufgelöst. Vergewissern Sie sich die Versionsinformationen über Ihre Plattform überprüfen, wenn Sie untersuchen stürzt ab.

### <a name="see-also"></a>Siehe auch

- [SDK-Dokumentation - Versionsinformationen][Link 5]
- [SDK-Dokumentation - Upgrade Führungslinien][Link 5]

## <a name="app-store-upload-failures"></a>App Store Upload-Fehler

### <a name="issue"></a>Problem
- Fehler im Zusammenhang mit die neueste Version der app Apple, Google oder im Windows-App Store hochladen.

### <a name="causes"></a>Bewirkt, dass

- App speichert blockieren apps manchmal bestimmte Features aktiviert (z. B. im Apple Store wird verhindert, dass die Verwendung von IDFV im apps im Speicher und der GooglePlay Store wird verhindert, dass die gemeinsame Nutzung von Anwendungsinformationen zwischen apps). 
- Stellen Sie sicher, dass Sie die Versionsinformationen zu Ihren Plattform und die aktuelle Version des SDK aktivieren, sollten Sie eine app zum Store hochladen können.

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
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
 
