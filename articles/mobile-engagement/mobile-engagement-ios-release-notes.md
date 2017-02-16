<properties
    pageTitle="Azure Engagement für Mobile iOS SDK Versionsinformationen | Microsoft Azure"
    description="Neuesten Updates und Verfahren für iOS SDK für Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="piyushjo" />

#<a name="azure-mobile-engagement-ios-sdk-release-notes"></a>Azure Engagement für Mobile iOS SDK Freigeben von Notizen

##<a name="400-09122016"></a>4.0.0 (09/12/2016)

-   Feste Benachrichtigung keine Aktion auf 10-iOS-Geräte.
-   Deaktivieren Sie XCode 7 ein.

##<a name="324-06302016"></a>3.2.4 (30/06/2016)

-   Feste Aggregation zwischen technische Protokolle und andere Protokolle.

##<a name="323-06072016"></a>3.2.3 (07/06/2016)

-   Den Fehler behoben, wo wird Übermittlung Feedback nicht gemeldet, wenn die app im Hintergrund befindet.
-   Optimiert das Senden von Protokollen technische.

##<a name="322-04072016"></a>3.2.2 (07/04/2016)

-   Problem behoben, klicken Sie auf HTTP-Absage der manchmal zu einem Absturz führen führt.

##<a name="321-12112015"></a>3.2.1 (11/12/2015)

-   Die Verzögerung behoben, wenn eine neue app-Instanz durch eine Benachrichtigung mit Tiefe Links ausgelöst wird

##<a name="320-10082015"></a>3.2.0 (10/08/2015)

-   Aktiviert die Bitcode im SDK zu vereinfachen **Xcode 7**konzipiert.
-   Feste Fehlern im Zusammenhang mit innerhalb der app-Benachrichtigungen.
-   Versucht die innerhalb der app-Benachrichtigungen Zuverlässigkeit bei niedriger Akkukapazität und anderen Szenarios.
-   Entfernt zusätzliche Console Protokolle von 3rd Party Bibliothek generiert.

##<a name="310-08262015"></a>3.1.0 (26/08/2015)

-   Beheben Sie iOS 9 Kompatibilität Fehler mit einer Drittanbieter-Bibliothek aus. Es wurde Absturz verursacht, während senden Ergebnisse, Anwendungsinformationen oder zusätzliche Daten abfragt.

##<a name="300-06192015"></a>3.0.0 (19/06/2015)

-   Mobile Engagement wird im Hintergrund Pushbenachrichtigungen verwendet.
-   Unterstützung für iOS abgelegt 4.X. Beginnend mit dieser Version der Zielliste Bereitstellung Ihrer Anwendung muss mindestens iOS 6.

##<a name="220-05212015"></a>2.2.0 (05/21/2015)

-   Die Recherchemöglichkeiten für Mobile Geräte-Id für Geräte < iOS 6 basiert nun auf einer GUID zum Zeitpunkt der Installation generiert.

##<a name="210-04242015"></a>2.1.0 (24/04/2015)

-   Hinzugefügten Swift Kompatibilität.
-   Wenn Sie eine Benachrichtigung klicken, ausgeführt die Aktion, die URL jetzt ist rechts aus, nach die Anwendung geöffnet wird.
-   Hinzugefügte fehlende Kopf-Datei in SDK-Paket.
-   Ein Problem behoben, wenn die Mobile Engagement Absturz Reporter deaktiviert wurde.

##<a name="200-02172015"></a>2.0.0 (02/17/2015)

-   Erste Version von Azure mobilen Engagement
-   Konfiguration AppId/SdkKey wird durch die Konfiguration einer Verbindungszeichenfolge ersetzt.
-   Entfernt die API zum Senden und Empfangen von Nachrichten von beliebigen XMPP von beliebigen XMPP Einheiten.
-   Entfernt die API zum Senden und Empfangen von Nachrichten zwischen Geräten.
-   Zur Verbesserung der Sicherheit.
-   Verlauf SmartAd entfernt.
