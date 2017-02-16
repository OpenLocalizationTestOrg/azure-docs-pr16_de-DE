<properties
   pageTitle="Optimieren der ExpressRoute routing | Microsoft Azure"
   description="Diese Seite enthält Details zum Optimieren, wenn ein Kunde mehrere ExpressRoute Schaltkreise enthält, die zwischen Microsoft und der vom Kunden Unternehmen Netzwerk verbinden routing."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="charwen"/>

# <a name="optimize-expressroute-routing"></a>Optimieren der ExpressRoute Routing
Wenn Sie mehrere ExpressRoute Schaltkreise haben, haben Sie mehr als einen Pfad für die Verbindung zu Microsoft. Daher nicht optimalen routing kommen immer - kann d. h., der Datenverkehr einen mehr Pfad zum Erreichen von Microsoft und Microsoft mit Ihrem Netzwerk dauern. Je mehr Netzwerkpfad, je höher der Wartezeit. Wartezeit hat direkten Einfluss auf Leistung und Benutzer Komfort. In diesem Artikel werden die veranschaulichen dieses Problems und erläutert, wie Sie mithilfe der standardmäßigen Weiterleitung Technologies routing optimieren.

## <a name="suboptimal-routing-case-1"></a>Nicht optimalen Weiterleitung Fall 1
Werfen Sie einen Einblick in die Weiterleitung Problem durch ein Beispiel für ein. Angenommen Sie, Sie haben zwei Büros in den USA, eine in Los Angeles und eine in New York. Ihre Büros besteht auf einem WAN Wide Area Network (), entweder Ihr eigenes Backbonenetzwerk oder des Dienstanbieters IP VPN erfolgen kann. Sie haben zwei ExpressRoute Schaltkreise, eine in uns "Westen" und eine im Kontaktformular Osten, die auch über die WAN-verbunden sind. Offensichtlich, haben Sie zwei Pfade für die Verbindung mit dem Microsoft-Netzwerk. Jetzt stellen Sie sich vor Azure-Bereitstellung (z. B. Azure App-Verwaltungsdienst) uns Westen und Osten uns. Ihre Absicht besteht darin, die Benutzer in Los Angeles zu Azure uns Westen und Ihre Benutzer in New York nach Azure uns OST aufgrund Ihrer Dienstadministrator gibt bekannt, dass Benutzer in jeder Niederlassung die benachbarten Azure Dienste für optimale Erfahrung zugreifen. Leider funktioniert der Plan auch für die Benutzer Nord, aber nicht für Benutzer Westen. Die Ursache des Problems ist vor. Auf jede Verbindung ExpressRoute kündigen wir Ihnen, das Präfix in Azure uns Ost (23.100.0.0/16), und das Präfix in Azure uns "Westen" (13.100.0.0/16). Wenn Sie nicht wissen, welche Präfix aus welcher Region ist, können Sie nicht anders zu behandeln. Beide Präfixe sind näher an uns Osten als uns "Westen" und dadurch beide Office-Benutzer weiterleiten, um die Verbindung ExpressRoute im Kontaktformular Osten, möglicherweise WAN-Netzwerks vorstellen. Am Ende haben Sie viele Trauriges Benutzer im Büro Los Angeles.

![](./media/expressroute-optimize-routing/expressroute-case1-problem.png)

### <a name="solution-use-bgp-communities"></a>Lösung: Verwenden Sie BGP Communitys
Um routing für beide Office-Benutzer zu optimieren, müssen Sie wissen, welche Präfix aus Azure uns Westen und die von Azure uns Osten ist. Wir codiert werden diese Informationen mithilfe von [Werten BGP-Community](expressroute-routing.md). Wir haben einen eindeutigen BGP Community Wert jeder Azure Region, z. B. "12076:51004" für uns Osten, "12076:51006" für uns "Westen" zugewiesen. Nun wissen, welche Präfix aus welcher Azure Bereich ist, können Sie konfigurieren, welche ExpressRoute Verbindung bevorzugte sein soll. Da wir die BGP verwenden, um die exchange-routing Informationen, können BGPs lokalen Voreinstellung Sie routing beeinflussen. In diesem Beispiel können Sie einen höheren lokale Preference Wert zu 13.100.0.0/16 in uns "Westen" als im Kontaktformular Osten und auf ähnliche Weise einen höheren lokale Preference Wert 23.100.0.0/16 im Kontaktformular Osten als in uns "Westen" zuweisen. Dieser Konfiguration wird sicherzustellen, dass, wenn beide Pfade an Microsoft verfügbar sind, die Benutzer in Los Angeles die Verbindung ExpressRoute in uns Westen dauert Verbindung zum Azure uns "Westen", die Benutzer in New York zu Azure uns Osten der ExpressRoute im Kontaktformular Osten ergreifen. Routing ist auf beiden Seiten optimiert. 

