<properties
   pageTitle="Inhalte Delivery Network (CDN) Anleitungen | Microsoft Azure"
   description="Anleitungen auf Inhalt Delivery Network (CDN) zum Übermitteln von hohe Bandbreite von Inhalten in Azure gehostet wird."
   services="cdn"
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/30/2016"
   ms.author="masashin"/>

# <a name="content-delivery-network-cdn-guidance"></a>Leitfaden für Content Delivery Network (CDN)

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>(Übersicht)
Microsoft Azure Content Delivery Network (CDN) bietet Entwicklern eine globale Lösung für die Bereitstellung hoher Bandbreite Inhalte, die in Azure oder einen anderen Speicherort gehostet wird. Das CDN können Sie öffentlich verfügbar von Azure Blob-Speicher, eine Webanwendung, virtuellen Computern, Anwendungsordner oder anderen HTTP-/HTTPS-Speicherort geladen Objekte zwischenspeichern. Bei strategische Speicherorte maximalen Bandbreite für die Bereitstellung von Inhalten für Benutzer bereitstellen kann der Cache CDN gehalten werden. Das CDN wird in der Regel für die Bereitstellung von statischen Inhalt wie Bilder, Stylesheets, Dokumente, Dateien, clientseitige Skripts und HTML-Seiten verwendet.

Sie können auch das CDN als Cache verwenden, für die dynamische Inhalte, wie etwa einen PDF-Bericht oder ein Diagramm auf der Grundlage der angegebenen Eingaben; Wenn Sie denselben Eingabewerten durch andere Benutzer bereitgestellt werden sollte das Ergebnis identisch sein.

Die wichtigsten Vorteile der Verwendung des CDN sind unteren Wartezeit und schnellere Zustellung von Inhalten für Benutzer ohne Rücksicht auf ihrem geographischen Standort in Bezug auf das Rechenzentrum, in dem die Anwendung gehostet wird.  

![CDN-Diagramm](./media/best-practices-cdn/CDN.png)

Mithilfe das CDN sollte auch helfen, die Last auf auf Anwendung zu verringern, da es der Verarbeitung erforderlich zugreifen und den Inhalt vorführen erlassen wird. Diese Verkürzung laden kann dazu beitragen, zum Steigern der Leistung und Skalierbarkeit der Anwendung, als auch Hostinganbieter Kosten minimieren, indem Sie die von Verarbeitungsressourcen erforderlich, um eine bestimmte Ebene der Leistung und Verfügbarkeit zu erzielen.

## <a name="how-and-why-a-cdn-is-used"></a>Wie und warum wird eine CDN verwendet.

Typische Verwendung für eine CDN umfassen:  

+ Übermitteln von statischen Ressourcen für Clientanwendungen häufig von einer Website aus. Bilder, Stylesheets, Dokumente, Dateien, clientseitige Skripts, HTML-, HTML-Fragmenten oder andere Inhalte, die der Server nicht für jede Anforderung ändern muss, können diese Ressourcen werden. Die Anwendung kann Elemente zur Laufzeit erstellen und verfügbar machen, zu dem CDN (z. B., indem Sie eine Liste mit aktuellen Schlagzeilen erstellen), aber es nicht für jede Anforderung möglich ist.

+ Übermitteln von öffentlichen statischen und freigegebene Inhalte an Geräten wie Mobiltelefonen und Tablet PCs aus. Die Anwendung selbst ist ein Webdienst, der eine API zu Clients, die auf die verschiedenen Geräte bietet. Das CDN kann auch statische Datasets (über den Webdienst) für die Clients verwenden Sie möglicherweise den die Client-Benutzeroberfläche generieren, vorführen. Beispielsweise könnten das CDN verteilen JSON oder XML-Dokumente verwendet werden.

+ Bereitstellung von gesamten Websites, die nur öffentlichen statischen Inhalt an Clients, bestehen, ohne dass eine dedizierte Ressourcen zu berechnen.

+ Streaming Videodateien an den Client bei Bedarf an. Video Vorteile aus dem niedriger Wartezeit und zuverlässigen Connectivity aus den ganzen Welt Datencentern die CDN Verbindungen anbieten verfügbar.

