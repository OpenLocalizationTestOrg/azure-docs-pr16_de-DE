<properties
    pageTitle="Leistungsanalyse Kante in Azure CDN | Microsoft Azure"
    description="Leistungsanalyse Kante Knoten in Microsoft Azure CDN. Kante Leistung Analytics wird die Verwendung der detaillierte Informationen den Datenverkehr und die Bandbreite für das CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Kante Knoten Leistungsanalyse in Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>(Übersicht)
Kante Leistung Analytics wird die Verwendung der detaillierte Informationen den Datenverkehr und die Bandbreite für das CDN. Diese Informationen kann dann verwendet werden, beliebte Statistiken, zu generieren, denen Sie erhalten Sie Einblick in wie Ihre Bestände jederzeit werden werden können zwischengespeichert und an den Kunden geliefert. Wiederum, können Sie eine Strategie zum Optimieren der Übermittlung von Inhalten bilden und zum bestimmen, welche Probleme sein sollen angegangen bessere Nutzung das CDN. Daher nicht nur werden Sie für optimale Leistung mit Daten Übermittlung können, sondern auch Ihre CDN Verwaltungskosten werden.

> [AZURE.NOTE] Alle Berichte verwenden UTC/GMT Notation aus, wenn einen Uhrzeit angeben.

## <a name="reports-and-log-collection"></a>Log-Sammlung und Berichte

CDN Aktivitätsdaten müssen vom Modul Kante Leistung Analytics gesammelt werden, bevor sie Berichte daran erstellen kann. Dieses Verfahren Websitesammlung tritt auf, nachdem Sie einen Tag und es behandelt die Aktivität, die während des vorherigen Tages stattgefunden. Dies bedeutet, die einen Bericht Statistiken darstellen eine Stichprobe von Statistiken des Tages gleichzeitig erkannt wurde verarbeitet und nicht zu empfehlen unbedingt enthalten die vollständige Menge der Daten für den aktuellen Tag. Die primäre Funktion dieser Berichte ist zum Bewerten der Leistung. Sie sollten nicht für Abrechnung Zwecke oder genauen numerischen Statistiken verwendet werden.

> [AZURE.NOTE] Die unformatierten Daten aus dem Kante Leistung analytisches Berichte generiert werden, ist mindestens 90 Tage lang verfügbar.

## <a name="dashboard"></a>Dashboard

Das Kante Leistung Analytics Dashboard verfolgt aktuelle und zurückliegende CDN-Verkehr über ein Diagramm und Statistik. Verwenden Sie dieses Dashboard, um kürzlich geführten und langfristiges Trends auf die Leistung des CDN Datenverkehr für Ihr Konto zu erkennen.

In diesem Dashboard besteht aus:

* Ein interaktives Diagramm, das die Visualisierung von wichtigen Kriterien und Trends ermöglicht.
* Eine Zeitachse, die einen Eindruck davon, langfristig Muster für Sie wichtigen Kriterien und Trends enthält.
* Wichtigen Kriterien und statistische Informationen wird unser Netzwerk CDN Website Datenverkehr gemessen nach allgemeine Leistung, Verwendung und Effizienz verbessert.

### <a name="accessing-the-edge-performance-dashboard"></a>Zugreifen auf die Kante Leistungsdashboard

1. Klicken Sie aus dem CDN Profil Blade auf die Schaltfläche **Verwalten** .

    ![Schaltfläche zum Verwalten von CDN Profil blade](./media/cdn-edge-performance/cdn-manage-btn.png)

    Verwaltungsportal CDN wird geöffnet.

2. Zeigen Sie auf der Registerkarte **Analytics** , und zeigen Sie auf den **Rand Leistung Analytics** Flyout.  Klicken Sie auf **Dashboard**.

    Das Kante Knoten Analytics Dashboard wird angezeigt.

### <a name="chart"></a>Diagramm

Das Dashboard enthält ein Diagramm, das eine Metrik nachverfolgt, über den Zeitraum aktiviert in die Zeitachse, die direkt unterhalb dieses angezeigt wird.  Eine Zeitachse, Diagramme zur letzten zwei Jahren CDN Tätigkeit nach oben wird direkt unterhalb des Diagramms angezeigt.

#### <a name="using-the-chart"></a>Mithilfe des Diagramms

