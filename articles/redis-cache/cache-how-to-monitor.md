<properties 
    pageTitle="So Azure Redis Cache überwachen | Microsoft Azure" 
    description="Informationen Sie zum Status und Performance Überwachen Ihrer Instanzen Azure Redis Cache" 
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
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-monitor-azure-redis-cache"></a>Zum Überwachen der Azure Redis Cache

Azure Redis Cache bietet mehrere Optionen zum Überwachen der Cacheinstanzen. Sie können Kennzahlen anzeigen, Kennzahlen Diagramme, um die Startboard anheften, anpassen den Datums- und Zeitbereich der Überwachung Diagramme, hinzufügen und entfernen Kennzahlen aus der Diagramme, und Festlegen von Benachrichtigungen, wenn bestimmte Bedingungen erfüllt sind. Diese Tools können Sie den Integritätsstatus Ihrer Azure Redis Cache Instanzen überwachen und können Sie Ihre Zwischenspeichern Applikationen verwalten.

Wenn Cache Diagnose aktiviert sind, sind Kennzahlen zur Azure Redis Cache Instanzen ungefähr alle 30 Sekunden gesammelt und gespeichert, sodass die Kennzahlen Diagramme angezeigt und von Warnungsregeln ausgewertet werden kann.

Cache Kennzahlen werden mithilfe der Redis gesammelten [Informationen](http://redis.io/commands/info) Befehl. Weitere Informationen zu den verschiedenen Informationen für jeden zu verwendenden Werte Zwischenspeichern Metrisch, finden Sie unter [Verfügbare statistische Werte und Intervalle reporting](#available-metrics-and-reporting-intervals).

Cache Kennzahlen, [Durchsuchen](cache-configure.md#configure-redis-cache-settings) der Cache Instanz der [Azure-Portal](https://portal.azure.com)anzeigen zu können. Kennzahlen zur Azure Redis Cache Instanzen sind auf dem Blade **Redis Kennzahlen** zugegriffen werden.

![Kennzahlen redis][redis-cache-redis-metrics-blade]

>[AZURE.IMPORTANT] Wenn die folgende Meldung in das Blade **Redis Kennzahlen** angezeigt wird, führen Sie die Schritte im Abschnitt [Cache Diagnose aktivieren](#enable-cache-diagnostics) Cache Diagnose aktivieren aus.
>
>`Monitoring may not be enabled. Click here to turn on Diagnostics.`

Das Blade **Redis Kennzahlen** weist **Überwachung** Diagramme, in denen Kennzahlen Cache angezeigt. Jedes Diagramm kann durch Hinzufügen oder Entfernen von Kennzahlen und Ändern des Intervalls reporting angepasst werden. Zum Anzeigen und Konfigurieren von Vorgängen und Benachrichtigungen, hat das Blade **Redis Cache** einen Abschnitt mit **Vorgängen** , der Cache **Ereignisse** und **Warnungsregeln**angezeigt werden.

## <a name="enable-cache-diagnostics"></a>Cache Diagnose aktivieren

Azure Redis Cache bietet Ihnen die Möglichkeit, Diagnose Daten in einem Speicherkonto gespeichert ist, sodass Sie alle Programme, die Sie zugreifen und Sie die Daten direkt verarbeiten möchten, verwenden können. Damit Cache Diagnose gesammelt werden kann muss gespeichert und angezeigt im Portal Azure Speicher-Konto konfiguriert sein. Caches in der gleichen Region und Abonnement freigeben das gleiche Diagnose Speicher-Konto, und wenn die Konfiguration geändert wird, gilt für alle Caches in das Abonnement, die in diesem Bereich werden.

Navigieren Sie zum Aktivieren und Konfigurieren der Cache Diagnose, an die Blade **Redis Cache** für Ihre Cacheinstanz aus. Wenn Diagnose noch nicht aktiviert sind, wird eine Meldung anstelle eines Diagramms Diagnose angezeigt.

![Cache Diagnose aktivieren][redis-cache-enable-diagnostics]

Klicken Sie auf die Nachricht an das Blade **Metrisch** anzuzeigen, und klicken Sie auf **diagnoseeinstellungen** , um zu aktivieren und konfigurieren Sie die Diagnose Einstellungen für die Instanz des Diensts Cache.

![Diagnose][redis-cache-diagnostic-settings]

![Konfigurieren der Diagnose][redis-cache-configure-diagnostics]

Klicken Sie auf die Schaltfläche **auf** , um Cache Diagnose aktivieren, und zeigen Sie die Diagnosekonfiguration.

Klicken Sie auf den Pfeil rechts neben der **Speicher-Konto** ein Speicherkonto diagnostische Daten aufnehmen auswählen. Wählen Sie zur Optimierung der Systemleistung ein Speicherkonto in derselben Region als dem Cache.

Sobald die diagnoseeinstellungen konfiguriert sind, klicken Sie auf **Speichern** , um die Konfiguration zu speichern. Beachten Sie, dass dauert möglicherweise einen Moment, damit die Änderungen wirksam werden.

>[AZURE.IMPORTANT] Caches in der gleichen Region und Abonnement Freigeben der gleichen Speicher-Diagnose, und wenn die Konfiguration geänderte (Diagnose aktiviert/deaktiviert oder Ändern des Speicherkontos) gilt für alle Caches in das Abonnement, die in diesem Bereich sind.

Untersuchen Sie zum Anzeigen der gespeicherten Metrik, die die Tabellen in Ihrem Speicherkonto mit Namen, die mit beginnen `WADMetrics`. Weitere Informationen zu den Zugriff auf die gespeicherten Metrik außerhalb der Azure-Portal finden Sie unter der Stichprobe [Access Redis Cache Überwachung Daten](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) .

>[AZURE.NOTE] Nur Kennzahlen, die in der ausgewählten Speicher-Konto gespeichert werden, werden im Azure-Portal angezeigt. Wenn Sie Speicherplatz Konten ändern, die Daten in das Speicherkonto zuvor konfiguriert bleiben zum Download zur Verfügung, aber nicht im Azure-Portal angezeigt.  

## <a name="available-metrics-and-reporting-intervals"></a>Verfügbare statistische Werte und Intervalle reporting

Cache Kennzahlen werden mit verschiedenen reporting Zeiten, einschließlich **vergangenen Stunde**, **heute**, **letzten Woche**und **benutzerdefinierte**gemeldet. Das Blade **Metrisch** für jedes Diagramm Metrik zeigt die durchschnittliche, maximale und minimale Werte für jede Metrik im Diagramm, und einige Kennzahlen anzeigen einer Summe für das Intervall reporting. 

Jede Metrik enthält zwei Versionen. Eine Metrik misst Performance für den gesamten Cache und für Caches, die [Cluster](cache-how-to-premium-clustering.md), eine zweite Version der Metrik verwenden, enthält `(Shard 0-9)` in die Namen Measures Leistung für eine einzelne Shard in einem Cache. Wenn beispielsweise ein Cache 4 mehrere Shards hinweg, `Cache Hits` ist die Gesamtmenge der Treffer für den gesamten Cache und und `Cache Hits (Shard 3)` ist nur die Treffer für die Shard des Caches.

>[AZURE.NOTE] Auch wenn der Cache mit keine verbundenen aktiven Clientanwendungen im Leerlauf befindet, wird möglicherweise einige Cacheaktivität, z. B. verbundenen Clients, arbeitsspeicherauslastung und ausgeführten Vorgänge angezeigt. Diese Aktivität ist während des Importvorgangs einer Instanz Azure Redis Cache normal.

| Metrisch            | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cache-Treffer        | Die Anzahl der erfolgreichen Key Suchvorgänge während des Intervalls der angegebenen reporting. Diese ordnet `keyspace_hits` Befehl aus der Redis [Informationen](http://redis.io/commands/info) .                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Cachefehler      | Die Anzahl der Fehler beim Key Suchvorgänge während des Intervalls der angegebenen reporting. Diese ordnet `keyspace_misses` aus dem Befehl Redis INFO. Cachefehler nicht zwangsläufig, dass es ist ein Problem mit dem Cache. Beispielsweise wird bei Verwendung das Cache-Aside programming Muster Anwendung ersten für ein Element im Cache. Wenn das Element nicht vorhanden (Cache entgehen) ist, wird das Element aus der Datenbank abgerufen und dem Cache für eine erneute Verwendung hinzugefügt. Cachefehler sind normale Verhalten für die Programmierung Cache-Aside Muster. Ist die Anzahl der Cachefehler höher als erwartet, überprüfen Sie die Anwendungslogik, die füllt und liest aus dem Cache. Wenn Elemente aus dem Cache aufgrund Arbeitsspeicherdruck entfernt wird werden, und klicken Sie dann möglicherweise einige-Cache-Fehler, aber wäre eine bessere Metrik zum Überwachen der Arbeitsspeicherdruck in `Used Memory` oder `Evicted Keys`. |
| Verbundene Clients | Die Anzahl der Clientverbindungen mit dem Cache während des Intervalls der angegebenen reporting. Diese ordnet `connected_clients` aus dem Befehl Redis INFO. Versucht, eine nachfolgende Verbindung zum Cache schlägt fehl, wenn der [Verbindung Grenzwert](cache-configure.md#default-redis-server-configuration) erreicht ist. Beachten Sie, dass, auch wenn es keine aktiven Clientanwendung sind, es kann immer noch ein paar Instanzen von verbundenen Clients aufgrund interner Prozesse und Verbindungen.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Entfernten Tasten      | Die Anzahl der Elemente aus dem Cache entfernt werden, während das angegebene reporting Intervall aufgrund von der `maxmemory` bestehen. Diese ordnet `evicted_keys` aus dem Befehl Redis INFO.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Abgelaufene Tasten      | Die Anzahl der Elemente, aus dem Cache während des angegebenen reporting Intervalls abgelaufen. Dieser Wert wird zugeordnet `expired_keys` aus dem Befehl Redis INFO.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Ruft ab              | Die Anzahl der Get-Vorgänge aus dem Cache während des Intervalls der angegebenen reporting. Dieser Wert ist die Summe der folgenden Werte aus der Redis Informationen Befehl ' alle ': `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, und `cmdstat_getrange`, und entspricht der Summe der Cachezugriffe und Fehler während des Intervalls reporting.                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Redis Server laden | Der Prozentsatz der Zyklen, in denen der Redis-Server beschäftigt Verarbeitung und nicht warten für Nachrichten im Leerlauf ist. Wenn dieser Zähler 100 ist, bedeutet, dass Redis Server verfügt über eine Leistung Obergrenze Treffer und die CPU kann nicht verarbeitet, arbeiten Sie eine schneller. Wenn die hoher Auslastung für Redis Server angezeigt werden, und klicken Sie dann die Timeout Ausnahmen im Client angezeigt werden. In diesem Fall sollten Sie erwägen, dieselbe Skalierung nach oben oder Partitionieren der Daten in mehreren Caches.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Datensätze              | Die Anzahl der Set-Vorgänge in den Cache während des Intervalls der angegebenen reporting. Dieser Wert ist die Summe der folgenden Werte aus der Redis Informationen Befehl ' alle ': `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange`, und `cmdstat_setnx`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Gesamte Vorgänge  | Die Gesamtzahl der Befehle, die vom Cacheserver während des angegebenen reporting Intervalls verarbeitet. Dieser Wert wird zugeordnet `total_commands_processed` aus dem Befehl Redis INFO. Beachten Sie, dass wenn Azure Redis Cache für Pub/Sub rein keine Kennzahlen für werden `Cache Hits`, `Cache Misses`, `Gets`, oder `Sets`, aber es werden `Total Operations` Kennzahlen, die die Verwendung der Cache für Vorgänge Pub/Sub wiedergeben.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Verwendeter Speicher       | Die Speichermenge Cache für Schlüssel/Wert-Paare im Cache in MB während des angegebenen reporting Intervalls verwendet. Dieser Wert wird zugeordnet `used_memory` aus dem Befehl Redis INFO. Dies schließt nicht Metadaten oder Fragmentierung.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Verwendeter Speicher RSS   | Der Cache verwendete Speichermenge in MB während des angegebenen reporting Intervalls, einschließlich Fragmentierung und Metadaten. Dieser Wert wird zugeordnet `used_memory_rss` aus dem Befehl Redis INFO.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| CPU               | Die CPU-Auslastung des Servers Azure Redis Cache als Prozentsatz während des Intervalls der angegebenen reporting. Dieser Wert wird das Betriebssystem zugeordnet `\Processor(_Total)\% Processor Time` Performance-Zähler.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Cache gelesen        | Lesen Sie die Menge der Daten aus dem Cache in MB pro Sekunde (MB/s) während des Intervalls der angegebenen reporting. Dieser Wert wird von Karten Schnittstelle Netzwerk abgeleitet, die den virtuellen Computern unterstützen, der den Cache für hostet und nicht bestimmten Redis. **Dieser Wert entspricht der Netzwerk-Bandbreite von dieser Cache verwendet. Wenn Sie Benachrichtigungen für Server Seite Netzwerk Bandbreitengrenzwerte einrichten möchten, klicken Sie dann erstellen Sie ihn mit diesem `Cache Read` Zähler. Finden Sie in [dieser Tabelle](cache-faq.md#cache-performance) für die beobachteten Bandbreitengrenzwerte für die verschiedenen Ebenen und Größen Preise Cache.**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Cache Schreiben       | Die Menge der Daten im Cache in MB pro Sekunde (MB/s) während der angegebenen geschrieben reporting Intervall. Dieser Wert wird von Karten Schnittstelle Netzwerk abgeleitet, die den virtuellen Computern unterstützen, der den Cache für hostet und nicht bestimmten Redis. Dieser Wert entspricht der Netzwerk-Bandbreite der Daten im Cache, die vom Client gesendet werden.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |


## <a name="how-to-view-metrics-and-customize-charts"></a>So Kennzahlen anzeigen und Anpassen von Diagrammen

Sie können einen Überblick über die Metrik für Ihren Cache auf das Blade **Redis Kennzahlen** anzeigen. Wählen Sie **Alle Einstellungen**der **Redis Kennzahlen** Zugriff auf Blade > **Redis Kennzahlen**.

![Kennzahlen redis][redis-cache-redis-metrics]


Das Blade **Redis Kennzahlen** enthält die folgenden Diagramme.

| Kennzahlen Diagramm redis | Angezeigten Kennzahlen     |
|------------------|-------------------|
| Cache Lese- und Cache Schreiben | Cache gelesen |
|                            | Cache Schreiben |
| Verbundene Clients      | Verbundene Clients |
| Treffer und Fehlern  | Cache-Treffer        |
|                  | Cachefehler      |
| Befehle gesamt   | Gesamte Vorgänge  |
| Ruft und Mengen angezeigt werden    | Ruft ab              |
|                  | Datensätze              |
| CPU-Auslastung         | CPU           |
| Arbeitsspeicherauslastung      | Verwendeter Speicher   |
|                   | Verwendeter Speicher RSS |
| Redis Server laden | Server laden   |
| Anzahl der Schlüssel | Schlüssel insgesamt |
|           | Entfernten Tasten |
|           | Abgelaufene Tasten |


Für eine ausführlichere Ansicht der Metrik, die in einem bestimmten Diagramm und zum Anpassen des Diagramms, klicken Sie auf das gewünschte Diagramm aus dem Blade **Redis Kennzahlen** das Blade **Metrik** für das Diagramm angezeigt werden.

![Metrische blade][redis-cache-metric-blade]

Alle Benachrichtigungen, die auf die Metrik angezeigt, indem Sie ein Diagramm festgelegt sind, werden am unteren Rand der **Metrisch** Blade für Diagramm aufgeführt.

Wählen Sie zum Hinzufügen oder Entfernen von Kennzahlen oder Ändern des Intervalls für die Berichterstattung, **Diagramm bearbeiten**aus.

Zum Hinzufügen oder entfernen Kriterien aus dem Diagramm, klicken Sie auf das Kontrollkästchen neben dem Namen der Metrik. Um das reporting Intervall ändern möchten, klicken Sie auf das gewünschte Intervall. Wenn Sie den **Diagrammtyp**ändern möchten, klicken Sie auf das gewünschte Format. Nachdem Sie die gewünschten Änderungen vorgenommen werden, klicken Sie auf **Speichern**. 

![Bearbeiten des Diagramms][redis-cache-edit-chart]

Beim Klicken auf **Speichern** werden Ihre Änderungen beibehalten, bis Sie das Blade **Metrisch** lassen. Wenn Sie später wieder sind, wird die ursprüngliche metrische und Zeitraums erneut angezeigt. Weitere Informationen zum Anpassen von Diagrammen finden Sie unter [Kriterien für Monitor Service](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

Wenn die Kriterien für einen bestimmten Zeitraum in einem Diagramm anzeigen möchten, bewegen Sie die Maus über einen bestimmten Balken oder Punkte auf das Diagramm, das auf die gewünschte Zeit entspricht, und die Metrik für dieses Intervall angezeigt werden.

![Anzeigen von Details zu Diagramm][redis-cache-view-chart-details]

## <a name="how-to-monitor-a-premium-cache-with-clustering"></a>Wie Sie einen Cache Premium mit Cluster überwachen

Premium Caches, die [Cluster](cache-how-to-premium-clustering.md) aktiviert haben, können bis zu 10 mehrere Shards hinweg haben. Jede Shard verfügt über eine eigene Kennzahlen, und diese Kennzahlen aggregiert, um die Kriterien für den Cache als Ganzes bereitstellen. Jede Metrik enthält zwei Versionen. Eine Metrik measures Leistung für den gesamten Cache und eine zweite Version der Metrik, die umfasst `(Shard 0-9)` in die Namen Measures Leistung für eine einzelne Shard in einem Cache. Wenn beispielsweise ein Cache 3 mehrere Shards hinweg, `Cache Hits` ist die Gesamtmenge der Treffer für den gesamten Cache und und `Cache Hits (Shard 2)` ist nur die Treffer für die Shard des Caches.

Jede Überwachung Diagramm zeigt der obersten Ebene Metrik für den Cache zusammen mit der Metrik für jede Shard Cache.

![Monitor][redis-cache-premium-monitor]

Beim Bewegen der Maus über die Datenpunkte zeigt die Details zu diesem Zeitpunkt Zeitpunkt an. 

![Monitor][redis-cache-premium-point-summary]

Größere Werte sind in der Regel die zusammengefassten Werte für den Cache die kleineren Werten der einzelnen Metrik für die Shard. Beachten Sie, dass in diesem Beispiel es drei mehrere Shards hinweg gibt und den Cachetreffer gleichmäßig auf die mehrere Shards hinweg verteilt werden.

![Monitor][redis-cache-premium-point-shard]

Das Diagramm, um eine erweiterte Ansicht auf das Blade **Metrisch** anzeigen klicken Sie auf ausführlicher anzeigen zu können.

![Monitor][redis-cache-premium-chart-detail]

Standardmäßig umfasst jede Diagramm der Zähler auf oberster Ebene Cache Leistung sowie die Leistungsindikatoren für die einzelnen mehrere Shards hinweg. Sie können diese auf das **Diagramm bearbeiten** Blade anpassen.

![Monitor][redis-cache-premium-edit]

Weitere Informationen über die verfügbaren Leistungsindikatoren finden Sie unter [Verfügbare statistische Werte und Intervalle reporting](#available-metrics-and-reporting-intervals).


## <a name="operations-and-alerts"></a>Vorgänge und Benachrichtigungen

Abschnitt mit den **Vorgängen** auf dem **Cache Redis** Blade hat **Ereignisse** und **Warnungsregeln** Abschnitte.

![Oeprations][redis-cache-operations-events]

Um eine Liste der zuletzt verwendete Cache Vorgänge anzuzeigen, klicken Sie auf das Diagramm **Ereignisse** , um das Blade **Ereignisse** anzuzeigen. Beispiele für Vorgänge sind abrufen und erneutes Generieren Zugriffstasten, und die Aktivierung und Auflösung von Warnungsregeln. Weitere Informationen zu jedem Ereignis klicken Sie auf das Ereignis in das Blade **Ereignisse** .

Weitere Informationen zu Ereignissen finden Sie unter [Anzeigen von Ereignissen und Überwachungsprotokolle](../monitoring-and-diagnostics/insights-debugging-with-events.md).

Im Abschnitt **Warnungsregeln** zeigt die Anzahl der Benachrichtigungen für die Cacheinstanz aus. Eine Benachrichtigung Regel können Sie Ihre Cacheinstanz überwachen und erhalten eine e-Mail, wenn ein bestimmte metrischer Wert den in der Regel definierten Schwellenwert erreicht. 

Warnungsregeln ungefähr fünf Minuten ausgewertet werden, und wenn eine Regel aktiviert ist, werden alle konfigurierten Benachrichtigungen gesendet. Benachrichtigen Regel Vorgänge und Benachrichtigungen werden nicht sofort verarbeitet; Möglicherweise gibt es eine Verzögerung von mehreren Minuten anzugeben, bevor Sie eine Regel aktiviert wurde und Benachrichtigungen gesendet werden.

Warnungsregeln können angezeigt und aus dem Blade **Metrik** für eine bestimmte Überwachung Diagramm oder aus dem Blade **Warnungsregeln** festgelegt werden.

Warnungsregeln haben die folgenden Eigenschaften.

| Regel-Eigenschaft                 | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ressource                            | Die Ressource durch die Regel ausgewertet wird. Wenn Sie eine Regel aus einem Redis Cache erstellen, ist der Cache der Ressource an.                                                                                                                                                                                                                                                                                                                                                  |
| Namen                                | Name, der die Regel in der aktuellen Cacheinstanz identifiziert.                                                                                                                                                                                                                                                                                                                                                                                       |
| Beschreibung                         | Optionale Beschreibung für die Regel.                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Metrisch                              | Die Metrik beobachtet werden, indem Sie die Regel. Eine Liste der Cache Kennzahlen finden Sie unter Verfügbare statistische Werte und Intervalle reporting.                                                                                                                                                                                                                                                                                                                                             |
| Bedingung                           | Der Bedingungsoperator für die Regel. Mögliche Angaben sind: größer als, größer als oder gleich, kleiner als, kleiner als oder gleich                                                                                                                                                                                                                                                                                                                             |
| Schwellenwert                           | Der Wert verwendet, um mit der Metrik mit dem Operator angegeben haben, indem Sie die Bedingungseigenschaft vergleichen. Je nach der Metrik und diesen Wert möglicherweise Bytes/Sekunde, Bytes, % oder Anzahl.                                                                                                                                                                                                                                                                                     |
| Periode zurück                              | Gibt den Zeitraum, über den Mittelwert der mittlere Wert der Metrik für den Vergleich benachrichtigen Regel verwendet wird. Ist der Zeitraum über der letzten Stunde, wird der Durchschnittswert von die Metrik für das vorherige Stundenintervall für den Vergleich verwendet. Wenn Sie möchten benachrichtigt werden, wenn der Schwellenwert aufgrund einer Sammlung Aktivität erfüllt ist, ist eine kürzere Frist geeignet. Wenn Sie benachrichtigt werden, wenn es längeren Aktivitäten über dem Schwellenwert, verwenden Sie längere aus. |
| E-Mail-Dienst und Co-Administratoren | Der Wert true, werden die Dienstadministrator und gemeinsame Administrator per e-Mail, wenn die Benachrichtigung aktiviert ist.                                                                                                                                                                                                                                                                                                                                                                    |
| Zusätzliche Administrator-e-Mail      | Optional: e-Mail-Adresse für einen zusätzlichen Administrator benachrichtigt werden, wenn die Benachrichtigung aktiviert ist.                                                                                                                                                                                                                                                                                                                                                                    |

Pro Regel Aktivierung wird nur eine Benachrichtigung gesendet. Nachdem Sie den Schwellenwert für eine Regel überschritten wird, und eine Benachrichtigung gesendet wird, wird die Regel erst die Metrik unter den Schwellenwert fällt nicht erneut ausgewertet. Wenn die Metrik später den Schwellenwert überschreitet, wird die Benachrichtigung wieder aktiviert, und eine neue Benachrichtigung gesendet wird.

Um alle die Warnungsregeln für Ihre Cacheinstanz anzuzeigen, klicken Sie auf das Webpart **Warnungsregeln** in das Blade **Redis Cache** . Um nur die Warnungsregeln anzuzeigen, die eine bestimmte Metrik verwenden, navigieren Sie zu der **Metrisch** Blade für das Diagramm, das die Metrik enthält.

![Warnungsregeln][redis-cache-alert-rules]

Um eine Regel hinzuzufügen, klicken Sie auf **Hinzufügen einer Benachrichtigung für** aus dem Blade **Metrisch** oder das **Warnungsregeln** Blade. 

Geben Sie die gewünschte Regelkriterien in das **Hinzufügen einer Benachrichtigung** Regel Blade aus, und klicken Sie auf **OK**. 

![Regel hinzufügen][redis-cache-add-alert]

>[AZURE.NOTE] Wenn Sie eine Regel erstellt wurde, indem Sie auf **Add Benachrichtigung** aus dem Blade **Metrisch** , werden nur die angezeigt wird, klicken Sie auf das Diagramm in dieser Blade Metrik in der Dropdownliste **Metrisch** angezeigt. Wenn Sie eine Regel erstellt wurde, indem Sie auf **Add Benachrichtigung** aus dem Blade **Warnungsregeln** , sind alle Cache Kriterien in der Dropdownliste **Metrisch** verfügbar.

Nachdem eine Benachrichtigung Regel es gespeichert wurde angezeigt wird, klicken Sie auf das Blade **Warnungsregeln** und klicken Sie auf das **Metrisch** Blade für alle Diagramme, die die Metrik verwendet in der Regel angezeigt. Um eine Regel zu bearbeiten, klicken Sie auf den Namen der Regel benachrichtigen das Blade **Regel bearbeiten** angezeigt werden. Aus dem Blade **Regel bearbeiten** können Sie die Eigenschaften der Regel bearbeiten, löschen oder deaktivieren die Regel oder die Regel wieder aktivieren, wenn sie zuvor deaktiviert wurde.

>[AZURE.NOTE] Alle Änderungen, die Sie an die Eigenschaften der Regel dauert einen Moment, bevor sie auf das Blade **Warnungsregeln** oder das Blade **Metrisch** angezeigt werden.

Wenn eine Regel aktiviert ist, eine e-Mail-Nachricht wird gesendet, je nach Konfiguration der Benachrichtigung Regel und ein Symbols wird im Webpart auf den **Cache Redis** Blade **Warnungsregeln** angezeigt.

Gelöst werden, wenn Sie nicht mehr die Benachrichtigung Bedingung wahr ist, wird eine Benachrichtigung Regel angesehen. Sobald die Regel Bedingung gelöst ist, wird das Symbol "Benachrichtigung" mit einem Häkchen ersetzt. Weitere Informationen zu benachrichtigen Vorgänge und Lösungen klicken Sie auf das Webpart **Ereignisse** klicken Sie auf das Blade **Redis Cache** zu Ereignisse das Blade **Ereignisse** anzuzeigen.

Weitere Informationen zu Benachrichtigungen in Azure finden Sie unter [-Benachrichtigung empfangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

## <a name="metrics-on-the-redis-cache-blade"></a>Klicken Sie auf das Redis Cache Blade Kennzahlen

Das Blade **Cache Redis** zeigt die folgenden Kategorien der Kennzahlen.

-   [Überwachen von Diagrammen](#monitoring-charts)
-   [Verwendung von Diagrammen](#usage-charts)

### <a name="monitoring-charts"></a>Überwachen von Diagrammen

Im Abschnitt **Überwachung** weist **Treffer und Zugriffe**, **abgerufen und festgelegt**, **Verbindungen**und **Befehle gesamt** Diagramme.

![Überwachen von Diagrammen][redis-cache-monitoring-part]

Die folgende Metrik anzeigen die **Überwachung** Diagramme

| Diagramm für die Überwachung | Kennzahlen Cache     |
|------------------|-------------------|
| Treffer und Fehlern  | Cache-Treffer        |
|                  | Cachefehler      |
| Ruft und Mengen angezeigt werden    | Ruft ab              |
|                  | Datensätze              |
| Verbindungen      | Verbundene Clients |
| Befehle gesamt   | Gesamte Vorgänge  |

Informationen zum Anzeigen der Metrik und Anpassen die einzelnen Diagramme in diesem Abschnitt finden Sie unter den folgenden Abschnitt [So Kennzahlen anzeigen und Anpassen von Diagrammen Kennzahlen](#how-to-view-metrics-and-customize-charts) .

### <a name="usage-charts"></a>Verwendung von Diagrammen

Im Abschnitt **Verwendung** weist **Redis Server laden**, **Arbeitsspeicherauslastung**, **Netzwerk-Bandbreite**und **CPU-Auslastung** Diagramme und auch die **Preise Ebene** für die Cacheinstanz angezeigt.

![Verwendung von Diagrammen][redis-cache-usage-part]

Zeigt die **Preise Ebene** der Cache Preise gestuft, und kann mit [dem Maßstab](cache-how-to-scale.md) den Cache an eine andere Preisgestaltung Ebene verwendet.

Die folgende Metrik anzeigen die **Verwendung** Diagramme

| Verwendung-Diagramm       | Kennzahlen Cache |
|-------------------|---------------|
| Redis Server laden | Server laden   |
| Arbeitsspeicherauslastung      | Verwendeter Speicher   |
| Netzwerk-Bandbreite | Cache Schreiben   |
| CPU-Auslastung         | CPU           |

Informationen zum Anzeigen der Metrik und Anpassen die einzelnen Diagramme in diesem Abschnitt finden Sie unter den folgenden Abschnitt [So Kennzahlen anzeigen und Anpassen von Diagrammen Kennzahlen](#how-to-view-metrics-and-customize-charts) .
  
<!-- IMAGES -->

[redis-cache-enable-diagnostics]: ./media/cache-how-to-monitor/redis-cache-enable-diagnostics.png

[redis-cache-diagnostic-settings]: ./media/cache-how-to-monitor/redis-cache-diagnostic-settings.png

[redis-cache-configure-diagnostics]: ./media/cache-how-to-monitor/redis-cache-configure-diagnostics.png

[redis-cache-monitoring-part]: ./media/cache-how-to-monitor/redis-cache-monitoring-part.png

[redis-cache-usage-part]: ./media/cache-how-to-monitor/redis-cache-usage-part.png

[redis-cache-metric-blade]: ./media/cache-how-to-monitor/redis-cache-metric-blade.png

[redis-cache-edit-chart]: ./media/cache-how-to-monitor/redis-cache-edit-chart.png

[redis-cache-view-chart-details]: ./media/cache-how-to-monitor/redis-cache-view-chart-details.png

[redis-cache-operations-events]: ./media/cache-how-to-monitor/redis-cache-operations-events.png

[redis-cache-alert-rules]: ./media/cache-how-to-monitor/redis-cache-alert-rules.png

[redis-cache-add-alert]: ./media/cache-how-to-monitor/redis-cache-add-alert.png

[redis-cache-premium-monitor]: ./media/cache-how-to-monitor/redis-cache-premium-monitor.png

[redis-cache-premium-edit]: ./media/cache-how-to-monitor/redis-cache-premium-edit.png

[redis-cache-premium-chart-detail]: ./media/cache-how-to-monitor/redis-cache-premium-chart-detail.png

[redis-cache-premium-point-summary]: ./media/cache-how-to-monitor/redis-cache-premium-point-summary.png

[redis-cache-premium-point-shard]: ./media/cache-how-to-monitor/redis-cache-premium-point-shard.png

[redis-cache-redis-metrics]: ./media/cache-how-to-monitor/redis-cache-redis-metrics.png

[redis-cache-redis-metrics-blade]: ./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png