+ Im Allgemeinen Verbesserung die Oberfläche für die Benutzer, insbesondere ansässig alles andere als Datencenter sich die Anwendung befindet. Diese Benutzer möglicherweise höhere Wartezeit Weise beeinträchtigt werden. Ein großer Anteil die Gesamtgröße des Inhalts in einer Webanwendung ist oft statisch, und mithilfe das CDN kann dazu beitragen, dass die Leistung und generelle Benutzer auftreten, da entfällt die Anforderung zur Bereitstellung der Anwendungs auf mehrere Daten Centers verwalten.

+ Behandeln von Applications wachsende Auslastung, die Lösungen IoT (Internet der Dinge) unterstützen. Die große Anzahl solcher Geräte und die Einheiten überlastet ist erforderlich zum Verarbeiten von übertragenen Nachrichten und Verwalten von Firmware Update Verteilung direkt an jedem Gerät war einfach Anwendung.

+ Umgang mit Spitzen und Spannungsanstiege Nachfrage, ohne dass die Anwendung skalieren, vermeiden die infolge laufende Kosten erhöht. Beispielsweise, wenn ein Update für ein Betriebssystem für ein Gerät, beispielsweise ein bestimmtes Modell des Routers oder für einen Consumer-Gerät, beispielsweise ein Smartphone freigegeben ist, kann eine große Höchstwert in Demand während des Downloads von Millionen von Benutzern und Geräten über einen kurzen Zeitraum.

Die folgende Liste zeigt Beispiele für die Median Zeit bis zum ersten Byte aus verschiedenen geografischen Standorten. Die Ziel-Web-Rolle wird auf Azure "Westen" uns bereitgestellt. Es gibt eine starke Beziehung zwischen größer steigern aufgrund der CDN und Näherung zu einem Knoten CDN. Eine vollständige Liste der Speicherorte aus Azure CDN Knoten steht bei [Azure Content Delivery Network (CDN) Knoten Speicherorte](./cdn/cdn-pop-locations.md).


|| Zeit (ms) bis zum ersten Byte (Ursprung) | Zeit (ms) zum ersten (CDN) |Verbesserung von % CDN Zeit|
|-------------|------------------------|--------------------|------------------|
|\*SAN JOSÉ|  47.5                  | 46.5               |         2 %  |
|\*\*Dulles, VA|     109   |     40,5   |       169 % |
|Buenos Aires, AR| 210 | 151 | 39 %|
|\*London, Großbritannien| 195   | 44 | 343 %|
|Shanghai, CN| 242  | 206    | 17 %   |
|\*Singapur | 214    | 74       | 189 %  |
|\*Tokio, JP  |  163 |    48    |     204 %   |
|Seoul, KR|  190      |    190    |     0 %   |


\*Verfügt über einen Azure CDN-Knoten in derselben Stadt ein.  
\*\*Verfügt über einen Azure CDN-Knoten in einer benachbarten Stadt ein.  

## <a name="challenges"></a>Probleme  

Es gibt mehrere Probleme in Betracht ziehen, bei der Planung das CDN verwenden:  

+ **Bereitstellung**. Sie müssen entscheiden, den Ursprung aus dem wird das CDN den Inhalt abrufen und Sie müssen, ob den Inhalt in mehrere Speichersystem (wie in der CDN und einen anderen Speicherort) bereitstellen.

  Ihrer Anwendung Bereitstellung Verfahren muss das Verfahren zum Bereitstellen von statischen Inhalt und Ressourcen sowie zum Bereitstellen von Dateien der Anwendung, wie z. B. ASPX-Seiten berücksichtigen. Möglicherweise müssen Sie beispielsweise gleichzusetzen zum Laden von Inhalten in Azure Blob-Speicher implementieren.

+ **Versionsverwaltung und Cache-Steuerelement**. Sie müssen berücksichtigen, wie Sie statischen Inhalt aktualisiert und neuere Versionen bereitstellen. Der Inhalt CDN möglicherweise [geleert](./cdn/cdn-purge-endpoint.md) Azure-Portal verwenden, wenn neuere Versionen der Ihre Bestände jederzeit verfügbar sind. Dies ist eine ähnliche Herausforderung, clientseitige Zwischenspeichern, z. B., das in einem Webbrowser auftritt, zu verwalten.

+ **Testen**. Es kann zum Testen der lokalen Ihrer Einstellung CDN beim Entwickeln und Testen der Anwendung lokal oder in einer staging-Umgebung schwierig sein.

+ **Search-Engine Optimierung (SEO)**. Inhalt, z. B. Bilder und Dokumente aus einer anderen Domäne bereitgestellt werden, wenn Sie das CDN verwenden. Dies kann einen Effekt auf SEO für diesen Inhalt haben.

