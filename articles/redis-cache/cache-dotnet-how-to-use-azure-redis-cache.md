<properties 
    pageTitle="So Azure Redis Cache verwenden | Microsoft Azure" 
    description="Erfahren Sie, wie steigern die Leistung Azure Anwendungen mit Azure Redis Cache" 
    services="redis-cache,app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="dotnet" 
    ms.topic="hero-article" 
    ms.date="08/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache"></a>So Azure Redis Cache verwenden

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

In diesem Handbuch wird gezeigt, wie die ersten Schritte mit **Azure Redis Cache**. Microsoft Azure Redis Cache basiert auf der gängiger open Source Redis Cache. Sie erhalten Sie zentralen Zugriff auf einen sicheren, dedizierten Redis Cache, von Microsoft verwaltet werden. Ein Cache erstellt Azure Redis Cache kann aus einer beliebigen Anwendung in Microsoft Azure zugegriffen werden.

Microsoft Azure Redis Cache ist in den folgenden Ebenen verfügbar:

-   **Grundlegende** – einzelnen Knoten. Vielfache Größen bis zu 53 GB.
-   **Standard** – zwei Knoten primär/Replikat. Vielfache Größen bis zu 53 GB. 99,9 VEREINBARUNG ZUM SERVICELEVEL %.
-   **Premium** – zwei Knoten primär/Replikat mit bis zu 10 mehrere Shards hinweg. Mehrere Größen von 6 GB zu 530 GB (wenden Sie sich an uns Weitere). Alle Ebenen Standard-Funktionen und weitere einschließlich Unterstützung für [Redis Cluster](cache-how-to-premium-clustering.md), [dauerhaften Redis](cache-how-to-premium-persistence.md)und [Azure-virtuellen Netzwerk](cache-how-to-premium-vnet.md). 99,9 VEREINBARUNG ZUM SERVICELEVEL %.

Pro Ebene unterscheidet sich in Bezug auf die Features und Preise. Informationen zu Preisen finden Sie unter [Cache Preise Details][].

In diesem Handbuch wird gezeigt, wie die mit C [StackExchange.Redis][] Kunde\# Code. Die Szenarios dieser gehören **Erstellen und Konfigurieren eines Caches**, **Cache Clients konfigurieren**und **Hinzufügen und Entfernen von Objekten aus dem Cache**. Weitere Informationen zur Verwendung von Azure Redis Cache finden Sie im Abschnitt [Nächste Schritte][] . Ein schrittweises Lernprogramm der Erstellung einer ASP.NET-MVC Web app mit Redis Cache finden Sie unter [So erstellen Sie eine Web-App mit Redis Cache](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>
## <a name="get-started-with-azure-redis-cache"></a>Erste Schritte mit Azure Redis Cache

Erste Schritte mit Azure Redis Cache ist einfach. Um anzufangen, bereitgestellt und einen Cache konfigurieren. Als Nächstes konfigurieren Sie die Cache-Clients, damit er auf den Cache zugreifen können. Sobald die Cache-Clients konfiguriert sind, können Sie mit ihnen arbeiten beginnen.

-   [Erstellen Sie den cache][]
-   [Konfigurieren der Cache-clients][]

<a name="create-cache"></a>
## <a name="create-a-cache"></a>Erstellen Sie einen cache

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Zugriff auf Ihr Cache dahinter wird erstellt

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Weitere Informationen zum Konfigurieren von Ihren Caches finden Sie unter [Konfigurieren von Azure Redis Cache](cache-configure.md).

<a name="NuGet"></a>
## <a name="configure-the-cache-clients"></a>Konfigurieren der Cache-clients

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Nachdem Sie Ihr Clientprojekt zum Zwischenspeichern konfiguriert ist, können Sie die in den folgenden Abschnitten für das Arbeiten mit dem Cache beschriebenen Verfahren verwenden.

<a name="working-with-caches"></a>
## <a name="working-with-caches"></a>Arbeiten mit Caches

Die Schritte in diesem Abschnitt beschreiben, wie Sie allgemeine Aufgaben mit Cache ausführen.

