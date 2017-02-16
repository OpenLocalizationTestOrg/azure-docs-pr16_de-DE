<properties 
    pageTitle="So erstellen und Verwalten von Azure Redis Cache Schnittstelle der Azure Line (Azure CLI) | Microsoft Azure" 
    description="Erfahren Sie, wie die Azure CLI auf einer beliebigen Plattform installiert, wie zur gemeinsamen Nutzung von eine Verbindung mit Ihrem Konto Azure und zum Erstellen und Verwalten eines Redis Caches über die Befehlszeile Azure." 
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

# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a>So erstellen und Verwalten von Azure Redis Cache Schnittstelle der Azure Line (Azure CLI)

> [AZURE.SELECTOR]
- [PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)

Die CLI Azure ist eine großartige Möglichkeit zum Verwalten Ihrer Azure-Infrastruktur von jeder Plattform. In diesem Artikel wird das Erstellen und Verwalten Ihrer Azure Redis Cache Instanzen mithilfe der CLI Azure veranschaulicht.

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Erstellen und Verwalten von Azure Redis Cache Instanzen Azure CLI verwenden möchten, müssen Sie die folgenden Schritte ausführen.

-   Sie müssen ein Azure-Konto. Wenn Sie eine besitzen, können Sie es in wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.
-   [Installieren die Azure CLI](../xplat-cli-install.md).
-   Verbinden Sie Ihre Azure CLI-Installation mit einer persönlichen Azure-Konto oder mit einer geschäftlichen oder Schule Azure-Konto, und melden Sie sich über die Azure CLI mit den `azure login` Befehl. Wenn Sie Informationen über die Unterschiede und auswählen, finden Sie unter [Verbinden zu einem Azure Abonnement aus der Azure Line Interface (CLI Azure)](../xplat-cli-connect.md).
-   Wechseln Sie vor dem Ausführen eine der folgenden Befehle, die CLI Azure in Ressourcenmanager Modus durch Ausführen der `azure config mode arm` Befehl. Weitere Informationen finden Sie unter [festlegen den Ressourcenmanager Azure-Modus](../xplat-cli-azure-resource-manager.md#set-the-azure-resource-manager-mode).

## <a name="redis-cache-properties"></a>Redis Cacheeigenschaften

Die folgenden Eigenschaften werden beim Erstellen und Aktualisieren von Redis Cache Instanzen verwendet.

| Eigenschaft            | Wechseln                                  | Beschreibung                                                                                                                                                                                                                                          |
|---------------------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Namen                | -n,--name                              | Der Name des Caches Redis.                                                                                                                                                                                                                             |
| Ressourcengruppe      | -g,---Ressourcengruppe                    | Name der Ressourcengruppe.                                                                                                                                                                                                                          |
| Speicherort            | -l, – Speicherort                          | Speicherort, um den Cache zu erstellen.                                                                                                                                                                                                                            |
| Größe                | -Z, – Größe                              | Größe des Caches Redis. Gültige Werte: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]                                                                                                                                                                  |
| SKU                 | -X, – sku                               | Redis SKU. Eine der sollten: [grundlegende, Standard, Premium]                                                                                                                                                                                             |
| EnableNonSslPort    | -e, – aktivieren-nicht-Ssl-port               | EnableNonSslPort-Eigenschaft des Caches Redis. Fügen Sie diese Kennzeichnung hinzu, wenn Sie nicht SSL-Port für Ihren Cache aktivieren möchten.                                                                                                                                    |
| Redis Konfiguration | -c,--Redis-Konfiguration               | Redis Konfiguration. Geben Sie eine Textzeichenfolge JSON formatierte Konfigurationsschlüssel und Werte hier ein. Format: "{" ":""," ":" "}"                                                                                                                                     |
| Redis Konfiguration | -f,--Redis Konfigurationsdatei          | Redis Konfiguration. Geben Sie den Pfad einer Datei mit der Konfigurationsschlüssel und Werte hier ein. Format für den Dateieintrag: {"": "","": ""}                                                                                                                |
| Anzahl der shard         | -R,--Shard-zählen                       | Anzahl der mehrere Shards hinweg auf einem Premium Cluster Cache mit Cluster erstellen.                                                                                                                                                                               |
| Virtuelles Netzwerk     | + V, – virtuelles Netzwerk                   | Wenn Sie Ihren Cache in einem VNET hosten, gibt die genauen Cloud Ressource-ID des virtuellen Netzwerks zum Bereitstellen des Caches Redis in an. Beispiel-Format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Typ des Schlüssels            | t-,--Schlüssel-Typ                          | Typ des Schlüssels erneuern. Gültige Werte: [primäre, sekundäre]                                                                                                                                                                                             |
| StaticIP            | -p,--statische Ip-< Ip-statische >             | Wenn Sie Ihren Cache in einem VNET hosten, gibt eine eindeutige IP-Adresse im Subnetz für den Cache an. Wenn nicht angegeben, wird eine für Sie aus dem Subnetz ausgewählt.                                                                                                |
| Subnetz              | t,--Subnetz<subnet>                    | Wenn Sie Ihren Cache in einem VNET hosten, gibt den Namen der im Subnetz, in dem den Cache bereitgestellt.                                                                                                                                                    |
| VirtualNetwork      | + V, – virtuelles Netzwerk < virtuelle-Netzwerk > | Wenn Sie Ihren Cache in einem VNET hosten, gibt die genauen Cloud Ressource-ID des virtuellen Netzwerks zum Bereitstellen des Caches Redis in an. Beispiel-Format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Abonnement        | -s, – Abonnement                      | Der Abonnementbezeichner.                                                                                                                                                                                                                         |

