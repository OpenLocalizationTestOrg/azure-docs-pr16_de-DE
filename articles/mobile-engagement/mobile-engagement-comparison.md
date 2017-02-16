<properties
    pageTitle="Vergleich von Azure Mobile Engagement mit anderen ähnlichen Azure-Diensten"
    description="Vergleich von Azure Mobile Engagement mit anderen ähnliche Azure Services - HockeyApp, AppInsights, Benachrichtigung Hubs"
    services="mobile-engagement"
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="comparing-azure-mobile-engagement-with-other-similar-azure-services"></a>Vergleich von Azure Mobile Engagement mit anderen ähnlichen Azure-Diensten

Die Liste der Dienste von Microsoft Azure angeboten wird jemals erweitert und manchmal vielleicht Sie, wie Azure Mobile Engagement diese anderen Dienst abweicht, die Sie gerade lesen oder zu hören. In diesem Artikel versucht, deaktivieren Sie die Verwirrung und leitet Sie Azure Mobile Engagement auswählen, wenn es für Ihre Verwendung geeigneter ist. 
 
Azure Mobile Engagement ist ein Dienst speziell **für digitale Händler/CMOs** vorgesehen sind, aber konnte von verwendet werden alle **mobile-app Besitzer oder Publisher** , möchte die Verwendung, Aufbewahrung und Monetization ihre mobile-Apps zu erhöhen. 

*Es ist eine Software als Service (SaaS) Benutzer-Engagement Plattform, die bietet Daten basierende Einsichten in die Verwendung der app, Benutzer in Echtzeit Segmentierung, und je nach Kontext bewusst Pushbenachrichtigungen und in der app messaging ermöglicht.* 

Wenn Sie diese Definition aufteilen, haben wir die folgenden Hauptmerkmale wodurch hervorgehoben wird auch der zugehörigen eindeutigen nutzen:

1.  **Je nach Kontext bewusst Pushbenachrichtigungen und in der app messaging**
        
    Dies ist der Fokus Core des Produkts – zielgerichteten und individuellen Pushbenachrichtigungen ausführen. Und damit dies passieren, sammeln wir Rich-Verhalten Analytics-Daten. 

2.  **Daten basierende Einsichten in die Verwendung der app**

    Wir erläutern die notwendigen Cross Platform SDKs auszuwählen, die Verhalten Analytics über die app-Benutzer sammeln. Beachten Sie den Ausdruck Verhalten Analytics (anstelle von Leistung Analytics), da wir möchten wie die app-Benutzer die app verwenden. Wir grundlegende Leistung Analytics-Daten zu Fehlern, Abstürzen usw. aber das heißt nicht den Fokus Core des Produkts zu ermitteln. 

3.  **Benutzer in Echtzeit Segmentierung**

    Nachdem Sie die app-Benutzer Verhalten Analytics-Daten gesammelt haben, können wir Sie zum Segment Ihre Zielgruppe anhand verschiedener Parameter und erfassten Daten, die Ihnen ermöglichen, führen Sie den Pushbenachrichtigungen massensendungen zu ermitteln. 

4.  **Software-as-a-Service (SaaS):**

    Wir haben ein Portal separaten aus dem Azure Verwaltungsportal interagieren und umfassende Verhalten Analysen über die app-Benutzer anzeigen und Ausführen von Pushbenachrichtigungen Marketingkampagnen optimiert ist. Das Produkt wird darüber, wie Sie im Handumdrehen schnellen Einstieg!   
 
Mit diesen Unterschieden in Hand hier wie mit anderen anscheinend ähnliche Azure Angebote verglichen – Beachten Sie, dass im Artikel führen Sie eine Ebene Featurevergleich nicht akzeptiert eine weitere Szenario-basierten Ansatz, um zu definieren, welches Produkt am besten geeignet ist:
 
