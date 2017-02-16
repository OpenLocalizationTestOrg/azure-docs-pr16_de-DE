<properties
   pageTitle="Einführung in Application Gateway | Microsoft Azure"
   description="Diese Seite enthält einen Überblick über die Application Gateway-Dienst für Layer 7 den Lastenausgleich, einschließlich Gateway Größe und SSL Auslagern HTTP Lastenausgleich, auf Grundlage von Cookies Sitzung Zugehörigkeit zu laden."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>

# <a name="application-gateway-overview"></a>Übersicht über die Anwendung Gateway

## <a name="what-is-application-gateway"></a>Was ist eine Anwendungsgateway

Microsoft Azure Application Gateway bietet Anwendung Übermittlung Controller (ADC) als Dienst, verschiedene Lastenausgleich Layer 7-Funktionen, die für eine Anwendung. Damit können Sie Webfarm Produktivität zu optimieren, indem Sie Verschiebung CPU stark SSL-Beendigung mit dem Gateway-Anwendung. Es stellt auch andere routing Layer 7-Funktionen, einschließlich der Funktion RUNDEN Robert Verteilung von eingehendem, Cookie basierend Sitzung Zugehörigkeit, Weiterf Grundlage URL und Möglichkeit, mehrere Websites hinter einem einzelnen Anwendungsgateway gehostet. Application Gateway, weist ebenfalls eine Web-Anwendung Firewall (WAF), die eine Anwendung gegen die meisten der OWASP Top 10 Web Leitfaden schützen. Application Gateway kann als Internet zugänglichen Gateway, internen nur Gateway oder eine Kombination aus beiden konfiguriert werden. Anwendungsgateway ist vollständig Azure verwaltet, skalierbare und hochgradig verfügbar. Darüber umfangreiche Menge von Diagnose und Protokollierungsfunktionen für eine bessere Verwaltbarkeit. Anwendungsgateway funktioniert mit virtuellen Computern, die Cloud-Diensten und internen oder externen zugänglichen Webanwendungen.

