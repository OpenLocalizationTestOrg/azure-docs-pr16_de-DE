<properties 
    pageTitle="Windows Phone Silverlight-SDK (Übersicht)" 
    description="Übersicht über die Windows Phone Silverlight-SDK Azure mobilen Engagement"                     
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone Silverlight-SDK Übersicht für Azure mobilen Engagement

Beginnen Sie hier, um die Details zum Azure Mobile Engagement in einem Windows Phone Silverlight-App integrieren zu erhalten. Wenn Sie zuerst Probieren Sie es verwenden möchten, stellen Sie sicher, dass Sie unsere [15 Minuten Lernprogramm](mobile-engagement-windows-phone-get-started.md)abgeschlossen haben.

Klicken Sie auf, um die- [SDK Inhalt](mobile-engagement-windows-phone-sdk-content.md) anzuzeigen

##<a name="integration-procedures"></a>Integrationsverfahren

1. Beginnen Sie hier: [wie Mobile Engagement in Ihrer app für Windows Phone Silverlight integriert werden soll.](mobile-engagement-windows-phone-integrate-engagement.md)

2. Für Benachrichtigungen: [wie Reichweite (Benachrichtigungen) in Ihrer app für Windows Phone Silverlight integriert werden soll.](mobile-engagement-windows-phone-integrate-engagement-reach.md)

3. Planen der Implementierung kategorisieren: [So verwenden Sie erweiterte Mobile Projektdauer tagging-API in Ihrem Windows Phone Silverlight-app](mobile-engagement-windows-phone-use-engagement-api.md)

##<a name="release-notes"></a>Freigeben von Notizen

###<a name="330-04192016"></a>3.3.0 (04/19/2016)
Teil der *MicrosoftAzure.MobileEngagement* Nuget Verpacken **v3.4.0**

-   Hinzugefügten "TestLogLevel" API zu aktivieren/deaktivieren/Filter Console Protokolle vom SDK ausgegeben.

Frühere Version finden Sie die [vollständige Version von Notizen](mobile-engagement-windows-phone-release-notes.md)

##<a name="upgrade-procedures"></a>Upgrade-Verfahren

Wenn Sie bereits eine ältere Version unseres SDK in die Anwendung integriert haben, müssen Sie die folgenden Punkte beachten beim Upgrade des SDKS.

Möglicherweise müssen Sie mehrere Verfahren ausführen, wenn Sie mehrere Versionen von SDK verpasst haben. Finden Sie die vollständige [Upgrade Verfahren](mobile-engagement-windows-phone-upgrade-procedure.md)aus. Beispielsweise wenn Sie migrieren von 0.10.1 zu 0.11.0 Sie zuerst das Verfahren "von 0.9.0 zu 0.10.1" führen müssen dann das Verfahren "von 0.10.1 zu 0.11.0".

###<a name="from-200-to-330"></a>Aus 2.0.0 zu 3.3.0

####<a name="test-logs"></a>Testen von Protokollen

Vom SDK gefertigt Console-Protokolle können jetzt aktiviert/deaktiviert/gefiltert werden. Um dies anzupassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen der Werte aus der `EngagementTestLogLevel` Enumeration, z. B.:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Aktualisieren von älteren Versionen

Finden Sie unter [Upgrade Verfahren](mobile-engagement-windows-phone-upgrade-procedure.md)
 