+ **Content-Sicherheit**. Viele CDN-Diensten wie Azure CDN bieten derzeit keine Arten von Access-Steuerelement für den Inhalt.

+ **Client-Sicherheit**. Clients möglicherweise aus einer Umgebung eine Verbindung herstellen, die keinen Zugriff auf Ressourcen auf das CDN zulässt. Dies kann einer Umgebung Sicherheit eingeschränkt sein, die Beschränkung des Zugriffs auf nur eine Reihe von bekannten Quellen, oder eine, die Laden von Ressourcen von etwas anderem als den Seitenursprung wird verhindert, dass. Eine alternative Implementierung ist erforderlich, um diese Fälle zu behandeln.

+ **Stabilität**. Das CDN stellt einen möglichen einzelnen des Fehlers für eine Anwendung. Es hat eine niedrigere Verfügbarkeit Vereinbarung zum SERVICELEVEL als also BLOB-Speicher (die zum Übermitteln von Inhalten direkt verwendet werden kann) möglicherweise müssen Sie einen Fallbackmechanismus für kritische Inhalte Implementierung in Betracht ziehen.

  Sie können Ihre CDN Verfügbarkeit der Inhalte, die Bandbreite, die übertragene Daten, die Treffer überwachen, Cache Treffer Verhältnis und Cache Metrik vom Azure-Portal in [Echtzeit](./cdn/cdn-real-time-stats.md) und [Zusammenfassungsberichte](./cdn/cdn-analyze-usage-patterns.md).

Szenarien, in dem CDN einschließen weniger nützlich sein können:  

+ Wenn der Inhalt eine niedrige Treffer hat möglicherweise es nur mehrmals zugegriffen werden, während es gültig ist (bestimmt durch die Einstellung seiner Time-to-live). Das erste Mal, das ein Element heruntergeladen wurde, anfallen Sie zwei Transaktion Gebühren vom Ursprung zu dem CDN, und klicken Sie dann aus dem CDN an den Kunden.

+ Wenn die Daten privat, beispielsweise für großen Unternehmen oder Lieferung Kette Ökosysteme ist.


## <a name="general-guidelines-and-good-practices"></a>Allgemeine Richtlinien und gute Methoden

Verwenden das CDN ist eine gute Möglichkeit, die Belastung Ihrer Anwendung minimieren und Maximieren Verfügbarkeit und Leistung. Sie sollten die Verwendung dieser Strategie für alle entsprechenden Inhalt und alle Ressourcen, die Sie Anwendung verwendet. Beachten Sie die Punkte in den folgenden Abschnitten, beim Entwerfen Ihrer Strategie für das CDN verwenden:  


### <a name="origin"></a>Origin

Bereitstellen von Inhalt über das CDN erfordert einfach Sie einen HTTP- und HTTPS-Endpunkt angeben, den der Dienst CDN zugreifen und den Inhalt Zwischenspeichern verwendet werden.

Der Endpunkt kann einen Azure Blob-Speichercontainer angeben, der den statischen Inhalt enthält, die, den Sie durch das CDN vorführen möchten. Der Container muss als öffentlich markiert werden. Nur Blobs in einem öffentlichen Container, die öffentliche Lesezugriff verfügen über die CDN zur Verfügung.

Der Endpunkt kann einen Ordner namens **Cdn** im Stammverzeichnis der eine Anwendung berechnen Layer (beispielsweise eine Webrolle oder eines virtuellen Computers) angeben. Klicken Sie auf das CDN werden die Ergebnisse von Besprechungsanfragen für Ressourcen, einschließlich der dynamische Ressourcen wie ASPX-Seiten zwischengespeichert werden. Die minimale Cachedauer ist 300 Sekunden. Alle kürzere Frist wird verhindert, dass den Inhalt zu dem CDN bereitgestellt wird (siehe die Überschrift *Cache Control* unter Weitere Informationen).

Wenn Sie Web-Apps Azure verwenden, wird der Endpunkt, indem Sie beim Erstellen der CDN Instanz die Website auswählen auf den Stammordner der Website festgelegt. Der gesamte Inhalt für die Website wird der CDN verfügbar sein.

In den meisten Fällen bietet zeigt Ihre CDN-Endpunkt an einen Ordner innerhalb der Ebenen Berechnen der Anwendung mehr Flexibilität und Kontrolle. Beispielsweise vereinfacht es aktuelle und zukünftige Weiterleitung Anforderungen verwalten und statischen Inhalt wie Miniaturansichten dynamisch zu generieren.