* Standardmäßig wird der Cache Effizienz Abschlag (Disagio) der letzten 30 Tage im Diagramm angezeigt werden.
* Ein Diagramm dieses Typs wird aus Daten sortiert täglich generiert.
* Auf einem Tag auf das Liniendiagramm Auswahlspalte wird ein Datum und der Wert der Metrik zu diesem Zeitpunkt anzugeben.
* Klicken Sie auf Hervorheben der Wochenenden eine Überlagerung der helle graue vertikale Balken ein-und ausblenden, die Wochenenden auf dem Diagramm darstellen. Diese Art von Überlagerung eignet sich für Muster im Datenverkehr über der Wochenenden identifizieren.
* Klicken Sie auf Ansicht ein Jahr vor eine Überlagerung der Aktivität des Vorjahrs im gleichen Zeitraum in das Diagramm ein-und ausblenden. Diese Art von Vergleich bietet einen Einblick in langfristiges CDN Verwendungsmustern. Die oberen rechten Ecke des Diagramms enthält eine Legende, die das für jede Liniendiagramm Farbe angibt.

#### <a name="updating-the-chart"></a>Aktualisieren des Diagramms

* Zeitbereich: Führen Sie eine der folgenden Aktionen aus:
    * Wählen Sie die gewünschte Region in die Zeitachse aus. Das Diagramm wird mit Daten aktualisiert werden, die den ausgewählten Zeitraum entspricht.
    * Doppelklicken Sie auf das Diagramm, um alle verfügbaren zurückliegende Daten bis zu einem Maximum von zwei Jahren anzeigen.
* Maßstab: Klicken Sie auf das Diagramm-Symbol, das neben der gewünschten Metrik angezeigt wird. Das Diagramm und die Zeitachse werden mit Daten für die entsprechenden Metrik aktualisiert werden.


### <a name="key-metrics-and-statistics"></a>Wichtigen Kriterien und Statistik

#### <a name="efficiency-metrics"></a>Effizienz Kennzahlen

Der Zweck des folgenden Kennzahlen ist zu sehen, ob die Cache-Effizienz verbessert werden kann. Die Hauptvorteile von Cache-Effizienz abgeleitet werden:

* Reduzierte Last auf dem Ausgangsserver die zu führen können:
    * Bessere Leistung für die Web-Server.
    * Reduzierte Betrieb Kosten.
* Verbesserte Daten Übermittlung Beschleunigung, da mehr Anfragen direkt aus dem CDN bereitgestellt werden.

Feld | Beschreibung
------|------------
Cache-Effizienz | Gibt den Prozentsatz der übertragenen Daten, die aus Cache bereitgestellt wurde. Diese metrischen Maßnahmen, wenn eine zwischengespeicherte Version des angeforderten Inhalts direkt aus dem CDN (Kante Servers) zum Anfordern (z. B. Webbrowser) bereitgestellt wurde
Drücken Sie Zins | Gibt den Prozentsatz der Anfragen an, aus dem Cache wurden. Diese metrischen Maßnahmen, wenn eine zwischengespeicherte Version des angeforderten Inhalts wurde direkt aus dem CDN (Kante Servers) zum Anfordern (z. B. Webbrowser) bereitgestellt.
% des Remote-Byte - keine Config Cache | Gibt den Prozentsatz des Datenverkehrs, der von Origin-Servern zu dem CDN (Kante Servers) gesendet wurde, die als Ergebnis der umgehen-Cache-Funktion (HTTP-Regeln-Engine) nicht zwischengespeichert werden.
% des Remote-Byte - abgelaufen Cache | Gibt den Prozentsatz der Datenverkehr, der von Origin-Servern, zu dem CDN (Kante Servers gesendet wurde) als Ergebnis veralteten Inhalten erneuten an.

#### <a name="usage-metrics"></a>Verwendung von Kriterien

Der Zweck des folgenden Kennzahlen stellen Sie einen Einblick in die folgenden Kosten-Ausschneiden Maßnahmen bereit:

* Minimieren Betrieb Kosten durch das CDN.
* Verringern die CDN Ausgaben durch Cache Effizienz und Komprimierung.

> [AZURE.NOTE] Datenverkehr Lautstärke Zahlen darstellen Datenverkehr, der in die Berechnung des Verhältnissen und Prozentsätzen verwendet wurde, und zeigt möglicherweise nur einen Teil des gesamten Datenverkehrs für große Kunden.

