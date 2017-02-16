<properties
    pageTitle="Funktionsweise der Datenverkehr Manager | Microsoft Azure"
    description="In diesem Artikel wird erläutert, wie Azure Datenverkehr Manager funktioniert"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="how-traffic-manager-works"></a>Funktionsweise der Datenverkehr-Manager

Azure Datenverkehr-Manager können Sie die Verteilung der Verkehr über Ihre Anwendungsendpunkte zu steuern. Ein Endpunkt ist jeder Internet zugänglichen Dienst innerhalb oder außerhalb von Azure gehostet.

Datenverkehr Manager bietet zwei wesentliche Vorteile:

1. Verteilung der Datenverkehr entsprechend eines von mehreren [Methoden Datenverkehr-routing](traffic-manager-routing-methods.md)
2. [Kontinuierliche Überwachung der Endpunkt Gesundheit](traffic-manager-monitoring.md) und automatisches Failover, wenn die Endpunkte fehl

Wenn ein Client versucht, die Verbindung mit eines Diensts, müssen sie zuerst der DNS-Name des Diensts in eine IP-Adresse aufgelöst werden. Anschließend verbindet der Client mit dieser IP-Adresse den Zugriff auf Dienste.

**Der wichtigste Punkt zu verstehen ist, dass Datenverkehr Manager Ebene der DNS-Einträge.**  Den Datenverkehr Manager verwendet DNS Clients an bestimmten Dienstendpunkte basierend auf den Regeln der Datenverkehr-routing Methode direkte. Clients eine Verbindung herstellen, um die ausgewählten Endpunkt **direkt**. Datenverkehr Manager ist keinen Proxy oder ein Gateway. Datenverkehr Manager wird den Datenverkehr zwischen dem Client und der Dienst nicht angezeigt werden.

## <a name="traffic-manager-example"></a>Beispiel für den Datenverkehr-Manager

Contoso-Unternehmen haben ein neues Partnerportal entwickelt. Die URL für dieses Portal ist https://partners.contoso.com/login.aspx. Die Anwendung wird in drei Bereiche eines Azure gehostet werden. Zum Verbessern der Verfügbarkeit und globale Leistung maximieren, können sie Datenverkehr-Manager den Clientdatenverkehr in den nächsten verfügbaren Endpunkt verteilen.

Um diese Konfiguration zu erzielen:

- Diese drei Instanzen von deren Dienst bereitgestellt werden. Die DNS-Namen für diese Bereitstellungen sind 'Contoso-us.cloudapp .net', 'Contoso-eu.cloudapp .net' und 'Contoso-asia.cloudapp .net'.
- Diese dann Datenverkehr Manager Profil, mit dem Namen 'contoso.trafficmanager.net', erstellen und konfigurieren, um die "Leistung" Datenverkehr-routing Methode über die drei Endpunkte verwenden.
- Schließlich konfigurieren sie ihren Domänennamen Eitelkeit 'partners.contoso.com' auf 'contoso.trafficmanager.net', verwenden einen DNS-CNAME-Eintrag verweisen.

![Datenverkehr Manager DNS-Konfiguration][1]

> [AZURE.NOTE] Wenn Sie eine Domäne Eitelkeit mit Azure Datenverkehr Manager verwenden, müssen Sie einen CNAME verwenden, Ihren Domänennamen Eitelkeit auf Ihren Domänennamen Datenverkehr Manager verweisen. Erstellen Sie einen CNAME 'Spitze' (oder Root) einer Domäne führen Sie in DNS-Standards nicht zulässig. Daher können keine CNAME für 'contoso.com' ('naked' Domäne bezeichnet) erstellt werden. Sie können nur einen CNAME für eine Domäne unter "contoso.com", beispielsweise "www.contoso.com" erstellen. Wenn Sie diese Einschränkung umgehen, empfehlen wir eine einfache HTTP-Umleitung auf direkte Anfragen für 'contoso.com', um einen alternativen Namen ein, beispielsweise "www.contoso.com".

## <a name="how-clients-connect-using-traffic-manager"></a>Wie Clients eine Verbindung herstellen mit Datenverkehr-Manager

Sie fortfahren aus dem vorherigen Beispiel, wenn ein Client die Seite https://partners.contoso.com/login.aspx anfordert, führt der Client die folgenden Schritte aus, um die DNS-Namen auflösen und eine Verbindung herzustellen:

