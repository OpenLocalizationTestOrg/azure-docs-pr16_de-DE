<properties
    pageTitle="Verwenden von Azure Redis Cache mit Node.js | Microsoft Azure"
    description="Erste Schritte mit Azure Redis Cache Node.js und Node_redis verwenden."
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Verwenden von Azure Redis Cache mit Node.js

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis Cache erhalten Sie zentralen Zugriff auf einen sicheren, dedizierten Redis Cache, von Microsoft verwaltet werden. Aus jeder Anwendung in Microsoft Azure zugegriffen werden der Cache.

In diesem Thema wird gezeigt, wie den ersten Schritten mit Azure Redis Cache Node.js verwenden. Ein weiteres Beispiel für die Verwendung von Azure Redis Cache mit Node.js finden Sie unter [Erstellen einer Node.js Chat-Anwendung mit Socket.IO auf einer Website Azure](../app-service-web/web-sites-nodejs-chat-app-socketio.md).


## <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie [Node_redis](https://github.com/mranney/node_redis)an:

    npm install redis

In diesem Lernprogramm verwendet [Node_redis](https://github.com/mranney/node_redis). Beispiele für die Verwendung von anderen Node.js-Clients finden Sie unter der einzelnen Dokumentation für die Node.js-Clients am [Node.js Redis Clients](http://redis.io/clients#nodejs)aufgeführt.

## <a name="create-a-redis-cache-on-azure"></a>Erstellen Sie einen Redis Cache auf Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Abrufen der Host Name und Access-Taste

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Verbinden Sie mit der Verwendung von SSL cache

Die neuesten Versionen von [Node_redis](https://github.com/mranney/node_redis) bieten Unterstützung für das Herstellen einer Verbindung mit Azure Redis Cache über SSL. Im folgenden Beispiel wird die Herstellung der Verbindung zum Azure Redis Cache mit den SSL-Endpunkt des 6380. Ersetzen Sie `<name>` mit dem Namen der dem Cache und `<key>` entweder mit der primären oder sekundären Schlüssel als beschrieben, im vorherigen Abschnitt [Host Name und Access Schlüssel abzurufen](#retrieve-the-host-name-and-access-keys) .

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Fügen Sie eines Beitrags zu den Cache hinzu und Abrufen

Im folgende Beispiel wird gezeigt, wie zum Verbinden mit einer Instanz Azure Redis Cache speichern und Abrufen eines Elements aus dem Cache werden kann. Weitere Beispiele für die Verwendung von Redis mit dem [Node_redis](https://github.com/mranney/node_redis) -Client finden Sie unter [http://redis.js.org/](http://redis.js.org/).

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
    
    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });
    
    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Ergebnis:

    OK
    value


## <a name="next-steps"></a>Nächste Schritte

- So Sie [Monitor](cache-how-to-monitor.md) die Integrität des Ihren Cache können zu [Cache Diagnose aktivieren](cache-how-to-monitor.md#enable-cache-diagnostics) .
- Lesen Sie die offizielle [Redis Dokumentation](http://redis.io/documentation).