![](./media/expressroute-optimize-routing/expressroute-case1-solution.png)

## <a name="suboptimal-routing-case-2"></a>Nicht optimalen Weiterleitung Fall 2
Hier ein weiteres Beispiel, in dem Verbindungen von Microsoft einen mehr Pfad zur Verbindung mit Ihrem Netzwerk ausführen. Verwenden Sie in diesem Fall lokalen Exchange-Servern und Exchange Online in einer [hybridumgebung](https://technet.microsoft.com/library/jj200581%28v=exchg.150%29.aspx)aus. Ihre Büros sind mit einem WAN verbunden. Sie ankündigen die Präfixe Ihrer Server lokal in beiden Ihren Büros an Microsoft über die zwei ExpressRoute Schaltkreise. Exchange Online initiieren Verbindungen auf die Server lokal in Fällen, wie z. B. Migration von Postfächern. Leider ist die Verbindung zu Ihrem Office Los Angeles an die Verbindung ExpressRoute im Kontaktformular Osten vor dem durchlaufen die gesamte Continent zurück zu den Westen weitergeleitet. Die Ursache des Problems ähnelt der ersten Phase. Ohne alle Hinweistext kann nicht das Microsoft-Netzwerk feststellen, welche Kunden Präfix möglichst ähnlich uns Osten ist und der in der Nähe uns "Westen" befindet. Dies geschieht falschen Pfad zu Ihrem Office in Los Angeles auswählen.

![](./media/expressroute-optimize-routing/expressroute-case2-problem.png)

### <a name="solution-use-as-path-prepending"></a>Lösung: Verwenden Sie AS Pfad vorangestellt wird.
Es gibt zwei Lösungen für das Problem. Die erste ist, dass Sie einfach Ihre lokalen Präfix für Ihre Office 177.2.0.0/31, auf die ExpressRoute Verbindung in uns "Westen" Los Angeles ankündigen und Ihrem lokalen für Ihre Office New York 177.2.0.2/31, auf die Verbindung ExpressRoute im Kontaktformular Osten Präfix. Es gibt daher nur einen Pfad für die Verbindung zu den einzelnen Ihren Büros Microsoft. Besteht keine Mehrdeutigkeit und routing optimiert ist. Mit diesem Entwurf müssen Sie Ihre Failoverstrategie anzustellen. Den Fall, dass der Pfad über ExpressRoute an Microsoft fehlerhaft ist, müssen Sie sicherstellen, dass Exchange Online noch mit Ihrem lokalen Server herstellen kann. 

Die zweite Lösung besteht darin, dass Sie beide Präfixe auf beide ExpressRoute Schaltkreise ankündigen weiterhin außerdem, die Sie machen ist ein Hinweis, welches Präfix in der Nähe des Büros welche eine. Da wir die BGP AS Pfad vorangestellt wird unterstützt, können Sie den AS-Pfad für Ihre Präfix zum routing beeinflussen konfigurieren. In diesem Beispiel können Sie den Pfad AS 172.2.0.0/31 im Osten uns, damit wir die Verbindung ExpressRoute in uns "Westen" für den Datenverkehr für dieses Präfix (wie unsere Netzwerk nach der Pfad für dieses Präfix verkürzen im Westen Meinung wird) lieber verlängern. Auf ähnliche Weise können Sie den Pfad AS 172.2.0.2/31 in uns "Westen" verlängern, damit wir in uns Osten die ExpressRoute-Verbindung bevorzugen wird. Routing ist für beide Büros optimiert. Mit diesem Entwurf ist eine ExpressRoute Verbindung fehlerhafte, erreichen Exchange Online noch Sie über ein anderes ExpressRoute Verbindung und Ihre WAN. 

>[AZURE.IMPORTANT] Wir entfernen privaten als Zahlen in den AS-Pfad für die Präfixe auf Microsoft Peering empfangen. Sie müssen anfügen öffentlichen als Zahlen in den AS-Pfad für die Weiterleitung für Microsoft Peering beeinflussen.

![](./media/expressroute-optimize-routing/expressroute-case2-solution.png)

>[AZURE.IMPORTANT] Während die hier angegebenen Beispiele für Microsoft und öffentliche Peerings sind, unterstützen wir die gleichen Funktionen für die Private peering. Funktioniert auch, innerhalb einer einzelnen ExpressRoute Verbindung, um die Auswahl der primären und sekundären Pfade beeinflussen den Pfad AS vorangestellt wird.
