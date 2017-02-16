<properties 
    pageTitle="Konfigurieren von Beibehaltung der Daten für einen Premium Azure Redis Cache" 
    description="Informationen Sie zum Konfigurieren und Verwalten von Daten Beibehaltung der Premium Ebene Azure Redis Cache Instanzen" 
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
    ms.date="09/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-data-persistence-for-a-premium-azure-redis-cache"></a>Konfigurieren von Beibehaltung der Daten für einen Premium Azure Redis Cache

Azure Redis Cache weist verschiedene Cache Angebote die Flexibilität bei der Wahl der Cachegröße und Funktionen, einschließlich der neuen Premium Stufe zur Verfügung zu stellen.

Die Azure Redis Cache Premium Stufe umfasst Features wie Cluster, Beibehaltung und virtual Network-Unterstützung. Dieser Artikel beschreibt, wie eine Instanz der Premium Azure Redis Cache Beibehaltung konfigurieren.

Informationen zu anderen Premium Cachefeatures finden Sie unter [Einführung in die Ebene Azure Redis Cache Premium](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Was ist die Beibehaltung der Daten?
Redis Beibehaltung ermöglicht Ihnen, die in Redis gespeicherte Daten beibehalten werden. Sie können auch Momentaufnahmen und Sichern Sie die Daten, die Sie im Fall eines Hardware-Fehlers laden können. Dies ist ein großer Vorteil gegenüber Basic oder Standard-Ebene, wobei alle Daten im Speicher gespeichert wird und Datenverluste beim Ausfall eines Cache Knoten, in dem unten sind möglich. 

Azure Redis Cache bietet Redis Beibehaltung [RDB Modell](http://redis.io/topics/persistence)und der Daten in einem Azure-Speicher-Konto Speicherort verwenden. Wenn Beibehaltung konfiguriert ist, behält Azure Redis Cache eine Momentaufnahme des Caches Redis in einem Redis Binärformat auf einem Datenträger, auf der Basis einer konfigurierbaren Sicherung Häufigkeit aus. Wenn ein schwerwiegendes Ereignis, die die primären und die Replikatgruppenbezeichner Cache deaktiviert eintritt, wird der Cache mit den letzten Snapshot wiederhergestellt.

Beibehaltung kann während der Erstellung des Caches für, und klicken Sie auf das Blade **Einstellungen** für vorhandene Premium Caches aus dem **Cache für neue Redis** Blade konfiguriert werden.

## <a name="create-a-premium-cache"></a>Erstellen Sie einen Premium-cache

Um einen Cache erstellen und konfigurieren Beibehaltung, [Azure-Portal](https://portal.azure.com) anmelden, und klicken Sie auf **neu**->**Daten + Speicher**>**Redis Cache**.

![Erstellen Sie einen Redis Cache][redis-cache-new-cache-menu]

Um Beibehaltung konfigurieren möchten, wählen Sie zunächst eine **Premium** Caches in das Blade **Wählen Sie Ihre Preisgestaltung Ebene aus** .

![Wählen Sie Ihre Preisgestaltung Ebene aus.][redis-cache-premium-pricing-tier]

Nachdem eine Ebene Preise Premium ausgewählt ist, klicken Sie auf **Redis Beibehaltung**.

![Beibehaltung redis][redis-cache-persistence]

Die Schritte im folgenden Abschnitt beschrieben Redis Beibehaltung auf dem neuen Premium Cache zu konfigurieren. Nachdem Redis Beibehaltung konfiguriert ist, klicken Sie auf **Erstellen** , um Ihren neuen Premium Cache mit Redis Beibehaltung zu erstellen.

## <a name="configure-redis-persistence"></a>Konfigurieren von Redis Beibehaltung

Redis Beibehaltung auf das Blade **Redis Daten Beibehaltung** konfiguriert ist. Für neue Caches wird diese Blade während der Erstellungsprozess Cache zugegriffen, wie im vorherigen Abschnitt beschrieben. Für vorhandene Caches erfolgt das Blade **Redis Beibehaltung von Daten** aus dem Blade **Einstellungen** für Ihren Cache.

![Redis Einstellungen][redis-cache-settings]

Um Redis Speicherung zu aktivieren, klicken Sie auf **aktiviert** , um RDB (Redis Datenbank) Sicherung aktivieren. Um Redis Beibehaltung auf einem zuvor aktiviert Premium Cache zu deaktivieren, klicken Sie auf **deaktiviert**.

Um die Sicherung Intervall konfigurieren, wählen Sie eine **Sicherungskopie Häufigkeit** aus der Dropdownliste aus. Auswahlmöglichkeiten gehören **15 Minuten**, **30 Minuten**, **60 Minuten**, **6 Stunden**, **12 Stunden**und **24 Stunden**. Dieses Intervall wird gestartet, nach der vorherigen Sicherung erfolgreich abgeschlossen, und wenn es abläuft, wird eine neue Sicherung initiiert Countdown durchzuführen.

Klicken Sie auf **Speicher-Konto** das Speicherkonto verwenden aus, und wählen Sie entweder den **Primärschlüssel** oder **sekundären Schlüssel** aus der Dropdownliste **Storage Key** zu verwenden. Sie müssen ein Speicherkonto in derselben Region als Cache auswählen, und ein Konto **Premium Speicher** wird empfohlen, weil Premium Speicher höheren Durchsatz weist. 

>[AZURE.IMPORTANT] Wenn Sie die Taste Speicherplatz für Ihr Konto Beibehaltung erneut generiert wird, müssen Sie den gewünschten Schlüssel aus der Dropdownliste **Storage Key** rechoose.

![Beibehaltung redis][redis-cache-persistence-selected]

Klicken Sie auf **OK** , um die Konfiguration Beibehaltung zu speichern.

Die nächsten (oder ersten Sicherung für neue Caches) wird nach Ablauf der Sicherungsdatei Häufigkeit Intervalls initiiert.



## <a name="persistence-faq"></a>Beibehaltung häufig gestellte Fragen

Die folgende Liste enthält Antworten auf häufig gestellte Fragen zur Azure Redis Cache Beibehaltung.

-   [Kann ich die Beibehaltung einer zuvor erstellten Cache aktivieren?](#can-i-enable-persistence-on-a-previously-created-cache)
-   [Kann ich die Sicherung Häufigkeit ändern, nachdem ich den Cache erstellen?](#can-i-change-the-backup-frequency-after-i-create-the-cache)
-   [Warum bei ich eine Sicherung Häufigkeit von 60 Minuten es mehr als 60 Minuten zwischen Sicherungskopien beträgt?](#why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
-   [Was geschieht mit den alten Sicherungskopien, wenn eine neue Sicherung erfolgt?](#what-happens-to-the-old-backups-when-a-new-backup-is-made)
-   [Was passiert, wenn ich zu einer anderen Größe skaliert haben und eine Sicherung wiederhergestellt, die vor der Skalierung Operation durchgeführt wurde?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Kann ich die Beibehaltung einer zuvor erstellten Cache aktivieren?

Ja, kann Redis Beibehaltung bei der Erstellung des Caches für sowohl auf vorhandenen Premium Caches konfiguriert werden.

### <a name="can-i-change-the-backup-frequency-after-i-create-the-cache"></a>Kann ich die Sicherung Häufigkeit ändern, nachdem ich den Cache erstellen?

Ja, können Sie die Sicherung Häufigkeit auf das Blade **Redis Beibehaltung der Daten** ändern. Anweisungen finden Sie unter [Beibehaltung Redis konfigurieren](#configure-redis-persistence).

### <a name="why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Warum bei ich eine Sicherung Häufigkeit von 60 Minuten es mehr als 60 Minuten zwischen Sicherungskopien beträgt?

Zusätzliche Intervall wird nicht gestartet, bis das vorherige Sicherung erfolgreich abgeschlossen wurde. Wenn die Sicherungsdatei Häufigkeit 60 Minuten ist, und einen Sicherung Prozess 15 Minuten dauert erfolgreich abgeschlossen, nicht zur nächste Sicherung bis 75 Minuten nach dem Start der vorherigen Sicherung gestartet.

### <a name="what-happens-to-the-old-backups-when-a-new-backup-is-made"></a>Was geschieht mit den alten Sicherungskopien, wenn eine neue Sicherung erfolgt?

Alle Sicherungskopien mit Ausnahme des letzten werden automatisch gelöscht. Dieser Löschvorgang möglicherweise nicht sofort erfolgt jedoch ältere Sicherungskopien endlos nicht beibehalten werden.

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Was passiert, wenn ich zu einer anderen Größe skaliert haben und eine Sicherung wiederhergestellt, die vor der Skalierung Operation durchgeführt wurde?

-   Wenn Sie auf einen größeren skaliert haben, gibt es keine Auswirkung.
-   Wenn Sie auf eine kleinere Größe skaliert haben und Sie haben einen benutzerdefinierten [Datenbanken](cache-configure.md#databases) festlegen, ist größer als die [Datenbanken beschränken](cache-configure.md#databases) für Ihre neue Größe, die Daten in diesen Datenbanken ist nicht wiederhergestellt werden. Weitere Informationen finden Sie unter [ist meine benutzerdefinierten Datenbanken festlegen während der Skalierung betroffenen?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
-   Wenn Sie auf eine kleinere Größe skaliert haben und kann in der geringerer Größe zu halten Sie alle Daten aus der letzten Sicherung aus Platzgründen nicht, werden Tasten während des Wiederherstellungsvorgangs, in der Regel mithilfe der Richtlinie [Allkeys-Lru](http://redis.io/topics/lru-cache) Entfernung entfernt werden.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie mehr Premium Cachefeatures verwenden.

-   [Einführung in die Ebene Azure Redis Cache Premium](cache-premium-tier-intro.md)
  
<!-- IMAGES -->

[redis-cache-new-cache-menu]: ./media/cache-how-to-premium-persistence/redis-cache-new-cache-menu.png

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-persistence-selected]: ./media/cache-how-to-premium-persistence/redis-cache-persistence-selected.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