1.  [HockeyApp](https://azure.microsoft.com/services/hockeyapp/) ist die Microsoft Lösung für mobile DevOps. Mit, können Sie Builds Betatester verteilen, Absturzdaten sammeln und Benutzerfeedback erhalten. Es kann auch in Visual Studio Team Services UFI-Bereitstellungen einfach erstellen und Arbeit Element Integration integriert. 
    
    Hier der Fokus befindet sich auf DevOps und Sammeln von Leistung Analytics-Daten zu den mobilen apps. Für beide HockeyApps Integration einhandeln möglicherweise und Mobile Engagement in der app und die werden nicht ungewöhnliche da, obwohl es einige Überlappung in die gesammelten Daten gibt, unterscheidet sich des Fokus Core der Produkte und im Hinblick auf andere Ziele für Sie verhelfen.  

2.  [Anwendung Einsichten](../application-insights/app-insights-overview.md) Wenn Ihre mobile-app einen serverseitigen aufweist, klicken Sie dann verwenden Sie Anwendung Einsichten zum Überwachen der Web-Server-Seite der app, aber für mobile Seite Clientanwendungen, sollten Sie HockeyApp verwenden. 

3.  [Benachrichtigung Hubs](https://azure.microsoft.com/services/notification-hubs/) bietet einen einfachen d. h., Sie kein SDK in der mobilen app integriert benötigen und können Sie Vollzugriff auf Ihre mobile-app und dies zulässt Pushbenachrichtigungen mit Grundfunktionen tagging senden. Hierbei handelt es sich hervorragend für eine app Besitzer, die kleiner zu verwendet und Weitere Informationen zu senden/kommunizieren von Informationen sofort für ihre app-Benutzer (Übertragen einer großen Population) interessiert. 

    Beachten Sie den Fokus auf schnelle Benachrichtigungen mit grundlegenden Segmentierungsfunktion hervorragende senden. 

Werfen wir nun einige Szenarien:

1.  TIM ist Teil des digitalen marketing Teams bei Adventure Works-Store wodurch mobilen apps für Benutzer veröffentlicht. Ziel des TIM wird sichergestellt, dass die Benutzer, die ihre mobile apps herunterladen weiterhin verwenden und häufig. Diese erhöht nicht nur eine Marke anfügen, die mit der app-Benutzer, sondern auch Monetization erhöht die app-Benutzer mithilfe der mobilen app Einkäufe wirkt. Hierfür müssen Tim zielgerichtete Benachrichtigungen für die app-Benutzer senden, die sie hilfreich sein, können sie die app zu öffnen und wieder zur es häufig. Dies ist die Stelle, an der Tim Mobile Engagement hilfreiche. 

2.  JoAnn ist Teil der mobilen apps von Adventure Works das Entwicklungsteam und für ein Produkt Details abstürzen, Ausnahmen, melden Sie sich und Leistung werden aus einer app gefunden wird. Dies ist die Stelle, an der Joann HockeyApp hilfreiche. JoAnn konnte in der gleichen app, um das Beste beider sowohl HockeyApp für ihre Entwicklertools konzentrieren Szenarien Azure Mobile Engagement für Tim integrieren. 

3.  Robert gehört das Entwicklungsteam der mobilen apps bei Contoso News Netzwerk- und sämtliche möchte Sie besteht darin, versenden News Benachrichtigungen für alle Benutzer ohne viel Segmentierung unterbrechen, sobald die News Warnung ausgegeben wird. Dies ist die Stelle, an der Robert Benachrichtigung Hubs hilfreiche. In diesem Szenario ist es jedoch möglich mobile-app im Laufe der Zeit, eine Vorbedingung viel umfangreichere Segmentierung und Anzeigen von Details zu der app-Benutzers Verhalten auftritt. Zu diesem Zeitpunkt, Robert Azure Mobile Engagement ausgewertet werden müssen. 
 
Um Wiederholung, der Zweck eines Auftrags für Mobile ist nicht nur Analytics sammeln – es ist nicht "noch eine andere Analytics Produkt von Microsoft". Es ist zum Senden von zielgerichteten Pushbenachrichtigungen und für diese Verwendung sammeln wir Verhalten Analytics-Daten der Fokus bleibt jedoch Pushbenachrichtigungen, die wodurch am sinnvollsten für die app-Benutzer sind, damit es nicht als Spam, über enthalten ist senden. 

Weitere Details – sehen Sie sich in diesem [kurzen Video](mobile-engagement-overview.md) zu Mobile Engagement kurz gesagt. 

