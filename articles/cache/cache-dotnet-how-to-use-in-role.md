<properties 
    pageTitle="So verwenden Sie In der Rolle Cache (.NET) | Microsoft Azure" 
    description="Erfahren Sie, wie Azure In Rolle Cache verwenden. Die Beispiele in C#-Code geschrieben sind und die .NET-API." 
    services="cache" 
    documentationCenter=".net" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>






# <a name="how-to-use-in-role-cache-for-azure-cache"></a>Verwendung von In-Rolle Cache für Azure Cache

In diesem Handbuch wird gezeigt, wie die ersten Schritte mit **In Rolle Cache für Azure Cache**. Die Beispiele in C geschrieben werden\# code ein, und verwenden Sie die .NET-API. Die Szenarios dieser gehören **Konfigurieren eines Clusters Cache**, **Cache Clients konfigurieren**, **Hinzufügen und Entfernen von Objekten aus dem Cache, Speicherns ASP.NET Sitzungsstatus im Cache**und **ASP.NET Seite Ausgabe Zwischenspeichern Verwenden des Caches aktivieren**. Weitere Informationen zur Verwendung In Rolle Cache finden Sie im Abschnitt [Nächste Schritte][] .

>[AZURE.IMPORTANT]Gemäß letztes Jahr [Ankündigung](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)wird Azure verwaltet Cachediensts und Azure In Rolle Cache Dienst auf 30 November 2016 deaktiviert werden. Wird unsere empfohlen, [Azure Redis Cache](https://azure.microsoft.com/services/cache/)verwenden. Informationen zum Migrieren finden Sie unter [Migrieren von Cachediensts Azure Redis Cache verwaltet](../redis-cache/cache-migrate-to-redis.md).

<a name="what-is"></a>
## <a name="what-is-in-role-cache"></a>Was ist In Rolle Cache?

In der Rolle Cache stellt eine Zwischenspeichern den Azure-Clientanwendungen bereit. Zwischenspeichern höhere Leistung durch vorübergehend Speichern von Informationen im Speicher aus anderen Quellen Back-End- und kann die Kosten für die Datenbanktransaktionen in der Cloud zu verringern. Cache in Rolle umfasst die folgenden Features:

-   Vorgefertigte ASP.NET-Anbieter für die Sitzung Zustand und seiner ursprünglichen Seite Ausgabe zwischenspeichern, aktivieren Beschleunigung von Webanwendungen ohne Anwendungscode ändern zu müssen.
-   Alle serialisierbaren verwalteten Objekts – beispielsweise zwischengespeichert: CLR-Objekte, Zeilen, XML, binäre Daten.
-   Konsistentes Entwicklungsmodell sowohl Azure und Windows Server AppFabric.

In der Rolle Cache bietet eine Möglichkeit zum Zwischenspeichern mithilfe von einen Teil des Arbeitsspeichers, die die Rolleninstanzen in Ihrer Azure-Cloud-Dienste (auch bekannt als gehostete Services) hosten virtuellen Computern ausführen. Größere Flexibilität im Hinblick auf Bereitstellungsoptionen vorhanden sind, die Caches sehr groß sein können und haben keine Cache bestimmten Kontingent Einschränkungen.

>[AZURE.IMPORTANT] Beginnend mit Azure SDK 2.6, stellt Microsoft Azure-Speicher SDK Version 4.3 In Rolle Cache dar. In früheren Versionen von Azure SDK verwendet In Rolle Cache Azure-Speicher SDK 1.7. Applikationen In Rolle Cache mit Versionen von Azure SDK verwenden, bevor 2.6 auf Azure SDK 2.6 vor Azure-Speicher Version zu migrieren, sollte außer 2011-08-18 auf 1 August 2016 Betrieb. Weitere Informationen finden Sie unter [Azure SDK 2.6 Versionsinformationen - Cache In Rolle](../azure-sdk-dotnet-release-notes-2-6.md#in-role-cache-updates) und [Microsoft Azure-Speicher Dienst Version freistellen Update: Erweiterung 2016](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

Klicken Sie auf Rolleninstanzen Zwischenspeichern weist die folgenden Vorteile:

-   Bezahlen Sie keine Premium Zwischenspeichern. Sie Zahlen nur für die Ressourcen berechnen, hosten Cache.
-   Cache Kontingente und begrenzungsebene vermieden werden.
-   Bietet eine bessere Kontrolle und Grad der Isolation an. 
-   Verbesserte Leistung
-   Automatisch Größen zwischengespeichert, wenn Rollen oder verkleinern skaliert werden. Effektiv skaliert den Arbeitsspeicher, der zum Zwischenspeichern von nach oben oder unten beim Hinzufügen oder Entfernen von Rolleninstanzen verfügbar ist.
-   Stellt voller Qualität Entwicklung Time-Debuggen. 
-   Unterstützt das Protokoll den Memcache.

Darüber hinaus bietet das Zwischenspeichern auf Rolleninstanzen dieser konfigurierbaren Optionen aus:

-   Konfigurieren Sie eine dedizierte Funktion zum Zwischenspeichern oder gemeinsame suchen auf vorhandene Rollen Zwischenspeichern. 
-   Stellen Sie Ihren Cache mit mehreren Clients in der gleichen Cloud-Service-Bereitstellung zur Verfügung.
-   Erstellen Sie mehrere benannten Caches mit anderen Eigenschaften aus.
-   Konfigurieren der hohen Verfügbarkeit optional auf einzelne Caches.
-   Verwenden von erweiterten Zwischenspeichern Funktionen wie Regionen, kategorisieren und Benachrichtigungen.

Dieses Handbuch bietet einen Überblick über die erste Schritte mit Cache In Rolle. Ausführlichere Informationen zu diesen Funktionen, die über den Bereich Erste Schritte sind Leitfaden Sie, finden Sie unter [Übersicht der In der Rolle Cache][].

<a name="getting-started-cache-role-instance"></a>
## <a name="getting-started-with-in-role-cache"></a>Erste Schritte mit Cache In Rolle

In der Rolle Cache bietet eine Möglichkeit, aktivieren Sie das Zwischenspeichern mithilfe des Speichers, der auf den virtuellen Computern, die als Host ist, Ihre Rolleninstanzen. Die Rolle Instanzen, hosten, die Ihre Caches als **Cachecluster**bekannt sind. Es gibt zwei Bereitstellungstopologien zum Zwischenspeichern von Rolleninstanzen aus:

-   **Dedizierter Rolle** Zwischenspeichern - die Rolleninstanzen ausschließlich zum Zwischenspeichern verwendet werden.
-   **Taste Rolle** Zwischenspeichern - Cache die virtuellen Computer Ressourcen (Bandbreite, CPU und Arbeitsspeicher) für die Anwendung freigegeben.

Zum Zwischenspeichern von auf Rolleninstanzen müssen Sie einen Cachecluster konfigurieren, und konfigurieren Sie die Clients Cache, damit er den Cachecluster zugreifen können.

-   [Konfigurieren Sie den Cachecluster][]
-   [Konfigurieren der Cache-clients][]

<a name="enable-caching"></a>
## <a name="configure-the-cache-cluster"></a>Konfigurieren Sie den Cachecluster

Um eine **Taste Rolle** Cachecluster konfigurieren, wählen Sie die Rolle aus, in der den Cachecluster gehostet werden soll. Mit der rechten Maustaste in der Rolleneigenschaften im **Explorer Lösung** , und wählen Sie **Eigenschaften**aus.

![RoleCache1][RoleCache1]

Wechseln Sie zur Registerkarte **Zwischenspeichern** , aktivieren Sie das Kontrollkästchen **Zwischenspeichern aktivieren** , und geben Sie die gewünschten Optionen zum Zwischenspeichern. Wenn es in einer **Worker-Rolle** oder **ASP.NET Webrolle**aktiviert ist, ist die standardmäßige Konfiguration **Taste Rolle** Zwischenspeichern mit 30 % des Arbeitsspeichers der Rolleninstanzen für Zwischenspeichern vorgesehen sind. Ein Standardcache automatisch konfiguriert ist, und gegebenenfalls weitere benannten Caches erstellt werden und dieser Caches werden den belegten Speicher gemeinsam nutzen.

![RoleCache2][RoleCache2]

Um eine **Rolle dedizierte** Cachecluster konfigurieren zu können, fügen Sie eine **Worker-Rolle Cache** zu einem Projekt.

![RoleCache7][RoleCache7]

Wenn eine **Worker-Rolle Cache** zu einem Projekt hinzugefügt wurde, ist die standardmäßige Konfiguration **Dedizierter Rolle** Zwischenspeichern.

![RoleCache8][RoleCache8]

Nachdem Zwischenspeichern aktiviert ist, kann das Cache Cluster Speicher-Konto konfiguriert werden. In der Rolle Cache erfordert ein Konto Azure-Speicher. Dieses Speicherkonto wird verwendet, um die von Konfigurationsdaten über den Cachecluster zu halten, die von allen virtuellen Computern zugegriffen wird, die den Cachecluster bilden. Dieses Speicherkonto auf der Registerkarte **Zwischenspeichern** der Cache Cluster Rollenseite, direkt oberhalb der **Einstellungen des Caches für benannte**angegeben.

![RoleCache10][RoleCache10]

>Wenn dieses Speicherkonto konfiguriert ist die Rollen können nicht gestartet. 

Die Größe des Caches wird durch eine Kombination der Größe der Rolle, die Anzahl der Instanzen der Rolle des virtuellen Computer bestimmt und gibt an, ob der Cachecluster als dedizierte Funktion oder am selben Standort Rolle Cachecluster konfiguriert ist.

>Dieser Abschnitt enthält eine Übersicht über die vereinfachte auf die Cachegröße konfigurieren. Weitere Informationen zu Cachegröße und weitere Kapazität Planungsfaktoren sind finden Sie unter [In Rolle Kapazität Cache Planungsaspekte][].

Mit der rechten Maustaste in der Rolleneigenschaften im **Explorer Lösung** , und wählen Sie **Eigenschaften**, um die Größe des virtuellen Computers und die Anzahl der Rolleninstanzen zu konfigurieren.

![RoleCache1][RoleCache1]

Wechseln Sie zur Registerkarte **Konfiguration** . Die **Anzahl der Instanzen** Standardwert ist 1, und die Standardeinstellung **virtueller Speicher** ist **klein**.

![RoleCache3][RoleCache3]

Gesamten Speichers für die Größen virtuellen Computer sieht wie folgt aus: 

-   **Klein**: 1,75 GB
-   **Mittel**: 3,5 GB
-   **Groß**: 7 GB
-   **ExtraLarge**: 14 GB


> Diese Größen Arbeitsspeicher darstellen die Gesamtmenge Arbeitsspeicher verfügbar, um den virtuellen Computer, die über die OS-Cache-Prozess, Zwischenspeichern von Daten und Anwendung freigegeben ist. Weitere Informationen zum Konfigurieren von virtuellen Computern Größen finden Sie unter [So konfigurieren Sie virtuellen Computern Größen][]. Beachten Sie, dass Cache auf **ExtraSmall** virtueller Computer Größen nicht unterstützt wird.

Wenn **Taste Rolle** Zwischenspeichern angegeben ist, wird die Cachegröße bestimmt, durch den angegebenen Prozentsatz des Arbeitsspeichers virtuellen Computern annehmen. Wenn **Dedizierter Rolle** Zwischenspeichern angegeben ist, werden alle verfügbaren Arbeitsspeicher des virtuellen Computers zum Zwischenspeichern verwendet. Wenn zwei Rolleninstanzen konfiguriert sind, wird der kombinierte Speicher der virtuellen Computer verwendet. Dies bildet einen Cachecluster, in dem der verfügbare Zwischenspeichern Arbeitsspeicher verteilt über mehrere Rolleninstanzen jedoch an die Clients des Caches als einzelne Ressource angezeigt. Konfigurieren zusätzliche Rolleninstanzen erhöht die Cachegröße auf die gleiche Weise. Wenn Sie um die Einstellungen für die Bereitstellung von eines Caches der gewünschten Größe zu ermitteln, können Sie Kapazität Planung Kalkulationstabelle die in [In Rolle Kapazität Cache Planungsaspekte][]fallen.

Nachdem der Cachecluster konfiguriert ist, können Sie den Cache Clients auf den Cache zugreifen dürfen konfigurieren.

<a name="NuGet"></a>
## <a name="configure-the-cache-clients"></a>Konfigurieren der Cache-clients

Um einen Cache In-Rolle Cache zuzugreifen, muss die Clients innerhalb der gleichen Bereitstellung. Ist der Cachecluster eine dedizierte Funktion Cachecluster, sind die Clients anderen Rollen in der Bereitstellung. Ist der Cachecluster eine gemeinsame am Rolle Cachecluster, können die Clients entweder die anderen Rollen in der Bereitstellung, oder die Rollen selbst, hosten Cachecluster handeln. Ein NuGet-Paket wird vorausgesetzt, die jeder Clientrolle konfiguriert werden, die den Cache greift auf verwendet werden kann. Zum Konfigurieren einer Rolle Zugriff auf einen Cachecluster mithilfe des Zwischenspeichern NuGet-Pakets mit der rechten Maustaste im Projekts Rolle im **Solution Explorer** , und wählen Sie **NuGet-Pakete verwalten**. 

![RoleCache4][RoleCache4]

Wählen Sie **In der Rolle Cache**aus, klicken Sie auf **Installieren**, und klicken Sie dann auf **ich stimme**.

>Wenn **In Rolle Cache** nicht in der Liste angezeigt wird Geben Sie in das Textfeld **Suchen Online** **WindowsAzure.Caching** , und wählen Sie ihn in den Suchergebnissen aus.

![RoleCache5][RoleCache5]

NuGet-Paket führt mehrere Aufgaben: die Konfigurationsdatei der Rolle die erforderliche Konfiguration hinzugefügt, die Datei ServiceConfiguration.cscfg der Azure-Anwendung eine Cache Client diagnostic Ebene Einstellung hinzugefügt und fügt die erforderlichen Verweise hinzu.

>Für ASP.NET Webrollen fügt das Zwischenspeichern NuGet-Paket auch, dass zwei Abschnitten zu web.config kommentiert. Der erste Abschnitt ermöglicht Sitzungszustand im Cache gespeichert werden soll, und der zweite Abschnitt ermöglicht ASP.NET Seite Ausgabe Zwischenspeichern. Weitere Informationen finden Sie unter [How To: Store ASP.NET Session State im Cache] und [How To: Store ASP.NET Ausgabe Zwischenspeichern von Seiten im Cache][].

NuGet-Paket fügt die folgenden Konfigurationselemente Ihrer Rolle des web.config oder app.config hinzu. Einen Abschnitt **DataCacheClients** und einen Abschnitt **CacheDiagnostics** werden unter dem Element **ConfigSections** hinzugefügt. Ist kein **ConfigSections** Element vorhanden, wird eine als untergeordnetes Element des Elements **Konfiguration** erstellt.

    <configSections>
      <!-- Existing sections omitted for clarity. -->

      <section name="dataCacheClients" 
               type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" 
               allowLocation="true" 
               allowDefinition="Everywhere" />
    
      <section name="cacheDiagnostics" 
               type="Microsoft.ApplicationServer.Caching.AzureCommon.DiagnosticsConfigurationSection, Microsoft.ApplicationServer.Caching.AzureCommon" 
               allowLocation="true" 
               allowDefinition="Everywhere" />
    </configSections>

Diese neue Abschnitte enthalten Verweise auf ein Element **DataCacheClients** und einem **CacheDiagnostics** -Element. Diese Elemente werden auch die **Konfiguration** Element hinzugefügt.

    <dataCacheClients>
      <dataCacheClient name="default">
        <autoDiscover isEnabled="true" identifier="[cache cluster role name]" />
        <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
      </dataCacheClient>
    </dataCacheClients>
    <cacheDiagnostics>
      <crashDump dumpLevel="Off" dumpStorageQuotaInMB="100" />
    </cacheDiagnostics>

Nachdem die Konfiguration hinzugefügt wurde, ersetzen Sie **[Cluster Rollenname zwischengespeichert]** durch den Namen der Rolle, die den Cachecluster hostet.

>**[Name der Cache Cluster Rolle]** nicht mit dem Namen der Rolle ersetzt wird wird, hostet Cachecluster, und klicken Sie dann auf **beiden** ausgelöst, wenn der Cache für eine innere **DatacacheException** mit der Meldung "keine solche Rolle ist nicht vorhanden" zugegriffen werden kann.

NuGet-Paket wird ebenfalls eine **ClientDiagnosticLevel** Einstellung **ConfigurationSettings** der Cache Client-Rolle ServiceConfiguration.cscfg hinzugefügt. Im folgende Beispiel wird im Abschnitt **WebRole1** aus einer Datei ServiceConfiguration.cscfg mit einem **ClientDiagnosticLevel** von 1, welche ist die Standardeinstellung **ClientDiagnosticLevel**.

    <Role name="WebRole1">
      <Instances count="1" />
      <ConfigurationSettings>
        <!-- Existing settings omitted for clarity. -->
        <Setting name="Microsoft.WindowsAzure.Plugins.Caching.ClientDiagnosticLevel" 
                 value="1" />
      </ConfigurationSettings>
    </Role>

>In der Rolle Cache bietet einen neuen Cache-Server und einer Diagnoseprotokollen Cache Client-Ebene. Die Diagnose eine einzelne Einstellung, die die Ebene von Diagnoseinformationen zum Zwischenspeichern von erfassten konfiguriert ist. Weitere Informationen finden Sie unter [Problembehandlung und Diagnose für Cache In Rolle][]

NuGet-Paket fügt außerdem Verweise auf die folgenden Assemblys hinzu:

-   Microsoft.ApplicationServer.Caching.Client.dll
-   Microsoft.ApplicationServer.Caching.Core.dll
-   Microsoft.WindowsFabric.Common.dll
-   Microsoft.WindowsFabric.Data.Common.dll
-   Microsoft.ApplicationServer.Caching.AzureCommon.dll
-   Microsoft.ApplicationServer.Caching.AzureClientHelper.dll

Wenn Ihre Rolle einer ASP.NET Web-Rolle ist, wird auch der folgenden Assemblyverweis hinzugefügt:

-   Microsoft.Web.DistributedCache.dll.

Nachdem Sie Ihr Clientprojekt zum Zwischenspeichern konfiguriert ist, können Sie die in den folgenden Abschnitten für das Arbeiten mit dem Cache beschriebenen Verfahren verwenden.

<a name="working-with-caches"></a>
## <a name="working-with-caches"></a>Arbeiten mit Caches

Die Schritte in diesem Abschnitt beschreiben, wie Sie allgemeine Aufgaben mit Zwischenspeichern ausführen.

-   [Gewusst wie: Erstellen eines Objekts DataCache][]
-   [Gewusst wie: Hinzufügen und Abrufen ein Objekts aus dem Cache][]
-   [Gewusst wie: Geben Sie den Ablauf eines Objekts im Cache][]
-   [Gewusst wie: Speichern ASP.NET Session State im Cache][]
-   [Gewusst wie: Speichern ASP.NET Seite Ausgabe Zwischenspeichern im Cache][]

<a name="create-cache-object"></a>
## <a name="how-to-create-a-datacache-object"></a>Gewusst wie: Erstellen eines Objekts DataCache

Programmgesteuertes mit einem Cache arbeiten möchten, benötigen Sie einen Verweis auf den Cache. Fügen Sie Folgendes an den Anfang aller Dateien aus dem Cache In Rolle verwendet werden soll:

    using Microsoft.ApplicationServer.Caching;

>Wenn Visual Studio die Typen in der mit erkennt Anweisung auch nach der Installation des Zwischenspeichern NuGet-Pakets, die die notwendigen Verweise hinzufügt, stellen Sie sicher, dass das Zielprofil für das Projekt .NET Framework 4.0 oder höher ist, und wählen Sie eines der Profile, die keine **Client-Profil**angegeben ist. Informationen zum Konfigurieren von Clients Cache finden Sie unter [Konfigurieren der Cache Clients][].

Es gibt zwei Methoden zum Erstellen eines Objekts **DataCache** aus. Die erste Möglichkeit ist einfach ein **DataCache**, den Namen des gewünschten Caches erstellen.

    DataCache cache = new DataCache("default");

Sobald die **DataCache** instanziiert wird, können Sie es verwenden Interaktion mit den Cache wie in den folgenden Abschnitten beschrieben.

Wenn die zweite Möglichkeit verwenden möchten, erstellen Sie ein neues **DataCacheFactory** Objekt in der Anwendung, die mit dem Standardkonstruktor aus. Dadurch wird den Cacheclient die Einstellungen in der Konfigurationsdatei verwenden. Rufen Sie entweder die **GetDefaultCache** Abschreibung der neuen **DataCacheFactory** Instanz eines **DataCache** Objekt zurückgegeben werden, oder die **GetCache** , und dem Cache übergeben. Diese Methoden zurück ein **DataCache** -Objekt, das verwendet werden kann, um den Cache programmgesteuert zugreifen.

    // Cache client configured by settings in application configuration file.
    DataCacheFactory cacheFactory = new DataCacheFactory();
    DataCache cache = cacheFactory.GetDefaultCache();
    // Or DataCache cache = cacheFactory.GetCache("MyCache");
    // cache can now be used to add and retrieve items. 

<a name="add-object"></a>
## <a name="how-to-add-and-retrieve-an-object-from-the-cache"></a>Gewusst wie: Hinzufügen und Abrufen ein Objekts aus dem Cache

Um ein Element in den Cache hinzuzufügen, kann die Abschreibung **Hinzufügen** oder die **setzen** verwendet werden. Die **Add** -Methode fügt das angegebene Objekt im Cache, sortiert nach den Wert des Parameters Key an.

    // Add the string "value" to the cache, keyed by "item"
    cache.Add("item", "value");

Wenn ein Objekt mit demselben Schlüssel bereits im Cache ist, wird ein **DataCacheException** mit folgender Nachricht ausgelöst:

> ErrorCode:SubStatus: Es wird zum Erstellen eines Objekts mit einem Schlüssel, der bereits im Cache versucht wird. Zwischenspeichern akzeptiert nur eindeutige Werte für Schlüssel für Objekte.

Zum Abrufen eines Objekts mit einem bestimmten Schlüssel kann die **Get** -Methode verwendet werden. Wenn das Objekt vorhanden ist, wird es zurückgegeben, und Null wird zurückgegeben, wenn dies nicht der Fall.

    // Add the string "value" to the cache, keyed by "key"
    object result = cache.Get("Item");
    if (result == null)
    {
        // "Item" not in cache. Obtain it from specified data source
        // and add it.
        string value = GetItemValue(...);
        cache.Add("item", value);
    }
    else
    {
        // "Item" is in cache, cast result to correct type.
    }

Die Methode **setzen** fügt das Objekt mit dem angegebenen Schlüssel zum Cache, wenn er nicht vorhanden, oder das Objekt ersetzt, wenn er vorhanden ist.

    // Add the string "value" to the cache, keyed by "item". If it exists,
    // replace it.
    cache.Put("item", "value");

<a name="specify-expiration"></a>
## <a name="how-to-specify-the-expiration-of-an-object-in-the-cache"></a>Gewusst wie: Geben Sie den Ablauf eines Objekts im Cache

Standardmäßig ablaufen Elemente im Cache 10 Minuten, nachdem sie im Cache platziert werden. Dies kann der Einstellung **Time to Live (min)** in den Rolleneigenschaften der Rolle konfiguriert werden, die den Cachecluster hostet.

![RoleCache6][RoleCache6]

Es gibt drei Typen von **Ablauf Typ**: **keine**, **absoluten**und **Gleitende Fenster**. Diese konfigurieren, wie **Time to Live (min)** verwendet wird, und den Ablauf zu ermitteln. Die Standardeinstellung ist **Typ Ablauf** **absoluten**, was bedeutet, dass der Countdown für ein Element den Ablauf beginnt, wenn das Element in den Cache platziert wird. Nachdem die angegebene Zeitspanne für ein Element abgelaufen ist, läuft das Element ab. Wenn **Gleitende Fenster** angegeben ist, wird der Ablauf Countdown für ein Element jedes Mal, wenn das Element im Cache zugegriffen wird zurückgesetzt, und das Element wird nicht ablaufen, bis seit der letzten den Zugriff auf die angegebene Zeitspanne abgelaufen ist. Wenn **keines** angegeben ist, klicken Sie dann auf **0** **Time to Live (min)** festgelegt werden müssen, und Elemente nicht ablaufen, und bleiben gültig, solange sie im Cache sind.

Gegebenenfalls ein Timeoutintervall verlängern oder verkürzen als was in den Rolleneigenschaften konfiguriert ist kann eine bestimmte Dauer angegeben werden, wenn ein Element hinzugefügt oder im Cache mithilfe der Überladung der **Hinzufügen** und **setzen** , die einen Parameter **TimeSpan** ausführen aktualisiert wird. Im folgenden Beispiel wird der **Wert** Cache, sortiert nach **Element**mit einem Timeout von 30 Minuten hinzugefügt.

    // Add the string "value" to the cache, keyed by "item"
    cache.Add("item", "value", TimeSpan.FromMinutes(30));

Zum Anzeigen des verbleibenden Timeout Intervalls eines Elements im Cache kann die **GetCacheItem** -Methode verwendet werden, um ein Objekt **DataCacheItem** abzurufen, die Informationen über das Element im Cache, einschließlich des verbleibenden Timeoutintervalls enthält.

    // Get a DataCacheItem object that contains information about
    // "item" in the cache. If there is no object keyed by "item" null
    // is returned. 
    DataCacheItem item = cache.GetCacheItem("item");
    TimeSpan timeRemaining = item.Timeout;

<a name="store-session"></a>
## <a name="how-to-store-aspnet-session-state-in-the-cache"></a>Gewusst wie: Speichern ASP.NET Session State im Cache

Die Session State Provider für In Rolle Cache sind Out-of-Process-Speicher für ASP.NET Applications. Diese Anbieter können Sie Ihre Sitzungszustand in einem Azure Cache anstatt im Speicher oder in einer SQL Server-Datenbank gespeichert. Um den Zwischenspeichern Sitzung Zustand Anbieter verwenden, konfigurieren Sie zuerst den Cachecluster, und konfigurieren Sie die Anwendung ASP.NET zum Zwischenspeichern von mit dem Zwischenspeichern NuGet Package in [Erste Schritte mit In Rolle Cache][]beschriebenen. Wenn das Zwischenspeichern NuGet-Paket installiert ist, fügt einen Abschnitt auskommentierte in web.config, die die erforderliche Konfiguration für eine Anwendung ASP.NET In Rolle Cache mit der Session State Provider enthält hinzu.

    <!--Uncomment this section to use In-Role Cache for session state caching
    <system.web>
      <sessionState mode="Custom" customProvider="AFCacheSessionStateProvider">
        <providers>
          <add name="AFCacheSessionStateProvider" 
            type="Microsoft.Web.DistributedCache.DistributedCacheSessionStateStoreProvider,
                  Microsoft.Web.DistributedCache" 
            cacheName="default" 
            dataCacheClientName="default"/>
        </providers>
      </sessionState>
    </system.web>-->

>Wenn Ihre web.config keinen ist dies kommentiert, Abschnitt nach der Installation von der Zwischenspeichern NuGet-Paket, stellen Sie sicher, dass die neuesten NuGet-Paket-Manager aus [NuGet-Paket-Manager-Installation][]installiert ist, und klicken Sie dann deinstallieren und das Paket.

Kommentieren Sie zum Aktivieren der Session State Provider für Cache In Rolle im angegebenen Abschnitt ein. Der Standard-Cache wird in den bereitgestellten Codeausschnitt angegeben. Wenn Sie einen anderen Cache verwenden möchten, geben Sie den gewünschten Cache in das Attribut **CacheName** aus.

Weitere Informationen zur Verwendung von Zwischenspeichern Sitzung Zustand Dienstanbieter finden Sie unter [Session State Provider für Cache In Rolle][].

<a name="store-page"></a>
## <a name="how-to-store-aspnet-page-output-caching-in-the-cache"></a>Gewusst wie: Speichern ASP.NET Seite Ausgabe Zwischenspeichern im Cache

Die Ausgabe-Cache-Anbieter für In Rolle Cache sind Out-of-Process-Speicher für den Cache Ausgabedaten. Diese Daten ist speziell für vollständige HTTP-Antworten (Seite Zwischenspeichern der Ausgabe). Der Anbieter schließt an die neue Ausgabe Cache Anbieter Erweiterbarkeitspunkt, der in ASP.NET 4 eingeführt wurde. Den Ausgabe Cacheanbieter verwenden, konfigurieren Sie zuerst den Cachecluster, und klicken Sie dann Konfigurieren Ihrer Anwendung ASP.NET für das Paket NuGet Zwischenspeichern verwenden, wie unter [Erste Schritte mit In Rolle Cache][]Zwischenspeichern. Wenn das Zwischenspeichern NuGet-Paket installiert ist, fügt folgende auskommentierte Abschnitt in web.config, die die erforderliche Konfiguration für eine Anwendung ASP.NET mit den Ausgabe-Cache-Anbieter für In Rolle Cache enthält hinzu.

    <!--Uncomment this section to use In-Role Cache for output caching
    <caching>
      <outputCache defaultProvider="AFCacheOutputCacheProvider">
        <providers>
          <add name="AFCacheOutputCacheProvider" 
            type="Microsoft.Web.DistributedCache.DistributedCacheOutputCacheProvider,
                  Microsoft.Web.DistributedCache" 
            cacheName="default" 
            dataCacheClientName="default" />
        </providers>
      </outputCache>
    </caching>-->

>Wenn Ihre web.config keinen ist dies kommentiert, Abschnitt nach der Installation von der Zwischenspeichern NuGet-Paket, stellen Sie sicher, dass die neuesten NuGet-Paket-Manager aus [NuGet-Paket-Manager-Installation][]installiert ist, und klicken Sie dann deinstallieren und das Paket.

Kommentieren Sie zum Aktivieren der Ausgabe-Cache-Anbieter für Cache In Rolle im angegebenen Abschnitt ein. Der Standard-Cache wird in den bereitgestellten Codeausschnitt angegeben. Wenn Sie einen anderen Cache verwenden möchten, geben Sie den gewünschten Cache in das Attribut **CacheName** aus.

Hinzufügen einer Richtlinie **OutputCache** auf jeder Seite die Ausgabe zwischengespeichert werden soll.

    <%@ OutputCache Duration="60" VaryByParam="*" %>

Die zwischengespeicherte Seitendaten bleibt in diesem Beispiel wird im Cache für 60 Sekunden, und eine andere Version von der Seite für jede Parameterkombination zwischengespeichert werden soll. Weitere Informationen zu den verfügbaren Optionen finden Sie unter [OutputCache Richtlinie][].

Weitere Informationen zur Verwendung der Ausgabe-Cache-Anbieter für In Rolle Cache, finden Sie unter [Ausgabe Cache Datenanbieter für Cache In Rolle][].

<a name="next-steps"></a>
## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Caches In Rolle bearbeitet haben, führen Sie die folgenden Links, um zu erfahren, wie komplexere Zwischenspeichern Aufgaben ausführen.

-   Finden Sie in der MSDN-Referenz: [In Rolle Cache][]
-   Informationen zum Migrieren zu In Rolle Cache: [migrieren, um In Rolle][]
-   Schauen Sie sich die Beispiele: [In Rolle Cache Beispiele][]
-   Anzeigen der [maximalen Leistung: beschleunigen Ihrer Cloud Services-Clientanwendungen mit Azure Zwischenspeichern][] Sitzung von TechEd 2013 In Rolle Cache

<!-- INTRA-TOPIC LINKS -->
[Nächste Schritte]: #next-steps
[What is In-Role Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Getting Started with the In-Role Cache Service]: #getting-started-cache-service
[Prepare Your Visual Studio Project to Use In-Role Cache]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Erste Schritte mit Cache In Rolle]: #getting-started-cache-role-instance
[Konfigurieren Sie den Cachecluster]: #enable-caching
[Configure the desired cache size]: #cache-size
[Konfigurieren der Cache-clients]: #NuGet
[Working with Caches]: #working-with-caches
[Gewusst wie: Erstellen eines Objekts DataCache]: #create-cache-object
[Gewusst wie: Hinzufügen und Abrufen ein Objekts aus dem Cache]: #add-object
[Gewusst wie: Geben Sie den Ablauf eines Objekts im Cache]: #specify-expiration
[Gewusst wie: Speichern ASP.NET Session State im Cache]: #store-session
[Gewusst wie: Speichern ASP.NET Seite Ausgabe Zwischenspeichern im Cache]: #store-page
[Target a Supported .NET Framework Profile]: #prepare-vs-target-net
 
<!-- IMAGES --> 
[RoleCache1]: ./media/cache-dotnet-how-to-use-in-role/cache8.png
[RoleCache2]: ./media/cache-dotnet-how-to-use-in-role/cache9.png
[RoleCache3]: ./media/cache-dotnet-how-to-use-in-role/cache10.png
[RoleCache4]: ./media/cache-dotnet-how-to-use-in-role/cache11.png
[RoleCache5]: ./media/cache-dotnet-how-to-use-in-role/cache12.png
[RoleCache6]: ./media/cache-dotnet-how-to-use-in-role/cache13.png
[RoleCache7]: ./media/cache-dotnet-how-to-use-in-role/cache14.png
[RoleCache8]: ./media/cache-dotnet-how-to-use-in-role/cache15.png
[RoleCache10]: ./media/cache-dotnet-how-to-use-in-role/cache17.png
  
<!-- LINKS -->
[Konfigurieren von virtuellen Computern Größen]: http://go.microsoft.com/fwlink/?LinkId=164387
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[In der Rolle Cache Kapazität Planungsaspekte]: http://go.microsoft.com/fwlink/?LinkId=252651
[Beispiele für in Rolle Cache]: http://msdn.microsoft.com/library/jj189876.aspx
[In der Rolle Cache]: http://go.microsoft.com/fwlink/?LinkId=252658
[In der Rolle Cache]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[Maximale Performance: Ihre Cloud Services-Clientanwendungen mit Azure Zwischenspeichern zu erhöhen.]: http://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/WAD-B326#fbid=kmrzkRxQ6gU
[Migrieren Sie In Rolle Cache]: http://msdn.microsoft.com/library/hh914163.aspx
[NuGet-Paket-Manager-Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Die Ausgabe Cache Datenanbieter für Cache In Rolle]: http://msdn.microsoft.com/library/windowsazure/gg185662.aspx
[OutputCache Richtlinie]: http://go.microsoft.com/fwlink/?LinkId=251979
[Übersicht über In Rolle Cache]: http://go.microsoft.com/fwlink/?LinkId=254172
[Session State-Anbieter für Cache In Rolle]: http://msdn.microsoft.com/library/windowsazure/gg185668.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Problembehandlung und Diagnose für Cache In Rolle]: http://msdn.microsoft.com/library/windowsazure/hh914135.aspx
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx

[Which Azure Cache offering is right for me?]: cache-faq.md#which-azure-cache-offering-is-right-for-me
 