Feld | Beschreibung
------|------------
Speichern von Bytes | Gibt die durchschnittliche Anzahl von Bytes übertragen für jede Anforderung served aus dem CDN (Kante Servers) in der Auftraggeber (z. B. Webbrowser) an.
Keine Cache Config Byte Zins | Gibt den Prozentsatz des Datenverkehrs served aus dem CDN (Kante Servers) zu enthaltenen (z. B. Webbrowser), die aufgrund der Umgehung-Cache-Funktion nicht zwischengespeichert werden.
Komprimierte Byte Zins | Gibt den Prozentsatz der Datenverkehr aus dem CDN (Kante Servers) zum Anfordern (z. B. Webbrowser) in einem komprimierten Format an.
Bytes ausgehend | Zeigt an, der Datenmenge in Bytes, die aus dem CDN (Kante Server) an (z. B. Webbrowser) enthaltenen übermittelt wurden.  
Bytes In | Zeigt an, die Datenmenge in Bytes von anfordern (z. B. Webbrowser) gesendet zu dem CDN (Kante Servers).
Byte-Fernbedienung | Zeigt die Datenmenge in Bytes, die von CDN und Kunden Origin-Servern gesendet werden, zu dem CDN (Kante Server) an.

#### <a name="performance-metrics"></a>Leistungswerte

Diese Kennzahlen dient zum Nachverfolgen von CDN Gesamtsystemleistung für Ihre Datenverkehr.

Feld | Beschreibung
------|------------
Zins übertragen | Gibt die durchschnittliche Rate an Inhalt aus dem CDN an übertragen wurde.
Dauer | Gibt die durchschnittliche Zeitspanne Host in Millisekunden, es an eine Anlage an (z. B. Webbrowser) vorführen.
Komprimierte Anforderung Zins | Gibt den Prozentsatz der Treffer, die aus dem CDN (Kante Server) an (z. B. Webbrowser) enthaltenen übermittelt wurden in einem komprimierten Format an.
4xx Fehler Zins | Gibt den Prozentsatz der Treffer, die ein 4xx Statuscode generiert.
5xx Fehler Zins | Gibt den Prozentsatz der Treffer, die ein 5xx Statuscode generiert.
Treffer | Gibt die Anzahl der Anfragen nach Inhalten CDN an.

#### <a name="secure-traffic-metrics"></a>Sicheren Datenverkehr Kennzahlen

Diese Kennzahlen dient zum Nachverfolgen von CDN Leistung für HTTPS-Datenverkehr.

Feld | Beschreibung
------|------------
Secure Cache-Effizienz | Gibt den Prozentsatz der für HTTPS-Anfragen an wurden, aus dem Cache übertragene Daten an. Diese metrischen Maßnahmen, wenn eine zwischengespeicherte Version des angeforderten Inhalts wurde direkt aus dem CDN (Kante Servers) zum Anfordern (z. B. Webbrowser) über bereitgestellt HTTPS.
Sichere Datenübertragung Zins | Gibt die durchschnittliche Rate Inhalt aus dem CDN (Kante Servers) zum Anfordern (z. B. Webserver) übertragen wurde über HTTPS an.
Durchschnittliche Secure Dauer | Gibt die durchschnittliche Zeitspanne Host in Millisekunden, es an eine Anlage an (z. B. Webbrowser) über HTTPS vorführen.
Secure Treffer | Gibt die Anzahl der HTTPS-Anfragen nach Inhalten CDN an.
Secure Bytes ausgehend | Gibt die Menge der HTTPS-Datenverkehr in Bytes, die aus dem CDN (Kante Server) an (z. B. Webbrowser) enthaltenen übermittelt wurden.

## <a name="reports"></a>Berichte

Jeder Bericht in diesem Modul enthält ein Diagramm und statistische Daten zu Bandbreite und den Datenverkehr Verwendung für unterschiedliche Arten von Kennzahlen (z. B. HTTP Statuscodes, Cache Statuscodes, anfordern URL usw..). Diese Informationen können verwendet werden, stürzen tieferen wie Inhalte an Ihre Kunden bereitgestellt werden und CDN Verhalten zum Verbessern der Leistung von Daten Übermittlung zu optimieren.

### <a name="accessing-the-edge-performance-reports"></a>Zugreifen auf die Kante Performance-Berichte

1. Klicken Sie aus dem CDN Profil Blade auf die Schaltfläche **Verwalten** .

    ![Schaltfläche zum Verwalten von CDN Profil blade](./media/cdn-edge-performance/cdn-manage-btn.png)

    Verwaltungsportal CDN wird geöffnet.

