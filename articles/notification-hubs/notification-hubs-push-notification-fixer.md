<properties 
    pageTitle="Azure Benachrichtigung Hubs - Diagnose Richtlinien" 
    description="Richtlinien zum häufige Probleme mit Azure Benachrichtigung Hubs diagnostizieren." 
    services="notification-hubs" 
    documentationCenter="Mobile" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs---diagnosis-guidelines"></a>Azure Benachrichtigung Hubs - Diagnose Richtlinien

##<a name="overview"></a>(Übersicht)

Eine der am häufigsten gestellten Fragen, die wir von Kunden Azure Benachrichtigung Hubs hören ist wie um zu ermitteln, warum sie eine Benachrichtigung gesendet wird aus ihrer Anwendung Back-End-angezeigt werden angezeigt, klicken Sie auf das Client-Gerät -, wo und warum Benachrichtigungen gelöscht wurden und wie Sie dieses Problem zu beheben. In diesem Artikel werden wir geleitet durch die verschiedenen Gründe, warum Benachrichtigungen möglicherweise erhalten gelöscht oder nicht einhandeln, auf Geräten. Wir werden auch über Methoden ansehen, in denen Sie analysieren und die Ursache ermitteln können. 

Zunächst einmal ist es wichtig, zu verstehen, wie die Benachrichtigung Hubs Azure, Benachrichtigungen zu den Geräten legt.
![][0]

In einen normalen senden Benachrichtigung Fluss wird die Nachricht aus der **Anwendung Back-End-** an **Azure Benachrichtigung Hub (NH)** gesendet, der wiederum einige Verarbeitung auf alle Registrierungen Berücksichtigung der konfigurierten Kategorien und Tag mithilfe von Ausdrücken bedeutet "Ziele", d. h. alle Registrierungen, die der Pushbenachrichtigung erhalten müssen ermitteln. Diese Registrierungen können über eine oder alle unserer unterstützten Plattformen - iOS, Google, Windows, Windows Phone, erstrecken Kindle und Baidu für Android China. Sobald die Ziele eingerichtet wurden, NH dann schiebt, Benachrichtigungen, teilen, über mehrere Stapel von Registrierungen, auf dem Geräteplattform bestimmte **Pushbenachrichtigungen Benachrichtigung Service (PNS)** – z. B. APNS für Apple, GCM für Google usw.. NH authentifiziert mit den entsprechenden PNS basierend auf den Anmeldeinformationen, die Sie in der klassischen Azure-Portal auf der Seite konfigurieren Benachrichtigung Hub festgelegt werden. Der PNS leitet dann die Benachrichtigungen zu den jeweiligen **Client-Geräte**. Dies ist die Plattform empfohlen Möglichkeit zum Vorführen Pushbenachrichtigungen und beachten Sie, dass der endgültigen Abschnitt der Benachrichtigungsübermittlung zwischen der PNS-Plattform und das Gerät stattfindet. Daher müssen vier Hauptkomponenten - *Client*, *Anwendung Back-End-*, *Azure Benachrichtigung Hubs (NH)* und *Pushbenachrichtigungen Benachrichtigung Services (PNS)* und eine der folgenden verursacht möglicherweise Benachrichtigungen erste gelöscht. Weitere Details für diese Architektur ist auf [Benachrichtigung Hubs Übersicht]zur Verfügung.

Fehler beim Übermitteln von Benachrichtigungen möglicherweise während der anfänglichen Test/Staging geschehen, phase, die ein Konfigurationsproblem hindeuten, oder es kann vorkommen, in der Herstellung, in dem alle oder einige der Benachrichtigungen abgelegten, der angibt, einige tieferen Anwendungs oder Muster Problem messaging geworden ist. Im Abschnitt betrachten unter wir verschiedene abgelegten Benachrichtigungen Szenarien von allgemeinen bis hin zu den manchen Art, von denen einige offensichtlich finden Sie möglicherweise und einige andere nicht so viel. 

##<a name="azure-notifications-hub-mis-configuration"></a>Azure Benachrichtigungen Hub fehlerhafte Konfiguration 

