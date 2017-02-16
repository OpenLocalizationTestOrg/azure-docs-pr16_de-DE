<properties 
    pageTitle="Importieren und Exportieren von Daten in Azure Redis Cache | Microsoft Azure" 
    description="Informationen Sie zum Importieren und Exportieren von Daten an und von BLOB-Speicher mit Ihrem Premium Azure Redis Cache Instanzen" 
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

# <a name="import-and-export-data-in-azure-redis-cache"></a>Importieren und Exportieren von Daten in Azure Redis Cache

Import/Export ist ein Azure Redis Cache Daten Verwaltungsvorgang, wodurch Sie Daten in Azure Redis Cache importieren oder Exportieren von Daten aus Azure Redis Cache durch Importieren und Exportieren von Momentaufnahme Redis-Cache-Datenbank (RDB) aus einem Cache Premium auf einer Seitenblob in einem Azure-Speicher-Konto an. Import/Export-können Sie zwischen verschiedenen Azure Redis Cache Instanzen migrieren oder den Cache mit Daten vor der Verwendung zu füllen.

In diesem Artikel bietet einen Leitfaden für das Importieren und Exportieren von Daten mit Azure Redis Cache und stellt die Antworten auf häufig gestellte Fragen.

>[AZURE.IMPORTANT] Import/Export befindet sich in der Vorschau und steht nur für [Premium Ebene](cache-premium-tier-intro.md) Caches.

## <a name="import"></a>Importieren

Importieren kann Redis kompatibel RDB Datei(en) aus einem beliebigen Redis Server ausgeführt in einem beliebigen Cloud oder -Umgebung, einschließlich Redis unter Linux, Windows oder eine beliebige Cloudanbieter, z. B. Amazon-Webdiensten und andere einbinden verwendet werden. Importieren von Daten ist eine einfache Möglichkeit, einen Cache mit vorab eingetragenen Daten zu erstellen. Während des Importvorgangs Azure Redis Cache lädt die Dateien RDB aus Azure-Speicher in den Speicher und anschließend die Tasten in den Cache eingefügt.