-   [Verbinden mit dem cache][]
-   [Hinzufügen und Abrufen von Objekten aus dem cache][]
-   [Arbeiten Sie mit .NET Objekte im cache](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>
## <a name="connect-to-the-cache"></a>Verbinden mit dem cache

Mit einem Cache programmgesteuert arbeiten möchten, benötigen Sie einen Verweis auf den Cache. Fügen Sie die folgenden an den Anfang aller Dateien aus der Sie den StackExchange.Redis-Client verwenden, um eine Azure Redis Cache zugreifen möchten.

    using StackExchange.Redis;

>[AZURE.NOTE] Der StackExchange.Redis-Client erfordert .NET Framework 4 oder höher.

Die Verbindung zum Azure Redis Cache vom verwaltet wird der `ConnectionMultiplexer` Class. Diese Klasse freigegeben und in der Clientanwendung wiederverwendet werden soll, und muss nicht auf Basis pro Vorgang erstellt werden. 

Um eine Verbindung mit einer Azure Redis Cache und eine Instanz von einem verbundenen zurückgegeben werden `ConnectionMultiplexer`, rufen Sie die statische `Connect` Methode, und übergeben den Cache Endpunkt und die Taste wie im folgenden Beispiel wird. Verwenden Sie die Taste vom Azure-Portal als Kennwortparameter generiert.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] Warnung: Nie Store-Anmeldeinformationen im Quellcode. Um dieses Beispiel einfach zu halten, erhalte ich diese im Quellcode angezeigt. Informationen zum Speichern von Anmeldeinformationen finden Sie unter [wie Anwendung Zeichenfolgen und Verbindung Zeichenfolgen Arbeit][] .

Wenn Sie nicht SSL verwenden möchten, legen Sie entweder `ssl=false` oder Auslassen der `ssl` Parameter.