Azure Benachrichtigung Hubs erforderlich selbst im Kontext des Entwicklers Anwendung der jeweiligen PNS erfolgreich Benachrichtigungen senden können Authentifizierung. Dies ist möglich, indem der Entwickler, erstellen ein Konto Entwicklertools mit der jeweiligen Plattform (Google, Apple, Windows usw.), und klicken Sie dann Registrieren ihrer Anwendung, wo sie Anmeldeinformationen abzurufen, die im Portal unter Benachrichtigung Hubs Konfigurationsabschnitt konfiguriert sein müssen. Wenn keine Benachrichtigungen über herstellen, sollten erster Schritt stellen Sie sicher, dass die richtigen Anmeldeinformationen im Infobereich Hub Zuordnen der Anwendungs, die unter ihre Plattform bestimmte Entwicklertools Konto erstellt konfiguriert sind. Sie werden unsere [Erste Schritte-Lernprogramme] über diesen Vorgang in einer Weise Schritt für Schritt übernommen hilfreich sein. Hier sind einige allgemeinen fehlerhafte Konfigurationen aus:

1. **Allgemeine**
 
    eine) stellen Sie sicher, dass Ihr Benachrichtigung Hub Name (ohne Tippfehler) identisch ist:

    - Stelle, an der Sie im Desktopclient erfassen möchten, 
    - Stelle, an der Sie Benachrichtigungen aus der Back-End-senden,  
    - Sie haben, in dem die Anmeldeinformationen PNS konfiguriert und 
    - Dessen SAS Anmeldeinformationen, die Sie auf dem Client und die Back-End-konfiguriert haben. 
        
    b) stellen Sie sicher, dass Sie die richtigen SAS Konfigurationszeichenfolgen auf dem Client und der Anwendung Back-End-verwenden. Als Faustregel müssen Sie die **DefaultListenSharedAccessSignature** auf dem Client und **DefaultFullSharedAccessSignature** auf die Anwendung Back-End-verwenden (wodurch Berechtigung kann eine Benachrichtigung zu der NH senden sind)

2. **Konfiguration von Apple Pushbenachrichtigungen-Benachrichtigung (APNS)**
 
    Sie müssen zwei verschiedenen Hubs - eine für die Herstellung verwalten und zu einer anderen Testzwecken Zweck. Dies bedeutet hochladen das Zertifikat, das Sie in der geschützten Umgebung mit einem separaten Hub verwenden möchten, und das Zertifikat, das Sie in der Herstellung mit einem separaten Hub verwenden möchten. Versuchen Sie nicht, verschiedene Arten von Zertifikaten an denselben Hub hochladen, da es Benachrichtigung bei der Zeile nach unten führen kann. Wenn Sie sich an einer Stelle finden, in dem Sie verschiedene Typen von Zertifikat versehentlich an denselben Hub hochgeladen haben, empfiehlt es sich Hub löschen, und beginnen Sie von neuem. Wenn Sie aus irgendeinem Grund, Sie nicht im Hub dann zumindest löschen können, müssen Sie alle vorhandenen Registrierungen vom Hub löschen. 

3. **Konfiguration von Google Cloud Messaging (GCM)** 

    eine) stellen Sie sicher, dass Sie unter Projekt Cloud "Google Cloud Messaging für Android" aktivieren möchten. 
    
    ![][2]
    
    b) stellen Sie sicher, dass "Server Key" erstellen, während den Anmeldeinformationen auftritt, welche NH Authentifizierung mit GCM verwendet wird. 
    
    ![][3]
    
    c) stellen Sie sicher, dass Sie "Projekt-ID" auf dem Client konfiguriert haben also eine vollständig numerischen Entität, die Sie aus dem Dashboard herunterladen können:
    
    ![][1]

##<a name="application-issues"></a>Anwendungsprobleme

1) **Tags / Kategorisieren von Ausdrücken**

