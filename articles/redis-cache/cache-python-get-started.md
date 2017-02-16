<properties
    pageTitle="Verwenden von Azure Redis Cache mit Python | Microsoft Azure"
    description="Erste Schritte mit Azure Redis Cache Python verwenden"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/16/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-python"></a>Verwenden von Azure Redis Cache mit Python

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

In diesem Thema wird gezeigt, wie den ersten Schritten mit Azure Redis Cache Python verwenden.


## <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie [Redis-hierhin](https://github.com/andymccurdy/redis-py).


## <a name="create-a-redis-cache-on-azure"></a>Erstellen Sie einen Redis Cache auf Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Abrufen der Host Name und Access-Taste

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Aktivieren Sie den Endpunkt nicht SSL

Einige Redis-Clients nicht unterstützen SSL und werden standardmäßig die [nicht-SSL-Anschluss für neue Instanzen von Azure Redis Cache deaktiviert ist](cache-configure.md#access-ports). Zum Zeitpunkt der Erstellung dieses Dokuments wird nicht der [Redis-hierhin](https://github.com/andymccurdy/redis-py) Client SSL unterstützen. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Fügen Sie eines Beitrags zu den Cache hinzu und Abrufen


    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Ersetzen Sie `<name>` mit Ihrem Namen Cache und `key` mit Ihrer Access-Taste.


<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