![Verbindung Niederlassung mit den Datenverkehr-Manager][2]

1. Der Client sendet eine DNS-Abfrage an seinen konfigurierten rekursive DNS-Dienst Problembehebung partners.contoso.com' Name'. Ein rekursive DNS-Dienst, auch als bezeichnet einen Dienst 'lokale DNS' hosten DNS-Domänen nicht direkt. Der Client verteilt lieber, die Arbeit der Kontakt zu verschiedenen autorisierende DNS-Dienste über das Internet zum Auflösen eines DNS-Namens erforderlich sind.
2. Um den DNS-Namen zu beheben, findet der rekursive DNS-Dienst die Namenserver für die Domäne "contoso.com". Klicken Sie dann kontaktiert diesen Namenserver zum Anfordern des DNS-Eintrags 'partners.contoso.com'. Die contoso.com DNS-Server zurück, den CNAME-Eintrag, der auf contoso.trafficmanager.net verweist.
3. Als Nächstes findet der rekursive DNS-Dienst die Namenserver für die Domäne 'trafficmanager.net', die von der Azure Datenverkehr Manager-Dienst bereitgestellt werden. Es sendet eine Anforderung für den DNS-Eintrag 'contoso.trafficmanager.net' klicken Sie dann auf diesen DNS-Server.
4. Die Namenserver Datenverkehr Manager erhalten die Anforderung. Sie wählen Sie einen Endpunkt basierend auf:

    * Der konfigurierten Zustand des jeder Endpunkt (Endpunkte deaktiviert werden nicht zurückgegeben.)
    * Jedes Endpunkts, wie durch die Integrität Datenverkehr Manager bestimmt der aktuelle Zustand überprüft. Weitere Informationen finden Sie unter [Den Datenverkehr Manager Endpunkt überwachen](traffic-manager-monitoring.md).
    * Der gewählten Datenverkehr-routing Methode. Weitere Informationen finden Sie unter [Den Datenverkehr Manager Routing Methoden](traffic-manager-routing-methods.md).

5. Der ausgewählte Endpunkt wird als eine andere DNS CNAME-Eintrag zurückgegeben. In diesem Fall lassen Sie uns Angenommen Sie, Contoso-us.cloudapp.net zurückgegeben wird.
6. Als Nächstes findet der rekursive DNS-Dienst die Namenserver für die Domäne 'cloudapp.net' ein. Es diese Namenserver zum anfordern 'Contoso-us.cloudapp .net' Kontakte DNS-Eintrag. DNS-'A' Datensatz, enthält die IP-Adresse des US-basierten Service-Endpunkts an zurückgegeben.
7. Rekursive DNS-Dienst konsolidiert die Ergebnisse, und gibt eine einzelne DNS-Antwort an den Client.
8. Der Client empfängt die DNS-Ergebnisse und eine Verbindung mit der angegebenen IP-Adresse. Der Client eine Verbindung mit der Anwendung-Endpunkt direkt nicht über den Datenverkehr-Manager. Da es sich um einen HTTPS-Endpunkt handelt, der Client erfolgt den notwendigen SSL/TLS-Handshake, und anschließend wird eine HTTP GET-Anforderung für die ' / login.aspx' Seite.

Rekursive DNS-Dienst speichert die DNS-Antworten, die sie empfängt. Die DNS-Auflösung auf dem Clientgerät zwischengespeichert auch das Ergebnis aus. Zwischenspeichern ermöglicht nachfolgende DNS-Abfragen schneller durch Verwenden von Daten aus dem Cache, anstatt Abfragen von anderen Namenservern beantwortet werden. Die Dauer des Caches wird durch die Eigenschaft "Time-to-live" (TTL) für jeden DNS-Eintrag bestimmt. Kürzere Werte führen schneller Cache Ablauf und daher mehr Schleifen auf die Namenserver Datenverkehr-Manager. Mehr Werte bedeuten, dass es in direkter Datenverkehr nicht an einem Fehler beim Endpunkt länger dauern kann. Datenverkehr-Manager können Sie konfigurieren die TTL in den Datenverkehr Manager DNS-Antworten, verwendet Sie den Wert auswählen können, der die Anforderungen Ihrer Anwendung am besten Salden aktivieren.

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="what-ip-address-does-traffic-manager-use"></a>Welche IP-Adresse wird Datenverkehr Manager verwendet?

