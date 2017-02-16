<properties
    pageTitle="Verwalten von Azure Redis Cache mit Azure PowerShell | Microsoft Azure"
    description="Erfahren Sie, wie Verwaltungsaufgaben für Azure Redis Cache mithilfe der PowerShell Azure."
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

# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Verwalten von Azure Redis Cache mit Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)

In diesem Thema zeigt Sie wie ausführen allgemeine Aufgaben wie erstellen, aktualisieren und Ihre Azure Redis Cache Instanzen, wie Zugriffstasten neu zu erstellen und zum Anzeigen von Informationen über Ihre Caches skalieren. Eine vollständige Liste der Azure Redis Cache PowerShell-Cmdlets finden Sie unter [Azure Redis Cache Cmdlets](https://msdn.microsoft.com/library/azure/mt634513.aspx).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)][Klassisch](../resource-manager-deployment-model.md#classic-deployment-characteristics).

## <a name="prerequisites"></a>Erforderliche Komponenten

Wenn Sie Azure PowerShell bereits installiert haben, müssen Sie Azure PowerShell Version 1.0.0 oder höher. Sie können die Version von Azure PowerShell überprüfen, die Sie mit dem folgenden Befehl in die Befehlszeile Azure PowerShell installiert haben.

    Get-Module azure | format-table version

[AZURE.INCLUDE [powershell-preview](../../includes/powershell-preview-inline-include.md)]

Zuerst müssen Sie bei Azure mit dieser Befehl anmelden.

    Login-AzureRmAccount

Geben Sie im Dialogfeld Microsoft Azure-in die e-Mail-Adresse Ihre Azure-Konto und das Kennwort ein.

Wenn Sie mehrere Azure-Abonnements haben, müssen Sie als Nächstes Ihres Abonnements Azure festlegen. Führen Sie diesen Befehl zum Anzeigen einer Liste von Ihrer aktuellen Abonnements.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Wenn Sie das Abonnement angeben möchten, führen Sie den folgenden Befehl ein. Im folgenden Beispiel wird der Namen des Abonnements `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Bevor Sie Windows PowerShell Azure Ressourcenmanager verwenden können, benötigen Sie Folgendes:

- Windows PowerShell, Version 3.0 oder 4.0. Um die Version von Windows PowerShell gefunden haben, geben Sie ein:`$PSVersionTable` und überprüfen Sie den Wert `PSVersion` 3.0 oder 4.0 ist. Um eine kompatible Version zu installieren, finden Sie unter [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) oder [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Um ausführliche Hilfe für alle Cmdlet zu gelangen, die in diesem Lernprogramm angezeigt werden, verwenden Sie das Cmdlet Hilfe.

    Get-Help <cmdlet-name> -Detailed

Beispiel zum Aufrufen von Hilfe für die `New-AzureRmRedisCache` Cmdlet, Typ:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-azure-government-cloud-or-azure-china-cloud"></a>Herstellen einer Verbindung mit Azure Government Cloud oder Azure China Cloud

Standardmäßig der Azure-Umgebung ist `AzureCloud`, die die globale Azure-Cloud-Instanz darstellt. Verwenden Sie die Verbindung zu einer anderen Instanz der `Add-AzureRmAccount` -Befehl mit den `-Environment` oder -`EnvironmentName` Befehlszeilenoption mit den gewünschten Umgebung oder den Umgebungsnamen.

Um die Liste der verfügbaren Umgebungen sehen zu können, führen die `Get-AzureRmEnvironment` Cmdlet.

### <a name="to-connect-to-the-azure-government-cloud"></a>Verbindung in der Cloud Azure Government

Verwenden Sie zum Verbinden mit der Cloud Azure Government einen der folgenden Befehle aus.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

oder

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Verwenden Sie zum Erstellen eines Caches in der Cloud Azure Government eine der folgenden Orte aus.

-   USGov Virginia
-   USGov Iowa

Weitere Informationen zu den Azure Government Cloud finden Sie unter [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) und [Microsoft Azure Government Developer Guide](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Die Verbindung zur Azure China Cloud

Verwenden Sie zum Verbinden mit der Cloud Azure China einen der folgenden Befehle aus.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

oder

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Verwenden Sie zum Erstellen eines Caches in der Cloud Azure China eine der folgenden Orte aus.

-   China OST
-   Nord-China

Weitere Informationen zu den Azure China Cloud finden Sie unter [AzureChinaCloud für Azure von 21Vianet in China betrieben](http://www.windowsazure.cn/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Eigenschaften für die Azure Redis Cache PowerShell

Die folgende Tabelle enthält die Eigenschaften und Beschreibungen für häufig verwendete Parameter beim Erstellen und Verwalten von Ihrer Azure Redis Cache Instanzen mithilfe der PowerShell Azure.

| Parameter          | Beschreibung                                                                                                                                                                                                        | Standard  |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| Namen               | Name des Caches                                                                                                                                                                                                  |          |
| Speicherort           | Speicherort des Caches                                                                                                                                                                                              |          |
| ResourceGroupName  | Ressource Gruppennamen in der zum Erstellen des Caches                                                                                                                                                                   |          |
| Größe               | Die Größe des Caches. Gültige Werte sind: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB                                                                     | 1GB      |
| ShardCount         | Die Anzahl der mehrere Shards hinweg zu erstellen, wenn einen Premium Cache erstellen mit Cluster aktiviert. Gültige Werte sind: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10                                                                                                      |          |
| SKU                | Gibt die SKU des Caches an. Gültige Werte sind: grundlegende, Standard, Premium                                                                                                                                         | Standard |
| RedisConfiguration | Gibt die Konfiguration Einstellungen Redis an. Finden Sie weitere Informationen zu jeder Einstellung [RedisConfiguration Eigenschaften](#redisconfiguration-properties) in der folgenden Tabelle aus. |          |
| EnableNonSslPort   | Gibt an, ob der Port nicht SSL aktiviert ist.                                                                                                                                                                     | Falsch    |
| MaxMemoryPolicy    | Für diesen Parameter ist veraltet: Verwenden Sie stattdessen die RedisConfiguration.                                                                                                                                              |          |
| StaticIP           | Wenn Sie Ihren Cache in einem VNET hosten, gibt eine eindeutige IP-Adresse im Subnetz für den Cache an. Wenn nicht angegeben, wird eine für Sie aus dem Subnetz ausgewählt.                                                                                                                     |          |
| Subnetz             | Wenn Sie Ihren Cache in einem VNET hosten, gibt den Namen der im Subnetz, in dem den Cache bereitgestellt.                                                                                                                  |          |
| VirtualNetwork     | Wenn Sie Ihren Cache in einem VNET hosten, gibt an, die Ressource-ID des der VNET in dem Cache bereitgestellt.                                                                                                             |          |
| KeyType            | Gibt an, welche Zugriffstaste zu generieren, wenn Sie Tastenkombinationen erneuern. Gültige Werte sind: primäre, sekundäre |  |                                                                                                                                                                                                              |          |


### <a name="redisconfiguration-properties"></a>RedisConfiguration Eigenschaften

| Eigenschaft                      | Beschreibung                                                                                                          | Preise Ebenen       |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------|---------------------|
| RDB Sicherung aktiviert            | Gibt an, ob [Redis Daten Beibehaltung](cache-how-to-premium-persistence.md) aktiviert ist                                     | Nur Premium        |
| RDB-Speicher-Verbindungszeichenfolge | Die Verbindungszeichenfolge in dem Speicherkonto [Redis Beibehaltung der Daten](cache-how-to-premium-persistence.md)       | Nur Premium        |
| RDB-Sicherung-Häufigkeit          | Die Sicherung Häufigkeit für [Redis Beibehaltung der Daten](cache-how-to-premium-persistence.md)                               | Nur Premium        |
| Maxmemory reserviert            | Konfiguriert die [reservierte Arbeitsspeicher](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) für Prozesse nicht im cache | Standard- und Premium |
| Maxmemory-Richtlinie              | Konfiguriert die [Entfernung Richtlinie](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) für den cache           | Alle Preise Ebenen   |
| Benachrichtigen-Schlüssellänge zur Verfügung-Ereignisse        | [Schlüssellänge zur Verfügung Benachrichtigungen](cache-configure.md#keyspace-notifications-advanced-settings) konfiguriert                     | Standard- und Premium |
| Hash-Max-Ziplist-Einträge      | [Optimieren des Arbeitsspeichers](http://redis.io/topics/memory-optimization) für kleine aggregate Datentypen konfiguriert          | Standard- und Premium |
| Hash-Max-Ziplist-Wert        | [Optimieren des Arbeitsspeichers](http://redis.io/topics/memory-optimization) für kleine aggregate Datentypen konfiguriert          | Standard- und Premium |
| Set-Max-Intset-Einträge        | [Optimieren des Arbeitsspeichers](http://redis.io/topics/memory-optimization) für kleine aggregate Datentypen konfiguriert          | Standard- und Premium |
| Zset-Max-Ziplist-Einträge      | [Optimieren des Arbeitsspeichers](http://redis.io/topics/memory-optimization) für kleine aggregate Datentypen konfiguriert          | Standard- und Premium |
| Zset-Max-Ziplist-Wert        | [Optimieren des Arbeitsspeichers](http://redis.io/topics/memory-optimization) für kleine aggregate Datentypen konfiguriert          | Standard- und Premium |
| Datenbanken                     | Konfiguriert die Anzahl der Datenbanken. Diese Eigenschaft kann nur bei der Erstellung des Caches für konfiguriert werden.                          | Standard- und Premium |

## <a name="to-create-a-redis-cache"></a>So erstellen einen Redis Cache

Neue Azure Redis Cache Instanzen werden mithilfe des Cmdlets [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) erstellt.

>[AZURE.IMPORTANT] Sie Erstellen eines Abonnements mithilfe des Azure-Portals einen Cache Redis ersten Mal im Portal registriert die `Microsoft.Cache` Namespace für dieses Abonnement. Wenn Sie versuchen, den ersten Redis Cache in einem Abonnement mithilfe der PowerShell zu erstellen, müssen Sie zuerst die Namespace mit dem folgenden Befehl registrieren; andernfalls Cmdlets wie `New-AzureRmRedisCache` und `Get-AzureRmRedisCache` fehl.
>
>`Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`

Um eine Liste der verfügbaren Parameter und deren Beschreibung für finden Sie unter `New-AzureRmRedisCache`, mit dem folgenden Befehl ausführen.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed
    
    NAME
        New-AzureRmRedisCache
    
    SYNOPSIS
        Creates a new redis cache.
    
    
    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]
    
    
    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.
    
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to create.
    
        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.
    
        -Location <String>
            Location in which to create the redis cache.
    
        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}
    
        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Um einen Cache mit Standardparameter zu erstellen, führen Sie den folgenden Befehl ein.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, und `Location` Parameter erforderlich sind, aber die übrigen sind optional und haben Standardwerte. Ausführen des vorherigen Befehls erstellt eine Instanz der Standard-SKU Azure Redis Cache für die angegebene Name, Position und Ressourcengruppe, die 1 GB Größe mit den nicht-SSL-Anschluss deaktiviert ist.

Erstellen Sie einen Cache Premium, geben Sie eine Größe von B1 (6 bis 60 GB), P2 (13 bis 130 GB), P3 (26 bis 260 GB), oder P4 (53 bis 530 GB). Klicken Sie zum Aktivieren der Cluster angeben einer Shard zählen mithilfe der `ShardCount` Parameter. Im folgenden Beispiel wird einen P1 Premium Cache mit 3 mehrere Shards hinweg. Ein P1 Premium Cache beträgt 6 GB Größe, und da wir drei mehrere Shards hinweg angegeben ist die Gesamtgröße 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Angeben der Werte für die `RedisConfiguration` Parameter, setzen Sie die Werte in `{}` als Schlüssel/Wert-Paare wie `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Im folgenden Beispiel wird einen standard 1 GB Cache mit `allkeys-random` Maxmemory Richtlinie und Schlüssellänge zur Verfügung Benachrichtigungen konfiguriert mit `KEA`. Weitere Informationen finden Sie unter [Schlüssellänge zur Verfügung Benachrichtigungen (Erweiterte Einstellungen)](cache-configure.md#keyspace-notifications-advanced-settings) und [Maxmemory-Richtlinie und Maxmemory reserviert](cache-configure.md#maxmemory-policy-and-maxmemory-reserved).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>
## <a name="to-configure-the-databases-setting-during-cache-creation"></a>So konfigurieren Sie die Einstellung während der Erstellung des Caches für Datenbanken

Die `databases` Einstellung kann nur während der Erstellung des Caches für konfiguriert werden. Im folgenden Beispiel wird eine Premium P3 (26 GB) Cache mit 48 Datenbanken mithilfe des Cmdlets [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) .

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Weitere Informationen zu den `databases` Eigenschaft finden Sie unter [Cache für Azure Redis Standard-Server-Konfiguration](cache-configure.md#default-redis-server-configuration). Weitere Informationen zum Erstellen eines Caches mithilfe des Cmdlets [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) finden Sie unter dem vorherigen Abschnitt [ein Cache Redis erstellt](#to-create-a-redis-cache) .

## <a name="to-update-a-redis-cache"></a>Aktualisieren einen Redis cache

Azure Cache Redis Instanzen werden mithilfe des Cmdlets [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) aktualisiert.

Um eine Liste der verfügbaren Parameter und deren Beschreibung für finden Sie unter `Set-AzureRmRedisCache`, mit dem folgenden Befehl ausführen.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed
    
    NAME
        Set-AzureRmRedisCache
    
    SYNOPSIS
        Set redis cache updatable parameters.
    
    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]
    
    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to update.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Die `Set-AzureRmRedisCache` Cmdlet kann verwendet werden, um Eigenschaften wie aktualisieren `Size`, `Sku`, `EnableNonSslPort`, und die `RedisConfiguration` Werte. 

Mit dem folgende Befehl wird die Maxmemory-Richtlinie für den Redis Cache mit dem Namen MyCache aktualisiert.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>
## <a name="to-scale-a-redis-cache"></a>Um einen Cache Redis skalieren

`Set-AzureRmRedisCache`können verwendet werden, um einen Cache Azure Redis skalieren Instanz fest, wann die `Size`, `Sku`, oder `ShardCount` Eigenschaften geändert werden. 

>[AZURE.NOTE]Einen mithilfe der PowerShell Cache Skalierung unterliegt dieselben Grenzwerte und Richtlinien als Skalierung einen Cache vom Azure-Portal. Sie können an eine andere Preisgestaltung Ebene mit den folgenden Einschränkungen skalieren.
>
>-  Sie können nicht an eine niedrigere Preisgestaltung aus eine höhere Preisgestaltung Stufe skalieren.
>    -    Sie können nicht aus einem Cache **Premium** auf **Standard** oder eine **einfache** Cache skalieren.
>    -    Sie können nicht aus einem **Standard** Cache auf eine **einfache** Cache skalieren.
>-  Sie können auf einen **Standard** Cache aus einem **grundlegende** Cache skalieren, aber Sie können die Größe nicht gleichzeitig ändern. Wenn Sie eine andere Größe benötigen, können Sie einen nachfolgenden Skalierung Vorgang auf die gewünschte Größe ausführen.
>-  Sie können nicht aus einem **grundlegende** Cache direkt an einen **Premium** Cache skalieren. Sie müssen **grundlegende** auf **Standard** in einem einzigen Skalierung Vorgang und dann von **Standard** in **Premium** in einem nachfolgenden Skalierung-Vorgang skalieren.
>-  Sie können keine Skalieren von einer großen nach unten, um die **C0 (250 MB)** Größe.
>
>Weitere Informationen finden Sie unter [So skalieren Azure Redis Cache](cache-how-to-scale.md).

Im folgenden Beispiel wird veranschaulicht, wie einen Cache mit dem Namen skalieren `myCache` an einen 2,5 GB Cache. Beachten Sie, dass dieser Befehl sowohl ein Basic oder einen Standard Cache passt.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Nachdem dieser Befehl ausgegeben wird, wird der Status des Caches zurückgegeben (ähnlich wie das Aufrufen `Get-AzureRmRedisCache`). Beachten Sie, dass die `ProvisioningState` ist `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB
    
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Wenn der Skalierung Vorgang abgeschlossen ist, ist die `ProvisioningState` ändert sich in `Succeeded`. Wenn Sie einem nachfolgenden Skalierung Vorgang, wie etwa das Ändern von grundlegenden in Standard, und klicken Sie dann Ändern der Größe, vornehmen müssen müssen Sie warten, bis der vorherige Vorgang abgeschlossen ist, oder Sie eine Fehlermeldung, ähnlich wie der folgende erhalten.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>Um Informationen zu einem Redis Cache erhalten

Sie können Informationen zu einem Cache mithilfe des Cmdlets [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) abrufen.

Um eine Liste der verfügbaren Parameter und deren Beschreibung für finden Sie unter `Get-AzureRmRedisCache`, mit dem folgenden Befehl ausführen.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed
    
    NAME
        Get-AzureRmRedisCache
    
    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.
    
    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.
    
        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.
    
        If no parameters are given than it will return details about all caches the current subscription.
    
    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Um Informationen für alle Caches in das aktuelle Abonnement zurückgeben, ausführen `Get-AzureRmRedisCache` ohne Parameter.

    Get-AzureRmRedisCache

Um Informationen für alle Caches in einer bestimmten Ressourcengruppe zurückgeben, ausführen `Get-AzureRmRedisCache` mit den `ResourceGroupName` Parameter.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Um Informationen zu einem bestimmten Cache zurückzukehren, ausführen `Get-AzureRmRedisCache` mit den `Name` Parameter mit dem Namen des Caches, und die `ResourceGroupName` Parameter, die mit diesen Cache Ressourcengruppe.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>Zum Abrufen der Tastenkombinationen für einen Redis cache

Um die Zugriffstasten für Ihren Cache abzurufen, können Sie das Cmdlet " [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) " verwenden.

Um eine Liste der verfügbaren Parameter und deren Beschreibung für finden Sie unter `Get-AzureRmRedisCacheKey`, mit dem folgenden Befehl ausführen.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed
    
    NAME
        Get-AzureRmRedisCacheKey
    
    SYNOPSIS
        Gets the accesskeys for the specified redis cache.
    
    
    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Rufen Sie zum Abrufen der Tasten für Ihren Cache der `Get-AzureRmRedisCacheKey` Cmdlet und übergeben den Namen der Cache den Namen der Ressourcengruppe, die der Cache enthält.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>Tastenkombinationen für Ihren Cache Redis neu erstellen.

Wenn die Tastenkombinationen für Ihren Cache neu generieren, können Sie das Cmdlet " [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) " verwenden.

Um eine Liste der verfügbaren Parameter und deren Beschreibung für finden Sie unter `New-AzureRmRedisCacheKey`, mit dem folgenden Befehl ausführen.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed
    
    NAME
        New-AzureRmRedisCacheKey
    
    SYNOPSIS
        Regenerates the access key of a redis cache.
    
    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]
    
    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.
    
        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    
Aufrufen, um den primären oder sekundären Schlüssel für Ihren Cache neu erstellen, die `New-AzureRmRedisCacheKey` Cmdlet übergeben Sie den Namen, Ressourcengruppe, und geben Sie entweder `Primary` oder `Secondary` für die `KeyType` Parameter. Im folgenden Beispiel wird die sekundäre Zugriffstaste für einen Cache neu erstellt.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary
    
    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>So löschen Sie einen Redis cache

Verwenden Sie zum Löschen eines Redis Caches das Cmdlet [Entfernen-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) ein.

Um eine Liste der verfügbaren Parameter und deren Beschreibung für finden Sie unter `Remove-AzureRmRedisCache`, mit dem folgenden Befehl ausführen.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed
    
    NAME
        Remove-AzureRmRedisCache
    
    SYNOPSIS
        Remove redis cache if exists.
    
    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>
    
    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.
    
        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.
    
        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.
    
        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Im folgenden Beispiel, mit dem Namen des Caches `myCache` wird entfernt.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>So importieren Sie einen Redis cache

Importieren von Daten in einer Azure Redis Cache Instanz mit der `Import-AzureRmRedisCache` Cmdlet.

>[AZURE.IMPORTANT] Import/Export ist nur für [Premium Ebene](cache-premium-tier-intro.md) Caches verfügbar. Weitere Informationen zu importieren/exportieren finden Sie unter [Importieren und Exportieren von Daten in Azure Redis Cache](cache-how-to-import-export-data.md).

Um eine Liste der verfügbaren Parameter und deren Beschreibung für finden Sie unter `Import-AzureRmRedisCache`, mit dem folgenden Befehl ausführen.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed
    
    NAME
        Import-AzureRmRedisCache
    
    SYNOPSIS
        Import data from blobs to Azure Redis Cache.
    
    
    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.
    
        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Mit dem folgende Befehl importiert Daten aus dem vom SAS Uri in Azure Redis Cache angegebenen Blob.


    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>So exportieren Sie einen Redis cache

Exportieren von Daten aus einer Instanz Azure Redis Cache mit den `Export-AzureRmRedisCache` Cmdlet.

>[AZURE.IMPORTANT] Import/Export ist nur für [Premium Ebene](cache-premium-tier-intro.md) Caches verfügbar. Weitere Informationen zu importieren/exportieren finden Sie unter [Importieren und Exportieren von Daten in Azure Redis Cache](cache-how-to-import-export-data.md).

Um eine Liste der verfügbaren Parameter und deren Beschreibung für finden Sie unter `Export-AzureRmRedisCache`, mit dem folgenden Befehl ausführen.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed
    
    NAME
        Export-AzureRmRedisCache
    
    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.
    
    
    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Prefix <String>
            Prefix to use for blob names.
    
        -Container <String>
            SAS url of container where data should be exported.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Mit dem folgende Befehl exportiert Daten aus einer Instanz Azure Redis Cache in den vom SAS Uri angegebenen Container.


        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>Einen Redis Cache neu starten.

Sie können neu starten, Ihre Azure Redis Cache Instanz mithilfe der `Reset-AzureRmRedisCache` Cmdlet.

>[AZURE.IMPORTANT] Neustart ist nur für [Premium Ebene](cache-premium-tier-intro.md) Caches verfügbar. Weitere Informationen zu einem Neustart Ihren Cache finden Sie unter [Cache Administration - neu zu starten](cache-administration.md#reboot).

Um eine Liste der verfügbaren Parameter und deren Beschreibung für finden Sie unter `Reset-AzureRmRedisCache`, mit dem folgenden Befehl ausführen.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed
    
    NAME
        Reset-AzureRmRedisCache
    
    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.
    
    
    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".
    
        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.
    
        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.
    
        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Mit dem folgende Befehl wird beide Knoten des angegebenen Caches neu gestartet.

    
        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force
    

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Windows PowerShell mit Azure finden Sie unter den folgenden Ressourcen:

- [Azure Redis Cache Cmdlet-Dokumentation auf MSDN](https://msdn.microsoft.com/library/azure/mt634513.aspx)
- [Azure Ressourcenmanager Cmdlets](http://go.microsoft.com/fwlink/?LinkID=394765): erfahren, wie Sie die Cmdlets im Modul AzureResourceManager verwenden.
- [Ressourcen mithilfe von Gruppen zum Verwalten Ihrer Azure Ressourcen](../resource-group-template-deploy-portal.md): Informationen zum Erstellen und Verwalten von Ressourcengruppen im Azure-Portal.
- [Azure-Blog](http://blogs.msdn.com/windowsazure): erfahren Sie mehr über die neuen Features in Azure.
- [Windows PowerShell-Blog](http://blogs.msdn.com/powershell): erfahren Sie mehr über die neuen Features in Windows PowerShell.
- ["Hallo, Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): praktisches-Tipps und Tricks aus der Windows PowerShell-Community zu erhalten.
