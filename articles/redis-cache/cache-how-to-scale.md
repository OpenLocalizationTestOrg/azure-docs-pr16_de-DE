<properties 
    pageTitle="Zum Skalieren Azure Redis Cache | Microsoft Azure" 
    description="Erfahren Sie, wie Ihre Azure Redis Cache Instanzen skalieren" 
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
    ms.date="09/07/2016" 
    ms.author="sdanie"/>

# <a name="how-to-scale-azure-redis-cache"></a>Zum Skalieren Azure Redis Cache

>[AZURE.NOTE] Azure Redis Cache Skalierung Feature gibt es zurzeit in der Vorschau. 

Azure Redis Cache weist verschiedene Cache Angebote die Flexibilität bei der Wahl der Cachegröße und Funktionen zur Verfügung zu stellen. Wenn Sie die Anforderungen Ihrer Anwendung ändern sich nach ein Cache erstellt wurde, können Sie die Größe des Cache über das Blade **Preisgestaltung Stufe ändern** im [Portal Azure](https://portal.azure.com)skalieren.

## <a name="when-to-scale"></a>Wann skalieren

Sie können die Features [für die Überwachung](cache-how-to-monitor.md) von Azure Redis Cache verwenden, zum Überwachen der Gesundheit und Leistung Cache Anwendungen und helfen festzustellen, ob eine Notwendigkeit zum Skalieren des Caches besteht. 

Sie können die folgende Metrik, um festzustellen, ob Sie skalieren müssen überwachen.

-   Redis Server laden
-   Arbeitsspeicherauslastung
-   Netzwerk-Bandbreite
-   CPU-Auslastung

Wenn Sie feststellen, dass der Cache nicht mehr die Anforderungen Ihrer Anwendung erfüllt ist, können Sie ändern, auf einen Cache vergrößern oder verkleinern, die Preise Ebene, die für eine Anwendung richtig ist. Weitere Informationen zu ermitteln, welcher Cache Preise Ebene verwenden finden Sie unter [Was Cache Angebot und Größe Redis verwendet werden sollen](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-a-cache"></a>Einen Cache skalieren
Zum Skalieren des Caches, [Durchsuchen, um den Cache für Dokumente](cache-configure.md#configure-redis-cache-settings) im [Azure-Portal](https://portal.azure.com) , und klicken Sie auf **Einstellungen**, **Preise zu stufen**.

Klicken Sie auf das Webpart **Preise Ebene** in der **Cache Redis** Blade.

![Preise Ebene][redis-cache-pricing-tier-part]

Aktivieren der gewünschten Preise aus dem Blade **Preise gestuft** gestuft, und klicken Sie auf **auswählen**.

![Preise Ebene][redis-cache-pricing-tier-blade]

>[AZURE.NOTE] Sie können an eine andere Preisgestaltung Ebene mit den folgenden Einschränkungen skalieren.
>
>-  Sie können nicht an eine niedrigere Preisgestaltung aus eine höhere Preisgestaltung Stufe skalieren.
>    -    Sie können nicht aus einer **Premium** Cache auf **Standard** oder eine **einfache** Cache skalieren.
>    -    Sie können nicht aus einem **Standard** Cache auf eine **einfache** Cache skalieren.
>-  Sie können auf einen **Standard** Cache aus einem **grundlegende** Cache skalieren, aber Sie können die Größe nicht gleichzeitig ändern. Wenn Sie eine andere Größe benötigen, können Sie einen nachfolgenden Skalierung Vorgang auf die gewünschte Größe ausführen.
>-  Sie können nicht aus einem **grundlegende** Cache direkt an einen **Premium** Cache skalieren. Sie müssen **grundlegende** auf **Standard** in einem einzigen Skalierung Vorgang und dann von **Standard** in **Premium** in einem nachfolgenden Skalierung-Vorgang skalieren.
>-  Sie können keine Skalieren von einer großen nach unten, um die **C0 (250 MB)** Größe.

Während der Cache an die neue Preisgestaltung Schicht Skalierung ist, wird der Status **Skalierung** in das Blade **Redis Cache** angezeigt.

![Skalierung][redis-cache-scaling]

Wenn dieselbe Skalierung abgeschlossen ist, ändert sich der Status von **Skalierung** aus, in **ausgeführt**.

## <a name="how-to-automate-a-scaling-operation"></a>Wie Sie einen Skalierung Vorgang automatisieren

Neben der Skalierung Ihrer Azure Redis Cache Instanzen der Azure-Portal an, Sie können Skalieren mit Azure Redis Cache PowerShell-Cmdlets, Azure CLI, und klicken Sie mit der Microsoft Azure Management Bibliotheken (MAML). 

-   [Skala mit PowerShell](#scale-using-powershell)
-   [Skala mit Azure CLI](#scale-using-azure-cli)
-   [Skala mit MAML](#scale-using-maml)

### <a name="scale-using-powershell"></a>Skala mit PowerShell

Sie können Ihre Azure Redis Cache Instanzen mit PowerShell mithilfe des Cmdlets [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) Skalieren bei der `Size`, `Sku`, oder `ShardCount` Eigenschaften geändert werden. Im folgenden Beispiel wird veranschaulicht, wie einen Cache mit dem Namen skalieren `myCache` an einen 2,5 GB Cache. 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Weitere Informationen zu skalieren mit PowerShell finden Sie unter [so einen Redis Cache mithilfe der Powershell skalieren](cache-howto-manage-redis-cache-powershell.md#scale).

### <a name="scale-using-azure-cli"></a>Skala mit Azure CLI

Um Ihre Azure Redis Cache Instanzen mit Azure CLI zu skalieren, rufen Sie die `azure rediscache set` Befehl und übergeben Sie die gewünschte Konfiguration Änderungen, die eine neue Größe, Sku oder Clustergröße, je nach den gewünschten Skalierung Vorgang enthalten.

Weitere Informationen zu skalieren mit Azure CLI finden Sie unter [Ändern einer vorhandenen Redis Cache Einstellungen](cache-manage-cli.md#scale).

### <a name="scale-using-maml"></a>Skala mit MAML

Zum Skalieren Ihrer Azure Redis Cache Instanzen der [Microsoft Azure Management Bibliotheken (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/)verwenden, rufen Sie die `IRedisOperations.CreateOrUpdate` Methode, und übergeben die neue Größe für die `RedisProperties.SKU.Capacity`.

    static void Main(string[] args)
    {
        // For instructions on getting the access token, see
        // https://azure.microsoft.com/documentation/articles/cache-configure/#access-keys
        string token = GetAuthorizationHeader();

        TokenCloudCredentials creds = new TokenCloudCredentials(subscriptionId,token);

        RedisManagementClient client = new RedisManagementClient(creds);
        var redisProperties = new RedisProperties();

        // To scale, set a new size for the redisSKUCapacity parameter.
        redisProperties.Sku = new Sku(redisSKUName,redisSKUFamily,redisSKUCapacity);
        redisProperties.RedisVersion = redisVersion;
        var redisParams = new RedisCreateOrUpdateParameters(redisProperties, redisCacheRegion);
        client.Redis.CreateOrUpdate(resourceGroupName,cacheName, redisParams);
    }

Weitere Informationen finden Sie unter der Stichprobe [Redis Cache verwalten verwenden MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) .

## <a name="scaling-faq"></a>Häufig gestellte Fragen zu skalieren

Die folgende Liste enthält Antworten auf häufig gestellte Fragen zur Azure Redis Cache Skalierung.

-   [Kann an, von oder innerhalb einer Premium Cache werden skaliert?](#can-i-scale-to-from-or-within-a-premium-cache)
-   [Nach dem Skalieren habe ich meine Cache Namen oder Access Tasten ändern?](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
-   [Wie funktioniert die Skalierung?](#how-does-scaling-work)
-   [Verliere Daten aus meinem Cache während der Skalierung ich?](#will-i-lose-data-from-my-cache-during-scaling)
-   [Ist meine benutzerdefinierten Datenbanken betroffenen während der Skalierung festlegen?](#is-my-custom-databases-setting-affected-during-scaling)
-   [Werden meine Cache während der Skalierung zur Verfügung?](#will-my-cache-be-available-during-scaling)
-   [Vorgänge, die nicht unterstützt werden](#operations-that-are-not-supported)
-   [Wie lange dauert Skalierung bis?](#how-long-does-scaling-take)
-   [Wie kann ich feststellen, wenn die Skalierung abgeschlossen ist?](#how-can-i-tell-when-scaling-is-complete)
-   [Warum ist dieses Feature in der Vorschau an?](#why-is-this-feature-in-preview)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a>Kann an, von oder innerhalb einer Premium Cache werden skaliert?

-   Sie können nicht aus einem Cache **Premium** auf eine **einfache** oder **Standard** Preise Ebene skalieren.
-   Sie können aus einem **Premium** Cache Ebene in eine andere Preise skalieren.
-   Sie können nicht aus einem **grundlegende** Cache direkt an einen **Premium** Cache skalieren. Sie müssen zuerst von **grundlegenden** in **Standard** in einem einzigen Skalierung Vorgang und dann von **Standard** in **Premium** in einem nachfolgenden Skalierung-Vorgang skalieren.
-   Wenn Sie Cluster aktiviert, wenn Sie Ihren Cache **Premium** erstellt haben, können Sie [die Größe der Zuordnungseinheiten ändern](cache-how-to-premium-clustering.md#cluster-size). Zu diesem Zeitpunkt können Sie nicht auf einem bereits vorhandenen Cache, der ohne Cluster erstellte Cluster aktivieren.

    Weitere Informationen finden Sie unter [So konfigurieren Sie für einen Premium Azure Redis Cache Cluster](cache-how-to-premium-clustering.md).

### <a name="after-scaling-do-i-have-to-change-my-cache-name-or-access-keys"></a>Nach dem Skalieren habe ich meine Cache Namen oder Access Tasten ändern?

Nein, sind Ihre Cache Namen und Tasten unverändert während eines Vorgangs Skalierung.

### <a name="how-does-scaling-work"></a>Wie funktioniert die Skalierung?

-   Wenn Sie ein **einfache** Cache auf einen anderen Schriftgrad skaliert wird, wird beendet und ein neuer Cache bereitgestellt wurde, verwenden die neue Größe. Während dieses Zeitraums der Cache nicht verfügbar ist, und alle Daten im Cache geht verloren.
-   Wenn ein **einfache** Cache auf einem **Standard** Cache skaliert wird, ein Replikat Cache bereitgestellt wird, und die Daten aus dem Cache primären dem Cache Replikat kopiert. Cache bleibt während der Skalierung Prozess verfügbar.
-   Wenn ein **Standard** -Cache zu einer anderen Größe oder an einen **Premium** Cache skaliert wird, ist eines der Replikate fahren und nach der Bereitstellung erneut auf die neue Größe und die Daten, die über übertragen, und klicken Sie dann das andere Replikat einen Failover ausführt, bevor er erneut bereitgestellt, ähnlich wie der Prozess, der während ein Fehler bei einer der Cache Knoten ist.

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a>Verliere Daten aus meinem Cache während der Skalierung ich?

-   Wenn Sie ein **einfache** Cache auf eine neue Größe skaliert wird, alle Daten geht verloren, und der Cache während des Importvorgangs Skalierung nicht verfügbar ist.
-   Wenn Sie ein **einfache** Cache auf einem **Standard** Cache skaliert wird, werden in der Regel die Daten im Cache beibehalten.
-   Wenn ein **Standard** Cache wird zu einer größeren Größe oder Ebene skaliert, oder ein **Premium** Cache skaliert zu vergrößern, wird in der Regel alle Daten beibehalten. Einen **Standard** oder **Premium** Cache nach unten bis zum geringerer Größe skalieren, können Daten verloren, je nachdem, wie viele Daten sind im Cache beim Skalieren ist im Zusammenhang mit der neuen Größe sein. Wenn Sie Daten verloren geht, wenn Skalierung nach unten, werden Schlüssel mithilfe der [Allkeys-Lru](http://redis.io/topics/lru-cache) Entfernung Richtlinie entfernt. 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a>Ist meine benutzerdefinierten Datenbanken betroffenen während der Skalierung festlegen?

Einige Preisgestaltung Ebenen haben andere [Datenbanken Grenzwerte](cache-configure.md#databases), damit es sind einige Überlegungen beim Skalierung nach unten If so konfiguriert, einen benutzerdefinierten Wert für dass die `databases` während der Erstellung des Caches festlegen.

-   Beim Skalieren zu einer Preisgestaltung Ebene mit einer untere `databases` Grenzwert als der aktuellen Ebene:
    -   Wenn Sie mit die Standardanzahl von arbeiten `databases` welche 16 für alle Preise Stufen ist, werden keine Daten verloren.
    -   Wenn Sie eine benutzerdefinierte Anzahl von arbeiten `databases` , die innerhalb der Grenzwerte für die Ebene, dem Sie sind Skalierung, dies, fällt `databases` Einstellung wird beibehalten und keine Daten verloren.
    -   Wenn Sie eine benutzerdefinierte Anzahl von arbeiten `databases` , die die Grenzwerte der neuen Stufe, überschreitet die `databases` Einstellung werden verringert, der der neue Stufe zugewiesen wird und alle Daten in den entfernten Datenbanken geht verloren.
-   Beim Skalieren zu einer Preisgestaltung Ebene mit dem gleichen oder höhere `databases` Beschränkung als die aktuelle Ebene der `databases` Einstellung wird beibehalten und keine Daten verloren.

Beachten Sie, dass Standard- und Premium Caches einer 99,9 % Vereinbarung zum SERVICELEVEL für Verfügbarkeit haben, aber es keine Vereinbarung zum SERVICELEVEL für Datenverlust gibt.

### <a name="will-my-cache-be-available-during-scaling"></a>Werden meine Cache während der Skalierung zur Verfügung?

-   **Standard-** und **Premium** Caches bleiben während des Importvorgangs Skalierung verfügbar.
-   **Grundlegende** Caches während der Skalierung von Vorgängen zu einer anderen Größe offline sind, bleiben jedoch verfügbar beim Skalieren **von** auf **Standard**.

### <a name="operations-that-are-not-supported"></a>Vorgänge, die nicht unterstützt werden

-   Sie können nicht an eine niedrigere Preisgestaltung aus eine höhere Preisgestaltung Stufe skalieren.
    -    Sie können nicht aus einem Cache **Premium** auf **Standard** oder eine **einfache** Cache skalieren.
    -    Sie können nicht aus einem **Standard** Cache auf eine **einfache** Cache skalieren.
-   Sie können auf einen **Standard** Cache aus einem **grundlegende** Cache skalieren, aber Sie können die Größe nicht gleichzeitig ändern. Wenn Sie eine andere Größe benötigen, können Sie einen nachfolgenden Skalierung Vorgang auf die gewünschte Größe ausführen.
-   Sie können nicht direkt an einen **Premium** Cache aus einem **grundlegende** Cache skalieren. Sie müssen **grundlegende** auf **Standard** in einem einzigen Skalierung Vorgang und dann von **Standard** in **Premium** in einem nachfolgenden Skalierung-Vorgang skalieren.
-   Sie können keine Skalieren von einer großen nach unten, um die **C0 (250 MB)** Größe.

Wenn ein Vorgang Skalierung fehlschlägt, der Dienst wird versuchen, den Vorgang wiederherstellen und Cache wird die ursprüngliche Größe zurückgesetzt.

### <a name="how-long-does-scaling-take"></a>Wie lange dauert Skalierung bis?

Dauert die Skalierung ungefähr 20 Minuten, je nachdem, wie viele Daten im Cache vorhanden ist.

### <a name="how-can-i-tell-when-scaling-is-complete"></a>Wie kann ich feststellen, wenn die Skalierung abgeschlossen ist?

Azure-Portal können Sie den aktuellen Vorgang Skalierung anzeigen. Nach Abschluss der Skalierung ändert sich der Status des Caches für die **Ausführung von**.

### <a name="why-is-this-feature-in-preview"></a>Warum ist dieses Feature in der Vorschau an?

Veröffentlichen wir werden diese Funktion, um Ihr Feedback zu gelangen. Basierend auf dem Feedback, wird wir dieses Feature zur allgemeinen Verfügbarkeit gebracht schnell freigeben.





  
<!-- IMAGES -->
[redis-cache-pricing-tier-part]: ./media/cache-how-to-scale/redis-cache-pricing-tier-part.png

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