2. Zeigen Sie auf der Registerkarte **Analytics** , und zeigen Sie auf den **Rand Leistung Analytics** Flyout.  Klicken Sie auf **http-große Objekt**.

    Bildschirm Kante Knoten Analytics Berichte wird angezeigt.

Bericht | Beschreibung
-------|------------
Tägliche Zusammenfassung | Ermöglicht das tägliche Datenverkehr Trends über einen angegebenen Zeitraum anzeigen. Jeder Balken in diesem Diagramm steht für ein bestimmtes Datum. Die Größe des Balkens zeigt an, die Gesamtzahl der Treffer, die zu diesem Zeitpunkt aufgetreten sind.
Zusammenfassung pro Stunde | Können Sie stündlich Datenverkehr Trends über einen angegebenen Zeitraum anzeigen. Jeder Balken in diesem Diagramm steht für eine einzelne Stunde an einem bestimmten Datum. Die Größe des Balkens zeigt an, die Gesamtzahl der Treffer, die in dieser Stunde aufgetreten sind.
Protokolle | Zeigt den Datenverkehr zwischen die Protokolle HTTP und HTTPS-Strukturplan an. Ringdiagramm zeigt an, den Prozentsatz der Treffer, die für jede Art von Protokoll aufgetreten sind.
HTTP-Methoden | Können Sie schnelle eine Vorstellung zu erhalten, welche HTTP-Methoden zum Anfordern von Daten verwendet werden. Üblicherweise sind die am häufigsten verwendeten Methoden der HTTP-Anforderung abrufen, Kopf und bereitstellen. Ringdiagramm zeigt an, den Prozentsatz der Treffer, die für jeden HTTP-Anforderungsmethode aufgetreten sind.
URLs | Enthält ein Diagramm, in dem die obersten 10 angeforderten URLs angezeigt. Für jede URL ist ein Balken angezeigt. Die Höhe des Balkens zeigt an, wie viele Zugriffe, die bestimmte URL, in dem Zeitraum generiert hat vom Bericht abgedeckt. Statistik für die ersten 100 angeforderten URLs direkt unterhalb dieses Diagramm angezeigt werden.
CNAMEs | Enthält ein Diagramm, das im oberen Bereich zeigt 10 CNAMEs zum Posten im Zeitverlauf Anfordern eines Berichts umfassen. Statistik für die ersten 100 angeforderte CNAMEs direkt unterhalb dieses Diagramm angezeigt werden.
Ursprung | Enthält ein Diagramm, in dem die obersten 10 CDN angezeigt oder Kunden Origin-Servern aus denen Vermögenswerte über einen angegebenen Zeitraum angefordert wurden. Statistik für die ersten 100 angeforderte CDN oder Kunden Origin-Servern direkt unterhalb dieses Diagramm angezeigt werden. Nach dem Namen in die Option Directory Name definiert werden Kunden Origin-Servern identifiziert.
Geo wird | Zeigt an, wie viel von Ihren Verkehr über einen bestimmten Punkt-of-Presence (POP) weitergeleitet wird. Die Abkürzung aus drei Buchstaben stellt einen POP in unserem Netzwerk CDN.
Clients | Enthält ein Diagramm, die den Top 10-Benutzer angezeigt werden, die über einen angegebenen Zeitraum Posten angefordert hat. Im Sinne dieses Berichts gelten alle Besprechungsanfragen, die aus derselben IP-Adresse stammen vom selben Client werden. Statistiken für die obersten 100 Kunden werden direkt unterhalb dieses Diagramm angezeigt. Dieser Bericht eignet sich zum Download Aktivität Muster für die obersten Clients ermitteln.
Cache Status | Bietet eine ausführliche Analyse der Cacheverhalten, die Vorgehensweisen für insgesamt durch den Endbenutzer verbessern Offenlegen kann. Da-Cache-Treffer die schnellste Leistung kommen, können Sie Daten für die Übermittlung Geschwindigkeiten durch Minimieren-Cache-Fehler und abgelaufenen Cachetreffer optimieren.
KEINE Details | Enthält ein Diagramm, in dem die obersten 10 URLs für Anlagen angezeigt, für die Aktualität des Inhalts verbessert Cache nicht über einen angegebenen Zeitraum aktiviert wurde. Statistiken für die obersten 100 URLs für diese Arten von Elementen werden direkt unterhalb dieses Diagramm angezeigt.
CONFIG_NOCACHE-Details | Enthält ein Diagramm, in dem die Top 10-URLs für Anlagen angezeigt, die aufgrund der vom Kunden CDN Konfiguration nicht zwischengespeichert wurden. Diese Arten von Elementen wurden direkt aus dem Ausgangsserver bereitgestellt. Statistiken für die obersten 100 URLs für diese Arten von Elementen werden direkt unterhalb dieses Diagramm angezeigt.
UNCACHEABLE Details | Enthält ein Diagramm, in dem die obersten 10 URLs für Anlagen angezeigt, die aufgrund von Anforderung Kopfzeilendaten nicht zwischengespeichert werden konnte. Statistiken für die obersten 100 URLs für diese Arten von Elementen werden direkt unterhalb dieses Diagramm angezeigt.
TCP_HIT-Details | Enthält ein Diagramm, in dem die obersten 10 URLs für Anlagen angezeigt, die aus dem Cache sofort beantwortet werden. Statistiken für die obersten 100 URLs für diese Arten von Elementen werden direkt unterhalb dieses Diagramm angezeigt.
TCP_MISS-Details | Enthält ein Diagramm, in dem die obersten 10 URLs für Anlagen angezeigt, die den Cachestatus TCP_MISS aufweisen. Statistiken für die obersten 100 URLs für diese Arten von Elementen werden direkt unterhalb dieses Diagramm angezeigt.
TCP_EXPIRED_HIT-Details | Enthält ein Diagramm, in dem die obersten 10 URLs für veraltete Anlagen angezeigt, die direkt aus dem POP bereitgestellt wurden. Statistiken für die obersten 100 URLs für diese Arten von Elementen werden direkt unterhalb dieses Diagramm angezeigt.
TCP_EXPIRED_MISS-Details | Enthält ein Diagramm, die die obersten 10 URLs für veraltete Anlagen angezeigt werden, für die eine neue Version hatten, aus dem Ausgangsserver abgerufen werden soll. Statistiken für die obersten 100 URLs für diese Arten von Elementen werden direkt unterhalb dieses Diagramm angezeigt.
TCP_CLIENT_REFRESH_MISS-Details | Enthält ein Balkendiagramm, die die Top 10-URLs zeigt für Anlagen aus einer Ausgangsserver aufgrund einer nicht-Cache-Anforderung vom Client abgerufen wurden. Statistiken für die obersten 100 URLs für diese Dateitypen Anfragen werden direkt unterhalb dieses Diagramm angezeigt.
Typen von Client Anforderung | Gibt den Typ der Besprechungsanfragen, die von HTTP-Clients (z. B. Browser) vorgenommen wurden. Der Bericht enthält Ringdiagramm, der bietet einen Eindruck davon, wie Besprechungsanfragen behandelt werden. Bandbreite und den Datenverkehr Informationen für jeden Anforderung wird unterhalb des Diagramms angezeigt.
Benutzer-Agents | Enthält ein Balkendiagramm mit der Top 10-Benutzer-Agents zum Anfordern von Inhalten über unsere CDN. Normalerweise ist ein Benutzer-Agents einen Webbrowser, MediaPlayer oder einen Browser auf dem Mobiltelefon. Statistiken für die obersten 100 Benutzer-Agents werden direkt unterhalb dieses Diagramm angezeigt.
Verweise | Enthält ein Balkendiagramm anzeigen der obersten 10 Verweise auf Inhalte über unsere CDN zugegriffen. Eine Referenz ist in der Regel, die URL der Webseite oder der Ressource, die mit den Inhalt verknüpft. Ausführliche Informationen wird unterhalb des Diagramms für die ersten 100 Quellen bereitgestellt.
Komprimierungstypen | Enthält ein Ringdiagramm, die untersucht angeforderten Posten, ob er von unseren Servern Kante komprimiert wurden. Der Prozentsatz der komprimierten Posten wird durch den Typ der Komprimierung verwendet aufgeschlüsselt. Detaillierte Informationen werden für jede Komprimierungstyp und Status unterhalb des Diagramms bereitgestellt.
Dateitypen | Enthält ein Balkendiagramm, in dem die obersten 10 Dateitypen angezeigt, die durch unsere CDN für Ihr Konto angefordert wurden. Im Sinne des Berichts, ein Dateityp durch Dateinamenerweiterung der Anlage definiert ist, und geben Sie die Internet-Medien (z. B. HTML \[Text/html-\], htm \[Text/html-\], .aspx \[Text/html\]usw..). Detaillierte Informationen werden für die obersten 100 Dateitypen unterhalb des Diagramms bereitgestellt.
Eindeutige Dateien | Enthält ein Diagramm, das die Gesamtzahl der eindeutigen Posten gezeichnet hat, die über einen angegebenen Zeitraum an einem bestimmten Tag angefordert wurden.
Zusammenfassung der Token-Authentifizierung | Enthält ein Kreisdiagramm, der bietet einen schnellen Überblick auf, ob die angeforderte Posten durch Token-basierte Authentifizierung geschützt wurden. Geschützte Anlagen werden im Diagramm entsprechend den Ergebnissen des deren versuchten Authentifizierung angezeigt.
Token autorisierende verweigern Details | Enthält ein Balkendiagramm, mit dem Sie die Top 10-Anfragen anzeigen möchten, die aufgrund von Token-basierter Authentifizierung verweigert wurden.
HTTP-Antwort-Codes | Bietet einen Überblick über die HTTP-Status-Codes (z. B. 200 OK 403 Verboten, 404 nicht gefunden, usw.), wurden Ihre HTTP-Clients durch unsere Kante Server übermittelt. Ein Kreisdiagramm können Sie schnell beurteilen wie Ihre Bestände jederzeit bereitgestellt wurden. Detaillierte statistische Daten ist für jeden Antwortcode unterhalb des Diagramms bereitgestellt.
404-Fehlern | Enthält ein Balkendiagramm, mit dem Sie die Top 10-Anfragen anzeigen möchten, die einen 404 nicht gefunden Antwortcode geführt haben.
403-Fehlern | Enthält ein Balkendiagramm, mit dem Sie die Top 10-Anfragen anzeigen möchten, die einen 403 Verboten Antwortcode geführt haben. Ein 403 Verboten Antwortcode tritt auf, wenn eine Anforderung nach einem Kunden Ausgangsserver oder eine Kante Server auf unseren POP abgelehnt wird.
4xx-Fehler | Enthält ein Balkendiagramm, mit dem Sie die Top 10-Anfragen anzeigen möchten, die eine Antwortcode in den Bereich 400 geführt haben. 403 aus diesem Bericht ausgeschlossen werden nicht gefunden "und" 404 verboten Antwortcodes. Normalerweise tritt ein 4xx-Code als Antwort auf, wenn eine Anforderung infolge eines Fehlers Client abgelehnt wird.
504 Fehler | Enthält ein Balkendiagramm, mit dem Sie die Top 10-Anfragen anzeigen möchten, die einen 504 Gateway-Timeout Antwortcode geführt haben. Einen 504 Gateway-Timeout Antwortcode tritt ein Timeout auftritt, wenn ein HTTP-Proxy zur Kommunikation mit einem anderen Server versucht. Im Falle unserer CDN tritt einen 504 Gateway-Timeout Antwortcode in der Regel, wenn ein Kante Server Kommunikation mit einem Kunden Ausgangsserver herstellen kann.
Fehler vom Typ 502 | Enthält ein Balkendiagramm, mit dem Sie die Top 10-Anfragen anzeigen möchten, die einen 502 Ungültiger Gateway Antwortcode geführt haben. Einen 502 Ungültiger Gateway Antwortcode tritt auf, wenn ein Fehler bei HTTP-Protokoll zwischen einem Server und HTTP-Proxy. Im Falle unserer CDN tritt ein 502 Ungültiger Gateway Antwortcode in der Regel auf, wenn eines Kunden Ausgangsserver eine ungültige Antwort auf eine Kante Server gibt. Eine Antwort ist ungültig, wenn es analysiert werden kann oder unvollständig ist.
5xx-Fehler | Enthält ein Balkendiagramm, mit dem Sie die Top 10-Anfragen anzeigen möchten, die eine Antwortcode in den Bereich 500 geführt haben.  Aus diesem Bericht ausgeschlossen werden 502 Ungültiger Gateway und 504 Gateway-Timeout Antwortcodes.

## <a name="see-also"></a>Siehe auch
* [Azure CDN (Übersicht)](cdn-overview.md)
* [In Echtzeit Stats in Microsoft Azure CDN](cdn-real-time-stats.md)
* [Verwendung der Regeln-Engine HTTP-Standardverhalten überschreiben](cdn-rules-engine.md)
* [Erweiterte HTTP-Berichte](cdn-advanced-http-reports.md)