[Abfragezeichenfolgen](./cdn/cdn-query-string.md) können Sie Objekte im Cache zu unterscheiden, beim Übermitteln von Inhalten von dynamischen Quellen wie ASPX-Seiten ist. Allerdings kann dieses Verhalten durch deaktiviert werden eine Einstellung im Portal Azure, wenn Sie den CDN-Endpunkt angeben. Bei der Übermittlung von Inhalten aus Blob-Speicher werden Abfragezeichenfolgen als Zeichenfolgenliteralen behandelt, sodass zwei Artikel mit den gleichen Namen, jedoch andere Abfragezeichenfolgen als gesonderte Elemente auf das CDN gespeichert werden.

Sie können die URLs für Ressourcen, wie z. B. Skripts und andere Inhalte, um zu vermeiden, Verschieben von Dateien in den Ordner der CDN Origin nutzen.

Wenn Azure-Speicher Blobs mithilfe von Inhalt für die CDN zu halten, ist die URL der Ressourcen in Blobs Groß-/Kleinschreibung beachtet, für den Container und Blob.

Benutzerdefinierte Ursprung oder Azure Web Apps zu verwenden, geben Sie den Pfad zur CDN Instanz in die Links, um Ressourcen. Beispielsweise gibt Folgendes an eine Bilddatei im Ordner " **Bilder** " der Website, die über das CDN übermittelt werden können:

```XML
<img src="http://[your-cdn-endpoint].azureedge.net/Images/image.jpg" />
```

### <a name="deployment"></a>Bereitstellung

Möglicherweise müssen statischer Inhalt bereitgestellt unabhängig von der Anwendung, wenn Sie es nicht in der Anwendung Bereitstellungspaket oder Verfahren zum Einfügen und nach der Bereitstellung. Erwägen Sie wie folgt die versionsverwaltung auswirken Ansatz, mit dem Sie sowohl die Anwendungskomponenten und den Inhalt statische Ressource verwalten.

Beachten Sie, wie bündeln (kombinieren mehrere Dateien in einer Datei) und Skriptladeprogrammen (Entfernen überflüssiger Zeichen wie Leerzeichen, Zeilenumbruchzeichen für neue, Kommentare und andere Zeichen) für Skripts und CSS-Dateien verarbeitet werden. Hierbei handelt es sich um gängige Techniken, die können laden Zeiten für Clients zu verringern, und mit der Bereitstellung von Inhalten über die CDN kompatibel sind. Weitere Informationen finden Sie unter [Bundling und Skriptladeprogrammen](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification).

Wenn Sie den Inhalt einer zusätzlichen Position bereitstellen müssen, werden diese ein zusätzlicher Schritt im Bereitstellungsprozess. Wenn die Anwendung den Inhalt für die CDN aktualisiert, muss möglicherweise in regelmäßigen Abständen oder als Reaktion auf ein Ereignis, es aktualisierte Inhalte in einem beliebigen zusätzlichen Speicherorte als auch den Endpunkt für die CDN speichern.

Sie können keinen CDN Endpunkt für eine Anwendung im lokalen Azure Emulator in Visual Studio einrichten. Diese Einschränkung wirkt sich Einheit testen, Testen der Funktionalität und endgültige vor der Bereitstellung testen. Sie müssen für dieses zulassen, indem Sie einen alternativen bereit. Beispielsweise könnten Sie vorab Bereitstellen des Inhalts zu dem CDN mithilfe einer benutzerdefinierten Anwendung oder ein Programm und Testen der während des Zeitraums, in dem es zwischengespeichert sind. Verwenden Sie alternativ Kompilierung Richtlinien oder globale Konstanten zum Steuerelement aus, in dem die Ressourcen der Anwendung geladen wird. Beispielsweise wenn Debuggen-Modus ausgeführt konnte es Laden von Ressourcen wie clientseitige Skript bündeln und andere Inhalte aus einem lokalen Ordner, und verwenden das CDN aus, wenn im Release-Modus ausgeführt.

Beachten Sie, welcher Ansatz Komprimierung Ihrer CDN zur Unterstützung von soll:

+ Sie können [Komprimierung aktivieren](./cdn/cdn-improve-performance.md) , auf dem Server Origin, in dem Fall die CDN Komprimierung standardmäßig unterstützt, und komprimierte Inhalte an Clients in einem Format wie Zip oder Gzip vorführen. Bei der Verwendung eines Anwendungsordners als Endpunkt CDN möglicherweise der Server Teil des Inhalts automatisch auf die gleiche Weise wie komprimieren, wenn sie direkt mit einem Webbrowser oder andere Client vorführen. Das Format hängt von den Wert der Kopfzeile **Akzeptieren Codierung** in der Anforderung vom Client gesendet. In Azure wird standardmäßig automatisch Inhalt komprimieren, wenn CPU-Auslastung unter 50 % liegt. Wenn Sie die Anwendung hosten möchte einen Clouddienst verwenden, Ändern der Einstellungen erfordern möglicherweise mithilfe eines Vorgangs Start Komprimierung von dynamischen Ausgabe in IIS aktivieren. Weitere Informationen finden Sie unter [Aktivieren der Gzip-Komprimierung mit Microsoft Azure CDN über eine Web-Rolle](http://blogs.msdn.com/b/avkashchauhan/archive/2012/03/05/enableing-gzip-compression-with-windows-azure-cdn-through-web-role.aspx) .

+ Sie können Komprimierung direkt auf CDN Kante-Servern aktivieren, in denen Fall die CDN Komprimieren der Dateien, und es Endbenutzern dienen. Weitere Informationen finden Sie unter [Azure CDN Komprimierung](./cdn/cdn-improve-performance.md).

### <a name="routing-and-versioning"></a>Routing und versionsverwaltung

Möglicherweise müssen Sie verschiedene CDN Instanzen zu verschiedenen Zeiten verwenden. Angenommen, wenn Sie eine neue Version der Anwendung bereitstellen möglicherweise Sie möchten eine neue CDN verwenden und das alte CDN (gedrückt halten Inhalt in einem älteren Format) für frühere Versionen beibehalten. Wenn Sie als Inhalt Ursprung Azure Blob-Speicher verwenden, können Sie einfach einem separaten Speicherkonto oder einem separaten Container erstellen und den Endpunkt CDN darauf zeigen. Wenn Sie als Endpunkt CDN *Cdn* Stammordner innerhalb der Anwendung verwenden, können Sie URL-neuschreibung Vorgehensweisen direkte Anfragen zu einem anderen Ordner.

Verwenden Sie die Abfragezeichenfolge nicht in unterschiedlichen Versionen von der Anwendung in Links zu Ressourcen auf das CDN zu kennzeichnen, da beim Abrufen von Inhalten aus Azure Blob-Speicher, die Abfragezeichenfolge Teil des Ressourcennamens (der Blob-Name) ist ein. Dieser Ansatz kann auch beeinflussen, wie Ressourcen auf dem Client wird zwischengespeichert.

Neue Versionen von statischen Inhalt bereitstellen, wenn Sie eine Anwendung aktualisieren kann eine Herausforderung sein, wenn die vorherigen Ressourcen auf das CDN zwischengespeichert werden. Weitere Informationen finden Sie im Abschnitt *Cache Control*).

Erwägen Sie das CDN Inhalten Zugriff nach Land einschränken. Azure CDN ermöglicht Ihnen, je nach Land vom Ursprungspunkt Besprechungsanfragen filtern und Einschränken der Inhalts geleitet. Weitere Informationen finden Sie unter [Einschränken des Zugriffs auf Ihre Inhalte nach Ländern](./cdn/cdn-restrict-access-by-country.md).

###<a name="cache-control"></a>Cache-Steuerelement

Erwägen Sie zum Verwalten von innerhalb des Systems Zwischenspeichern. Beispielsweise können bei Verwendung eines Ordners als Ursprung CDN Sie die Cacheability Seiten angeben, die den Inhalt und die Inhalte Ablaufzeit für alle Ressourcen, die in einem bestimmten Ordner zu generieren. Sie können auch Cacheeigenschaften für das CDN und für die Verwendung standardmäßiger HTTP-Header Client angeben. Obwohl Sie sollte bereits verwalten werden auf dem Server Zwischenspeichern und Client, mit dem CDN hilft um zu Sie weitere aufmerksam zu machen, wie Ihre Inhalte zwischengespeichert wird und des Speicherorts für.

