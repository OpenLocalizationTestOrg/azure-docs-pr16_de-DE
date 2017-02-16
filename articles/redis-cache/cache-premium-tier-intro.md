<properties 
    pageTitle="Einführung in die Ebene Azure Redis Cache Premium | Microsoft Azure" 
    description="Erfahren Sie, wie Sie zum Erstellen und Verwalten von Redis Beibehaltung Cluster Redis und VNET Unterstützung für Ihre Premium Ebene Azure Redis Cache Instanzen" 
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

# <a name="introduction-to-the-azure-redis-cache-premium-tier"></a>Einführung in die Ebene Azure Redis Cache Premium
Azure Redis Cache handelt es sich um eine verteilt, verwaltete Cache, der Ihnen die hochgradig skalierbar und reagiert Applications erstellen können, indem Sie extrem schnellen Zugriff auf Ihre Daten erleichtert. 

Die neue Premium Ebene ist ein Enterprise bereit Ebene, wozu auch alle Features der Standard-Ebenen und vieles mehr, wie eine bessere Leistung, vergrößern Auslastung, Wiederherstellung, importieren/exportieren und verbesserte Sicherheit. Erfahren Sie mehr über die zusätzliche Features der Premium Cache Stufe lesen Sie weiter.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Eine bessere Leistung im Vergleich zu Standard oder Basic Ebene
**Eine bessere Leistung über Standard oder grundlegende Ebene.** Caches in der Ebene Premium werden auf Hardware bereitgestellt, dessen schnellere Prozessoren und bietet eine bessere Leistung im Vergleich zum Basic oder Standard Ebene. Premium Ebene Caches haben höhere Durchsatzraten und unteren Wartezeiten. 

**Durchsatz für den gleichen gleicher Breite Cache ist in Premium im Vergleich zu Standard Ebene höher.** Beispielsweise den Durchsatz einer 53 P4 GB (Premium) Cache ist 250K-Abfragen pro Sekunde im Vergleich zu 150 K für C6 (Standard).

