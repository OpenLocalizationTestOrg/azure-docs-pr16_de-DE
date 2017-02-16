<properties 
    pageTitle="Konfigurieren von Redis für einen Premium Azure Redis Cache Cluster | Microsoft Azure" 
    description="Informationen Sie zum Erstellen und Verwalten von Redis für Ihre Premium Ebene Azure Redis Cache Instanzen Cluster" 
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
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-redis-clustering-for-a-premium-azure-redis-cache"></a>Konfigurieren von Redis für einen Premium Azure Redis Cache Cluster
Azure Redis Cache weist verschiedene Cache Angebote, die Flexibilität bei der Wahl der Cachegröße und Funktionen, einschließlich der neuen Premium Stufe zur Verfügung zu stellen.

Die Azure Redis Cache Premium Stufe umfasst Features wie Cluster, Beibehaltung und virtual Network-Unterstützung. Dieser Artikel beschreibt, wie in einer Premium Azure Redis Cache Instanz Cluster konfigurieren.

Informationen zu anderen Premium Cachefeatures finden Sie unter [Einführung in die Ebene Azure Redis Cache Premium](cache-premium-tier-intro.md).

## <a name="what-is-redis-cluster"></a>Was ist Redis Cluster?
Azure Redis Cache bietet Redis Cluster als [in Redis implementiert](http://redis.io/topics/cluster-tutorial). Redis Cluster bietet Ihnen die folgenden Vorteile. 

-   Die Möglichkeit, das Dataset zwischen mehreren Knoten automatisch zu teilen. 
-   Die Möglichkeit, die Vorgänge, wenn eine Teilmenge der Knoten ist ein Fehler aufgetreten oder sind nicht mehr mit den Rest der Cluster kommunizieren. 
-   Weitere Durchsatz: Durchsatz linear erhöht, wenn Sie die Anzahl der mehrere Shards hinweg erhöhen. 
-   Weitere Arbeitsspeichergröße: linear erhöht, wenn Sie die Anzahl der mehrere Shards hinweg erhöhen.  

Finden Sie weitere Details zum Größe, Durchsatz und Bandbreite mit Premium Caches der [Azure Redis Cache häufig gestellte Fragen](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) . 

In Azure wird Redis Cluster als primär/Replikat Modell angeboten, bei denen jede Shard ein paar primär/Replikat mit Replikation ist, auf dem die Replikation durch Azure Redis cachediensts verwaltet. 

## <a name="clustering"></a>Cluster
Cluster wird auf dem **Neuen Redis Cache** Blade während der Erstellung des Caches für aktiviert. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Cluster wird auf dem Blade **Redis Cluster** konfiguriert.

![Cluster][redis-cache-clustering]

Sie können bis zu 10 mehrere Shards hinweg im Cluster haben. Klicken Sie auf **aktiviert** und schieben Sie den Schieberegler oder geben Sie eine Zahl zwischen 1 und 10 für **Shard zählen** , und klicken Sie auf **OK**.

Jede Shard ist ein primär/Replikat Cache Paar von Azure verwaltet werden, und die Gesamtgröße des Caches wird berechnet, indem Sie die Anzahl der mehrere Shards hinweg multiplizieren, indem Sie die Cachegröße in der Preisgestaltung Ebene ausgewählt. 

![Cluster][redis-cache-clustering-selected]

Nachdem der Cache erstellt wurde Sie damit eine Verbindung herstellen, und verwenden sie einfach wie einen nicht gruppierten Cache und Redis werden die Daten in den Cache mehrere Shards hinweg verteilen. Wenn Diagnose [aktiviert](cache-how-to-monitor.md#enable-cache-diagnostics)ist, Kennzahlen separat für jedes Shard erfasst werden, und können in den Cache Redis Blade [angezeigt](cache-how-to-monitor.md) werden. 

Beispiel-Code zum Arbeiten mit Cluster mit dem StackExchange.Redis-Client finden Sie unter den [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) Teil des Beispiels [Hallo Welt](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) .

<a name="cluster-size"></a>
## <a name="change-the-cluster-size-on-a-running-premium-cache"></a>Ändern der Clustergröße auf fortlaufendes Premium Cache

So ändern Sie die Größe der Zuordnungseinheiten auf fortlaufendes Premium Cache mit Cluster aktiviert ist, klicken Sie auf **(PREVIEW) Redis Clustergröße** aus dem Blade **Einstellungen** .

>[AZURE.NOTE] Beachten Sie, dass während die Ebene Azure Redis Cache Premium zur allgemeinen Verfügbarkeit gebracht freigegeben wurde, das Größe der Zuordnungseinheiten Redis Feature derzeit in der Vorschau.

![Redis Clustergröße][redis-cache-redis-cluster-size]

Um die Clustergröße zu ändern, verwenden Sie den Schieberegler oder geben Sie eine Zahl zwischen 1 und 10 in das Textfeld **Shard zählen** , und klicken Sie auf **OK** , um zu speichern.

## <a name="clustering-faq"></a>Häufig gestellte Fragen zu Cluster

Die folgende Liste enthält Antworten auf häufig gestellte Fragen zur Azure Redis Cache Cluster.

-   [Muss ich meine Clientanwendung Cluster verwenden ändern?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
-   [Wie werden die Tasten in einem Cluster verteilt?](#how-are-keys-distributed-in-a-cluster)
-   [Was ist der größte Cachegröße, die erstellt werden können?](#what-is-the-largest-cache-size-i-can-create)
-   [Unterstützen alle Redis-Clients Cluster?](#do-all-redis-clients-support-clustering)
-   [Wie schließe ich meine Cache an, wenn Cluster aktiviert ist?](#how-do-i-connect-to-my-cache-when-clustering-is-enabled)
-   [Kann ich direkt auf die einzelnen mehrere Shards hinweg Meine Cache Verbindung herstellen?](#can-i-directly-connect-to-the-individual-shards-of-my-cache)
-   [Kann ich für einen zuvor erstellte Cache Cluster konfigurieren?](#can-i-configure-clustering-for-a-previously-created-cache)
-   [Kann ich für einen grundlegenden oder Cache Cluster konfigurieren?](#can-i-configure-clustering-for-a-basic-or-standard-cache)
-   [Kann ich mit den Anbietern Redis ASP.NET Session State und Zwischenspeichern der Ausgabe Cluster verwenden?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)
-   [Ich erhalte bei Verwendung von StackExchange.Redis Ausnahmen verschieben und Cluster, was kann ich tun?](#i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do)

### <a name="do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering"></a>Muss ich meine Clientanwendung Cluster verwenden ändern?

-   Wenn Cluster aktiviert ist, steht nur die Datenbank 0. Wenn Ihre Clientanwendung mehrere Datenbanken verwendet, und es versucht Lese-oder Schreibzugriff auf eine andere Datenbank als 0, wird die folgende Ausnahme ausgelöst. `Unhandled Exception: StackExchange.Redis.RedisConnectionException: ProtocolFailure on GET --->` `StackExchange.Redis.RedisCommandException: Multiple databases are not supported on this server; cannot switch to database: 6`

    Weitere Informationen finden Sie unter [Redis Clusterspezifikation - Teilmenge implementiert](http://redis.io/topics/cluster-spec#implemented-subset).

-   Wenn Sie [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/)verwenden, müssen Sie 1.0.481 verwenden oder höher. Sie Verbinden mit Cache mithilfe derselben [Endpunkte, Ports, und die Tasten](cache-configure.md#properties) , mit denen Sie bei einer Verbindung mit einem Cache, die keinen Cluster aktiviert. Der einzige Unterschied ist, dass alle Lese- und schreibt mit 0-Datenbank ausgeführt werden müssen.
    -   Andere Clients möglicherweise andere systemvoraussetzungen. Finden Sie unter [Alle Redis-Clients unterstützen Cluster?](#do-all-redis-clients-support-clustering)
-   Wenn die Anwendung mehrere wichtige Vorgänge in einem einzigen Befehl zusammengefasst verwendet, müssen alle Tasten in der gleichen Shard befinden. Um dies zu erreichen, finden Sie unter [wie Tasten verteilt werden in einem Cluster?](#how-are-keys-distributed-in-a-cluster)
-   Wenn Sie Redis ASP.NET Session State Provider verwenden müssen, verwenden Sie 2.0.1 oder höher. Finden Sie unter [kann ich mithilfe von Cluster mit den Anbieter Redis ASP.NET Session State und Zwischenspeichern der Ausgabe?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)

### <a name="how-are-keys-distributed-in-a-cluster"></a>Wie werden die Tasten in einem Cluster verteilt?

Pro der Dokumentation Redis [Tasten Verteilung Modell](http://redis.io/topics/cluster-spec#keys-distribution-model) : der wichtige Platz ist in 16384 Steckplätze aufteilen. Jeder Schlüssel wird gehasht und einen dieser Steckplätze, die in den Knoten im Cluster verteilt sind zugewiesen. Sie können konfigurieren, welcher Teil der Schlüssel gehasht ist, um sicherzustellen, dass mehrere Tasten in der gleichen Shard mithilfe von Kategorien Hash befinden.

-   Mit einer Kategorie Hash - Tastenkombinationen, wenn Sie einen beliebigen Teil der Schlüssel in eingeschlossen ist `{` und `}`, nur diesen Teil der Schlüssel ist im Sinne bestimmen den Hash Slot eines Schlüssels gehasht. Beispielsweise würde die folgenden 3 Tasten in der gleichen Shard ansässig: `{key}1`, `{key}2`, und `{key}3` da nur die `key` Teil des Namens gehasht ist. Eine vollständige Liste der Tasten Hash Kategorie Spezifikationen finden Sie unter [Schlüssel Hashing Kategorien](http://redis.io/topics/cluster-spec#keys-hash-tags).
-   Tasten ohne ein Hash Tag - der Name des gesamten wird für das hashing verwendet. Das Ergebnis einer statistisch gleichmäßige Verteilung über die mehrere Shards hinweg des Caches.

Für optimale Leistung und Durchsatz empfehlen wir die Tasten gleichmäßig verteilen. Bei Verwendung von Tasten mit eine Hash Kategorie hat Zuständigkeit der Anwendung, um sicherzustellen, dass die Tasten werden gleichmäßig verteilte.

Weitere Informationen finden Sie unter [Schlüssel Verteilung Modell](http://redis.io/topics/cluster-spec#keys-distribution-model), [Redis Cluster Daten Sharding](http://redis.io/topics/cluster-tutorial#redis-cluster-data-sharding)und [Tasten Hashing Kategorien](http://redis.io/topics/cluster-spec#keys-hash-tags).

Beispiel-Code zum Arbeiten mit Cluster, und Suchen von Tasten in der gleichen Shard mit dem StackExchange.Redis-Client finden Sie unter den [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) Teil des Beispiels [Hallo Welt](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) .

### <a name="what-is-the-largest-cache-size-i-can-create"></a>Was ist die größte Cachegröße, die erstellt werden können?

Die größte Premium Cachegröße ist 53 GB. Sie können bis zu 10 gibt Ihnen eine maximale Größe von 530 GB mehrere Shards hinweg erstellen. Wenn Sie einen größeren benötigen, können Sie [Weitere Anforderung](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase). Weitere Informationen finden Sie unter [Azure Redis Cache Preise](https://azure.microsoft.com/pricing/details/cache/).

### <a name="do-all-redis-clients-support-clustering"></a>Unterstützen alle Redis-Clients Cluster?

Derzeit noch nicht alle Clients unterstützen Redis Cluster. StackExchange.Redis ist ein, die für die es nicht unterstützt. Finden Sie für Weitere Informationen zu anderen Clients im [Bereich mit den Cluster](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) Abschnitt der [Cluster Lernprogramm Redis](http://redis.io/topics/cluster-tutorial).

>[AZURE.NOTE] Wenn Sie als Ihren Kunden StackExchange.Redis verwenden, stellen Sie sicher, dass Sie die neueste Version von [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/) 1.0.481 oder höher für Cluster verwenden werden, damit Sie ordnungsgemäß funktioniert. Wenn Sie Probleme mit verschieben Ausnahmen haben, finden Sie unter [Verschieben von Ausnahmen](#move-exceptions) für Weitere Informationen.

### <a name="how-do-i-connect-to-my-cache-when-clustering-is-enabled"></a>Wie schließe ich meine Cache an, wenn Cluster aktiviert ist?

Sie können eine Verbindung herstellen, um Ihren Cache mit denselben [Endpunkte, Ports, und die Tasten](cache-configure.md#properties) , mit denen Sie beim Herstellen einer Verbindung zu einem Cache, die keinen Cluster aktiviert. Redis verwaltet die Cluster im Back-End, damit Sie sie aus Ihren Kunden verwalten besitzen.

### <a name="can-i-directly-connect-to-the-individual-shards-of-my-cache"></a>Kann ich direkt auf die einzelnen mehrere Shards hinweg Meine Cache Verbindung herstellen?

Dies ist formal nicht unterstützt. Besteht aus jeder Shard, unter dem Gesichtspunkt sind ein Paar der primären/Replikat-Cache, die als Cacheinstanz gemeinsam bekannt sind. Sie können mit dieser Cacheinstanzen mit dem Redis Cli Programm in den [instabil](http://redis.io/download) Zweig des Redis Repository am GitHub herstellen. Diese Version implementiert grundlegende Unterstützung beim Start mit der `-c` wechseln. Weitere Informationen finden Sie im [Bereich mit den Cluster](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) auf [http://redis.io](http://redis.io) in der [Redis Cluster Lernprogramm](http://redis.io/topics/cluster-tutorial).

Verwenden Sie für nicht-Ssl die folgenden Befehle aus.

    Redis-cli.exe –h <<cachename>> -p 13000 (to connect to instance 0)
    Redis-cli.exe –h <<cachename>> -p 13001 (to connect to instance 1)
    Redis-cli.exe –h <<cachename>> -p 13002 (to connect to instance 2)
    ...
    Redis-cli.exe –h <<cachename>> -p 1300N (to connect to instance N)

Ersetzen Sie für Ssl, `1300N` mit `1500N`.

### <a name="can-i-configure-clustering-for-a-previously-created-cache"></a>Kann ich für einen zuvor erstellte Cache Cluster konfigurieren?

Zurzeit können Sie nur aktivieren Cluster, wenn Sie einen Cache erstellen. Sie können die Größe der Zuordnungseinheiten ändern, nachdem der Cache wird erstellt, aber keine Cluster an einen Premium Cache hinzufügen oder Entfernen von Cluster aus einem Premium Cache nach der Erstellung des Caches. Ein Cache Premium mit Cluster aktiviert und nur eine Shard unterscheidet sich ein Cache Premium mit keine Cluster dieselbe Größe.

### <a name="can-i-configure-clustering-for-a-basic-or-standard-cache"></a>Kann ich für einen grundlegenden oder Cache Cluster konfigurieren?

Cluster ist nur für Premium Caches verfügbar.

### <a name="can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers"></a>Kann ich mit den Anbietern Redis ASP.NET Session State und Zwischenspeichern der Ausgabe Cluster verwenden?

-   **Redis Ausgabecache Anbieter** - keine Änderungen erforderlich.
-   **Redis Session State Provider** - Cluster, verwenden Sie [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 verwenden müssen oder höher oder eine Ausnahme ausgelöst wird. Dies ist eine Änderung abgeschnitten werden. Weitere Informationen finden Sie unter [Version 2.0.0 Bruchfestigkeit ändern Details](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

<a name="move-exceptions"></a>
### <a name="i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do"></a>Ich erhalte bei Verwendung von StackExchange.Redis Ausnahmen verschieben und Cluster, was kann ich tun?

Wenn Sie StackExchange.Redis verwenden und erhalten `MOVE` Ausnahmen bei Verwendung von Cluster, stellen Sie sicher, dass Sie [StackExchange.Redis 1.1.603](https://www.nuget.org/packages/StackExchange.Redis/) arbeiten oder höher. Informationen zum Konfigurieren von Ihren StackExchange.Redis verwenden finden Sie unter [Konfigurieren der Cache Clients](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie mehr Premium Cachefeatures verwenden.

-   [Einführung in die Ebene Azure Redis Cache Premium](cache-premium-tier-intro.md)
  
<!-- IMAGES -->

[redis-cache-clustering]: ./media/cache-how-to-premium-clustering/redis-cache-clustering.png

[redis-cache-clustering-selected]: ./media/cache-how-to-premium-clustering/redis-cache-clustering-selected.png

[redis-cache-redis-cluster-size]: ./media/cache-how-to-premium-clustering/redis-cache-redis-cluster-size.png