>[AZURE.NOTE] Der Port nicht SSL ist standardmäßig für neue Caches deaktiviert. Anweisungen zum Aktivieren des nicht SSL-Anschluss finden Sie unter die [Access-Ports](cache-configure.md#access-ports)...

Eine Möglichkeit zum Freigeben einer `ConnectionMultiplexer` Instanz in Ihrer Anwendung besteht darin, eine statische Eigenschaft, die eine verbundene Instanz, ähnlich wie im folgenden Beispiel gibt. Auf diese Weise Thread-sicher nur ein einzelnes verbunden Initialisierung `ConnectionMultiplexer` Instanz. In diesen Beispielen `abortConnect` auf False, was bedeutet, dass der Anruf erfolgreich ist, auch wenn keine Verbindung zum Azure Redis Cache hergestellt wird festgelegt ist. Eine der wichtigsten Features von `ConnectionMultiplexer` besteht darin, dass Connectivity automatisch an den Cache einmal das Netzwerkproblem wiederherstellen wird oder andere Ursachen aufgelöst werden.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

Weitere Informationen zu erweiterten Konfiguration Verbindungsoptionen finden Sie unter [StackExchange.Redis Konfigurationsmodell][].

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Nachdem die Verbindung hergestellt wurde, einen Verweis auf die Redis-Cache-Datenbank zurückkehren, indem die `ConnectionMultiplexer.GetDatabase` Methode. Das Objekt, das zurückgegeben wird, aus der `GetDatabase` Methode wird ein einfaches Pass-Through-Objekt und nicht gespeichert werden müssen.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Nun wissen, wie Sie eine Verbindung mit einer Instanz Azure Redis Cache und den Bezug zu der Cachedatenbank zurückgeben, werfen Sie einen Blick auf das Arbeiten mit dem Cache.

<a name="add-object"></a>
## <a name="add-and-retrieve-objects-from-the-cache"></a>Hinzufügen und Abrufen von Objekten aus dem cache

Elemente können in gespeichert werden und aus einem Cache abgerufen werden, mithilfe der `StringSet` und `StringGet` Methoden.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis Stores die meisten Daten als Zeichenfolgen Redis, aber diese Zeichenfolgen enthalten können viele Arten von Daten, einschließlich serialisierte binäre Daten, die für die Speicherung von .NET Objekte im Cache verwendet werden kann.

Beim Aufrufen von `StringGet`, wenn das Objekt vorhanden ist, es zurückgegeben wird, und wenn nicht, `null` zurückgegeben. In diesem Fall können Sie den Wert aus der Quelle für die gewünschten Daten abzurufen und im Cache zur späteren Verwendung zu speichern. Dies wird als Cache-Aside Muster bezeichnet.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Um den Ablauf eines Elements im Cache angeben möchten, verwenden Sie die `TimeSpan` Parameter der `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-the-cache"></a>Arbeiten Sie mit .NET Objekte im cache

Azure Redis Cache können .NET Objekte und zwischenzuspeichern einfache Datentypen, aber bevor ein Objekt .NET zwischengespeichert werden kann es serialisiert werden muss. Dies ist rein Entwickler der Anwendung, und der Entwicklertools Flexibilität bei der Auswahl des Serialisierungsprogramms.

Eine einfache Methode, um die Objekte serialisieren ist die Verwendung der `JsonConvert` Serialisierungsmethoden in [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) und Serialisieren an und von JSON. Im folgenden Beispiel wird eine Get und Set mit einer `Employee` Objektinstanz.


    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    
        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>
## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie die Grundlagen beherrschen, führen Sie die folgenden Links, um weitere Informationen zur Azure Redis Cache.

-   Schauen Sie sich die ASP.NET-Anbieter für Azure Redis Cache.
    -   [Azure Redis Session State Provider](cache-aspnet-session-state-provider.md)
    -   [Azure Redis Cache ASP.NET Ausgabe Cacheanbieter](cache-aspnet-output-cache-provider.md)
-   So Sie [Monitor](cache-how-to-monitor.md) die Integrität des Ihren Cache können zu [Cache Diagnose aktivieren](cache-how-to-monitor.md#enable-cache-diagnostics) . Sie können die Metrik Azure-Portal anzeigen, und Sie können auch [herunterladen und überprüfen](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) sie mithilfe der Tools von Ihrer Wahl.
-   Schauen Sie sich die [StackExchange.Redis-Cache-Client-Dokumentation][].
    -   Azure Redis Cache können aus vielen Redis-Clients und Entwicklungssprachen zugegriffen werden. Weitere Informationen finden Sie unter [http://redis.io/clients][].
-   Azure Redis Cache können auch mit Drittanbieter-Diensten und Tools wie Redsmin und Redis Desktop Manager verwendet werden.
    -   Weitere Informationen zu Redsmin finden Sie unter [Abrufen einer Verbindungszeichenfolge Azure Redis und verwenden es mit Redsmin][].
    -   Zugriff auf, und prüfen Sie Ihre Daten in Azure Redis Cache mit einer Benutzeroberfläche [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager)verwenden.
-   Finden Sie in der Dokumentation [redis][] und erfahren Sie mehr über die [Datentypen redis][] und [eine 15-minütiges Einführung in Datentypen Redis][].



<!-- INTRA-TOPIC LINKS -->
[Nächste Schritte]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Erstellen Sie den cache]: #create-cache
[Configure the cache]: #enable-caching
[Konfigurieren der Cache-clients]: #NuGet
[Working with Caches]: #working-with-caches
[Verbinden mit dem cache]: #connect-to-cache
[Hinzufügen und Abrufen von Objekten aus dem cache]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session

  
<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png






   
<!-- LINKS -->
[http://redis.IO/Clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[Abrufen einer Verbindungszeichenfolge Azure Redis und verwenden es mit Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis Konfigurationsmodell]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Informationen zur Preisgestaltung im Cache]: http://www.windowsazure.com/pricing/details/cache/
[Azure Portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis-Cache-Client-Dokumentation]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis Datentypen]: http://redis.io/topics/data-types
[eine Einführung in 15-minütiges Redis Datentypen]: http://redis.io/topics/data-types-intro

[Wie funktionieren Anwendung Zeichenfolgen und Verbindungszeichenfolgen]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