>[AZURE.NOTE] Bevor Sie beginnen den Importvorgang, stellen Sie sicher, dass Ihre Redis Datenbank (RDB) oder mehrere Dateien in Seitenblobs in Azure-Speicher, in die gleiche Region und das Abonnement wie Ihre Instanz Azure Redis Cache hochgeladen werden. Weitere Informationen finden Sie unter [Erste Schritte mit Azure Blob-Speicher](../storage/storage-dotnet-how-to-use-blobs.md). Wenn Sie die mit der Funktion [Azure Redis Cache exportieren](#export) RDB-Datei exportiert, die RDB-Datei bereits in einer Seitenblob gespeichert ist und für den Import bereit ist.

1. Importieren eine oder mehrere exportierte Cache Blobs, [Durchsuchen, um Ihren Cache](cache-configure.md#configure-redis-cache-settings) Azure-Portal, und klicken Sie auf **Importieren von Daten** aus dem Blade **Einstellungen** Ihrer Instanz Cache.

    ![Importieren von Daten][cache-import-data]

2. Klicken Sie auf **Blob(s) auswählen** , und wählen Sie das Speicherkonto, das die zu importierenden Daten enthält.

    ![Wählen Sie Speicherkonto][cache-import-choose-storage-account]

3. Klicken Sie auf den Container, der die zu importierenden Daten enthält.

    ![Wählen Sie container][cache-import-choose-container]

4. Wählen Sie eine oder mehrere Blobs zu importieren, indem Sie auf den Bereich links neben dem Namen Blob aus, und klicken Sie dann auf **auswählen**.

    ![Wählen Sie blobs][cache-import-choose-blobs]

5. Klicken Sie auf **Importieren** , um den Importvorgang zu starten.

    >[AZURE.IMPORTANT] Der Cache ist während des Importvorgangs nicht Cache Clients zugreifen können, und alle vorhandenen Daten im Cache werden gelöscht.

    ![Importieren][cache-import-blobs]

    Sie können den Fortschritt des Importvorgangs überwachen, mit der Benachrichtigungen aus dem Azure-Portal oder mithilfe der Ereignisse in der [Audit melden Sie sich](cache-configure.md#support-amp-troubleshooting-settings)anzeigen.

    ![Importieren des Vorgangsfortschritts][cache-import-data-import-complete] 


## <a name="export"></a>Exportieren

Exportieren können Sie die Daten aus Azure Redis Cache zu kompatibel RDB Datei(en) Redis exportieren. Sie können dieses Feature zum Verschieben von Daten aus einer Instanz von Azure Redis Cache in ein anderes oder einen anderen Redis Server verwenden. Während des Exportvorgangs wird eine temporäre Datei des virtuellen Computers, der die Redis Azure-Cache-Server-Instanz hostet erstellt, und die Datei mit dem Speicherkonto vorgesehenen hochgeladen wird. Nach Abschluss des Exportvorgangs mit entweder Status Erfolg oder Fehler, wird die temporäre Datei gelöscht.

1. Exportieren des aktuellen Inhalts Cache zu Speicher, [Navigieren Sie zu dem Cache](cache-configure.md#configure-redis-cache-settings) Azure-Portal, und klicken Sie auf **Exportieren von Daten** aus dem Blade **Einstellungen** Ihrer Instanz Cache.

    ![Wählen Sie Speichercontainer][cache-export-data-choose-storage-container]

2. Klicken Sie auf **Speichercontainer auswählen** , und wählen Sie das gewünschte Speicherkonto. Das Speicherkonto muss in der gleichen Abonnement und Region als dem Cache.

    >[AZURE.IMPORTANT] Import/Export funktioniert mit Seitenblobs, die von sowohl Classic-Cloud-Speicherkonten unterstützt werden, aber von [BLOB-Speicherkonten](../storage/storage-blob-storage-tiers.md#blob-storage-accounts) zu diesem Zeitpunkt nicht unterstützt.

    ![Speicher-Konto][cache-export-data-choose-account]

3. Wählen Sie den gewünschten Blob-Container aus, und klicken Sie auf **auswählen**. Um einen Container neu zu verwenden, klicken Sie auf **Container hinzufügen** , um es zunächst hinzufügen, und wählen Sie ihn aus der Liste aus.

    ![Wählen Sie Speichercontainer][cache-export-data-container]

4. Geben Sie ein **Präfix Blob-Name** ein, und klicken Sie auf **Exportieren** , um den Exportvorgang zu starten. Das Präfix Blob wird verwendet, um die Präfix der Names der Dateien, die von diesen Exportvorgang generiert.

    ![Exportieren][cache-export-data]

    Sie können den Fortschritt des Exportvorgangs überwachen, mit der Benachrichtigungen aus dem Azure-Portal oder mithilfe der Ereignisse in der [Audit melden Sie sich](cache-configure.md#support-amp-troubleshooting-settings)anzeigen.

    ![][cache-export-data-export-complete]

    Caches bleiben während des Exportvorgangs zur Verwendung verfügbar.


## <a name="importexport-faq"></a>Import/Export-häufig gestellte Fragen

Dieser Abschnitt enthält häufig gestellte Fragen zu den Import/Export-Funktion.

-   [Welche Preise leisten kann Import/Export verwenden?](#what-pricing-tiers-can-use-importexport)
-   [Können Daten aus einem beliebigen Redis Server werden importiert?](#can-i-import-data-from-any-redis-server)
-   [Werden meine Cache während eines Vorgangs importieren/exportieren zur Verfügung?](#will-my-cache-be-available-during-an-importexport-operation)
-   [Kann ich mit Redis Cluster importieren/exportieren verwenden?](#can-i-use-importexport-with-redis-cluster)
-   [Wie funktioniert die Import/Export-mit einem benutzerdefinierten Datenbanken festlegen?](#how-does-importexport-work-with-a-custom-databases-setting)
-   [Wie unterscheidet sich importieren/exportieren aus dem permanenten Redis?](#how-is-importexport-different-from-redis-persistence)
-   [Kann ich mithilfe der PowerShell, CLI oder anderen Management-Clients importieren/exportieren automatisieren?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
-   [Ich erhalte einen Timeoutfehler während meiner Import/Export-Vorgang. Was bedeutet es?](#i-received-a-timeout-error-during-my-importexport-operation.-what-does-it-mean)
-   [Ich habe einen Fehler beim Exportieren von Daten in Azure BLOB-Speicher. Was ist passiert?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage.-what-happened)


### <a name="what-pricing-tiers-can-use-importexport"></a>Welche Preise leisten kann Import/Export verwenden?

Import/Export ist nur in der Premium Preise Ebene verfügbar.

### <a name="can-i-import-data-from-any-redis-server"></a>Können Daten aus einem beliebigen Redis Server werden importiert?

Ja, können Sie zusätzlich zum Importieren von Daten aus Azure Redis Cache Instanzen exportiert, importieren RDB Dateien von jedem Redis Server ausgeführt in einem beliebigen Cloud oder -Umgebung, wie z. B. Linux, Windows oder cloud-Anbieter, wie beispielsweise Amazon-Webdiensten. Hierzu Hochladen der Datei RDB aus den gewünschten Redis Server in einer Seitenblob in einem Azure-Speicher-Konto, und importieren sie dann in der Premium Redis Azure-Cache-Instanz aus. Angenommen, möchten Sie exportieren Sie die Daten aus dem Cache für die Herstellung und importieren Sie es in den Cache als Teil einer staging-Umgebung zum Testen oder Migration verwendet. 

### <a name="will-my-cache-be-available-during-an-importexport-operation"></a>Werden meine Cache während eines Vorgangs importieren/exportieren zur Verfügung?

-   **Exportieren** - Caches noch verfügbar sind, und Sie können weiterhin Ihren Cache während eines Exportvorgangs verwenden.
-   **Importieren von** -Caches beim Starten eines Importvorgangs nicht verfügbar und zur Verwendung verfügbar machen, nach Abschluss des Importvorgangs verhindert.


### <a name="can-i-use-importexport-with-redis-cluster"></a>Kann ich mit Redis Cluster importieren/exportieren verwenden?

Ja, und Sie können Import/Export zwischen einem gruppierten und einer nicht gruppierten Cache. Seit Redis Cluster [unterstützt nur Datenbank 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), alle Daten in Datenbanken als 0 wird nicht importiert werden. Beim Importieren von Cachedaten mit gruppierten, werden die Tasten zwischen den mehrere Shards hinweg im Cluster verteilt. 

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Wie funktioniert die Import/Export-mit einem benutzerdefinierten Datenbanken festlegen?

Einige Preisgestaltung Ebenen haben andere [Datenbanken Grenzwerte](cache-configure.md#databases), damit es einige Überlegungen beim Importieren gibt, wenn Sie einen benutzerdefinierten Wert für konfiguriert die `databases` während der Erstellung des Caches festlegen.

-   Beim Importieren von in einer Preisgestaltung Ebene mit einer untere `databases` Beschränkung als die Ebene, aus dem Sie exportiert:
    -   Wenn Sie mit die Standardanzahl von arbeiten `databases` welche 16 für alle Preise Stufen ist, werden keine Daten verloren.
    -   Wenn Sie eine benutzerdefinierte Anzahl von arbeiten `databases` dieser fällt in die Grenzwerte für die Leiste, dem Sie importieren, keine Daten verloren.
    -   Wenn die exportierten Daten Daten in einer Datenbank, die die Grenzen der neuen Stufe überschreitet enthalten, werden die Daten aus diesen höheren Datenbanken nicht importiert.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Wie unterscheidet sich importieren/exportieren aus dem permanenten Redis?

Azure Redis Cache Beibehaltung können Sie in Redis zu Azure-Speicher gespeicherte Daten beibehalten werden. Wenn Beibehaltung konfiguriert ist, behält Azure Redis Cache eine Momentaufnahme des Caches Redis in einem Redis Binärformat auf einem Datenträger, auf der Basis einer konfigurierbaren Sicherung Häufigkeit aus. Wenn ein schwerwiegendes Ereignis, die die primären und die Replikatgruppenbezeichner Cache deaktiviert eintritt, werden die Cachedaten automatisch mit den letzten Snapshot wiederhergestellt. Weitere Informationen finden Sie unter [Beibehaltung der Daten für einen Premium Azure Redis Cache konfigurieren](cache-how-to-premium-persistence.md).

Importieren / Exportieren können Sie Daten in oder Exportieren von Azure Redis Cache. Es nicht konfigurieren sichern und Wiederherstellen mit Redis Beibehaltung.


### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Kann ich mithilfe der PowerShell, CLI oder anderen Management-Clients importieren/exportieren automatisieren?

Ja, finden Sie unter PowerShell Anweisungen [einen Redis Cache importieren](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) und [Exportieren von einen Redis Cache](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).



### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Ich erhalte einen Timeoutfehler während meiner Import/Export-Vorgang. Was bedeutet es?

Wenn Sie auf das Blade **Daten importieren** oder **Exportieren von Daten** für mehr als 15 Minuten, bevor Sie den Vorgang initiieren verbleiben, erhalten Sie ähnlich wie der folgende Fehlermeldung.

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Zum Beheben dieses, initiieren importieren oder Exportvorgang vor 15 Minuten Ablauf.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Ich habe einen Fehler beim Exportieren von Daten in Azure BLOB-Speicher. Was ist passiert?

Import/Export-funktioniert nur mit RDB-Dateien als Seitenblobs gespeichert. Andere Blob werden zu diesem Zeitpunkt, einschließlich BLOB-Speicher-Konten mit aktiven und aussagekräftige Ebenen nicht unterstützt.


## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie mehr Premium Cachefeatures verwenden.

-   [Einführung in die Ebene Azure Redis Cache Premium](cache-premium-tier-intro.md)    

  
<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png