Um zu verhindern, dass Objekte verfügbar ist, klicken Sie auf das CDN können Sie diese aus den Ursprung löschen (BLOB-Container oder einer Anwendung *Cdn* Stammordner), entfernen oder löschen Sie den Endpunkt CDN oder, im Falle von Blob-Speicher, stellen Sie den Container oder BLOB-private. Allerdings werden Elemente aus dem CDN entfernt werden nur, wenn ihre Time-to-live läuft ab. Wenn keine Ablauf Cachedauer angegeben ist (z. B., wenn der Inhalt von Blob-Speicher geladen wird), wird es von 7 Tagen auf das CDN zwischengespeichert werden.  Sie können auch manuell [einen Endpunkt CDN bereinigen](./cdn/cdn-purge-endpoint.md).

In einer Webanwendung können Sie mithilfe des *ClientCache* Elements im Abschnitt *system.webServer/staticContent* der Webkonfigurationsdatei Zwischenspeichern und Ablauf für den gesamten Inhalt festlegen. Denken Sie daran, dass Sie eine web.config-Datei in einem Ordner ablegen die Dateien in diesem Ordner und alle Unterordner auswirkt.

Wenn Sie den Inhalt für die CDN dynamisch (in Ihrer Anwendungscode beispielsweise) erstellt haben, stellen Sie sicher, dass Sie die Eigenschaft *Cache.SetExpires* auf jeder Seite angeben. Das CDN wird nicht die Ausgabe von Seiten im cache, in denen die Standardeinstellung für die Cacheability der *öffentlichen*verwendet.  Legen Sie die Gültigkeitsdauer Cache auf einen geeigneten Wert, um sicherzustellen, dass der Inhalt nicht verworfen und aus der Anwendung in sehr kurzen Intervallen erneut geladen.  

### <a name="security"></a>Sicherheit

Das CDN kann vorführen Inhalt über HTTPS (SSL) verwenden das Zertifikat, das von der CDN bereitgestellt ist, es aber auch verfügbar über HTTP auch. Sie können keine HTTP-Zugriff auf Elemente in der CDN blockieren. Möglicherweise müssen Sie die Verwendung von HTTPS statischen Inhalt anfordern, der in Seiten geladen über HTTPS (beispielsweise ein Warenkorb) zur Vermeidung von Browser Warnungen zu gemischter Inhalt angezeigt wird.

Das Azure CDN bietet keine Funktionen für die Steuerung des Zugriffs auf Sichern des Zugriffs auf Inhalte. Sie können nicht mit dem CDN freigegeben Access Signaturen (SAS) verwenden.

Wenn Sie clientseitige Skripts mithilfe das CDN vorführen, können Probleme auftreten, wenn diese Skripts einen Videoanruf *XMLHttpRequest* verwenden, um HTTP-Anfragen für andere Ressourcen wie Daten, Bilder oder Schriftarten in einer anderen Domäne zu gestalten. Viele Webbrowser verhindern, dass Cross-Origin-Ressource (CORS) freigeben, es sei denn, Sie legen Sie die entsprechende Antwort Kopfzeilen Webserver konfiguriert ist. Sie können das CDN zur Unterstützung von CORS konfigurieren:

+ Ist der Ursprung, aus dem Sie Inhalte liefern, Azure Blob-Speicher, können Sie den Dienst Eigenschaften eines *CorsRule* hinzufügen. Die Regel kann die zulässige Ursprung für Besprechungsanfragen, die zulässigen Methoden, wie z. B. GET und das Alter der maximalen CORS in Sekunden für die Regel (den Zeitraum, in dem der Client verknüpften Ressourcen anfordern muss, nach dem Laden des ursprünglichen Inhalts) angeben. Weitere Informationen finden Sie unter [Cross-Origin Ressource freigeben (CORS) Unterstützung für die Azure-Speicher-Dienste](http://msdn.microsoft.com/library/azure/dn535601.aspx).

+ Wenn Sie einen Ordner in der Anwendung, wie z. B. Stammordner *Cdn* , aus denen Sie Inhalte liefern, stammt, können Sie ausgehende Regeln in der Konfigurationsdatei der Anwendung eine *Access Steuerelement zulassen Origin* Kopfzeile auf allen Antworten festlegen konfigurieren. Weitere Informationen zum erneuten Schreiben von Regeln verwenden finden Sie unter [URL-Modul neu zu schreiben](http://www.iis.net/learn/extensions/url-rewrite-module).

### <a name="custom-domains"></a>Benutzerdefinierte Domänen

Das Azure CDN können Sie angeben, ein [benutzerdefinierter Domänenname](./cdn/cdn-map-content-to-custom-domain.md) , und es zum Zugriff auf Ressourcen über das CDN. Sie können auch einen benutzerdefinierten Unterdomänennamen verwenden einen *CNAME* -Eintrag in der DNS-Einträge einrichten. Mit diesem Ansatz können Sie eine zusätzliche Sicherheitsebene Abstraktion und Steuerelement bereitstellen.

Wenn Sie einen *CNAME*verwenden, können Sie SSL verwenden, da das CDN ein eigenen einzelne SSL-Zertifikat verwendet, und dieses Zertifikat nicht den Namen Ihrer benutzerdefinierten Domäne subdomain Record entspricht.

### <a name="cdn-fallback"></a>Alternative CDN

Sie sollten in Betracht ziehen, wie eine Anwendung eines Fehlers oder vorübergehenden Ausfall des der CDN bewältigen wird. Clientanwendungen möglicherweise Kopien der Ressourcen verwendet, die lokal (auf dem Client) bei einer vorherigen Anforderung zwischengespeichert wurden, oder Sie können den Code, der Fehler erkannt und stattdessen Ressourcen anfordert, vom Ursprung (die Anwendungsordner oder Azure Blob Container, auf die Ressourcen) einbeziehen, wenn das CDN nicht verfügbar ist.

### <a name="search-engine-optimisation"></a>Optimierung der Suche-engine

Wenn SEO ein wichtiges Kriterium in der Anwendung ist, führen Sie die folgenden Aufgaben:

+ Eine kanonische *Rel* -Kopfzeile in jeder Seite oder Ressourcen hinzufügen.

+ Verwenden Sie einen *CNAME* -Eintrag Unterdomäne und Zugriff auf die Ressourcen, die mit diesem Namen.

+ Erwägen Sie den Einfluss der, dass die IP-Adresse der CDN werden möglicherweise ein Land oder Region, die von der Anwendung selbst abweicht.

+ Bei Verwendung von Azure Blob-Speicher als Ursprung verwalten Sie die gleiche Dateistruktur für Ressourcen auf das CDN wie die Anwendungsordner.


### <a name="monitoring-and-logging"></a>Für die Überwachung und Protokollierung

Schließen Sie das CDN als Teil Ihrer Anwendung Überwachung Strategie zum Erkennen und Fehlern messen ein oder erweiterten Sie Wartezeit vorkommen.  Überwachung ist aus dem CDN Profil-Manager befindet sich auf der Portalseite Azure-Website verfügbar

Aktivieren Sie der Protokollierung für das CDN und überwachen Sie dieses Protokoll als Teil Ihrer täglichen.

Erwägen Sie das CDN Datenfluss für die von Verwendungsmustern zu analysieren. Azure-Portal bietet Tools, mit die Sie überwachen können:
+ Bandbreite,
+ Daten übertragen
+ Treffer (Statuscodes)
+ Cachestatus
+ Cache TREFFER Ratio und
+ Verhältnis zwischen der IPV4/IPV6 anfordert.

Weitere Informationen finden Sie unter [Analysieren CDN Verwendungsmustern](./cdn/cdn-analyze-usage-patterns.md).

### <a name="cost-implications"></a>Auswirkungen der Kosten

Sie unterliegen für ausgehende Datenübertragung aus dem CDN ab.  Darüber hinaus, wenn Sie Ihre Bestände jederzeit hosten Blob-Speicher verwenden, Sie für Speichertransaktionen unterliegen beim Laden des CDN der Daten aus der Anwendung. Sie sollten realistische Cache Ablauf Perioden für Inhalt, um die Aktualität sicherzustellen, aber nicht so kurz, dass entstehen wiederholten Neuladen des Inhalts aus der Anwendung festlegen oder BLOB-Speicher zu dem CDN.

Elemente, die sich selten heruntergeladen wurden werden die zwei Transaktion anfallen ohne jede erheblichen Verkürzung Server laden.

### <a name="bundling-and-minification"></a>Bündeln und Skriptladeprogrammen

Verwenden von bündeln und Skriptladeprogrammen verringern die Größe von Ressourcen wie JavaScript-Code und HTML-Seiten in der CDN gespeichert. Diese Strategie kann dazu beitragen reduzieren die Verarbeitungszeit für diese Elemente auf dem Client herunterzuladen.

Bundling und Skriptladeprogrammen können von ASP.NET behandelt werden. In einem Projekt MVC definieren Sie Ihre Pakete in *BundleConfig.cs*. Ein Verweis auf die verkleinerte Skript-Paket wird erstellt, durch die Methode *Script.Render* in der Regel in Code in der Ansichtsklasse aufrufen. Diese Referenz enthält eine Abfragezeichenfolge, die einen Hash enthält, die auf den Inhalt der das Paket basiert. Wenn das Paket Inhalt ändern, wird auch der generierte Hash geändert.  

Standardmäßig haben Azure CDN Instanzen die *Abfrage Zeichenfolge Status* Einstellung deaktiviert. Aktualisierte Skript Bündeln von der CDN ordnungsgemäß behandelt werden sollen müssen Sie die Einstellung *Abfrage Zeichenfolge Status* für die Instanz CDN aktivieren. Beachten Sie, dass es mindestens eine Stunde dauern kann, bevor Sie die Einstellung wirksam wird.


## <a name="example-code"></a>Beispielcode
Dieser Abschnitt enthält einige Beispiele für Codes und Techniken für die Arbeit mit dem CDN an.  


### <a name="url-rewriting"></a>URL-neuschreibung
Im folgenden Ausschnitt aus einer Datei Web.config im Stammverzeichnis der eine Anwendung Cloud Services gehostet wird veranschaulicht, wie [URL-neuschreibung](https://technet.microsoft.com/library/ee215194.aspx) ausführen, wenn das CDN verwenden. Anfragen der CDN für Inhalte, die zwischengespeichert wird werden auf bestimmte Ordner im Stammverzeichnis Anwendung, basierend auf den Typ der Ressource (z. B. Skripts und Bilder) geleitet.  


```XML
<system.webServer>
  ...
  <rewrite>
    <rules>
      <rule name="VersionedResource" stopProcessing="false">
        <match url="(.*)_v(.*)\.(.*)" ignoreCase="true" />
        <action type="Rewrite" url="{R:1}.{R:3}" appendQueryString="true" />
      </rule>
      <rule name="CdnImages" stopProcessing="true">
        <match url="cdn/Images/(.*)" ignoreCase="true" />
        <action type="Rewrite" url="/Images/{R:1}" appendQueryString="true" />
      </rule>
      <rule name="CdnContent" stopProcessing="true">
        <match url="cdn/Content/(.*)" ignoreCase="true" />
        <action type="Rewrite" url="/Content/{R:1}" appendQueryString="true" />
      </rule>
      <rule name="CdnScript" stopProcessing="true">
        <match url="cdn/Scripts/(.*)" ignoreCase="true" />
        <action type="Rewrite" url="/Scripts/{R:1}" appendQueryString="true" />
      </rule>
      <rule name="CdnScriptBundles" stopProcessing="true">
        <match url="cdn/bundles/(.*)" ignoreCase="true" />
        <action type="Rewrite" url="/bundles/{R:1}" appendQueryString="true" />
      </rule>
    </rules>
  </rewrite>
  ...
</system.webServer>
```

Schreiben Sie diese Regeln führen Sie die folgenden Umleitung:

+ Die erste Regel ermöglicht das Einbetten von einer Version in den Dateinamen, der eine Ressource, der dann ignoriert wird. *Filename_v123.jpg *wird beispielsweise als *Dateiname.jpg*neu geschrieben.

+ Die folgenden vier Regeln zeigen, wie Besprechungsanfragen umgeleitet werden, wenn Sie nicht, speichern die Ressourcen in einem Ordner namens *Cdn* * möchten im Stammverzeichnis der Webrolle. Ordnen Sie die Regeln der *Cdn/Bilder*; **; *Cdn/Inhalt Cdn/Skripts*, und *Cdn / bündeln * URLs auf ihre jeweiligen Root-Ordner in die Web-Rolle.

Beachten Sie, dass Sie Ihre Änderungen vorgenommen haben, um die Bündeln von Ressourcen erfordert URLs verwenden.   

## <a name="more-information"></a>Weitere Informationen


+ [Azure CDN](https://azure.microsoft.com/services/cdn/)
+ [Dokumentation Azure Content Delievery Network (CDN)](https://azure.microsoft.com/documentation/services/cdn/)
+ [Verwenden von Azure CDN](./cdn/cdn-create-new-endpoint.md)
+ [Integrieren von einem Cloud-Dienst mit Azure CDN](./cdn/cdn-cloud-service-with-cdn.md)
+ [Bewährte Methoden für das Microsoft Azure-Netzwerk Bereitstellung von Inhalten](https://azure.microsoft.com/blog/2011/03/18/best-practices-for-the-windows-azure-content-delivery-network/)
