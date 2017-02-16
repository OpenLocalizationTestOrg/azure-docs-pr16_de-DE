<properties
   pageTitle="Verwenden von Azure Redis Cache mit Java | Microsoft Azure"
    description="Erste Schritte mit Azure Redis Cache mit Java"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor=""/>

<tags
    ms.service="cache"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/24/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-java"></a>Verwenden von Azure Redis Cache mit Java

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Cache Redis erhalten, die Sie auf eine spezielle Zugriff, Redis Cache für Dokumente, die von Microsoft verwaltet werden. Aus jeder Anwendung in Microsoft Azure zugegriffen werden der Cache.

In diesem Thema wird gezeigt, wie den ersten Schritten mit Azure Redis Cache Java verwenden.

## <a name="prerequisites"></a>Erforderliche Komponenten

[Jedis](https://github.com/xetorthio/jedis) - Java-Client für Redis

In diesem Lernprogramm verwendet Jedis, jedoch können Sie alle Java-Client bei [http://redis.io/clients](http://redis.io/clients)aufgeführt.

## <a name="create-a-redis-cache-on-azure"></a>Erstellen Sie einen Redis Cache auf Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Abrufen der Host Name und Access-Taste

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Aktivieren Sie den Endpunkt nicht SSL

Einige Redis-Clients nicht unterstützen SSL und werden standardmäßig die [nicht-SSL-Anschluss für neue Instanzen von Azure Redis Cache deaktiviert ist](cache-configure.md#access-ports). Zum Zeitpunkt der Erstellung dieses Dokuments wird nicht der Client [Jedis](https://github.com/xetorthio/jedis) SSL unterstützen. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]




## <a name="add-something-to-the-cache-and-retrieve-it"></a>Fügen Sie eines Beitrags zu den Cache hinzu und Abrufen

    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Nächste Schritte

- So Sie [Monitor](https://msdn.microsoft.com/library/azure/dn763945.aspx) die Integrität des Ihren Cache können zu [Cache Diagnose aktivieren](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) .
- Lesen Sie die offizielle [Redis Dokumentation](http://redis.io/documentation).