Wenn Sie Kategorien oder Kategorie Ausdrücke für die Ihr Publikum segmentieren verwenden, ist es immer möglich, wenn Sie die Benachrichtigung senden möchten, ist es kein Ziel gefunden wurde für die Kategorien/Kategorie Ausdrücke basiert, die Sie in einem Anruf senden angeben. Es empfiehlt sich, überprüfen Ihre Registrierungen, um sicherzustellen, dass es gibt Tags, die beim Senden von Benachrichtigungen und überprüfen Sie die Benachrichtigung Lieferung nur von Clients mit diesen Registrierungen übereinstimmt. Z. B. Wenn alle, die mit Ihrem Registrierungen mit NH vorgenommen wurden die Kategorie "Politik" angenommen, und Sie sind eine Benachrichtigung mit der Kategorie "Sports" senden, wird es nicht zu einem beliebigen Gerät gesendet werden. Eine komplexe Groß-/Kleinschreibung könnte umfassen Kategorie Ausdrücke, wo Sie nur registriert mit "Kategorie A" oder "Kategorie B" jedoch beim Senden von Benachrichtigungen, haben Sie "Kategorie A & & Kategorie B" abgesehen. Tipps selbst diagnostizieren unten im Abschnitt gibt es Methoden, in denen Sie Ihre Registrierungen zusammen mit der Kategorien überprüfen können, die sie besitzen. 

2) **Vorlage Probleme**

Wenn Sie von Vorlagen mithilfe stellen Sie sicher, dass Sie bei [Vorlage Anleitungen]beschriebenen Richtlinien folgen. 

3) **Ungültige Registrierungen**

Wird ausgelöst, wenn die Benachrichtigung Hub ordnungsgemäß konfiguriert wurde und alle Kategorien/Kategorie Ausdrücke verwendet wurden, resultierender ordnungsgemäß im Feld Suchen nach gültiger Ziele, dem die Benachrichtigungen gesendet werden müssen, NH deaktivieren mehrere Verarbeitung Blätter parallel - jeden Stapel Senden von Nachrichten an eine Gruppe von Registrierungen. 

> [AZURE.NOTE] Da wir die Verarbeitung parallel durchzuführen, nicht wir die Reihenfolge gewährleistet, in der die Benachrichtigungen übermittelt werden. 

Nun ist Azure Benachrichtigungen Hub für ein "am äußersten einmaligem" Nachricht Übermittlung Modell optimiert. Dies bedeutet, dass wir eine Deduplizierung versuchen, damit keine Benachrichtigungen an ein Gerät mehrmals übermittelt werden. Um dies sicherzustellen, dass wir Registrierungen durchsuchen und stellen Sie sicher, dass diese nur eine Nachricht pro Geräte-ID. vor dem Senden tatsächlich der Nachricht an die PNS gesendet wird. Jeder Stapel an den PNS gesendet werden, die wiederum akzeptiert und Überprüfen von Registrierungen, ist es möglich, dass die PNS erkennt ein Fehler mit einem oder mehreren Registrierungen in einem Stapel, einen Fehler an Azure NH zurück und beendet die Verarbeitung, damit auch die Stapel vollständig ablegen. Dies gilt insbesondere mit APNS, die ein Streaming-Protokoll TCP verwendet. Obwohl wir für die am äußersten optimiert sind, nachdem die Übermittlung, in diesem Fall wir die fehlerhaften Registrierung aus unserer Datenbank entfernen, und wiederholen Sie die Benachrichtigungsübermittlung für den Rest der Geräte in dem jeweiligen Stapel.