Wie unter Funktionsweise der Datenverkehr Manager erläutert, funktioniert Datenverkehr Manager Ebene der DNS-Einträge. DNS-Antworten an Clients an den entsprechenden Dienstendpunkt direkte sendet. Clients dann Herstellen einer Verbindung mit der Endpunkt direkt, nicht über den Datenverkehr-Manager.

Daher bietet Datenverkehr Manager keinen Endpunkt oder IP-Adresse für Clients zum Herstellen einer Verbindung mit. Daher Sie gegebenenfalls statische IP-Adresse des Diensts müssen, die auf den Dienst, der nicht in den Datenverkehr Manager konfiguriert werden.

### <a name="does-traffic-manager-support-sticky-sessions"></a>Unterstützt den Datenverkehr-Manager 'Kurznotizen' Sitzungen?

Als erläutert [zuvor](#how-clients-connect-using-traffic-manager)funktioniert Datenverkehr Manager Ebene der DNS-Einträge aus. DNS-Antworten verwendet, um die direkte Clients an den entsprechenden Dienstendpunkt. Clients Herstellen einer Verbindung mit der Endpunkt direkt, nicht über den Datenverkehr-Manager. Daher sieht Datenverkehr Manager nicht den HTTP-Verkehr zwischen dem Client und auf dem Server.

Darüber hinaus gehört die Quelle IP-Adresse der DNS-Abfrage empfangen von Datenverkehr Manager zum rekursive DNS-Dienst, nicht auf dem Client. Daher Datenverkehr Manager hat keine Möglichkeit, eine einzelne Clients nachverfolgen und kann nicht 'Kurznotizen' Sitzungen implementieren. Diese Einschränkung ist für alle DNS-basierte Datenverkehr Managementsysteme allgemeine und ist nicht spezifisch in den Datenverkehr-Manager.

### <a name="why-am-i-seeing-an-http-error-when-using-traffic-manager"></a>Warum werden bei Verwendung von Datenverkehr Manager einen HTTP-Fehler angezeigt?

Als erläutert [zuvor](#how-clients-connect-using-traffic-manager)funktioniert Datenverkehr Manager Ebene der DNS-Einträge aus. DNS-Antworten verwendet, um die direkte Clients an den entsprechenden Dienstendpunkt. Clients dann Herstellen einer Verbindung mit der Endpunkt direkt, nicht über den Datenverkehr-Manager. Datenverkehr Manager unterstützt nicht finden Sie unter HTTP-Datenverkehr zwischen Client und Server. Daher muss alle HTTP-Fehler, die angezeigt werden, von Ihrer Anwendung zu kommen. Für den Client für die Verbindung mit der Anwendung werden alle DNS-Lösungsschritten abgeschlossen. Die enthält Eingriff, die Datenverkehr-Manager auf den Datenfluss Anwendung enthält.

Weitere Untersuchung sollten daher auf die Anwendung konzentrieren.

HTTP-Hostheader aus den Client Browser gesendet wird, der am häufigsten verwendeten Quelle Probleme. Stellen Sie sicher, dass die Anwendung konfiguriert ist, um die richtige Host Kopfzeile für den Domänennamen akzeptieren abgegeben haben. Endpunkte der Azure-App-Dienst verwenden finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für eine Web-app in Azure-App-Verwaltungsdienst mit den Datenverkehr-Manager](../app-service-web/web-sites-traffic-manager-custom-domain-name.md).

### <a name="what-is-the-performance-impact-of-using-traffic-manager"></a>Was ist der Leistung durch den Datenverkehr Manager verwenden?

Als erläutert [zuvor](#how-clients-connect-using-traffic-manager)funktioniert Datenverkehr Manager Ebene der DNS-Einträge aus. Da Clients mit Ihrer Endpunkte direkt verbunden werden, es gibt keine Auswirkung auf tatsächlich bei Verwendung von Datenverkehr Manager nach dem Herstellen der Verbindung die Leistung.

Da Applikationen Ebene der DNS-Manager Datenverkehr integriert, erfordert eine zusätzliche DNS-Suche in der DNS-Auflösung Kette eingefügt werden soll (siehe [den Datenverkehr Manager Beispiele](#traffic-manager-example)). Den Einfluss von Datenverkehr Manager termingerecht mit einer Auflösung von DNS ist minimal. Den Datenverkehr Manager verwendet ein globales Netzwerk der Namenserver, und [Anycast](https://en.wikipedia.org/wiki/Anycast) networking um sicherzustellen, dass die DNS-Abfragen an den nächsten verfügbaren Namenserver immer weitergeleitet werden. Darüber hinaus bedeutet Zwischenspeichern von DNS-Antworten, dass die zusätzliche DNS-Wartezeit tatsächlich mit den Datenverkehr-Manager nur in einen Bruch Sitzungen angewendet wird.

Die Leistung Methode leitet den Datenverkehr an den Endpunkt des nächsten verfügbar. Das Ergebnis ist, dass die Gesamtsystemleistung Auswirkung dieser Methode zugeordneten minimalen sein soll. Erhöhung der DNS-Wartezeit sollte vom unteren Netzwerkwartezeit an den Endpunkt versetzt werden soll.

### <a name="what-application-protocols-can-i-use-with-traffic-manager"></a>Welche Anwendungsprotokolle können mit den Datenverkehr Manager werden verwendet?

Als erläutert [zuvor](#how-clients-connect-using-traffic-manager)funktioniert Datenverkehr Manager Ebene der DNS-Einträge aus. Nachdem Sie die DNS-Suche abgeschlossen ist, Clients Herstellen einer Verbindung mit den Anwendungsendpunkt direkt, nicht über den Datenverkehr-Manager. Daher kann die Verbindung eine Anwendungsprotokoll verwenden. Den Datenverkehr Vorgesetzten Endpunkt integritätsprüfungen erfordern jedoch eine HTTP oder HTTPS-Endpunkt. Der Endpunkt für eine integritätsprüfung kann sich den Anwendungsendpunkt unterscheiden, die Clients eine Verbindung herstellen.

### <a name="can-i-use-traffic-manager-with-a-naked-domain-name"></a>Kann ich den Datenverkehr Manager mit einem 'naked' Domänennamen verwenden?

Nein. Die DNS-Standards erlauben keine CNAMEs gemeinsam mit anderen DNS-Einträge mit demselben Namen vorhanden ist. Die Spitze (oder die Quadratwurzel) einer DNS-Zone enthält immer zwei bereits vorhandenen DNS-Einträge; die SOA- und autorisierende NS-Einträge. Dies bedeutet, dass ein CNAME-Eintrag in der Zone Spitze erstellt werden kann, ohne den DNS-Standards verletzt.

Wie im [Beispiel Datenverkehr Manager](#traffic-manager-example)erläutert, erfordert Datenverkehr Manager einen DNS CNAME-Eintrag, den Eitelkeit DNS-Namen zuzuordnen. Angenommen, ordnen Sie www.contoso.com den Datenverkehr Manager Profil DNS-Namen contoso.trafficmanager.net. Darüber hinaus gibt das Datenverkehr-Manager-Profil einen zweiten CNAME-DNS um anzugeben, welcher Endpunkt der Client eine Verbindung herstellen soll.

Um dieses Problem zu umgehen, empfehlen wir eine HTTP-Umleitung zu direkten Verkehr über die naked Domänennamen zu einem anderen URL, der Datenverkehr Manager verwendet werden können. Beispielsweise können naked Domäne "contoso.com" Benutzer leiten Sie an den CNAME-Eintrag 'www.contoso.com', die auf den Datenverkehr Manager DNS Namen verweist.

Umfassende Unterstützung für naked Domänen in den Datenverkehr Manager ist in unseren Feature Rückstand erfasst. Sie können Ihre Unterstützung für dieses Feature Anforderung von [Abstimmungsschaltflächen dafür auf unsere Feedback Communitywebsite](https://feedback.azure.com/forums/217313-networking/suggestions/5485350-support-apex-naked-domains-more-seamlessly)registrieren.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Datenverkehr Manager [Endpunkt Überwachung und die automatische Failover](traffic-manager-monitoring.md).

Weitere Informationen zu den Datenverkehr Manager [Datenverkehrs-routing Methoden](traffic-manager-routing-methods.md).

<!--Image references-->
[1]: ./media/traffic-manager-how-traffic-manager-works/dns-configuration.png
[2]: ./media/traffic-manager-how-traffic-manager-works/flow.png