Weitere Informationen zu Größe, Durchsatz und Bandbreite mit Premium Caches finden Sie unter [Azure Redis Cache häufig gestellte Fragen](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Redis Beibehaltung der Daten
Die Ebene Premium können Sie zum Beibehalten der Cachedaten in einem Speicher Azure-Konto. Alle Daten wird in einem Cache Basic/Standard nur im Arbeitsspeicher gespeichert. Bei der zugrunde liegenden Infrastruktur können es Probleme möglichem sein. Es empfiehlt sich, mit dem Redis Daten Beibehaltung-Feature in der Ebene Premium um Stabilität vor Datenverlust zu vergrößern. Azure Redis Cache bietet RDB und AOF (in Kürze verfügbar) Optionen in [dauerhaften Redis](http://redis.io/topics/persistence). 

Informationen zum Konfigurieren von dauerhaften finden Sie unter [Beibehaltung für einen Premium Azure Redis Cache konfigurieren](cache-how-to-premium-persistence.md).

##<a name="redis-cluster"></a>Redis cluster
Wenn Sie möchten Caches größer als 53 GB erstellen oder Shard Daten in mehreren Redis Knoten möchten, können Sie Redis Cluster, die in der Premium Ebene verfügbar ist. Jeder Knoten besteht aus einem primären/Replikat Cache Paar von Azure, die für eine hohe Verfügbarkeit verwaltet werden. 

**Redis Cluster bietet Ihnen maximalen Dezimalstellen und Durchsatz.** Durchsatz erhöht linear, wenn Sie die Anzahl der mehrere Shards hinweg (Knoten) im Cluster erhöhen. Z. b. Erstellen Sie einen Cluster P4 von 10 mehrere Shards hinweg, und der verfügbare Durchsatz ist 250K * 10 = 2,5 Millionen Abfragen pro Sekunde. Bitte finden Sie im [Azure Redis Cache häufig gestellte Fragen](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) für weitere Details zu den Schriftgrad, Durchsatz und Bandbreite mit Premium Caches ein.

Um mit Cluster anzufangen, finden Sie unter [Cluster für einen Premium Azure Redis Cache konfigurieren](cache-how-to-premium-clustering.md).

##<a name="enhanced-security-and-isolation"></a>Verbesserte Sicherheit und isolation

In der Stufe Basic oder Standard erstellte Cache sind im öffentlichen Internet zugänglich. Zugriff auf den Cache ist eingeschränkt basierend auf die Access-Taste. Mit der Ebene Premium können Sie zu gewährleisten, dass nur Clients in einem bestimmten Netzwerk den Cache zugreifen können. Sie können Redis Cache in einem [Azure-virtuellen Netzwerk (VNet)](https://azure.microsoft.com/services/virtual-network/)bereitstellen. Sie können alle Funktionen von VNet wie Subnetze, Steuerelement-Richtlinien, und weitere Features zum weiteren Einschränken des Zugriffs auf Redis.

Weitere Informationen finden Sie unter [Konfigurieren von Virtual Network-Unterstützung für einen Premium Azure Redis Cache](cache-how-to-premium-vnet.md).

## <a name="importexport"></a>Import/Export

Import/Export ist ein Azure Redis Cache Daten Verwaltungsvorgang wodurch Sie Daten in Azure Redis Cache importieren oder Exportieren von Daten aus Azure Redis Cache durch Importieren und Exportieren von Momentaufnahme Redis-Cache-Datenbank (RDB) aus einem Cache Premium auf einer Seitenblob in einem Azure-Speicher-Konto an. So können Sie zwischen verschiedenen Azure Redis Cache Instanzen migrieren oder den Cache mit Daten vor der Verwendung zu füllen.

Importieren kann Redis kompatibel RDB Datei(en) aus einem beliebigen Redis Server ausgeführt in einem beliebigen Cloud oder -Umgebung, einschließlich Redis unter Linux, Windows oder eine beliebige Cloudanbieter, z. B. Amazon-Webdiensten und andere einbinden verwendet werden. Importieren von Daten ist eine einfache Möglichkeit, einen Cache mit vorab eingetragenen Daten zu erstellen. Während des Importvorgangs Azure Redis Cache lädt die Dateien RDB aus Azure-Speicher in den Speicher und anschließend die Tasten in den Cache eingefügt.

Exportieren können Sie die Daten aus Azure Redis Cache zu kompatibel RDB Datei(en) Redis exportieren. Sie können dieses Feature zum Verschieben von Daten aus einer Instanz von Azure Redis Cache in ein anderes oder einen anderen Redis Server verwenden. Während des Exportvorgangs wird eine temporäre Datei des virtuellen Computers, der die Redis Azure-Cache-Server-Instanz hostet erstellt, und die Datei mit dem Speicherkonto vorgesehenen hochgeladen wird. Nach Abschluss des Exportvorgangs mit entweder Status Erfolg oder Fehler, wird die temporäre Datei gelöscht.

Weitere Informationen finden Sie unter [So in Daten importieren und Exportieren von Daten aus Azure Redis Cache](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Neustart

Die Premium Ebene können Sie einen oder mehrere Knoten von der Cache für Dokumente bei Bedarf neu zu starten. So können Sie die Anwendung für Stabilität im Fall eines Fehlers testen. Sie können die folgenden Knoten neu starten.

-   Master-Knoten mit dem cache
-   Untergeordnete Knoten mit dem cache
-   Sowohl Master- und untergeordnete Knoten mit dem cache
-   Wenn Sie einen Cache Premium mit Cluster verwenden, können Sie das Master-Shape, untergeordnete oder beide Knoten für einzelne mehrere Shards hinweg im Cache neu starten

Weitere Informationen finden Sie unter [Starten](cache-administration.md#reboot) und [Neu starten, häufig gestellte Fragen](cache-administration.md#reboot-faq).

## <a name="schedule-updates"></a>Planen von updates

Das Feature "Geplante Updates" können Sie ein Wartungsfenster für Ihren Cache festzulegen. Wenn Sie das Wartungsfenster angegeben ist, wurden Redis Serverupdates während dieses Fenster vorgenommen. Um ein Wartungsfenster bestimmen, wählen Sie die gewünschten Tage aus, und geben Sie die Wartung starten Fenster Stunde für jeden Tag. Beachten Sie, dass das Fenster Wartung in UTC ist. 

Weitere Informationen finden Sie unter [Planen von Updates](cache-administration.md#schedule-updates) und [häufig gestellte Fragen zum Planen von Updates](cache-administration.md#schedule-updates-faq).

>[AZURE.NOTE] Nur Redis Server vorgenommene Aktualisierungen im Zeitfenster geplante Wartung. Das Wartungsfenster gilt nicht Azure Updates oder Updates für das Betriebssystem virtueller Computer.

## <a name="to-scale-to-the-premium-tier"></a>Klicken Sie auf der Ebene Premium skalieren

Um die Ebene Premium zu skalieren, einfach wählen Sie eine der leisten Premium in das Blade **Ändern Preise Ebene** . Sie können auch Ihren Cache zu der Premium Ebene mit PowerShell und CLI skalieren. Schrittweise Anweisungen finden Sie unter [Skalieren Azure Redis Cache](cache-how-to-scale.md) und [so einen Skalierung Vorgang automatisieren](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie einen Cache, und probieren Sie die neue Premium Ebene.

-   [Konfigurieren von Beibehaltung für einen Premium Azure Redis Cache](cache-how-to-premium-persistence.md)
-   [Konfigurieren von Virtual Network-Unterstützung für einen Premium Azure Redis Cache](cache-how-to-premium-vnet.md)
-   [So konfigurieren Sie für einen Premium Azure Redis Cache Cluster](cache-how-to-premium-clustering.md)
-   [So importieren von Daten in und Exportieren von Daten aus Azure Redis Cache](cache-how-to-import-export-data.md)
-   [Zum Verwalten von Azure Redis Cache](cache-administration.md)
  