Sie können Abrufen der Fehlerinformationen für den Versuch fehlgeschlagene Übermittlung gegen eine Registrierung mithilfe der Azure Benachrichtigung Hubs REST APIs: [pro Nachricht werden: erste Benachrichtigung Nachricht werden](https://msdn.microsoft.com/library/azure/mt608135.aspx) und [PNS Feedback](https://msdn.microsoft.com/library/azure/mt705560.aspx). Finden Sie unter [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) Code z. b.

##<a name="pns-issues"></a>PNS Probleme

Nachdem Sie die Benachrichtigung eingegangen ist, indem Sie die entsprechenden PNS ist es sich verpflichtet, die Benachrichtigung an das Gerät zu übermitteln. Azure Benachrichtigung Hubs liegt außerhalb des zulässigen hier das Bild und hat keine Kontrolle auf, wenn oder wenn die Benachrichtigung an das Gerät übermittelt werden konnte. Da die Plattform Benachrichtigungsdienste leistungsfähige sind, gehen Sie wie folgt Benachrichtigungen meist die Geräte aus der PNS in ein paar Sekunden erreicht haben. Wenn die PNS jedoch eingeschränkt ist dann Azure Benachrichtigung Hubs gilt deaktivieren Strategie einer exponentiellen zurück und falls der PNS für 30 Minuten nicht erreichbar bleibt dann einer Richtlinie direkte ablaufen, und legen Sie die Nachrichten endgültig. 

Wenn eine PNS versucht, eine Benachrichtigung vorführen, aber das Gerät offline ist, wird die Benachrichtigung durch die PNS für einen begrenzten Zeitraum gespeichert, und am Gerät übermittelt, sobald diese verfügbar wird. Nur eine aktuelle Benachrichtigung zu einer bestimmten app gespeichert ist. Wenn mehrere Benachrichtigungen gesendet werden, während das Gerät offline ist, bewirkt, dass jede neue Benachrichtigung über die vorherige Mitteilung verworfen werden. Dieses Verhalten zu halten nur die neueste Benachrichtigung wird als zusammenfügenden Benachrichtigungen in APNS und Falten der in GCM (bei Verwendung eines Reduzierung Schlüssels) bezeichnet. Wenn das Gerät längere Zeit offline bleibt, werden alle Benachrichtigungen, die sie für die gespeichert werden, wurden verworfen. Datenquellen - [APNS Anleitungen] & [GCM Anleitungen]

Sie können einen zusammenfügen Schlüssel mit Azure Benachrichtigung Hubs - übergeben, über einen HTTP-Header mithilfe der generisches `SendNotification` API (z. B. für .NET SDK – `SendNotificationAsync`) der HTTP-Header die als weitergegeben werden, auch akzeptiert besteht darin, die entsprechenden PNS. 

##<a name="self-diagnose-tips"></a>Tipps selbst diagnostizieren

Hier wird untersucht die verschiedenen Wege diagnostizieren und root keine Benachrichtigung Hub-Probleme verursachen:

###<a name="verify-credentials"></a>Überprüfen von Anmeldeinformationen

1. **PNS-Entwicklerportal**

    Überprüfen sie bei der entsprechenden PNS Entwicklerportal (APNS, GCM, WNS usw.) unsere [Lernprogramme für erste Schritte]mit.

2. **Azure Classic-portal**

    Wechseln Sie zur Registerkarte konfigurieren zu überprüfen und die Anmeldeinformationen entsprechen, mit denen von der PNS-Entwicklerportal abgerufen. 

    ![][4]

###<a name="verify-registrations"></a>Vergewissern Sie sich Registrierungen

1. **Visual Studio**

    Wenn Sie Visual Studio für die Entwicklung verwenden können Sie Verbinden mit Microsoft Azure und anzeigen und Verwalten einer Gruppe von Azure-Dienste einschließlich Benachrichtigungen Hub aus "Server-Explorer". Dies ist vor allem für Ihre Umgebung Test-/hilfreich. 

    ![][9]

    Sie können anzeigen und Verwalten von alle Registrierungen in Ihrem Hub das gut für einheitlichen Plattform eingestuft werden oder Registrierung der Dokumentvorlage, beliebige Tags, PNS Bezeichner, Registrierung-Id und das Ablaufdatum. Sie können auch eine Registrierung im laufenden Betrieb - bearbeiten sinnvoll ist, sagen, wenn Sie alle Kategorien bearbeiten möchten. 

    ![][8]
 
    > [AZURE.NOTE] Visual Studio-Funktionen, um Registrierungen bearbeiten darf nur während der Test-/mit eingeschränkte Anzahl von Registrierungen verwendet werden. Wenn es eine müssen Ihre Registrierungen in Massen zu beheben entsteht, sollten Sie mithilfe der Registrierung Export-/Importfunktionalität hier - [Importieren/Exportieren von Registrierungen] (https://msdn.microsoft.com/library/dn790624.aspx) beschrieben.

2. **Dienstbus-explorer**

    Viele Kunden verwenden ServiceBus Explorer hier - [ServiceBus-Explorer] zum Anzeigen und Verwalten von ihrem Hub Benachrichtigung beschrieben. Es ist ein open Quellprojekt aus code.microsoft.com - [ServiceBus Explorer-code]

###<a name="verify-message-notifications"></a>Vergewissern Sie sich SMS-Benachrichtigungen

1. **Azure klassischen-Portal**

    Sie können wechseln Sie zur Registerkarte "Debuggen" Test Benachrichtigungen an Kunden zu senden, ohne benötigen von einem beliebigen Dienst Back-End- und ausgeführt wird. 

    ![][7]

2. **Visual Studio**

    Sie können auch Test Benachrichtigungen aus der Comforts von Visual Studio senden:

    ![][10]

    Sie können weitere Informationen über die Visual Studio Benachrichtigung Hub Azure-Explorer Funktionen hier - 
    
    - [Übersicht über die im Vergleich mit einer Server-Explorer]
    - [Im Vergleich mit einer Server Explorer Blogbeitrag - 1]
    - [Im Vergleich mit einer Server Explorer Blogbeitrag - 2]

###<a name="debug-failed-notifications-review-notification-outcome"></a>Fehler beim Benachrichtigungen Debuggen / Benachrichtigung Ergebnis überprüfen

**EnableTestSend-Eigenschaft**

Wenn Sie eine Benachrichtigung über Hubs Benachrichtigung senden, zunächst sie einfach wird in die Warteschlange für NH ausführen, um alle seine Ziele ermitteln Verarbeitung und dann später NH sendet es an die PNS. Dies bedeutet, dass bei Verwendung von REST-API oder eine Client-SDK der erfolgreiche Rückgabe des Anrufs senden bedeutet, dass sich die Nachricht von erfolgreich mit Benachrichtigung Hub Warteschlange wurde. Nicht sehr einen Einblick in NH Gelegenheit haben geblieben, um die Nachricht an PNS senden. Wenn Ihre Benachrichtigung nicht auf dem Client-Gerät eintreffen ist, besteht die Möglichkeit, wenn NH versucht, die Nachricht an PNS übermitteln, ein Fehler ist, den z. b. die Nutzlastgröße die maximal zulässige durch die PNS überschritten, oder die Anmeldeinformationen NH Berechtigungen konfiguriert ungültig usw. sind. Um einen Einblick in die PNS Fehler angezeigt wird, haben wir eine Eigenschaft namens [EnableTestSend Features]eingeführt werden. Diese Eigenschaft wird automatisch aktiviert, wenn Sie aus dem Portal oder Visual Studio-Client Testnachrichten senden und daher ermöglicht es Ihnen, detaillierte Debuggen Informationen anzuzeigen. Sie können dies über das Beispiel, in dem sie jetzt ist, .NET SDK aufzeichnen APIs und werden auf alle Client-SDKs später hinzugefügt werden. Zur Nutzung dieser mit den restlichen Anruf lediglich fügen Sie einen Abfragezeichenfolgeparameter mit der "test" am Ende des Anrufs senden z. B. an 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Beispiel (.NET SDK)*
 
Nehmen Sie an, dass Sie zum Senden einer Benachrichtigung systemeigenen Spruch .NET SDK verwenden:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);
 
`result.State`wird einfach staatliche `Enqueued` am Ende der Ausführung ohne alle Einblick in Ihre Pushbenachrichtigungen geblieben. Mittlerweile die `EnableTestSend` boolesche Eigenschaft bei der Initialisierung der `NotificationHubClient` bekomme detaillierten Status bezüglich der PNS Fehler aufgetreten, das die Benachrichtigung gesendet. Hier der senden-Anruf erfordert zusätzliche Zeit zurück, da es nur zurückgegeben wird, nachdem NH die Benachrichtigung an PNS, um das Ergebnis festzustellen übermittelt wurde. 
 
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);
    
    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);
    
    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Beispiel für die Ausgabe*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong
 
