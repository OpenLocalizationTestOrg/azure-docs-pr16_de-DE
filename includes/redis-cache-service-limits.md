| Ressource                                    | Grenzwert                                  |
|---------------------------------------------|----------------------------------------|
| Cachegröße                                  | 530 GB ([Kontakt](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase) Weitere)                                  |
| Datenbanken                                   | 64                                     |
| Max verbundene clients                       | 40.000                                 |
| Redis Cache Replikate (für eine hohe Verfügbarkeit) | 1 |
| Mehrere Shards hinweg in einem Cache Premium mit Cluster    | 10 |

Azure Redis Cache beschränkt und Größen für jede Ebene Preisgestaltung unterscheiden. Die Preisen Ebenen und deren zugeordneten Größen finden Sie unter [Azure Redis Cache Preise](https://azure.microsoft.com/pricing/details/cache/).

Weitere Informationen zum Azure Redis Cache Konfiguration Grenzwerte finden Sie unter [Redis Standard-Server-Konfiguration](redis-cache/cache-configure.md#default-redis-server-configuration).

Da der Konfiguration und Verwaltung von Azure Redis Cache Instanzen von Microsoft abgeschlossen ist, werden nicht alle Redis Befehle in Azure Redis Cache unterstützt. Weitere Informationen finden Sie unter [Redis Befehle nicht unterstützte in Azure Redis Cache]((redis-cache/cache-configure.md#redis-commands-not-supported-in-azure-redis-cache).