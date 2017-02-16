<properties 
    pageTitle="Behandeln von Problemen mit der Azure Redis Cache | Microsoft Azure" 
    description="Erfahren Sie, wie häufige Probleme mit Azure Redis Cache auflösen." 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-troubleshoot-azure-redis-cache"></a>Behandeln von Problemen mit der Azure Redis Cache

Dieser Artikel enthält Hinweise zur Behandlung von den folgenden Kategorien Azure Redis Cache Probleme.

-   [Behandeln von Client Seite](#client-side-troubleshooting) – in diesem Abschnitt bietet Richtlinien auf identifizieren und Beheben von Problemen, die durch die Anwendung Herstellen einer Verbindung mit Azure Redis Cache verursacht.
-   [Problembehandlung bei Server-Seite](#server-side-troubleshooting) – in diesem Abschnitt bietet Richtlinien zum Erkennen und Beheben von Problemen, die auf dem Server Azure Redis Cache verursacht.
-   [StackExchange.Redis Timeout Ausnahmen](#stackexchangeredis-timeout-exceptions) – in diesem Abschnitt enthält Informationen zur Behebung von Problemen bei Verwendung des Clients StackExchange.Redis.


>[AZURE.NOTE] Mehrere Schritte zur Problembehandlung in diesem Handbuch enthalten Anweisungen zum Redis Befehle ausführen und Überwachen von verschiedenen Performance-Werte. Weitere Informationen und Anweisungen finden Sie unter Artikel im Abschnitt [zusätzliche Informationen](#additional-information) .

## <a name="client-side-troubleshooting"></a>Behandeln von Client-Seite


In diesem Abschnitt werden die Behandlung von Problemen, die auftreten, wenn eine Bedingung auf der Clientanwendung.

-   [Arbeitsspeicherdruck auf dem client](#memory-pressure-on-the-client)
-   [Burst des Datenverkehrs](#burst-of-traffic)
-   [Hohe Client CPU-Auslastung](#high-client-cpu-usage)
-   [Client Seite Bandbreite überschritten](#client-side-bandwidth-exceeded)
-   [Größe des großen Anforderung/Antwort](#large-requestresponse-size)
-   [Wo befindet sich meine Daten in Redis?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-the-client"></a>Arbeitsspeicherdruck auf dem client

#### <a name="problem"></a>Problem

Arbeitsspeicherdruck auf dem Clientcomputer führt zu allen Arten von Leistungsprobleme zu vermeiden, die Verarbeitung von Daten, die durch die Instanz Redis ohne Verzögerung gesendet wurde verzögert werden können. Wenn genügend Arbeitsspeicher zur Verfügung steht, muss das System Seitendaten in der Regel von Speicher wird auf virtuellen Speichers, der auf der Festplatte befindet. Diese *Seite fehlgeschlagene* bewirkt, dass das System langsamer erheblich ab.

#### <a name="measurement"></a>Maßeinheiten 

1.  Überwachen von arbeitsspeicherauslastung auf Computer, um sicherzustellen, dass die verfügbaren Arbeitsspeicher nicht überschritten wird. 
2.  Überwachen der `Page Faults/Sec` Performance-Zähler. Die meisten Systeme werden einige Fehlern durch eine Seite auch während des normalen Betriebs haben, schauen Sie sich daher für Spitzen in dieser Seite Fehlern Leistungszähler, die mit Zeitlimit entsprechen.

#### <a name="resolution"></a>Auflösung

Aktualisieren Sie Ihren Kunden auf einen größeren Client virtueller Speicher mit mehr Speicher oder vertiefen Sie Ihr Arbeitsspeicher Verwendungsmustern Arbeitsspeicher Consuption verringern.


### <a name="burst-of-traffic"></a>Burst des Datenverkehrs

#### <a name="problem"></a>Problem

Unerwartet Datenverkehr in Kombination mit schlecht `ThreadPool` Einstellungen Verzögerung bei der Verarbeitung Daten bereits vom Server Redis gesendet, aber noch nicht auf dem Client verbraucht führen können.

#### <a name="measurement"></a>Maßeinheiten 

Monitor wie Ihre `ThreadPool` Statistiken über einen Zeitraum mithilfe von Code [wie folgt](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs)ändern. Sie können auch schauen Sie sich die `TimeoutException` StackExchange.Redis Nachricht. Hier ist ein Beispiel:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

Klicken Sie in der obigen Nachricht sind einige Aspekte, die interessant sind:

 1. Beachten Sie, dass in der `IOCP` Abschnitt und die `WORKER` Abschnitt stehen Ihnen eine `Busy` Wert, der größer ist die `Min` Wert. Dies bedeutet, dass Ihre `ThreadPool` Einstellungen benötigen anpassen.
 2. Sie können auch finden Sie unter `in: 64221`. Dies zeigt an, dass 64211 Bytes bei den Kernel Sockets Layer eingegangen sind, aber noch nicht durch die Anwendung (z. B. StackExchange.Redis) noch nicht gelesen wurde. Dies bedeutet normalerweise, dass die Anwendung Daten aus dem Netzwerk ist nicht so schnell liest, wie der Server es Ihnen sendet.

#### <a name="resolution"></a>Auflösung

Konfigurieren der [ThreadPool Einstellungen](https://gist.github.com/JonCole/e65411214030f0d823cb) , um sicherzustellen, dass es sich bei Ihrem Pool unter Burst Szenarien schnell Skalieren wird nach oben.


### <a name="high-client-cpu-usage"></a>Hohe Client CPU-Auslastung

#### <a name="problem"></a>Problem

Hoher CPU-Auslastung auf dem Client weist eine, die das System mit der Arbeit nicht möglich, die gebeten abhalten ausführen. Dies bedeutet, dass der Client Fehler auftreten kann, um eine Antwort von Redis zeitgerecht verarbeiten, obwohl Redis sehr schnell die Antwort gesendet.

#### <a name="measurement"></a>Maßeinheiten

System breit CPU-Auslastung über das Azure-Portal oder der Zähler zugeordneten Leistung zu überwachen. Achten Sie darauf, dass Sie nicht *Prozess* CPU überwachen, da ein einzelner Prozess zur gleichen Zeit niedriger CPU-Auslastung haben, die gesamten System CPU hoch sein kann. Schauen Sie sich für Spitzen in CPU-Auslastung, die mit Zeitlimit entsprechen. Als Ergebnis hoher CPU-Auslastung möglicherweise auch dann angezeigt hoch `in: XXX` Werte in `TimeoutException` Fehlermeldungen wie im Abschnitt [Burst des Datenverkehrs](#burst-of-traffic) beschrieben.

>[AZURE.NOTE] StackExchange.Redis 1.1.603 und höher enthält die `local-cpu` in metrischen `TimeoutException` Fehlermeldungen. Vergewissern Sie sich über die neueste Version des [StackExchange.Redis NuGet-Paket](https://www.nuget.org/packages/StackExchange.Redis/). Es gibt Fehlern ständig den Code ein, um es weitere robuste zu machen Zeitlimit, damit die aktuellste Version verfügen wichtig ist behoben wird.

#### <a name="resolution"></a>Auflösung

Upgrade auf eine größere virtueller Speicher mit mehr CPU-Kapazität oder ermitteln, was CPU-Auslastung verursacht. 



### <a name="client-side-bandwidth-exceeded"></a>Client Seite Bandbreite überschritten

#### <a name="problem"></a>Problem

Andere gleicher Breite Clientcomputern haben Einschränkungen wie viel Bandbreite im Netzwerk aufweisen. Wenn der Client die verfügbare Bandbreite überschreitet, werden dann Daten nicht clientseitig verarbeitet werden so schnell, wie er der Server sendet. Dies kann zu Zeitlimit führen.

#### <a name="measurement"></a>Maßeinheiten

Überwachen Sie Ihrer Verwendung von Bandbreite wie Änderung im Zeitverlauf Code [wie folgt](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs)verwenden. Beachten Sie, dass dieser Code in einigen Umgebungen mit eingeschränkten Berechtigungen (wie Azure Websites) nicht erfolgreich ausgeführt werden kann.

#### <a name="resolution"></a>Auflösung 

Erhöhen Sie Client virtueller Speicher oder verringern Sie des Netzwerkbandbreitenverbrauchs.


### <a name="large-requestresponse-size"></a>Größe des großen Anforderung/Antwort

#### <a name="problem"></a>Problem

Eine große Nachfrage/Antwort kann Zeitlimit verursachen. Als Beispiel nehmen Sie an, der auf Ihrem Client konfigurierten Timeoutwert 1 Sekunde ist. Die Anwendung anfordert zwei Tasten (z. B. 'A' und 'B) zur gleichen Zeit (mit den gleichen physischen Netzwerkverbindung). Die meisten Clients unterstützen "Pipelining" von Besprechungsanfragen, so, dass beide Anfragen 'A' und 'B' bei der Übertragung auf dem Server eine nacheinander gesendet werden ohne die Antworten zu warten. Der Server sendet die Antworten wieder in derselben Reihenfolge an. Wenn die Antwort 'A' groß ist, kann genug sie fast das Timeout für die nachfolgenden Anforderungen Essen. 

Im folgende Beispiel wird dieses Szenario veranschaulicht. In diesem Szenario Anforderung 'A' und 'B' schnell gesendet werden, der Server startet schnell eine Antwort 'A' und 'B' senden, aber aufgrund von Daten durchstellen Zeiten, abrufen 'B' hinter den anderen Anfrage und Timeout hängen, obwohl schnell des Servers Antwort.

  	|-------- 1 Second Timeout (A)----------|
  	|-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Maßeinheiten

Dies ist eine schwer bemaßende. Im Wesentlichen müssen Sie Ihre Client-Code zum Nachverfolgen von großen Besprechungsanfragen und Antworten instrumentieren. 

#### <a name="resolution"></a>Auflösung

1.  Redis ist für eine große Anzahl von kleinen, statt ein paar großen Werten optimiert. Die bevorzugte Lösung besteht darin, Ihre Daten in verwandten kleineren Werten aufteilen. Finden Sie unter der [, was das ideale Größe für Wertebereich Redis ist? Beträgt 100KB groß?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) Posten Sie Details um Warum kleinere Werten vorgeschlagen werden.
2.  Vergrößern Sie Ihre virtuellen Computer (für Client und Server für Redis Cache), um höhere Bandbreite-Funktionen, die Daten durchstellen Zeiten für größere Antworten reduzieren zu erhalten. Beachten Sie die erste mehr Bandbreite auf nur auf dem Server oder nur auf der Client möglicherweise nicht ausreichend. Messen Sie Ihre Verwendung Bandbreite und vergleichen Sie ihn mit den Funktionen, die der Größe des virtuellen Computer Sie aktuell arbeiten.
3.  Erhöhen Sie die Anzahl der `ConnectionMultiplexer` Sie über verschiedene Verbindungen verwenden und Besprechungsanfragen Round-Robert Objekte.


### <a name="what-happened-to-my-data-in-redis"></a>Wo befindet sich meine Daten in Redis?

#### <a name="problem"></a>Problem

Ich erwartet für bestimmte Daten werden in meiner Redis Azure-Cache-Instanz, aber es bestehen scheint haben.

##### <a name="resolution"></a>Auflösung

Finden Sie unter [Meine Daten in Redis geblieben?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) für den möglichen Ursachen und Lösungen.


## <a name="server-side-troubleshooting"></a>Problembehandlung bei Server-Seite

In diesem Abschnitt werden die Behandlung von Problemen, die auftreten, wenn eine Bedingung auf dem Cacheserver.

-   [Arbeitsspeicherdruck auf dem server](#memory-pressure-on-the-server)
-   [Hohe CPU-Auslastung / Server zu laden.](#high-cpu-usage-server-load)
-   [Server Seite Bandbreite überschritten](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Arbeitsspeicherdruck auf dem server

#### <a name="problem"></a>Problem

Arbeitsspeicherdruck auf dem Server führt zu allen Arten von Leistungsprobleme zu vermeiden, die Verarbeitung von Besprechungsanfragen verzögert werden können. Wenn genügend Arbeitsspeicher zur Verfügung steht, muss das System Seitendaten in der Regel von Speicher wird auf virtuellen Speichers, der auf der Festplatte befindet. Diese *Seite fehlgeschlagene* bewirkt, dass das System langsamer erheblich ab. Es gibt mehrere Gründe für diesen Arbeitsspeicherdruck ein: 

1.  Sie haben den Cache vollständig ausgelastet Daten enthält. 
2.  Redis hoher Speicherfragmentierung - durch Speichern von großen Objekten in den meisten Fällen verursacht angezeigt wird (Redis ist für eine kleine Objekte optimiert – finden Sie unter der [Was das ideale Größe für Wertebereich Redis ist? Beträgt 100KB groß?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) Posten Sie Details). 

#### <a name="measurement"></a>Maßeinheiten

Redis macht zwei Kennzahlen, mit denen Sie dieses Problem identifizieren können. Ist die erste `used_memory` und der andere `used_memory_rss`. [Diese Kennzahlen](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) stehen im Azure-Portal oder über den Befehl [Redis Informationen](http://redis.io/commands/info) .

#### <a name="resolution"></a>Auflösung

Es gibt mehrere mögliche Änderungen, die Sie vornehmen können, um arbeitsspeicherauslastung fehlerfrei zu halten:

1. [Konfigurieren einer Richtlinie Arbeitsspeicher](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) und Ablaufzeiten auf den Tasten festlegen. Beachten Sie, dass dies möglicherweise nicht ausreichend, wenn Sie den Fragmentierung aufweisen.
2. [Konfigurieren eines Werts Maxmemory reserviert](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) , die groß genug ist für Speicherfragmentierung zukommen lassen.
3. Unterteilen Sie Ihre großen Cache Objekte in kleineren verknüpfte Objekte.
4. [Maßstab](cache-how-to-scale.md) für die eine größere Cachegröße.
5. Wenn Sie einen [Premium Cache mit Redis Cluster aktiviert](cache-how-to-premium-clustering.md) verwenden können Sie [die Anzahl der mehrere Shards hinweg erhöhen](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Hohe CPU-Auslastung / Server zu laden.

#### <a name="problem"></a>Problem

Hoher CPU-Auslastung kann dazu führen, dass clientseitig ausgeführt werden kann, um eine Antwort von Redis zeitgerecht verarbeiten, obwohl Redis sehr schnell die Antwort gesendet.

#### <a name="measurement"></a>Maßeinheiten

System breit CPU-Auslastung über das Azure-Portal oder der Zähler zugeordneten Leistung zu überwachen. Achten Sie darauf, dass Sie nicht *Prozess* CPU überwachen, da ein einzelner Prozess zur gleichen Zeit niedriger CPU-Auslastung haben, die gesamten System CPU hoch sein kann. Schauen Sie sich für Spitzen in CPU-Auslastung, die mit Zeitlimit entsprechen.

#### <a name="resolution"></a>Auflösung

[Farben-Skala](cache-how-to-scale.md) aus, um einen größeren Cache mit mehr CPU-Kapazität Stufen oder ermitteln, was CPU-Auslastung verursacht. 

### <a name="server-side-bandwidth-exceeded"></a>Server Seite Bandbreite überschritten

#### <a name="problem"></a>Problem

Andere gleicher Breite Cacheinstanzen haben Einschränkungen wie viel Bandbreite im Netzwerk aufweisen. Wenn der Server die verfügbare Bandbreite überschreitet, werden die Daten nicht als schnell an den Client gesendet. Dies kann zu Zeitlimit führen.

#### <a name="measurement"></a>Maßeinheiten

Sie können Überwachen der `Cache Read` Metrisch, also die Menge der Daten aus dem Cache in MB pro Sekunde (MB/s) während des Intervalls der angegebenen reporting gelesen. Dieser Wert entspricht der Netzwerk-Bandbreite von dieser Cache verwendet. Wenn Sie Benachrichtigungen für Server Seite Netzwerk Bandbreitengrenzwerte einrichten möchten, können Sie sie mit dem folgenden erstellen `Cache Read` Zähler. Vergleichen der Werte mit den Werten in [dieser Tabelle](cache-faq.md#cache-performance) für die beobachteten Bandbreitengrenzwerte für die verschiedenen Ebenen und Größen Preise Cache.

#### <a name="resolution"></a>Auflösung

Wenn Sie konsistent in der Nähe der beobachteten maximale Bandbreite für Ihre Preisgestaltung Ebene und Cache Größe sind, erwägen Sie die [Skalierung](cache-how-to-scale.md) zu einem Preisgestaltung Ebene oder Größe, die größer Bandbreite, verwenden die Werte in [dieser Tabelle](cache-faq.md#cache-performance) als Leitfaden enthält.


## <a name="stackexchangeredis-timeout-exceptions"></a>StackExchange.Redis Timeout Ausnahmen

StackExchange.Redis verwendet eine Konfiguration Festlegen von benannten `synctimeout` für synchroner Vorgänge, die über einen Standardwert von 1000 ms verfügt. Wenn Sie ein Anruf synchroner nicht in der vorgesehenen Zeit abgeschlossen ist, löst StackExchange.Redis Client einen Timeoutfehler ähnlich wie im folgenden Beispiel aus.

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Diese Fehlermeldung enthält Kennzahlen, die Sie zeigen Sie auf das Problem und der möglichen Lösung des Problems helfen können. Die folgende Tabelle enthält Details zu der Metrik zurück, die Nachricht an.

| Fehler Nachricht Metrisch | Details                                                                                                                                                                                                                                          |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Inst       | Klicken Sie in die letzte Uhrzeit Schicht: 0 Befehle ausgegeben wurden                                                                                                                                                                                              |
| Manager        | Ist der Manager Sockets Durchführung `socket.select` was bedeutet, dass das Betriebssystem an, die etwas zu tun; weist eines Sockets Gruppenmitglieder ist im Wesentlichen: der Reader ist nicht aktiv Lesen aus dem Netzwerk, da es wahrscheinlich nicht vorhanden ist, etwas zu tun |
| Warteschlange      | Es gibt 73 Vorgänge für insgesamt in Bearbeitung                                                                                                                                                                                                        |
| qu         | 6 der Vorgänge in Bearbeitung in der Warteschlange nicht gesendeter sind und noch nicht mit dem Netzwerk ausgehenden geschrieben wurde                                                                                                                                                           |
| QS         | 67 He in Bearbeitung Vorgänge auf dem Server gesendet wurden, jedoch eine Antwort steht noch nicht zur Verfügung. Die Antwort könnte `Not yet sent by the server` oder`sent by the server but not yet processed by the client.`                                                   |
| QC         | 0 der Vorgänge in Bearbeitung Antworten angezeigt haben, aber noch nicht als erledigt aufgrund warten auf der Fertigstellung Schleife markiert wurde                                                                                                                                      |
| WR         | Es gibt eine aktive Autor (d. h., dass die 6 nicht gesendeten Anfragen nicht ignoriert werden) Bytes/activewriters                                                                                                                                                   |
| in         | Es gibt keine aktiven Leser und 0 (null) Bytes stehen auf der NIC Bytes/Activereaders gelesen werden                                                                                                                                               |


### <a name="steps-to-investigate"></a>Schritte zum Ermitteln

1. Als bewährte Methode stellen Sie sicher, dass verwendetem das folgende Muster Verbindung bei Verwendung des StackExchange.Redis-Clients.


        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


    Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit dem StackExchange.Redis im Cache mit](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

2. Stellen Sie sicher, dass Ihre Azure Redis Cache und der Clientanwendung in derselben Region in Azure sind. Angenommen, Sie möglicherweise werden erste Zeitlimit Wenn Ihren Cache im südostasiatischen US aber im Client befindet sich in Westen US und den Vorgang nicht innerhalb der `synctimeout` Intervall oder Sie möglicherweise werden Zeitlimit bei erhalten Sie von Ihrem Computer lokale Entwicklung Debuggen. 

    Es wurde dringend empfohlen, dass den Cache und im Client in derselben Azure Region. Wenn Sie ein Szenario, die Cross Region Anrufe enthält haben, legen Sie die `synctimeout` Intervall auf einen Wert größer als das standardmäßige 1000 ms Intervall durch, einschließlich einer `synctimeout` Eigenschaft in der Verbindungszeichenfolge. Im folgenden Beispiel wird eine StackExchange.Redis Cache Verbindung Zeichenfolge Codeausschnitt mit einer `synctimeout` von 2000 ms.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...

3. Vergewissern Sie sich über die neueste Version des [StackExchange.Redis NuGet-Paket](https://www.nuget.org/packages/StackExchange.Redis/). Es gibt Fehlern ständig den Code ein, um es weitere robuste zu machen Zeitlimit, damit die aktuellste Version verfügen wichtig ist behoben wird.

4. Treten Besprechungsanfragen, die durch die Bandbreite auf dem Server oder Client gebunden erste sind, ist es dauert länger für diese durchführen und Zeitlimit bewirken. Wenn Ihre Timeout aufgrund Bandbreite auf dem Server ist, finden Sie unter [Server Seite Bandbreite überschritten](#server-side-bandwidth-exceeded). Um festzustellen, ob Ihre Timeout aufgrund Client Bandbreite ist, finden Sie unter [Client Seite Bandbreite überschritten](#client-side-bandwidth-exceeded).

6. Sind Sie die erste CPU auf dem Server oder auf dem Client gebunden?
    -   Überprüfen Sie, ob Sie von CPU auf Ihrem Client gebunden erste sind wodurch könnte die Anfrage nicht bearbeitet werden, in der `synctimeout` Intervall, wodurch einen Timeout. In einer größeren Client verschieben oder Verteilen der Last kann dazu beitragen dies steuern. 
    -   Kontrollkästchen, wenn CPU wiedergegeben werden, die auf dem Server gebunden ist, indem Sie für die Überwachung der `CPU` [Cache Leistung zu definieren](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Anfragen während Redis CPU-gebunden ist, können diese Anfragen auf Timeout verursachen. Um dies zu beheben können Sie verteilen Sie die Last über mehrere mehrere Shards hinweg in einem Cache Premium, oder aktualisieren Sie auf eine größere Größe oder Preisgestaltung Ebene. Weitere Informationen finden Sie unter [Server Seite Bandbreite überschritten](#server-side-bandwidth-exceeded).

7. Gibt es Befehle aufzeichnen lange Verarbeitungszeit auf dem Server? Befehle, die auf dem Redis-Server Verarbeitungszeit zu lange sind mit langer kann Zeitlimit verursachen. Einige Beispiele für zeitintensive Befehle sind `mget` mit einer großen Anzahl von Tasten, `keys *` oder falsch geschriebene Lua Skripts. Verbinden mit Ihrer Azure Redis Cache Instanz des Redis-Cli-Clients oder verwenden Sie die [Konsole Redis](cache-configure.md#redis-console) , und führen Sie den Befehl [SlowLog](http://redis.io/commands/slowlog) , um festzustellen, ob es Anfragen dauert länger gibt als erwartet. Redis Server und StackExchange.Redis sind für viele kleine Anfragen statt weniger umfangreiche Anfragen optimiert. Teilen Ihre Daten in kleinere Einheiten möglicherweise Dinge verbessern. 

    Informationen zum Herstellen einer Verbindung mit den Azure Redis Cache SSL-Endpunkt Redis-Cli und Stunnel verwenden finden Sie unter der [Ankündigung ASP.NET Session State Provider für Preview-Version Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) Blogbeitrag. Weitere Informationen finden Sie unter [SlowLog](http://redis.io/commands/slowlog).

8. Hoher Auslastung von Redis Server kann dazu führen, dass Zeitlimit. Sie können die Auslastung des Servers überwachen, indem Sie die Überwachung der `Redis Server Load` [Cache Leistung zu definieren](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Einem Server Laden von 100 (Maximalwert) gibt an, dass der Server Redis beschäftigt, ohne im Leerlauf Zeit, wurde Verarbeitung von Besprechungsanfragen. Um herauszufinden ob alle der Serverfunktion bestimmte Anfragen belegt sind, führen Sie den Befehl SlowLog wie im vorherigen Absatz beschrieben aus. Weitere Informationen finden Sie unter [hoher CPU-Auslastung / Server zu laden](#high-cpu-usage-server-load).

9. Anteil eines anderen Ereignisses auf dem Client, die ein Netzwerk Blip geführt haben könnten? Wenn es ein Ereignis wurde wie die Anzahl der Clientinstanzen Skalierung, nach oben oder unten oder Bereitstellen einer neuen Versionsverlaufs des Clients oder automatische Skalierung aktiviert ist, aktivieren Sie auf dem Client (Web, Worker-Rolle oder einer VM Iaas)? In unseren Tests, die wir gefunden haben, dass automatisch skalieren oder Skalierung nach oben/unten verursachen kann kann ausgehende Netzwerkkonnektivität für einige Sekunden verloren. StackExchange.Redis Code ist flexibel in Bezug auf solche Ereignisse und Verbindung erneut. Dieses Zeitraums der erneuten Verbindung können keine Anfragen in der Warteschlange Timeout.

10. Wurde es eine große Anforderung mehrere kleine Anfragen zum Redis Cache, deren überschritten vor? Der Parameter `qs` der Fehler gemeldet, wie viele Anfragen vom Client an den Server gesendet wurden, aber noch keine Antwort verarbeitet haben. Dieser Wert kann immer langsamer beibehalten, da StackExchange.Redis eine einzelne TCP-Verbindung verwendet und eine Antwort kann nur jeweils lesen. Obwohl ersten Timeout, verhindert nicht/vom Server gesendeten Daten, und andere Anforderungen werden blockiert, bis Abschluss dieses Auswertung verursacht. Eine Lösung besteht darin, die Wahrscheinlichkeit, dass Zeitlimit, um sicherzustellen, dass der Cache für Ihre Arbeitsbelastung ausreicht und Teilen große Werte in kleinere Einheiten zu minimieren. Eine weitere mögliche Lösung besteht darin, verwenden einen Pool von `ConnectionMultiplexer` Objekte in Ihren Kunden, und wählen Sie mindestens geladen `ConnectionMultiplexer` beim Senden einer neuen Anforderung. Sollten verhindern, dass einen einzelnen Timeout bewirken, dass andere Anforderungen zu außerdem Timeout.

11. Bei Verwendung von `RedisSessionStateprovider`, stellen Sie sicher, Sie haben das Timeout "Wiederholen" ordnungsgemäß festgelegt. `retrytimeoutInMilliseconds`sollten höher als `operationTimeoutinMilliseonds`, da andernfalls Versuche ohne auftritt. Im folgenden Beispiel `retrytimeoutInMilliseconds` auf 3000 festgelegt ist. Weitere Informationen finden Sie unter [ASP.NET Session State Provider für Azure Redis Cache](cache-aspnet-session-state-provider.md) und [wie Sie die Konfigurationsparameter Session State Provider und Ausgabe Cacheanbieter verwenden](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).


    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


12. Arbeitsspeicherauslastung auf dem Server Azure Redis Cache überprüfen, indem Sie [die Überwachung](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` und `Used Memory`. Redis wird gestartet, wenn Sie eine Richtlinie Entfernung aktiviert ist, wird entfernt Pfeiltasten wann `Used_Memory` die Cachegröße erreicht. Idealerweise `Used Memory RSS` nur sollten geringfügig über `Used memory`. Ein großer Unterschied bedeutet, dass es gibt Speicherfragmentierung (intern oder extern. Wenn `Used Memory RSS` ist kleiner als `Used Memory`, dies bedeutet Teil des Caches wurde vom Betriebssystem ausgetauscht wurden. In diesem Fall können Sie einige signifikanten Wartezeiten erwartet. Da Redis nicht steuern, wie seine Zuweisungen auf Seiten im Arbeitsspeicher, hohe zugeordnet werden `Used Memory RSS` ist oft als Ergebnis einer Sammlung in arbeitsspeicherauslastung. Wenn Redis Speicher freigibt, wird der Arbeitsspeicher wieder in die Zuweisung angegeben und die Zuweisung möglicherweise oder möglicherweise nicht zurück Arbeitsspeicher wieder an das System. Möglicherweise gibt es eine Abweichung zwischen den `Used Memory` Wert und Arbeitsspeicher Tagesdosis vom Betriebssystem gemeldet. Es ist möglicherweise aufgrund der Fakultät Arbeitsspeicher verwendet und von Redis, aber nicht die angegebene zurück, um das System freigegeben wurde. Um Hilfe Arbeitsspeicher Kompatibilitätsproblemen können Sie die folgenden Schritte ausführen.
    -   Aktualisieren Sie den Cache zu vergrößern, sodass Sie nicht gegenüber den Arbeitsspeicher Einschränkungen auf dem System ausgeführt werden.
    -   Legen Sie oft Ablauf auf die Tasten, damit die vorausschauende ältere Werte entfernt werden.
    -   Überwachen der der `used_memory_rss` Metrisch Zwischenspeichern. Wenn dieser Wert deren Cachegröße erreicht, werden Sie wahrscheinlich starten Leistungsprobleme angezeigt. Verteilen Sie die Daten über mehrere mehrere Shards hinweg, wenn Sie einen Premium Cache verwenden, oder aktualisieren Sie auf eine größere Cachegröße.

    Weitere Informationen finden Sie unter [Arbeitsspeicherdruck auf dem Server](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Weitere Informationen

-   [Welche Redis Cache Angebot und Größe sollte ich verwenden?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
-   [Wie kann ich testen die Leistung von meinem Cache und analysieren?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Wie kann ich Redis Befehle ausgeführt werden?](cache-faq.md#how-can-i-run-redis-commands)
-   [Zum Überwachen der Azure Redis Cache](cache-how-to-monitor.md)