## <a name="see-all-redis-cache-commands"></a>Finden Sie unter alle Cache Redis-Befehle

Verwenden Sie zum Anzeigen aller Redis-Cache-Befehle und deren Parameter der `azure rediscache -h` Befehl.

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a>Erstellen Sie einen Redis Cache

Um einen Redis Cache erstellen möchten, verwenden Sie den folgenden Befehl aus:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Weitere Informationen zu diesem Befehl Ausführen der `azure rediscache create -h` Befehl.

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a>Löschen einer vorhandenen Redis Cache

Wenn ein Cache Redis löschen möchten, verwenden Sie den folgenden Befehl aus:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Weitere Informationen zu diesem Befehl Ausführen der `azure rediscache delete -h` Befehl.

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>Liste aller Redis Caches in Ihrem Abonnement oder Ressourcengruppe

Um alle Redis Caches in Ihrem Abonnement oder Ressourcengruppe aufzulisten, verwenden Sie den folgenden Befehl aus:

    azure rediscache list [options]

Weitere Informationen zu diesem Befehl Ausführen der `azure rediscache list -h` Befehl.

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a>Anzeigen der Eigenschaften eines vorhandenen Redis Cache

Zum Anzeigen der Eigenschaften eines vorhandenen Redis Cache verwenden Sie den folgenden Befehl aus:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Weitere Informationen zu diesem Befehl Ausführen der `azure rediscache show -h` Befehl.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>
## <a name="change-settings-of-an-existing-redis-cache"></a>Ändern einer vorhandenen Redis Cache für

Um einen vorhandenen Redis Cache Einstellungen ändern möchten, verwenden Sie den folgenden Befehl aus:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Weitere Informationen zu diesem Befehl Ausführen der `azure rediscache set -h` Befehl.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a>Erneuern Sie die Taste Authentifizierung für eine vorhandene Redis Cache

Verwenden Sie die Taste Authentifizierung für eine vorhandene Redis Cache erneuern muss, den folgenden Befehl aus:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Geben Sie `Primary` oder `Secondary` für `key-type`.

Weitere Informationen zu diesem Befehl Ausführen der `azure rediscache renew-key -h` Befehl.

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Liste primären und sekundären Schlüssel für einen vorhandenen Redis Cache

Liste der vorhandene Redis Cache auf primären und sekundären Tasten verwenden Sie den folgenden Befehl aus:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Weitere Informationen zu diesem Befehl Ausführen der `azure rediscache list-keys -h` Befehl.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
