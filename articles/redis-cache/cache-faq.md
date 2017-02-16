<properties 
    pageTitle="Häufig gestellte Fragen zur Azure Redis-Cache | Microsoft Azure" 
    description="Erhalten Sie Antworten auf häufig gestellte Fragen, Muster und bewährte Methoden für Azure Redis Cache" 
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
    ms.date="10/18/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-faq"></a>Häufig gestellte Fragen zur Azure Redis-Cache

Erhalten Sie Antworten auf häufig gestellte Fragen, Muster und bewährte Methoden für Azure Redis Cache. 


## <a name="what-if-my-question-isnt-answered-here"></a>Was geschieht, wenn mein Frage hier beantwortet nicht zur Verfügung?

Wenn Ihre Frage hier nicht aufgeführt ist, informieren Sie uns, und wir helfen Ihnen eine Antwort zu finden.

-   Können Sie eine Frage im [Disqus Thread](#comments) am Ende dieses häufig gestellte Fragen und populärer mit der Azure-Cache-Team und andere Mitglieder der Community Informationen zu den in diesem Artikel.
-   Um eine größere Zielgruppe erreicht haben, können Sie eine Frage im [Cache MSDN-Forum Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) und populärer mit der Azure-Cache-Team und andere Mitglieder der Community.
-   Wenn Sie eine Besprechungsanfrage Feature vornehmen möchten, können Sie Ihre Anfragen und Ideen [Azure Redis Cache Benutzer Voicemail](https://feedback.azure.com/forums/169382-cache)senden.
-   Sie können auch eine e-Mail-Nachricht an uns bei [Azure Cache externen Feedback](mailto:azurecache@microsoft.com)senden.

## <a name="azure-redis-cache-basics"></a>Azure Cache Redis-Grundlagen

Die häufig gestellten Fragen in diesem Abschnitt beziehen sich auf einige der die Grundlagen des Azure Redis Cache.

-    [Was ist Azure Redis Cache?](#what-is-azure-redis-cache)
-    [Wie kann ich mit Azure Redis Cache gestartet erhalten?](#how-can-i-get-started-with-azure-redis-cache)

Die folgenden häufig gestellte Fragen Deckblatt grundlegende Konzepte und Fragen zur Azure Redis Cache und in eine der anderen Bereiche häufig gestellte Fragen beantwortet werden.

-   [Welche Redis Cache Angebot und Größe sollte ich verwenden?](#what-redis-cache-offering-and-size-should-i-use)
-   [Welche Redis-Cache-Clients kann ich verwenden?](#what-redis-cache-clients-can-i-use)
-   [Gibt es ein lokaler Emulator für Azure Redis Cache?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Wie werden die Gesundheit und Leistung von meinem Cache überwacht?](#how-do-i-monitor-the-health-and-performance-of-my-cache)



## <a name="planning-faqs"></a>Planen häufig gestellte Fragen

-   [Welche Redis Cache Angebot und Größe sollte ich verwenden?](#what-redis-cache-offering-and-size-should-i-use)
-   [Azure Cache Redis Leistung](#azure-redis-cache-performance)
-   [In welchen Region sollte ich meine Cache zu finden?](#in-what-region-should-i-locate-my-cache)
-   [Wie verwende ich Azure Redis Cache in Rechnung gestellt?](#how-am-i-billed-for-azure-redis-cache)
-   [Kann ich mit Azure Government Cloud oder Azure China Cloud Azure Redis Cache verwenden?](#can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud)



## <a name="development-faqs"></a>Entwicklung häufig gestellte Fragen

-   [Was mache StackExchange.Redis Konfigurationsoptionen?](#what-do-the-stackexchangeredis-configuration-options-do)
-   [Welche Redis-Cache-Clients kann ich verwenden?](#what-redis-cache-clients-can-i-use)
-   [Gibt es ein lokaler Emulator für Azure Redis Cache?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Wie kann ich Redis Befehle ausgeführt werden?](#how-can-i-run-redis-commands)
-   [Warum haben nicht Azure Redis Cache ein MSDN-Klasse Bibliothek Bezug wie einige der anderen Azure Dienste?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
-   [Kann ich als PHP Sitzungscache Azure Redis Cache verwenden?](#can-i-use-azure-redis-cache-as-a-php-session-cache)


## <a name="security-faqs"></a>FAQs zum Thema Sicherheit

-   [Wann sollte ich den Port nicht SSL für die Verbindung mit Redis aktivieren?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)


## <a name="production-faqs"></a>Herstellung häufig gestellte Fragen

-   [Was sind einige bewährte Methoden für die Herstellung?](#what-are-some-production-best-practices)
-   [Was sind einige der Aspekte, wenn Sie häufig verwendete Redis Befehle verwenden?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
-   [Wie kann ich testen die Leistung von meinem Cache und analysieren?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Wichtige Details ThreadPool Variation](#important-details-about-threadpool-growth)
-   [Aktivieren der Server globalen Katalog auf Weitere Durchsatz auf dem Client erhalten, wenn StackExchange.Redis verwenden](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)


## <a name="monitoring-and-troubleshooting-faqs"></a>Überwachen und zur Problembehandlung häufig gestellte Fragen

Die häufig gestellten Fragen in diesem Abschnitt hervorgehen allgemeine für die Überwachung und Problembehandlung bei Fragen. Weitere Informationen zur Überwachung und Problembehandlung der Azure Redis Cache Instanzen finden Sie unter [Azure Redis Cache überwachen](cache-how-to-monitor.md) und [Behebung von Azure Redis Cache](cache-how-to-troubleshoot.md).

-   [Wie werden die Gesundheit und Leistung von meinem Cache überwacht?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
-   [Meine Cache Diagnose Speicher kontoeinstellungen geändert, ist was passiert?](#my-cache-diagnostics-storage-account-settings-changed-what-happened)
-   [Warum ist Diagnose für einige neue Caches aber andere nicht aktiviert?](#why-is-diagnostics-enabled-for-some-new-caches-but-not-others)
-   [Warum erhalte ich Zeitlimit angezeigt?](#why-am-i-seeing-timeouts)
-   [Warum wurde mein Client aus dem Cache getrennt?](#why-was-my-client-disconnected-from-the-cache)


## <a name="prior-cache-offering-faqs"></a>Vorherige Cache Angebot häufig gestellte Fragen

-   [Welche Azure Cache Angebot ist richtig für mich?](#which-azure-cache-offering-is-right-for-me)


### <a name="what-is-azure-redis-cache"></a>Was ist Azure Redis Cache?

Azure Redis Cache basiert auf der beliebten [Redis Cache](http://redis.io)öffnen-Quelle. Sie erhalten Sie zentralen Zugriff auf einen sicheren, dedizierten Redis Cache, verwaltete von Microsoft und aus einer beliebigen Anwendung in Azure zugegriffen werden. Rufen Sie die Seite [Azure Redis Cache](https://azure.microsoft.com/services/cache/) auf Azure.com für eine ausführlichere Übersicht über.


### <a name="how-can-i-get-started-with-azure-redis-cache"></a>Wie kann ich mit Azure Redis Cache gestartet erhalten?

Es gibt mehrere Methoden, die Sie mit Azure Redis Cache loslegen können.

-    Sie können eine der unsere Lernprogramme verfügbar für [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md)und [Python](cache-python-get-started.md)Auschecken. 
-    [So erstellen Sie hohe Leistung Apps mithilfe von Microsoft Azure Redis Cache](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/)ferner zur Verfügung.
-    Sie können die Client-Dokumentation für die Clients Auschecken, die die Entwicklung des Projekts Sprache, um zu erfahren, wie Redis verwenden können. Es gibt viele Redis-Clients, die mit Azure Redis Cache verwendet werden können. Eine Liste der Redis-Clients finden Sie unter [http://redis.io/clients](http://redis.io/clients).


Wenn Sie bereits über ein Azure-Konto besitzen, können Sie:

-    [Öffnen Sie ein Azure-Konto kostenlos](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Sie erhalten Gutschriften, die mit dem Testen kostenpflichtiges Azure Services verwendet werden können. Auch nachdem die Gutschriften von verwendet werden, können Sie das Konto beibehalten und kostenlosen Azure Dienste und Features verwenden.
-    [Vorteile für Abonnenten Visual Studio aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Ihr Abonnement MSDN bietet Ihnen Gutschriften jeden Monat, die Sie für kostenpflichtiges Azure-Dienste verwenden können.


<a name="cache-size"></a>
### <a name="what-redis-cache-offering-and-size-should-i-use"></a>Welche Redis Cache Angebot und Größe sollte ich verwenden?
Jede Azure Redis Cache bietet verschiedene Detailebenen **Größe**, **Bandbreite**, **hohen Verfügbarkeit**und **Vereinbarung zum SERVICELEVEL** -Optionen.

Im folgenden sind die Kriterien für die Auswahl ein Angebot Cache.

-   **Arbeitsspeicher**: der Standard- und Standard Ebenen bieten 250 MB – 53 GB. Die Ebene Premium bietet bis zu 530 GB mit mehr verfügbaren [Anforderung](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase)an. Weitere Informationen finden Sie unter [Azure Redis Cache Preise](https://azure.microsoft.com/pricing/details/cache/).
-   **Netzwerk-Performance**: bei einer Arbeitsbelastung, die hohen Durchsatz erfordert die Ebene Premium bietet mehr Bandbreite im Vergleich zu Standard oder Basic. Außerdem innerhalb jeder Kategorie größere Größen Caches mehr Bandbreite aufgrund der zugrunde liegenden virtuellen Computer, der den Cache für hostet. Finden Sie in der [folgenden Tabelle](#cache-performance) für Weitere Informationen.
-   **Durchsatz**: Ebene der Premium bietet den maximalen Durchsatz unter verfügbaren. Wenn der Cacheserver oder Client die Bandbreitengrenzwerte erreicht, erhalten Sie Zeitlimit clientseitig. Weitere Informationen finden Sie in der folgenden Tabelle aus.
-   **Hohe Verfügbarkeit/Vereinbarung zum SERVICELEVEL**: Azure Redis Cache wird sichergestellt, dass ein Standard/Premium Cache mindestens 99,9 % der Zeit zur Verfügung steht. Weitere Informationen zu unseren Vereinbarung zum SERVICELEVEL finden Sie unter [Azure Redis Cache Preise](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). Der Vereinbarung zum SERVICELEVEL werden nur die Verbindung zu den Cache Endpunkten behandelt. Der Vereinbarung zum SERVICELEVEL befasst sich nicht auf Schutz vor Datenverlust aus. Es empfiehlt sich, mit dem Redis Daten Beibehaltung-Feature in der Ebene Premium um Stabilität vor Datenverlust zu vergrößern.
-   **Redis Daten Beibehaltung**: der Premium Ebene ermöglicht es Ihnen, die beibehalten der Cachedaten in einem Speicher Azure-Konto. Alle Daten wird in einem Cache Basic/Standard nur im Arbeitsspeicher gespeichert. Bei der zugrunde liegenden Infrastruktur können es Probleme möglichem sein. Es empfiehlt sich, mit dem Redis Daten Beibehaltung-Feature in der Ebene Premium um Stabilität vor Datenverlust zu vergrößern. Azure Redis Cache bietet RDB und AOF (in Kürze verfügbar) Optionen in Redis Beibehaltung. Weitere Informationen finden Sie unter [Beibehaltung für einen Premium Azure Redis Cache konfigurieren](cache-how-to-premium-persistence.md).
-   **Redis Cluster**: zwischengespeichert größer als 53 GB zu erstellen, oder zum Shard von Daten in mehreren Redis Knoten können Redis Cluster, der in der Premium Ebene verfügbar ist. Jeder Knoten besteht aus einem primären/Replikat Cache Paar hohen Verfügbarkeit. Weitere Informationen finden Sie unter [So konfigurieren Sie für einen Premium Azure Redis Cache Cluster](cache-how-to-premium-clustering.md).
-   **Verbesserte Sicherheit und Network Isolation**: Bereitstellung Azure virtuelle Netzwerk (VNET) bietet verbesserte Sicherheit und Isolation für Ihre Azure Redis Cache sowie Subnetze, Steuerelement-Richtlinien, und weitere Features zum weiteren Zugriff einschränken. Weitere Informationen finden Sie unter [Konfigurieren von Virtual Network-Unterstützung für einen Premium Azure Redis Cache](cache-how-to-premium-vnet.md).
-   **Konfigurieren von Redis**: In Standard und Premium Ebenen konfigurieren Sie Redis für Benachrichtigungen Schlüssellänge zur Verfügung.
-   **Maximale Anzahl von Clients verbunden**: Ebene der Premium bietet die maximale Anzahl von Clients, die mit Redis, mit einer höheren Nummer der Verbindungen für größere gleicher Breite Caches herstellen kann. [Bitte schlagen Sie in der Preisgestaltung Seite Details](https://azure.microsoft.com/pricing/details/cache/).
-   **Dedizierte Core für Server Redis**: In der Premium Ebene über alle Cachegröße eine dedizierte Core für Redis verfügen. In der Basic-Standard Ebenen der C1 Größe und höher haben eine dedizierte Core für Redis Server.
-   Größere virtueller Computer verfügen in der Regel mehr Bandbreite als kleinere, aber **Redis ist Single-threaded** müssen daher mehr als zwei Kerne bietet keine weitere Vorteile gegenüber dem nur zwei Kerne. Wenn der Cacheserver oder Client die Bandbreitengrenzwerte erreicht, erhalten Zeitlimit clientseitig Sie.
-   **Verbesserte Leistung**: Caches in der Ebene Premium werden auf Hardware, die schnellere Prozessoren weist bereitgestellt und bietet eine bessere Leistung im Vergleich zu der Stufe Basic oder Standard. Premium Ebene Caches haben höhere Durchsatzraten und unteren Wartezeiten.

<a name="cache-performance"></a>
### <a name="azure-redis-cache-performance"></a>Azure Cache Redis Leistung

Die folgende Tabelle zeigt die maximale Bandbreite Werte berechneter beim Testen von Standard in verschiedenen Formaten und Premium zwischengespeichert mit `redis-benchmark.exe` aus einer Iaas VM gegen den Endpunkt Azure Redis Cache. Beachten Sie, dass diese Werte sind nicht garantiert, und es keine Vereinbarung zum SERVICELEVEL für diese wird Zahlen, aber typische sollten. Sie sollten laden Testen Ihrer eigenen Anwendung, um die richtige Cachegröße für eine Anwendung zu bestimmen.

In dieser Tabelle können wir die folgenden Schlussfolgerungen ziehen.

-   Durchsatz für die Caches, die die gleiche Größe haben ist in der Premium Ebene im Vergleich zu den standardmäßigen Ebene höher. Mit einem 6 GB Cache beträgt beispielsweise Durchsatz von P1 140K RPS im Vergleich zu 49 K für C3.
-   Mit Redis Cluster wird der Durchsatz linear, wie Sie die Anzahl der mehrere Shards hinweg (Knoten) im Cluster erhöhen. Wenn Sie einen Cluster P4 10 mehrere Shards hinweg erstellen, klicken Sie dann der verfügbare Durchsatz beträgt beispielsweise 250K * 10 = 2,5 Millionen RPS.
-   Durchsatz bei vergrößern Key ist in der Premium Ebene im Vergleich zu der Standard Ebene höher.

| Preise Ebene             | Größe   | CPUs | Verfügbare Bandbreite                                    | Größe von 1 KB-Taste                            |
|--------------------------|--------|-----------|--------------------------------------------------------|------------------------------------------|
| **Standard Cachegröße** |        |           | **MB pro Sekunde (Mb/s) / Megabyte pro Sekunde (MB/s)** | **Abfragen pro Sekunde (RPS)**            |
| C0                       | 250 MB | Freigegeben    | 5 / 0.625                                              | 600                                      |
| C1                       | 1 GB   | 1         | 100 / 12,5                                             | 12200                                    |
| C2                       | 2,5 GB | 2         | 200 / 25                                               | 24000                                    |
| C3                       | 6 GB   | 4         | 400 / 50                                               | 49000                                    |
| C4                       | 13 GB  | 2         | 500 / 62,5                                             | 61000                                    |
| C5                       | 26 GB  | 4         | 1000 / 125                                             | 115000                                   |
| C6                       | 53 GB  | 8         | 2000 / 250                                             | 150000                                   |
| **Premium Cachegrößen**  |        | **CPUs pro shard**  |                                         | **Abfragen pro Sekunde (RPS), pro shard** |
| P1                       | 6 GB   | 2         | 1000 / 125                                             | 140000                                   |
| P2                       | 13 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P3                       | 26 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P4                       | 53 GB  | 8         | 4000 / 500                                             | 250000                                   |


Anweisungen zum Herunterladen von Redis-Tools wie `redis-benchmark.exe`, finden Sie unter der [wie kann ich die Redis Befehle ausführen?](#cache-commands) Abschnitt.

<a name="cache-region"></a>
### <a name="in-what-region-should-i-locate-my-cache"></a>In welchen Region sollte ich meine Cache zu finden?

Suchen Sie für optimale Leistung und niedrigsten Wartezeit Ihre Azure Redis Cache in der gleichen Region wie Ihre Clientanwendung Cache aus.

<a name="cache-billing"></a>
### <a name="how-am-i-billed-for-azure-redis-cache"></a>Wie verwende ich Azure Redis Cache in Rechnung gestellt?

Azure Cache Redis Preise ist [hier](https://azure.microsoft.com/pricing/details/cache/). Die Preise Seite listet die Preise als Stundensatz. Caches werden auf Basis pro Minute ab dem Zeitpunkt berechnet, die der Cache erst erstellt wird, die ein Cache gelöscht wird. Es ist keine Option zum Beenden oder Anhalten der Abrechnung von einem Cache aus.


## <a name="can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud"></a>Kann ich mit Azure Government Cloud oder Azure China Cloud Azure Redis Cache verwenden?

Ja, steht Azure Redis Cache Azure Government Cloud und Azure China Cloud. Beachten Sie, dass die URLs für den Zugriff auf und Verwalten von Azure Redis Cache in Azure Government Cloud unterscheiden und Azure China Cloud im Vergleich mit Azure öffentlichen Cloud. Weitere Informationen zum Aspekte beim Azure Redis Cache mit Azure Government Cloud und Azure China Cloud verwenden finden Sie unter [Azure-Datenbanken Government - Azure Redis Cache](../azure-government/documentation-government-services-database.md#azure-redis-cache) und [Azure China Cloud - Azure Redis Cache](https://www.azure.cn/documentation/services/redis-cache/).

Informationen zum Verwenden von Azure Redis Cache mit PowerShell in Azure Government Cloud und Azure China Cloud finden Sie unter [Herstellen einer Verbindung mit Azure Government Cloud oder Azure China Cloud](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).


<a name="cache-configuration"></a>
### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>Was mache StackExchange.Redis Konfigurationsoptionen?

StackExchange.Redis enthält viele Optionen. In diesem Abschnitt spricht über einige allgemeine Einstellungen auf. Ausführlichere Informationen zu StackExchange.Redis Optionen finden Sie unter [StackExchange.Redis Konfiguration](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md).

ConfigurationOptions|Beschreibung|Empfehlungen
---|---|---
AbortOnConnectFail|Beim Festlegen auf "True" die Verbindung nicht nach einem Netzwerkfehler verbunden werden.|Legen Sie auf falsch, und weisen Sie StackExchange.Redis automatisch erneut verbunden.
ConnectRetry|Wie oft wiederholen versucht, eine Verbindung während Initiale verbinden.| Finden Sie unter den folgenden Notizen Anleitung. |
ConnectTimeout|Timeout in Millisekunden für verbinden Vorgänge an.| Finden Sie unter den folgenden Notizen Anleitung. |

In den meisten Fällen sind die Standardwerte des Clients ausreichend. Sie können auch die Optionen basierend auf Ihrer Arbeitsbelastung optimieren.

-   **Wiederholungsversuche**
    -   ConnectRetry und die Faustregel ist, fehlschlägt ConnectTimeout schnell und wiederholen. Dies basiert auf Ihre Arbeitsbelastung und wie viel Zeit auf Mittelwert für Ihren Kunden einen Redis Befehl und eine Antwort erhalten hat.
    -   Lassen Sie StackExchange.Redis anstelle von Verbindungsstatus überprüft und erneut selbst automatisch erneut verbunden. **Verwenden die Eigenschaft ConnectionMultiplexer.IsConnected vermeiden**.
    -   Snowballing - manchmal können Sie in ein Problem, wo Sie Staus und dadurch snowballs und stellt nie wieder her, ausführen. In diesem Fall sollten Sie sich, dass die einen exponentiellen Backoff "Wiederholen" Algorithmus verwenden, wie im [allgemeinen Leitfaden wiederholen](best-practices-retry-general.md) beschrieben, die von der Gruppe Microsoft Patterns & Practices veröffentlicht.
-   **Timeoutwerte**
    -   Erwägen Sie die Arbeitsbelastung, und legen Sie die Werte entsprechend. Wenn Sie große Werte speichern, legen Sie das Timeout auf einen höheren Wert.
    -   Festlegen von `AbortOnConnectFail` für Sie die Verbindung mit falsch und lassen StackExchange.Redis wiederherstellen.
    -   Verwenden Sie eine einzelne ConnectionMultiplexer-Instanz für die Anwendung. Eine LazyConnection können zum Erstellen einer einzelnen Instanz, die von einer Verbindungseigenschaft zurückgegeben wird, siehe [Herstellen einer Verbindung mit der Klasse ConnectionMultiplexer im Cache mit](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
    -   Festlegen der `ConnectionMultiplexer.ClientName` Eigenschaft zu einer app-Instanz eindeutigen Namen zu Diagnosezwecken.
    -   Verwenden Sie mehrere `ConnectionMultiplexer` Instanzen für benutzerdefinierte Auslastung.
    -   Wenn Sie mit wechselnder Auslastung in Ihrer Anwendung verfügen, können Sie dieses Modell folgen. Beispiel:
    -   Sie können eine multiplexer für den Umgang mit großen Tasten haben.
    -   Sie können eine multiplexer für den Umgang mit kleinen Tasten haben.
    -   Sie können verschiedene Werte für die Verbindung Zeitlimit festlegen, und wiederholen Sie Logik für jede ConnectionMultiplexer, die Sie verwenden.
    -   Festlegen der `ClientName` Eigenschaft auf jeder multiplexer mit Diagnose helfen.
    -   Dies wird dazu führen, dass weitere optimiert Wartezeit pro `ConnectionMultiplexer`.

### <a name="what-redis-cache-clients-can-i-use"></a>Welche Redis-Cache-Clients kann ich verwenden?

Einer der großen Vorteile von Redis ist, dass viele Clients viele verschiedene Entwicklung unterstützten Sprachen. Eine aktuelle Liste von Clients finden Sie unter [Redis Clients](http://redis.io/clients). Finden Sie unter [Verwenden von Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md) Lernprogramme, in denen mehrere verschiedene Sprachen und Clients behandelt, und klicken Sie auf die gewünschte Sprache aus der Sprache wechseln am Anfang des Artikels.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>
### <a name="is-there-a-local-emulator-for-azure-redis-cache"></a>Gibt es ein lokaler Emulator für Azure Redis Cache?

Es gibt keine lokalen Emulator für Azure Redis Cache, aber Sie können die Version MSOpenTech Redis-server.exe über die [Befehlszeile Tools Redis](https://github.com/MSOpenTech/redis/releases/) auf Ihrem lokalen Computer ausführen und Verbinden mit eine ähnliche Erfahrung mit einem lokalen Cache Emulator erhalten sie, wie im folgenden Beispiel dargestellt.


    private static Lazy<ConnectionMultiplexer>
        lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect to a locally running instance of Redis to simulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


Optional können Sie eine Datei [redis.conf](http://redis.io/topics/config) genauer [Cache Standardeinstellungen](cache-configure.md#default-redis-server-configuration) für Ihr online Azure Redis Cache übereinstimmt, falls gewünscht konfigurieren.

<a name="cache-commands"></a>
### <a name="how-can-i-run-redis-commands"></a>Wie kann ich Redis Befehle ausgeführt werden?

Sie können die Befehle am [Redis Befehle](http://redis.io/commands#) aufgeführt wird, eine Ausnahme bilden jedoch die Befehle aufgeführt, die bei [Redis Befehle in Azure Redis Cache nicht unterstützt](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache). Zum Ausführen von Redis Befehle haben Sie mehrere Optionen aus.

-   Wenn Sie einen Cache Standard oder Premium verfügen, können Sie mithilfe der [Verwaltungskonsole Redis](cache-configure.md#redis-console)Redis-Befehle ausführen. Dadurch eine sichere Weise Redis Befehle in der Azure-Portal ausführen.
-   Sie können auch die Redis Befehlszeilentools verwenden. Führen Sie die folgenden Schritte aus, um diese zu verwenden.
-   Laden Sie die [Befehlszeile Tools Redis](https://github.com/MSOpenTech/redis/releases/).
-   Herstellen einer Verbindung mit dem Cache mit `redis-cli.exe`. Übergeben der Cache Endpunkt verwenden, wechseln Sie -h, und den Schlüssel mit - ein wie im folgenden Beispiel dargestellt.
-   `redis-cli -h <your cache="" name="">
.redis.cache.windows.net -a <key>
  `
  - Beachten Sie, dass die Redis Befehlszeilentools nicht mit den SSL-Anschluss funktionieren, aber Sie ein Programm wie können `stunnel` sicher die Tools an den SSL-Anschluss anmelden, anhand der Anweisungen im Blogbeitrag [Ankündigung ASP.NET Session State Provider für Redis Preview-Version](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) .

<a name="cache-reference"></a>
### <a name="why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Warum haben nicht Azure Redis Cache ein MSDN-Klasse Bibliothek Bezug wie einige der anderen Azure Dienste?

Microsoft Azure Redis Cache basiert auf der gängiger open Source Redis Cache und zugegriffen werden kann, indem Sie eine Vielzahl von [Clients Redis](http://redis.io/clients) die für viele programming Sprachen verfügbar sind. Jeder Client verfügt über eine eigene API, die die Redis-Cache-Instanz mithilfe von [Redis Befehle](http://redis.io/commands)aufgerufen werden.

Da jeder Client anders ist, müssen Sie nicht eine zentrale Klasse Bezug vorhanden ist, auf der MSDN; Jeder Client unterhält stattdessen eigener Dokumentation verweisen. Zusätzlich zu der Dokumentation gibt es mehrere Lernprogramme zeigt, wie Sie den ersten Schritten mit Azure Redis Cache mit verschiedenen Sprachen und Cache Clients. Um diesen Lernprogrammen zugreifen zu können, finden Sie unter [Verwenden von Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md) , und klicken Sie auf die gewünschte Sprache aus der Sprache wechseln am Anfang des Artikels.


### <a name="can-i-use-azure-redis-cache-as-a-php-session-cache"></a>Kann ich als PHP Sitzungscache Azure Redis Cache verwenden?

Ja, um Azure Redis Cache als PHP Sitzungscache verwenden zu können, geben die Verbindungszeichenfolge Ihrer Azure Redis Cache Instanz in `session.save_path`.

>[AZURE.IMPORTANT] Wenn Azure Redis Cache als PHP Sitzungscache verwenden zu können, müssen Sie die URL codiert werden die Sicherheit-Taste zum Verbinden mit dem Cache, wie im folgenden Beispiel dargestellt.
>
>`session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
>Wenn der Schlüssel nicht URL codiert ist, wird möglicherweise eine Ausnahme ähnlich wie der folgende angezeigt:`Failed to parse session.save_path`

Weitere Informationen zur Verwendung von Redis Cache als PHP Sitzungscache mit dem PhpRedis-Client finden Sie unter [Ereignishandler von PHP Sitzung](https://github.com/phpredis/phpredis#php-session-handler).



<a name="cache-ssl"></a>
### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Wann sollte ich den Port nicht SSL für die Verbindung mit Redis aktivieren?

Redis Server unterstützt keine SSL im Paket, aber Azure Redis Cache unterstützt. Wenn Sie die Verbindung, Azure Redis Cache herstellen und Ihren Kunden SSL, wie StackExchange.Redis unterstützt, sollten Sie SSL verwenden.

Beachten Sie, dass der Port nicht SSL standardmäßig für neue Instanzen von Azure Redis Cache deaktiviert ist. Wenn Sie Ihren Kunden SSL nicht unterstützt, müssen Sie den Port nicht SSL anhand der Anweisungen im Abschnitt [Zugriffsports](cache-configure.md#access-ports) des Artikels [Konfigurieren eines Cache in Azure Redis Cache](cache-configure.md) aktivieren.

Tools wie redis `redis-cli` funktionieren nicht mit den SSL-Anschluss, aber Sie können ein Programm wie `stunnel` sicher die Tools an den SSL-Anschluss anmelden, anhand der Anweisungen im Blogbeitrag [Ankündigung ASP.NET Session State Provider für Redis Preview-Version](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) .

Anweisungen zum Herunterladen der Redis Tools finden Sie unter der [wie kann ich die Redis Befehle ausführen?](#cache-commands) Abschnitt.



### <a name="what-are-some-production-best-practices"></a>Was sind einige bewährte Methoden für die Herstellung?

-   [Bewährte Methoden StackExchange.Redis](#stackexchangeredis-best-practices)
-   [Konfiguration und Konzepte](#configuration-and-concepts)
-   [Testen der Leistung](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>Bewährte Methoden StackExchange.Redis

-   Festlegen von `AbortConnect` false, lassen Sie dann die ConnectionMultiplexer automatisch erneut verbunden. [Details finden Sie hier](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
-   Wiederverwenden der ConnectionMultiplexer – kann nicht für jede Anforderung eine neue erstellen. Die `Lazy<ConnectionMultiplexer>` Muster [hier gezeigten](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) wird dringend empfohlen.
-   Redis funktioniert am besten mit kleineren Werten, erwägen Sie Sie vergrößern Daten in mehreren Tasten Zerkleinern. [Diese Diskussion Redis](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ)gilt 100kb "Groß". Lesen Sie [in diesem Artikel](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) für eine Beispiel-Problem, das durch große Werte verursacht werden kann.
-   Konfigurieren Sie Ihre [Einstellungen ThreadPool](#important-details-about-threadpool-growth) zur Vermeidung von Zeitlimit ein.
-   Verwenden Sie mindestens die standardmäßigen ConnectTimeout von 5 Sekunden ein. Diese Weise StackExchange.Redis genügend Zeit auf die Verbindung, bei einem Netzwerk Blip erneut herstellen kann.
-   Achten Sie darauf, dass der Leistung Kosten im Zusammenhang mit anderen Vorgänge, die ausgeführt werden. Beispielsweise die `KEYS` Befehl ist ein o(n)-Vorgang, und sollte vermieden werden. Die [Website redis.io](http://redis.io/commands/) enthält Details, um die Uhrzeit Komplexität für jeden Vorgang, der es unterstützt. Klicken Sie auf einen Befehl, um die Komplexität für jeden Vorgang angezeigt.

#### <a name="configuration-and-concepts"></a>Konfiguration und Konzepte

-   Verwenden Sie für genutzten Standard oder Premium in ein. Die grundlegende Ebene ist einem einzelnen Knotensystem mit keine Datenreplikation und keine Vereinbarung zum SERVICELEVEL. Verwenden Sie auch mindestens einen C1 Cache. C0 Caches sind für einfache Test-/Szenarien wirklich auffällt.
-   Denken Sie daran, dass Redis eines **In-Memory** -Datenspeicher ist. Lesen Sie [diesen Artikel](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) , damit Sie Szenarien kennen, in dem Datenverlust auftreten kann.
-   Entwickeln Sie Ihrem System so, dass sie Verbindung ahnen [aufgrund Patch und Failover](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md)behandelt werden kann.

#### <a name="performance-testing"></a>Testen der Leistung

-   Verwenden Sie zunächst `redis-benchmark.exe` einen Eindruck für mögliche Durchsatz abrufen, bevor Sie Ihre eigenen Perf überprüft. Beachten Sie, dass dieses Redis-Benchmarktests SSL, nicht unterstützt, sodass Sie [den Port ohne SSL über das Azure-Portal aktivieren](cache-configure.md#access-ports) , bevor Sie den Test ausgeführt wurde. Beispiele, finden Sie unter [wie lässt sich analysieren und testen die Leistung von meinem Cache?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   Der Client virtuellen Computer zu Testzwecken verwendet sollten in der gleichen Region wie Ihre Redis Cacheinstanz.
-   Wir empfehlen Dv2 virtueller Computer Reihe für Ihren Kunden verwenden, wie sie verfügen über eine bessere Hardware und die besten Ergebnisse erzielt werden können.
-   Vergewissern Sie sich, Ihren Kunden virtueller Computer, die Sie auswählen, hat mindestens viel computing und genutzt werden kann als Cache für die Sie testen. 
-   Aktivieren Sie VRSS auf dem Clientcomputer, wenn Sie unter Windows sind. [Details finden Sie hier](https://technet.microsoft.com/library/dn383582.aspx).
-   Premium Ebene Redis Instanzen werden besser Wartezeit und Durchsatz Netzwerk haben, weil sie auf eine bessere Hardware für CPU- und Netzwerk ausgeführt werden.

<a name="cache-redis-commands"></a>
### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Was sind einige der Aspekte, wenn Sie häufig verwendete Redis Befehle verwenden?

-   Sie sollten nicht bestimmte Redis Befehle ausführen Anspruch einen viel Zeit nehmen in Anspruch ohne Grundlegendes zu den Einfluss der folgenden Befehle.
    -   Führen Sie beispielsweise nicht den Befehl [Tasten](http://redis.io/commands/keys) in Herstellung wie lange, je nach der Anzahl der Schlüssel zurückgeben dauern kann. Redis ist ein Single-Thread-Server, und es nacheinander Befehle nacheinander verarbeitet. Wenn Sie andere Befehle nach Schlüssel ausgestellt haben, werden sie nicht bearbeitet werden, bis Redis der Tasten-Befehl ausgeführt. Die [Website redis.io](http://redis.io/commands/) enthält Details, um die Uhrzeit Komplexität für jeden Vorgang, der es unterstützt. Klicken Sie auf einen Befehl, um die Komplexität für jeden Vorgang angezeigt.
-   Verwende Key Größen – kleine Schlüsselwerte oder großen Schlüsselwerte ich? Im Allgemeinen hängt sie dem Szenario ein. Wenn Ihr Szenario größere Schlüssel erfordert können dann Anpassen der ConnectionTimeout und wiederholen die Werte und Anpassen Ihrer Logik "Wiederholen". Server im Hinblick auf Redis werden kleinere Werten beobachtet, haben Sie eine bessere Leistung.
-   Dies bedeutet nicht, dass Sie größere Werte in Redis speichern können. Sie müssen die folgenden Punkte beachten. Wartezeiten werden höhere. Wenn Sie über eine Reihe von Daten, die größer sind und eine, die kleiner ist, können mehrere Instanzen von ConnectionMultiplexer, jede mit einem anderen Satz von Timeout und wiederholen Sie den Werten konfiguriert ist, wie im vorherigen Abschnitt [Was mache StackExchange.Redis Konfigurationsoptionen](#cache-configuration) beschrieben.



<a name="cache-benchmarking"></a>
### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Wie kann ich testen die Leistung von meinem Cache und analysieren?

-   So Sie [Monitor](cache-how-to-monitor.md) die Integrität des Ihren Cache können zu [Cache Diagnose aktivieren](cache-how-to-monitor.md#enable-cache-diagnostics) . Sie können die Metrik Azure-Portal anzeigen, und Sie können auch [herunterladen und überprüfen](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) sie mithilfe der Tools von Ihrer Wahl.
-   Redis-benchmark.exe können Sie laden Test Ihrer Redis Server.
-   Stellen Sie sicher, dass die Last testen Client und den Cache Redis werden in der gleichen Region.
-   Redis-cli.exe verwenden und den Cache mit dem Befehl INFO zu überwachen.
-   Wenn Ihre Last hoher Speicherfragmentierung verursacht sollten Sie an eine größere Cachegröße von skalieren.
-   Anweisungen zum Herunterladen der Redis Tools finden Sie unter der [wie kann ich die Redis Befehle ausführen?](#cache-commands) Abschnitt.

So sieht ein Beispiel für Redis-benchmark.exe verwenden. Führen Sie diesen Befehl für genaue Ergebnisse eines virtuellen Computers in derselben Region als dem Cache.

-   Verwenden eine 1 k Nutzlast Test Pipelined festlegen Besprechungsanfragen

    Redis-benchmark.exe - h **Yourcache**. redis.cache.windows.net - ein **YourAccesskey** -t festlegen – n 1000000 -d 1024 - P 50
    
-   Test Pipelined erhalten anfordert, verwenden eine 1 k Nutzlast. 
    Hinweis: Die Menge test ausführen obigen zuerst zu Cache füllen
    
    Redis-benchmark.exe - h **Yourcache**. redis.cache.windows.net - ein **YourAccesskey** -t GET -n 1000000 - d 1024 - P 50

<a name="threadpool"></a>
### <a name="important-details-about-threadpool-growth"></a>Wichtige Details ThreadPool Variation

CLR-ThreadPool weist zwei Arten von Threads - "Worker" und "E/a-Abschlussport" (QuickInfos IOCP) Threads. 

-   Arbeitsthreads werden verwendet, wenn für Aspekten wie Verarbeitung `Task.Run(…)` oder `ThreadPool.QueueUserWorkItem(…)` Methoden. Diese Threads werden auch von verschiedenen Komponenten in der CLR verwendet, wenn die Arbeit an einem Hintergrund-Thread erfolgen muss.
-   IOCP Threads werden verwendet, wenn asynchrone EA geschieht (z. B. Lesen aus dem Netzwerk). 

Thread-Pool stellt neue Arbeitsthreads oder e/a-Abschlussthreads bei Bedarf (ohne Einschränkung), bis sie die Einstellung "Minimale" für jeden Thread erreicht. Standardmäßig ist die minimale Anzahl von Threads auf die Anzahl der Prozessoren auf einem System festgelegt. 

Nachdem Sie die Anzahl der vorhandenen (beschäftigt) Treffer threads, die der "minimale" Threadanzahl, ThreadPool wird, Barcode Rate, mit der ist Einschränkung neue Threads auf einen Thread pro 500 Millisekunden. Dies bedeutet, dass, wenn Ihr System einen Paketburst Arbeit benötigen einen IOCP Thread erhält, das Arbeit sehr schnell verarbeitet wird. Wenn die Burst Arbeit mehr als der Wert im Feld "Minimum" konfiguriert ist, werden es jedoch einige Verzögerung in Verarbeitung Teil der Arbeit während der ThreadPool auf eine der beiden folgenden Aktionen auftritt wartet.

1. Ein vorhandener Thread wird frei, um die Arbeit zu verarbeiten.
1. Keine vorhandener Thread wird 500 ms, kostenlos, damit ein neuer Thread erstellt wird.

Dies bedeutet, dass es, wenn die Anzahl der Threads beschäftigt Min Threads größer ist, Sie wahrscheinlich eine Verzögerung 500 ms bezahlen vor der Netzwerkdatenverkehr durch die Anwendung verarbeitet wird. Darüber hinaus ist es wichtig, zu beachten, dass, wenn Sie ein vorhandener Thread bleibt im Leerlauf für mehr als 15 Sekunden (basierend auf was ich Denken Sie daran), werden diese bereinigt und wiederholen Sie diesen Zyklus Wachstum und Schwindung kann.

Wenn eine Fehlermeldung Beispiel aus StackExchange.Redis Wir betrachten (1.0.450 erstellen oder höher), Sie sehen, dass jetzt ThreadPool Statistiken auf den Druck (IOCP und WORKER Informationen siehe unten).

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive, 
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0, 
    IOCP: (Busy=6,Free=994,Min=4,Max=1000), 
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

Im obigen Beispiel können Sie sehen, dass für IOCP Thread 6 beschäftigt Threads vorhanden sind und das System so konfiguriert ist, dass mindestens 4-Threads zulassen. In diesem Fall der Client würde haben wahrscheinlich gesehen zwei 500 ms verspätungen da 6 > 4.

Beachten Sie, dass StackExchange.Redis Zeitlimit erreicht werden können, wenn Wertzuwachs entweder IOCP oder ARBEITSKOLLEGEN Thread gedrosselt erhält.

### <a name="recommendation"></a>Empfehlungen

Mit diesen Informationen wird dringend empfohlen, dass Kunden den Wert minimale Konfiguration für IOCP und der WORKER auf einen Wert größer als den Standardwert festlegen. Wir können keine verleihen Stange Anleitungen was der Wert sein soll, da Sie der richtige Wert für eine Anwendung zu Höchst-Tiefst für eine andere Anwendung gehört. Mit dieser Einstellung kann auch die Leistung von anderen Teile des Anwendungen auswirken, sodass jeder Kunde diese Einstellung, damit ihre Bedürfnisse optimieren muss. Ein guter Ausgangspunkt ist 200 oder 300, testen und gibt je nach Bedarf.

So konfigurieren Sie diese Einstellung:

-   In ASP.NET, verwenden Sie die [Einstellung "MinIoThreads"][] unter den `<processModel>` Konfigurationselement in web.config. Wenn Sie innerhalb der WebSites Azure ausgeführt werden, wird diese Einstellung nicht durch die Konfigurationsoptionen verfügbar gemacht. Jedoch, Sie sollten weiterhin auf diese programmgesteuert fest (siehe unten) aus der Application_Start-Methode in global.asax.cs.

> **Wichtiger Hinweis:** in diesem Konfigurationselement angegebene Wert ist eine Einstellung *pro Core* . Beispielsweise, wenn Sie einen 4 Core-Computer haben und Ihre MinIOThreads Einstellung 200 zur Laufzeit sein soll, verwenden Sie `<processModel minIoThreads="50"/>`.

-   Verwenden Sie die [ThreadPool.SetMinThreads(...)](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) außerhalb von ASP.NET API.

<a name="server-gc"></a>
### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>Aktivieren der Server globalen Katalog auf Weitere Durchsatz auf dem Client erhalten, wenn StackExchange.Redis verwenden

Server globalen Katalog aktivieren kann, optimieren den Client und eine bessere Leistung und Durchsatz StackExchange.Redis nutzen können. Weitere Informationen zum globalen Katalog-Server und wie Sie es aktivieren finden Sie unter den folgenden Artikeln.

-   [So aktivieren Sie Server globalen Katalog](https://msdn.microsoft.com/library/ms229357.aspx)
-   [Grundlagen der Garbagecollection](https://msdn.microsoft.com/library/ee787088.aspx)
-   [Garbagecollection und Leistung](https://msdn.microsoft.com/library/ee851764.aspx)







<a name="cache-monitor"></a>
### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Wie werden die Gesundheit und Leistung von meinem Cache überwacht?

Microsoft Azure Redis Cache Instanzen können im [Portal Azure](https://portal.azure.com)überwacht werden. Sie können Kennzahlen anzeigen, Kennzahlen Diagramme, um die Startboard anheften, anpassen den Datums- und Zeitbereich der Überwachung Diagramme, hinzufügen und entfernen Kennzahlen aus der Diagramme, und Festlegen von Benachrichtigungen, wenn bestimmte Bedingungen erfüllt sind. Weitere Informationen finden Sie unter [Monitor Azure Redis Cache](cache-how-to-monitor.md).

Im Abschnitt **Support + Problembehandlung** des Redis Cache **Einstellungen** Blades enthält auch mehrere Tools für die Überwachung und Problembehandlung der Caches. 

-   **Behandeln von** Informationen zum häufige Probleme und Lösungsvorschläge.
-   **Überwachungsprotokolle** stellt Informationen bereit, klicken Sie auf Aktionen, die auf dem Cache ausgeführt. Sie können auch verwenden filtern, um diese Ansicht, um weitere Ressourcen zu erweitern.
-   **Ressource Gesundheit** Überwachungen die Ressource und Sie erfahren, wenn er wie erwartet ausgeführt wird. Weitere Informationen zum Dienst Gesundheit Azure Ressource finden Sie unter [Azure Ressourcenübersicht Dienststatus](../resource-health/resource-health-overview.md).
-   **Neue support-Anfragen** bietet Optionen, um eine Supportanfrage für Ihren Cache zu öffnen.

Diese Tools können Sie den Integritätsstatus Ihrer Azure Redis Cache Instanzen überwachen und können Sie Ihre Zwischenspeichern Applikationen verwalten. Weitere Informationen finden Sie unter [Support und Problembehandlung Einstellungen](cache-configure.md#support-amp-troubleshooting-settings).

### <a name="my-cache-diagnostics-storage-account-settings-changed-what-happened"></a>Meine Cache Diagnose Speicher kontoeinstellungen geändert, ist was passiert?

Caches in der gleichen Region und Abonnement Freigeben der gleichen Speicher-Diagnose, und ist die Konfiguration geänderte (Diagnose aktiviert/deaktiviert oder Ändern des Speicherkontos) gilt für alle Caches in das Abonnement, die in diesem Bereich sind. Wenn die Diagnose für Ihren Cache geändert haben, überprüfen Sie sehen, ob die Diagnose Einstellungen für einen anderen Cache in derselben Region und Abonnement geändert haben. Eine Möglichkeit besteht, zum Anzeigen der Überwachungsprotokolle für Ihren Cache für eine `Write DiagnosticSettings` Ereignis. Weitere Informationen zum Arbeiten mit Überwachungsprotokolle finden Sie unter [Anzeigen von Ereignissen und Überwachungsprotokolle](../monitoring-and-diagnostics/insights-debugging-with-events.md) und [Überwachen von Vorgängen mit Ressourcenmanager](../resource-group-audit.md). Weitere Informationen zum Überwachen von Ereignissen Azure Redis Cache finden Sie unter [Vorgänge und Benachrichtigungen](cache-how-to-monitor.md#operations-and-alerts).

### <a name="why-is-diagnostics-enabled-for-some-new-caches-but-not-others"></a>Warum ist Diagnose für einige neue Caches aber andere nicht aktiviert?

Caches in derselben Region und Abonnement Freigeben der gleichen Speicher-Diagnose. Wenn Sie einen neuen Cache in derselben Region und Abonnement als ein anderer Cache, der Diagnose aktiviert wurde erstellen, ist auf dem neuen Cache mit den gleichen Einstellungen Diagnose aktiviert.


<a name="cache-timeouts"></a>
### <a name="why-am-i-seeing-timeouts"></a>Warum erhalte ich Zeitlimit angezeigt?

Zeitlimit erfolgen im Client, mit denen Sie sprechen, um Redis zu erhalten. In den meisten Fällen liegt Redis Server kein Timeout. Wenn Sie ein Befehl auf dem Server Redis gesendet wird, der Befehl in die Warteschlange und Redis Server später nimmt den Befehl entgegen und ausgeführt. Ist der Client können Timeout während dieses Vorgangs und andernfalls wird eine Ausnahme jedoch auf der Seite einen ausgelöst. Weitere Informationen zur Behebung von Problemen mit Timeout finden Sie unter [Behandeln von Client-Seite](cache-how-to-troubleshoot.md#client-side-troubleshooting) und [StackExchange.Redis Timeout Ausnahmen](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).


<a name="cache-disconnect"></a>
### <a name="why-was-my-client-disconnected-from-the-cache"></a>Warum wurde mein Client aus dem Cache getrennt?

Es folgen einige allgemeine Grund für eine Cache trennen.

-   Clientseitige Ursachen
    -   Die Clientanwendung wurde erneut bereitgestellt.
    -   Die Clientanwendung hat einen Skalierung Vorgang ausgeführt.
        -   Wenn Cloud Services oder Web Apps kann dies durch automatische Skalierung sein.
    -   Der Netzwerke Layer clientseitig geändert.
    -   Vorübergehende Fehler im Client oder in den Netzplandiagramm-Knoten zwischen dem Client und auf dem Server.
    -   Die Bandbreite Schwellenwerte erreicht wurden.
    -   CPU-gebundene Vorgänge hat zu lange gedauert.
-   Serverseitige Ursachen
    -   Klicken Sie auf der standard-Cache Geschenk initiiert Azure Redis cachediensts einer Fail-Over vom primären Knoten auf den zweiten Knoten.
    -   Azure wurde die Instanz Patch, in dem der Cache bereitgestellt wurde
        -   Dies kann für Redis Serverupdates oder allgemeine virtueller Computer Wartung sein.







### <a name="which-azure-cache-offering-is-right-for-me"></a>Welche Azure Cache Angebot ist richtig für mich?

>[AZURE.IMPORTANT]Gemäß letztes Jahr [Ankündigung](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)wird Azure verwaltet Cachediensts und Azure In Rolle Cache Dienst auf 30 November 2016 deaktiviert werden. Wird unsere empfohlen, [Azure Redis Cache](https://azure.microsoft.com/services/cache/)verwenden. Informationen über das Migrieren finden Sie unter [Migrieren von Cachediensts Azure Redis Cache verwaltet](cache-migrate-to-redis.md).

### <a name="azure-redis-cache"></a>Azure Redis Cache
Azure Redis Cache ist im Allgemeinen in verfügbar bis zu 53 GB Größen und weist eine Verfügbarkeit Vereinbarung zum SERVICELEVEL von 99,9 %. Die neue [Ebene Premium](cache-premium-tier-intro.md) bietet Größen bis zu 530 GB und Support für Cluster, VNET und Beibehaltung, mit einem 99,9 % Vereinbarung zum SERVICELEVEL.

Azure Redis Cache bietet Kunden die Möglichkeit, verwenden Sie einen sicheren, dedizierten Redis Cache, von Microsoft verwaltet werden. Für dieses Angebot erhalten Sie die Rich-Funktionen und Netz von Redis, zuverlässigen Hostinganbieter und für die Überwachung von Microsoft bereitgestellt nutzen.

Im Gegensatz zu herkömmlichen Caches der Schlüssel-Wert-Paare nur behandelt, wird Redis meist für die hochgradig leistungsfähige Datentypen. Redis auch unterstützt die Ausführung von atomarer Operations mit diesen Typen, wie Anfügen an eine Zeichenfolge; der Wert in einem Hash erhöht; Drücken Sie nach einer Liste; Computing Schnittmenge, Union sowie die Differenz; oder erste das Element mit der höchsten Positionierung in einer Menge sortiert. Andere Features umfassen Unterstützung für Transaktionen, Pub/Sub, scripting, Schlüssel für eine begrenzte Time-to-live, Lua und Konfiguration Einstellungen wie mit einem herkömmlichen Cache Redis vornehmen.

Ein weiterer wichtiger Aspekt für Redis Erfolg wird die fehlerfrei und dynamische open-Source-Netz versehen erstellt. Dies wird in den unterschiedlichen Satz von Redis Clients verfügbar in mehreren Sprachen angezeigt. So können sie von nahezu jedem Arbeitsbelastung verwendet werden soll, wenn Sie innerhalb der Azure erstellen möchten. 

Weitere Informationen zu den ersten Schritten mit Azure Redis Cache finden Sie unter [verwenden Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md) und [Azure Redis Cache Dokumentation](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="managed-cache-service"></a>Verwaltete cachediensts
[Verwaltete Cache Dienst zu entfernenden 30 November 2016 festgelegt ist.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

### <a name="in-role-cache"></a>In der Rolle Cache
[In der Rolle Cache ist zu entfernenden 30 November 2016 festgelegt.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)





[Einstellung für die Konfiguration von "MinIoThreads"]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