Diese Meldung zeigt an, entweder ungültigen Anmeldeinformationen in der Benachrichtigung Hub oder ein Problem mit der Registrierungen am Hub konfiguriert sind und empfohlen wäre diese Registrierung löschen, und lassen Sie den Kunden es vor dem Senden der Nachricht neu erstellen. 
 
> [AZURE.NOTE] Beachten Sie, dass die Verwendung dieser Eigenschaft stark beschränkt und daher Sie nur diese in Test-/Umgebung mit eingeschränkten Satz von Registrierungen verwenden müssen. Wir Benachrichtigung nur Debuggen von 10 Geräte. Wir haben auch maximal Verarbeitung Debuggen sendet, 10 pro Minute sein. 

###<a name="review-telemetry"></a>Überprüfen Sie werden 

1. **Verwenden von Azure klassischen-Portal**

    Im Portal ermöglicht Ihnen einen schnellen Überblick über alle die Aktivität auf Ihre Benachrichtigung Hub zu erhalten. 
    
    eine) von der Registerkarte "Dashboard" können Sie eine zusammengesetzte Ansicht der die Registrierungen, Benachrichtigungen als auch Fehler pro Plattform anzeigen. 
    
    ![][5]
    
    b) Sie können auch viele andere Plattform bestimmten Kriterien hinzufügen, auf der Registerkarte "Monitor" zu betrachten Sie tieferen besonders PNS bestimmte Fehler zurückgegeben, wenn NH versucht, die Benachrichtigung an den PNS zu senden. 
    
    ![][6]
    
    c) Sie sollte beginnen Sie mit den **Eingehenden Nachrichten** **Registrierung Vorgänge**, **Erfolgreich Benachrichtigungen** und wechseln Sie zu pro Plattform Tab, um zu überprüfen, die bestimmte PNS-Fehler. 
    
    d) Wenn Sie den Benachrichtigung Hub mit den Authentifizierungseinstellungen falsch konfiguriert haben, werden Sie PNS Authentifizierungsfehler angezeigt. Dies ist ein guter Indikator die PNS Anmeldeinformationen überprüfen. 