Application Gateway ist eine dedizierte virtuelle Einheit für eine Anwendung und mehrerer Instanzen von Worker für Skalierbarkeit und hohe Verfügbarkeit umfasst. Wenn Sie einen Gateway erstellen, ein Endpunkt (öffentlichen VIP oder internen ILB IP) zugeordnet ist und für eingehende Netzwerkverkehr verwendet. Dieser VIP oder ILB IP wird von Azure Lastenausgleich bereitgestellt, Ebene Transport (TCP/UDP) arbeiten und alle eingehenden Netzwerkverkehr Lastenausgleich auf die Anwendungsgateway Worker Instanzen werden müssen. Die Anwendungsgateway dann leitet der Datenverkehr HTTP-/HTTPS auf deren Konfiguration basierend, ob es sich um eines virtuellen Computers ist cloud-Dienst, interne oder eine externe IP-Adresse. Für die Vereinbarung zum SERVICELEVEL und Preise, auf den Seiten [Vereinbarung zum SERVICELEVEL](https://azure.microsoft.com/support/legal/sla/) und [Preise](https://azure.microsoft.com/pricing/details/application-gateway/) zu verweisen.

## <a name="features"></a>Features

Application Gateway unterstützt derzeit Layer 7 Anwendung Übermittlung mit den folgenden Features:

- **[Web Anwendung Firewall (Preview)](application-gateway-webapplicationfirewall-overview.md)** – der Web-Anwendung Firewall (WAF) in Azure Application Gateway verhindert allgemeine webbasierten Angriffen wie SQL einfügen, websiteübergreifende Skriptingtools Angriffen und übernimmt Sitzung Webanwendungen.
- **Lastenausgleich HTTP** - Anwendungsgateway bietet die Funktion RUNDEN Robert Lastenausgleich. Lastenausgleich erfolgt auf Layer 7 und für nur Verkehr HTTP (S) verwendet wird.
- **Cookie-basierten Sitzung Zugehörigkeit** - dieses Feature ist nützlich, wenn eine Sitzung Benutzer auf dem gleichen Back-End bleiben soll. Das Gateway-Anwendung kann mithilfe von Gateway verwaltet Cookies nachfolgende Datenfluss aus einer Sitzung Benutzer an die gleiche Back-End für die Verarbeitung weiterleiten. Dieses Feature ist wichtig in Fällen, wo Sitzungszustand lokal auf Back-End-Server für eine Sitzung Benutzer gespeichert ist.
- **[Auslagern secure Sockets Layer (SSL)](application-gateway-ssl-arm.md)** – diese Funktion hat die Aufgabe, teure HTTPS-Datenverkehr Deaktivieren von Webservern entschlüsseln. Beenden des Verbindungs SSL auf dem Gateway-Anwendung, und die Anfrage dauerhaften verschlüsselte Server weiterleiten, ist der Webserver durch die entschlüsseln überflüssigen.  Application Gateway verschlüsselt wieder die Antwort vor dem Senden an den Client zurück. Dieses Feature ist sinnvoll, wenn sich die Back-End, im gleichen abgesichert virtuelle Netzwerk als Gateway Anwendung in Azure befindet, Szenarien.
- **[Ende zum SSL](application-gateway-backend-ssl.md)** - Anwendungsgateway unterstützt durchgehende Verschlüsselung des Datenverkehrs. Application Gateway macht's durch die SSL-Verbindung auf dem Anwendungsgateway beenden. Das Gateway dann gilt für den Datenverkehr die Weiterleitungsregeln, verschlüsselt wieder das Paket und leitet das Paket an den entsprechenden Back-End-basierend auf den Weiterleitungsregeln definiert. Eine Antwort vom Webserver führt den gleichen Prozess wieder an den Endbenutzer.
- **[URL-basierte des inhaltsroutings](application-gateway-url-route-overview.md)** – dieses Feature bietet die Möglichkeit, unterschiedliche Back-End-Server für andere Datenverkehr verwenden. Datenverkehr für einen Ordner auf dem Webserver oder einem CDN kann an einen anderen Back-End nicht benötigte Last auf Downloadzeit spezifische Inhalte bei den ersten Schritten nicht verringert weitergeleitet werden.
- **[Website mit mehreren routing](application-gateway-multi-site-overview.md)** - Anwendung Gateway für Sie zum Konsolidieren von bis zu 20 Websites auf einer einzelnen Anwendungsgateway ermöglicht.
- **[Unterstützung Websocket](application-gateway-websocket.md)** - Anwendung Gateways ein weiteres großartiges Feature ist die systemeigene Unterstützung für Websocket.
- **[Überwachen des Systemzustands](application-gateway-probe-overview.md)** - Anwendungsgateway bietet Standard-Dienststatus Back-End-Ressourcen überwachen und benutzerdefinierte zum Überwachen der spezifischerer Szenarien untersucht.

## <a name="benefits"></a>Vorteile

Application Gateway eignet sich für:

- Programme, die erfordern Anfragen der gleichen Benutzer/Client-Sitzung auf der gleichen Back-End-virtuellen Computern erreicht haben. Beispiele für diesen Anwendungen würde Einkaufswagen apps und e-Mail-Webservern Einkaufs-werden.
- Programme, die Web-Server-Farmen von SSL Beendigung Verwaltungsaufwand freigegeben werden soll.
- Anwendungen, wie ein Netzwerk Bereitstellung von Inhalten, erfordert, dass mehrere HTTP-Anforderungen auf derselben langer TCP-Verbindung weitergeleitet werden oder laden zu anderen Back-End-Servern verteilt.
- Programme, die Websocket-Datenverkehr unterstützen
- Allgemeine webbasierten Angriffen schützen Webanwendungen wie SQL einfügen, websiteübergreifende Skriptingtools Angriffen und Sitzung übernimmt.

Application Gateway Lastenausgleich, wie ein Azure verwaltete Dienst ermöglicht die Bereitstellung von einem Layer 7 Lastenausgleich hinter den Lastenausgleich Azure Software. Datenverkehr-Manager kann verwendet werden, das Szenario ausführen, wie in der folgenden Abbildung gezeigt. Wo Datenverkehr Manager Umleitung und Verfügbarkeit bietet, Lastenausgleich bietet Skalierbarkeit Region und Verfügbarkeit und Anwendungsgateway bietet Cross Region Layer 7 Lastenausgleich.

![asdasd](./media/application-gateway-introduction/tm-lb-ag-scenario.png)

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="gateway-sizes-and-instances"></a>Gateway Größen und Instanzen

Application Gateway wird derzeit in drei Größen angeboten: klein, Mittel und groß. Kleine Instanzengrößen dienen zur Entwicklung und Testen Szenarien.

Zurzeit sind zwei Skus für Application Gateway: WAF und Standard.

Sie können bis zu 50 Anwendungsgateways pro Abonnement erstellen, und jedes Anwendungsgateway kann bis zu 10 Instanzen haben. 20 http-Listener kann jede Anwendungsgateway bestehen. Eine vollständige Liste der Anwendung Gateway Grenzwerte finden Sie auf der Seite [Beschränkungen Service](../azure-subscription-service-limits.md#application-gateway) .

Die folgende Tabelle enthält eine durchschnittliche Leistungsdurchsatz für jede Instanz der Anwendung Gateway:

| Seitenantwort Back-End | Kleine | Mittel | Große|
|---|---|---|---|
| 6K | 7.5/s | 13/s | 50 MB / |
|100K | 35/s | 100 MB/s| 200 MB |

>[AZURE.NOTE] Diese Werte sind Näherungswerten für ein Gateway Anwendungsdurchsatz. Der tatsächliche Durchsatz hängt verschiedene Umgebungsdetails, wie z. B. Mittelwert Zeichenblattgröße Speicherort der Back-End-Instanzen und Verarbeitungszeit zu eine Seite dienen. Genaue Leistung Zahlen sollten Sie Ihre eigenen Tests ausführen, diese Werte werden nur für Leitfaden zum Planen der Kapazität bereitgestellt.

## <a name="health-monitoring"></a>Überwachen des Systemzustands

Azure Application Gateway überwacht automatisch die Integrität des die Back-End-Instanzen durch Basic oder benutzerdefinierten Gesundheit untersucht. Mithilfe von Gesundheit Prüfpunkte, um sicherzustellen, dass nur fehlerfrei Hosts auf den Datenverkehr reagieren. Weitere Informationen finden Sie unter [Application Gateway Gesundheit (Übersicht)](application-gateway-probe-overview.md).

## <a name="configuring-and-managing"></a>Konfigurieren und Verwalten von

Für den Endpunkt kann Anwendungsgateway eine öffentliche IP-Adresse, private IP- oder beides haben, wenn er konfiguriert ist. Application Gateway ist in einem virtuellen Netzwerk in einem eigenen Subnetz konfiguriert. Das Subnetz erstellt oder für die Anwendungsgateway verwendet keine anderen Ressourcenarten enthalten, die nur Ressourcen, die im Subnetz dürfen sind andere Anwendungsgateways. Um die Back-End-Back-End-Ressourcen zu sichern können Servern in einem anderen Subnetz im gleichen virtuellen Netzwerk als Anwendungsgateway enthalten sein. Diese zusätzlichen Subnetz gehören, die nicht für die Back-End-Applikationen erforderlich werden soll, solange die IP-Adresse des Gateways Anwendung erreicht werden kann, kann die Anwendungsgateway ADC-Funktionen für die Back-End-Server bereitstellt.

Sie können erstellen und Verwalten von ein Gateway mit REST-APIs, PowerShell-Cmdlets, Azure CLI oder [Azure-Portal](https://portal.azure.com/).

## <a name="next-steps"></a>Nächste Schritte

Nach dem Kennenlernen der Anwendung Gateways, können Sie [einen Gateway erstellen](application-gateway-create-gateway-portal.md) oder Sie können einen Lastenausgleich HTTPS-Verbindungen [erstellen einen Gateway SSL Auslagern](application-gateway-ssl-arm.md) .

So erstellen Sie einen Gateway mit URL-basierten des inhaltsroutings finden Sie unter So [Erstellen Sie einen Gateway mit URL-basierten routing](application-gateway-create-url-route-arm-ps.md) für Weitere Informationen.

