<properties 
    pageTitle="Beispiele für Azure Cache Redis | Microsoft Azure" 
    description="Informationen Sie zum Verwenden von Azure Redis Cache" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-samples"></a>Beispiele für Azure Redis Cache 

Dieses Thema enthält eine Liste Azure Redis Cache Beispiele, darüber liegendem Szenarien wie das Herstellen einer Verbindung mit einer Cache, lesen und Schreiben von Daten aus einem Cache, und verwenden die ASP.NET Redis Cache Anbieter. Einiger Beispiele sind herunterladbare Projekte und einige schrittweise Anleitung bereitstellen und Codeausschnitte enthalten, aber nicht zu einem Projekt herunterladbaren linken.

## <a name="hello-world-samples"></a>Hallo Welt Beispiele

In den Beispielen in diesem Abschnitt werden die Grundlagen zum Herstellen einer Verbindung mit einer Instanz Azure Redis Cache lesen und Schreiben von Daten in den Cache mithilfe einer Vielzahl von Sprachen angezeigt und Redis Clients.

Im Beispiel [Hallo Welt](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) gezeigt, wie verschiedene Cache Aktionen, die mit dem [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET Client ausführen.

Dieses Beispiel zeigt, wie Sie:

-   Verwenden Sie verschiedene Verbindungsoptionen
-   Lesen und Schreiben von Objekten zu und aus dem Cache mit synchroner als auch asynchrone Vorgänge
-   Verwenden Sie MGET/MSET Redis Befehle in Rückgabewerte von bestimmter Tasten
-   Redis Transaktionen Operationen
-   Arbeiten Sie mit Listen Redis und sortierte Mengen angezeigt werden
-   Speichern von .NET Objekte JsonConvert Serialisierungsprogramme verwenden
-   Verwenden Sie Redis Sätze tagging implementiert wird
-   Arbeiten mit Redis Cluster

Weitere Informationen finden Sie in der Dokumentation [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) auf Github und weitere Verwendung Szenarien finden Sie unter der [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) Unit Tests.

Azure Redis Cache mit Python verwenden wird gezeigt, [wie](cache-python-get-started.md) mit Azure Redis Cache Schritte Python und [Redis hierhin](https://github.com/andymccurdy/redis-py) Client verwenden.

[Arbeiten mit .NET Objekte im Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) wird gezeigt, eine Methode, um .NET Objekte serialisieren, damit Sie können diese schreiben und aus einer Instanz Azure Redis Cache zu lesen. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>Verwenden von Redis Cache als eine Skala, Rückwandplatine für ASP.NET SignalR

Das [Verwenden Redis Cache als eine Skala, Rückwandplatine für ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) Beispiel wird veranschaulicht, wie als Rückenwandplatine SignalR Azure Redis Cache verwendet werden kann. Weitere Informationen zu Rückwandplatine finden Sie unter [SignalR Scaleout mit Redis](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="redis-cache-customer-query-sample"></a>Redis Cache Kunden Abfrage Stichprobe

In diesem Beispiel veranschaulicht vergleicht Leistung zwischen den Zugriff auf Daten aus einem Cache und den Zugriff auf Daten aus dem dauerhaften Speicher. In diesem Beispiel sind zwei Projekte.

-   [Demo wie Redis Cache Leistung verbessern können, indem Sie Zwischenspeichern von Daten](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
-   [Startwert für die Datenbank und den Cache für die demo](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>ASP.NET Session State und Zwischenspeichern der Ausgabe

Das [Verwenden von Azure Redis Cache zum Speichern von ASP.NET SessionState und OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) Beispiel wird veranschaulicht, wie Sie Azure Redis Cache zum Speichern von ASP.NET Sitzung und Ausgabecache mithilfe der SessionState und OutputCache-Anbieter für Redis verwenden.

## <a name="manage-azure-redis-cache-with-maml"></a>Verwalten von Azure Redis Cache mit MAML

Das [Verwalten Azure Redis Cache mithilfe von Azure Management Bibliotheken](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) Beispiel veranschaulicht, wie können Sie Azure Management Bibliotheken zum Verwalten von - (erstellen / aktualisieren / löschen) dem Cache. 

## <a name="custom-monitoring-sample"></a>Benutzerdefinierte überwachen (Beispiel)

Das [Daten mit Access Redis Cache Überwachung](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) Beispiel wird veranschaulicht, wie Sie die Überwachung Daten für Ihren Azure Redis Cache außerhalb der Azure-Portal zugreifen können.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>Einen Twitter-Schreibweise Klonen mithilfe von PHP und Redis geschrieben

Im Beispiel [Retwis](https://github.com/SyntaxC4-MSFT/retwis) ist die Redis Hallo Welt. Es ist eine minimale Twitter-Schreibweise sozialer klonen, die mit Redis und PHP über den Client [Predis](https://github.com/nrk/predis) geschrieben. Der Quellcode soll sehr einfach sein und zur gleichen Zeit zum Anzeigen von anderen Redis Datenstrukturen.

## <a name="bandwidth-monitor"></a>Bandbreite monitor

Im Beispiel [Bandbreite Monitor](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) ermöglicht Ihnen, die auf dem Client verwendete Bandbreite zu überwachen. Um die Bandbreite messen, führen Sie das Beispiel auf dem Clientcomputer Cache, nehmen Sie Anrufe an den Cache und beobachten Sie die Bandbreite von der Bandbreite Monitor Stichprobe gemeldeten.