2) **Programmgesteuerten Zugriff**

Hier weitere Details- 

- [Access programmgesteuerten werden]
- [Telemetrieprotokoll Zugriff über APIs Stichprobe] 

> [AZURE.NOTE] Mehrere werden Zusammenhang Features wie **Importieren/Exportieren von Registrierungen**, **Werden Zugriff über APIs** usw. sind nur in Standard Ebene verfügbar. Wenn Sie versuchen, diese Features verwenden, wenn Sie in Free oder grundlegende Ebene sind erhalten Ausnahme Nachricht mit diesem Effekt Sie während der SDK und eine HTTP 403 (Verboten) verwenden, wenn sie direkt in die REST-APIs verwenden. Stellen Sie sicher, dass Sie aufgerufen haben nach Zeitphasen bis zum Standard über klassische Azure-Portal zu stufen.  

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png
 
<!-- LINKS -->
[Benachrichtigung Hubs (Übersicht)]: notification-hubs-push-notification-overview.md
[Erste Schritte-Lernprogramme]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Vorlage Anleitungen]: https://msdn.microsoft.com/library/dn530748.aspx 
[Leitfaden für APNS]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[GCM Anleitungen]: http://developer.android.com/google/gcm/adv.html
[Importieren/Exportieren von Registrierungen]: http://msdn.microsoft.com/library/dn790624.aspx
[ServiceBus-Explorer]: http://msdn.microsoft.com/library/dn530751.aspx
[ServiceBus Explorer-code]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Übersicht über die im Vergleich mit einer Server-Explorer]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[Im Vergleich mit einer Server Explorer Blogbeitrag - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[Im Vergleich mit einer Server Explorer Blogbeitrag - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend-Funktion]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Access programmgesteuerten werden]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Telemetrieprotokoll Zugriff über APIs Stichprobe]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

 