<properties 
    pageTitle="So verwenden Sie Azure verwaltete Cachediensts" 
    description="Erfahren Sie, wie steigern die Leistung Azure Anwendungen mit Azure verwaltete Cachediensts" 
    services="cache" 
    documentationCenter="" 
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

# <a name="how-to-use-azure-managed-cache-service"></a>So verwenden Sie Azure verwaltete Cachediensts

In diesem Handbuch wird gezeigt, wie die ersten Schritte mit **Azure verwaltete Cachediensts**. Die Beispiele in C geschrieben werden\# code ein, und verwenden Sie die .NET-API. Die Szenarios dieser gehören **Erstellen und Konfigurieren eines Caches**, **Cache Clients konfigurieren**, **Hinzufügen und Entfernen von Objekten aus dem Cache, Speicherns ASP.NET Sitzungsstatus im Cache**und **ASP.NET Seite Ausgabe Zwischenspeichern Verwenden des Caches aktivieren**. Weitere Informationen zur Verwendung von Azure Cache finden Sie im Abschnitt [Nächste Schritte][] .

>[AZURE.IMPORTANT]Gemäß letztes Jahr [Ankündigung](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)wird Azure verwaltet Cachediensts und Azure In Rolle Cache Dienst auf 30 November 2016 deaktiviert werden. Wird unsere empfohlen, [Azure Redis Cache](https://azure.microsoft.com/services/cache/)verwenden. Informationen zum Migrieren finden Sie unter [Migrieren von Cachediensts Azure Redis Cache verwaltet](../redis-cache/cache-migrate-to-redis.md).

<a name="what-is"></a>
## <a name="what-is-azure-managed-cache-service"></a>Was ist der Azure-Cachediensts verwaltet?

Azure verwaltete Cachediensts ist eine verteilte, skalierbare in-Memory-Lösung, die Ihnen ermöglicht, hochgradig skalierbar und reagiert Applications erstellen können, indem Sie extrem schnellen Zugriff auf Daten.

Azure verwaltete Cachediensts umfasst die folgenden Features:

-   Vorgefertigte ASP.NET-Anbieter für die Sitzung Zustand und seiner ursprünglichen Seite Ausgabe zwischenspeichern, aktivieren Beschleunigung von Webanwendungen ohne Anwendungscode ändern zu müssen.
-   Alle serialisierbaren verwalteten Objekts – beispielsweise zwischengespeichert: CLR-Objekte, Zeilen, XML, binäre Daten.
-   Konsistentes Entwicklungsmodell sowohl Azure und Windows Server AppFabric.

Verwaltete Cachediensts erhalten Sie zentralen Zugriff auf einen sicheren, dedizierten Cache, der von Microsoft verwaltet wird. Ein Cache erstellt mithilfe des Diensts für verwaltete Cache zugegriffen von Applications in Azure Azure Websites, Web & Arbeitskollegen Rollen und virtuellen Computern ausgeführt werden.

Verwaltete Cachediensts steht in drei Versionen zur Verfügung:

-   Basic - Cache in Größen von 128MB auf 1GB
-   Standard - Cache in Größen von 1GB und 10GB
-   Premium - Cache in Größen von 5GB auf 150GB

Pro Ebene unterscheidet sich in Bezug auf die Features und Preise. Die Features werden weiter unten in diesem Handbuch, sowie weitere Informationen zu Preisen behandelt, finden Sie unter [Cache Preise Details][].

Dieses Handbuch bietet einen Überblick über die erste Schritte mit Cachediensts verwaltet. Ausführlichere Informationen zu diesen Funktionen, die über den Bereich Erste Schritte sind Leitfaden Sie, finden Sie unter [Übersicht der Azure verwaltete Cachediensts][].

<a name="getting-started-cache-service"></a>
## <a name="getting-started-with-cache-service"></a>Erste Schritte mit Cachediensts

Erste Schritte mit verwalteten Cachediensts ist einfach. Um anzufangen, bereitgestellt und einen Cache konfigurieren. Als Nächstes konfigurieren Sie die Cache-Clients, damit er auf den Cache zugreifen können. Sobald die Cache-Clients konfiguriert sind, können Sie mit ihnen arbeiten beginnen.

-   [Erstellen Sie den cache][]
-   [Konfigurieren des Caches][]
-   [Konfigurieren der Cache-clients][]

<a name="create-cache"></a>
## <a name="create-a-cache"></a>Erstellen Sie einen cache

Cacheinstanzen in Cachediensts verwaltet werden mithilfe der PowerShell-Cmdlets erstellt. 

>Nachdem eine Instanz verwaltete Cachediensts mithilfe der PowerShell-Cmdlets können angezeigt und in der [Klassischen Azure-Portal][]konfiguriert erstellt wurde.

Öffnen Sie zum Erstellen einer verwalteten Cachediensts Instanz ein Azure PowerShell-Befehl-Fenster an.

>Anweisungen zum Installieren und Azure PowerShell verwenden finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell][].

Rufen Sie das Cmdlet [AzureAccount hinzufügen][] , und geben Sie die e-Mail-Adresse und Ihr Kennwort ein, der mit Ihrem Konto verbunden ist. Ein Abonnement wird standardmäßig ausgewählt und wird angezeigt, nachdem Sie das Cmdlet [Hinzufügen-AzureAccount][] aufrufen. Wenn Sie das Abonnement ändern möchten, rufen Sie das Cmdlet [AzureSubscription auswählen][] .

>Wenn Sie mit einem Zertifikat Azure PowerShell für Ihr Konto konfiguriert haben, können Sie diesen Schritt überspringen. Weitere Informationen zum Herstellen einer Verbindung Azure PowerShell mit Ihrem Azure-Konto finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell][].

Ein Abonnement wird standardmäßig ausgewählt und wird angezeigt. Wenn Sie das Abonnement ändern möchten, rufen Sie das Cmdlet [AzureSubscription auswählen][] .

Rufen Sie das Cmdlet " [New-AzureManagedCache][] ", und geben Sie den Namen, Region, Cache Angebot und Größe für den Cache.

Geben Sie unter **Name**einen Subdomainnamen für den Cache Endpunkt verwendet werden soll. Der Endpunkt muss eine Zeichenfolge zwischen sechs und zwanzig Zeichen enthalten, nur Kleinbuchstaben Zahlen und Buchstaben und muss mit einem Buchstaben beginnen.

Geben Sie einen Bereich für den Cache für den **Speicherort**an. Erstellen Sie für die Optimierung der Systemleistung den Cache in der gleichen Region wie die Clientanwendung Cache ein.

**SKU** und **Speicherkapazität** arbeiten zusammen, um die Größe des Caches zu bestimmen. Verwaltete Cachediensts steht in den folgenden drei Ebenen.

-   Grundlegende - Cache in Größen von 128MB auf 1GB in 128MB erhöht, mit einem Standardnamen cache
-   Standard - Cache in Größen von 1GB und 10GB in Schritten von 1GB mit Unterstützung für Benachrichtigungen und bis zu zehn benannten caches
-   Premium - Cache in Größen von 5GB auf 150GB in Schritten von 5GB mit Unterstützung für Benachrichtigungen, hohen Verfügbarkeit und bis zu zehn benannten caches

Wählen Sie **Sku** und **Arbeitsspeicher** , die die Anforderungen Ihrer Anwendung entspricht. Beachten Sie, dass einige Cachefeatures wie Benachrichtigungen und hohen Verfügbarkeit nur mit bestimmten Cache Angebote verfügbar sind. Weitere Informationen zur Auswahl der Cache Angebot und Größe, die für eine Anwendung am besten geeignet ist, finden Sie unter [Cache Angebote][].

 Im folgenden Beispiel wird ein einfache 128 MB Cache mit Namen Contosocache, in der geografischen Süd zentralen uns Region erstellt.

    New-AzureManagedCache -Name contosocache -Location "South Central US" -Sku Basic -Memory 128MB

>Eine vollständige Liste der Parameter und Werte, die zur Erstellung ein Caches verwendet werden können, finden Sie unter der [Neu AzureManagedCache][] Cmdlet-Dokumentation.

Nachdem das PowerShell-Cmdlet aufgerufen wird, kann für den Cache erstellt werden ein paar Minuten dauern. Nachdem der Cache erstellt wurde, der neue Cache weist eine `Running` Status und ist einsatzbereit mit Standardeinstellungen, und werden angezeigt und kann in der [Klassischen Azure-Portal][]konfiguriert. Zum Anpassen der Konfigurations von Ihren Cache finden Sie unter den folgenden Abschnitt [Konfigurieren des Caches][] .

Sie können den Status der Erstellung der Azure-PowerShell-Fenster überwachen. Nachdem der Cache zur Verfügung steht, wird das Cmdlet " [New-AzureManagedCache][] " der Daten zwischenspeichern angezeigt, wie im folgenden Beispiel dargestellt.

    PS C:\> Add-AzureAccount
    VERBOSE: Account "user@domain.com" has been added.
    VERBOSE: Subscription "MySubscription" is selected as the default subscription.
    VERBOSE: To view all the subscriptions, please use Get-AzureSubscription.
    VERBOSE: To switch to a different subscription, please use Select-AzureSubscription.
    PS C:\> New-AzureManagedCache -Name contosocache -Location "South Central US" -Sku Basic -Memory 128MB
    VERBOSE: Intializing parameters...
    VERBOSE: Creating prerequisites...
    VERBOSE: Verify cache service name...
    VERBOSE: Creating cache service...
    VERBOSE: Waiting for cache service to be in ready state...


    Name     : contosocache
    Location : South Central US
    State    : Active
    Sku      : Basic
    Memory   : 128MB



    PS C:\>




<a name="enable-caching"></a>
## <a name="configure-the-cache"></a>Konfigurieren des Caches

Die Registerkarte " **Konfigurieren** " für den Cache in der klassischen Azure-Portal ist, in dem Sie die Optionen für Ihren Cache konfigurieren. Jeder Cache weist einen **standardmäßigen** benannten Cache und den standardmäßigen und Premium Cache Angebote unterstützen bis zu neun zusätzliche benannten Caches, für insgesamt zehn. Jeder benannten Cache verfügt über eine eigene Gruppe von Optionen, mit die Sie Ihren Cache in einer hochgradig flexible Weise konfigurieren können.

![NamedCaches][NamedCaches]

Um einen benannten Cache erstellen möchten, geben Sie in das Feld **Name** den Namen des neuen Cache, geben Sie die gewünschten Optionen, klicken Sie auf **Speichern**, und klicken Sie auf **Ja,** um zu bestätigen. Klicken Sie zum Stornieren Änderungen **verwerfen**aus.

## <a name="expiry-policy-and-time-min"></a>Ablaufrichtlinie und die Uhrzeit (min) ##

Die **Ablaufrichtlinie** wird zusammen mit der zeiteinstellung **(min)** , um festzustellen, wann Elemente Cache abläuft. Es gibt drei Arten von Richtlinien zum Kennwortablauf: **absoluter**, **gleitende**und **nie**. 

Wenn **Absolute** angegeben wird, beginnt mit nach **Zeiten (min)** angegebene Ablaufintervall ein Elements im Cache hinzugefügt wird. Läuft ab nach dem Intervall entsprechende **(min)** verstrichen ist das Element aus. 

Beim **gleitende** angegeben ist, wird durch **Zeit (Minuten)** angegebenen Ablaufintervall jedes Mal zurückgesetzt, die ein Element im Cache zugegriffen wird. Das Element abläuft bis zum Ablauf der durch **Zeit (Minuten)** angegebenen Intervall nach dem letzten Zugriff auf das Element nicht.

Wenn **nie** angegeben ist, **Zeit (min)** muss auf **0**festgelegt sein und Elemente laufen nicht.

**Absolut** ist die standardmäßige Ablaufrichtlinie und 10 Minuten ist die Standardeinstellung für die **Uhrzeit (min)**. Die Ablaufrichtlinie für jedes Element in einem benannten Cache behoben wird, aber die **Uhrzeit (min)** für jedes Element mithilfe von **Hinzufügen** und **setzen** überladenen, die einen Timeout-Parameter angepasst werden können.

Weitere Informationen zu Entfernung und zum Ablauf Richtlinien finden Sie unter [Ablauf und Entfernung][].

## <a name="notifications"></a>Benachrichtigungen ##

Cache Benachrichtigungen, mit denen Ihre Applikationen asynchrone Benachrichtigungen, wenn eine Vielzahl Cache Operationen erhalten auftreten, auf dem Cachecluster. Cache Benachrichtigungen bieten auch lokal zwischengespeicherte Objekte automatische durchführen. Weitere Informationen finden Sie unter [Benachrichtigungen][].

>Benachrichtigungen stehen nur in der Standard- und Premium Cache Angebote und stehen nicht zur Verfügung, in das Angebot grundlegende Cache. Weitere Informationen finden Sie unter [Cache Angebote][].

## <a name="high-availability"></a>Hohe Verfügbarkeit ##

Wenn der hoher Verfügbarkeit aktiviert ist, besteht eine Sicherungskopie aller Elemente im Cache hinzugefügt. Wenn ein unerwarteter Fehler bei der primären Kopie des Elements auftritt, ist die Sicherungskopie weiterhin verfügbar.

Per Definition multipliziert die Verwendung der hohen Verfügbarkeit die Größe des erforderlichen Speichers für jedes zwischengespeicherte Element mit zwei. Betrachten Sie diese Auswirkung Arbeitsspeicher während Kapazität, Planung Aufgaben aus. Weitere Informationen finden Sie unter [Hohe Verfügbarkeit][].

>Hohe Verfügbarkeit steht nur in den Premium-Cache-Angebot und ist in der Basic oder Standard Cache Angebote nicht verfügbar. Weitere Informationen finden Sie unter [Cache Angebote][].

## <a name="eviction"></a>Entfernung ##

Wenn die Speicherkapazität verfügbar in einem Cache beibehalten möchten, ist mindestens die zuletzt verwendeten (LRU) Entfernung unterstützt. Wenn der Speichergrenzwert mehr belegt als, gibt Objekte aus dem Speicher, unabhängig davon, ob sie oder nicht, abgelaufen sind, bis der Arbeitsspeicherdruck erlassen wird entfernt.
Entfernung ist standardmäßig aktiviert. Wenn Entfernung deaktiviert ist, werden Elemente nicht werden aus dem Cache entfernt, wenn die Kapazität erreicht, und stattdessen sich und Add-Vorgängen tritt.

Weitere Informationen zu Entfernung und zum Ablauf Richtlinien finden Sie unter [Ablauf und Entfernung][].

Nachdem der Cache konfiguriert ist, können Sie den Cache Clients auf den Cache zugreifen dürfen konfigurieren.

<a name="NuGet"></a>
## <a name="configure-the-cache-clients"></a>Konfigurieren der Cache-clients

Ein Cache erstellt mithilfe des Diensts für verwaltete Cache zugegriffen werden aus Azure Applications Azure Websites, Web & Arbeitskollegen Rollen und virtuellen Computern ausgeführt. Ein NuGet-Paket wird vorausgesetzt, die die Konfiguration der Cache Clientanwendungen vereinfacht. 

Konfigurieren der Cache NuGet-Paket mit einer Clientanwendung, mit der rechten Maustaste im Projekts im **Solution Explorer** und wählen Sie **NuGet-Pakete verwalten**. 

![NuGetPackageMenu][NuGetPackageMenu]

Geben Sie im Textfeld **Suchen Online** **WindowsAzure.Caching** ein, und wählen Sie **Windows**  
**Azure** **Cache** aus den Ergebnissen. Klicken Sie auf **Installieren**, und klicken Sie dann auf **ich stimme**.

![NuGetPackage][NuGetPackage]

NuGet-Paket führt mehrere Aufgaben: die Konfigurationsdatei der Anwendung die erforderliche Konfiguration hinzugefügt und fügt die erforderlichen Verweise. Für Projekte, die Cloud Services fügt es auch eine Cache Client diagnostic Ebene Einstellung zu der Datei ServiceConfiguration.cscfg des Cloud-Dienst an.

>Für Projekte, die ASP.NET Web addiert das Cache NuGet-Paket auch, dass zwei Abschnitten zu web.config kommentiert. Der erste Abschnitt ermöglicht Sitzungszustand im Cache gespeichert werden soll, und der zweite Abschnitt ermöglicht ASP.NET Seite Ausgabe Zwischenspeichern. Weitere Informationen finden Sie unter [How To: Store ASP.NET Session State im Cache] und [How To: Store ASP.NET Ausgabe Zwischenspeichern von Seiten im Cache][].

NuGet-Paket fügt die folgenden Konfigurationselemente in Ihrer Anwendungs web.config oder app.config hinzu. Einen Abschnitt **DataCacheClients** und einen Abschnitt **CacheDiagnostics** werden unter dem Element **ConfigSections** hinzugefügt. Ist kein **ConfigSections** Element vorhanden, wird eine als untergeordnetes Element des Elements **Konfiguration** erstellt.

    <configSections>
      <!-- Existing sections omitted for clarity. -->
      <section name="dataCacheClients" 
        type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection,
              Microsoft.ApplicationServer.Caching.Core" 
        allowLocation="true" 
        allowDefinition="Everywhere" />
  
    <section name="cacheDiagnostics" 
        type="Microsoft.ApplicationServer.Caching.AzureCommon.DiagnosticsConfigurationSection,
              Microsoft.ApplicationServer.Caching.AzureCommon" 
        allowLocation="true" 
        allowDefinition="Everywhere" />


Diese neue Abschnitte enthalten Verweise auf ein Element **DataCacheClients** , die auch auf das Element **Konfiguration** hinzugefügt wird.

    <dataCacheClients>
      <dataCacheClient name="default">
        <!--To use the in-role flavor of Azure Caching, 
            set identifier to be the cache cluster role name -->
        <!--To use the Azure Caching Service,
            set identifier to be the endpoint of the cache cluster -->
        <autoDiscover isEnabled="true" identifier="[Cache role name or Service Endpoint]" />
        <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
        <!--Use this section to specify security settings for connecting to your cache. 
            This section is not required if your cache is hosted on a role that is a part
            of your cloud service. -->
        <!--<securityProperties mode="Message" sslEnabled="false">
          <messageSecurity authorizationInfo="[Authentication Key]" />
        </securityProperties>-->
      </dataCacheClient>
    </dataCacheClients>


Nachdem die Konfiguration hinzugefügt wurde, ersetzen Sie die folgenden zwei Elemente in der neu hinzugefügten Konfiguration aus.

1. Ersetzen Sie **[Name der Cache Rolle oder Service-Endpunkts an]** durch den Endpunkt, die angezeigt wird, auf dem Dashboard in der klassischen Azure-Portal an.

    ![Endpunkt][Endpoint]

2. Entfernen Sie die Kommentarzeichen im Abschnitts SecurityProperties, und Ersetzen Sie **[Authentifizierung Key]** mit der Authentifizierung-Taste, in der klassischen Azure-Portal gefunden werden kann, indem Sie auf **Schlüssel verwalten** aus dem Cache Dashboard.

    ![AccessKeys][AccessKeys]

>Diese Einstellungen müssen ordnungsgemäß konfiguriert werden, oder Clients werden nicht auf den Cache zugreifen können.

Für Projekte, die Cloud Services hinzugefügt NuGet-Paket auch eine **ClientDiagnosticLevel** Einstellung **ConfigurationSettings** der Cache Clientrolle in ServiceConfiguration.cscfg. Im folgende Beispiel wird im Abschnitt **WebRole1** aus einer Datei ServiceConfiguration.cscfg mit einem **ClientDiagnosticLevel** von 1, welche ist die Standardeinstellung **ClientDiagnosticLevel**.

    <Role name="WebRole1">
      <Instances count="1" />
      <ConfigurationSettings>
        <!-- Existing settings omitted for clarity. -->
        <Setting name="Microsoft.WindowsAzure.Plugins.Caching.ClientDiagnosticLevel" 
                 value="1" />
      </ConfigurationSettings>
    </Role>

>Die Client diagnostic Ebene konfiguriert die Ebene von Diagnoseinformationen für Cache Clients gesammelt Zwischenspeichern. Weitere Informationen finden Sie unter [Problembehandlung und Diagnose][]

NuGet-Paket fügt außerdem Verweise auf die folgenden Assemblys hinzu:

-   Microsoft.ApplicationServer.Caching.Client.dll
-   Microsoft.ApplicationServer.Caching.Core.dll
-   Microsoft.WindowsFabric.Common.dll
-   Microsoft.WindowsFabric.Data.Common.dll
-   Microsoft.ApplicationServer.Caching.AzureCommon.dll
-   Microsoft.ApplicationServer.Caching.AzureClientHelper.dll

Wenn Ihr Projekt eines Projekts ist, wird auch der folgenden Assemblyverweis hinzugefügt:

-   Microsoft.Web.DistributedCache.dll.

Nachdem Sie Ihr Clientprojekt zum Zwischenspeichern konfiguriert ist, können Sie die in den folgenden Abschnitten für das Arbeiten mit dem Cache beschriebenen Verfahren verwenden.

<a name="working-with-caches"></a>
## <a name="working-with-caches"></a>Arbeiten mit Caches

Die Schritte in diesem Abschnitt beschreiben, wie Sie allgemeine Aufgaben mit Cache ausführen.

-   [Gewusst wie: Erstellen eines Objekts DataCache][]
-   [Gewusst wie: Hinzufügen und Abrufen ein Objekts aus dem Cache][]
-   [Gewusst wie: Geben Sie den Ablauf eines Objekts im Cache][]
-   [Gewusst wie: Speichern ASP.NET Session State im Cache][]
-   [Gewusst wie: Speichern ASP.NET Seite Ausgabe Zwischenspeichern im Cache][]

<a name="create-cache-object"></a>
## <a name="how-to-create-a-datacache-object"></a>Gewusst wie: Erstellen eines Objekts DataCache

Programmgesteuertes mit einem Cache arbeiten möchten, benötigen Sie einen Verweis auf den Cache. Fügen Sie Folgendes an den Anfang aller Dateien aus dem Cache Azure verwenden soll:

    using Microsoft.ApplicationServer.Caching;

>Wenn Visual Studio die Typen in der mit erkennt Anweisung auch nach der Installation des Cache NuGet-Pakets, die die notwendigen Verweise hinzufügt, stellen Sie sicher, dass das Zielprofil für das Projekt .NET Framework 4 oder höher ist, und wählen Sie eines der Profile, die keine **Client-Profil**angegeben ist. Informationen zum Konfigurieren von Clients Cache finden Sie unter [Konfigurieren der Cache Clients][].

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

Standardmäßig ablaufen Elemente im Cache 10 Minuten, nachdem sie im Cache platziert werden. Dies kann der Einstellung **Zeit (min)** auf der Registerkarte Konfigurieren für Cache im klassischen Azure-Portal konfiguriert werden.

![NamedCaches][NamedCaches]

Es gibt drei Typen von **Ablaufrichtlinie**: **nie**, **absoluten**und **gleitenden**. Diese konfigurieren, wie die **Uhrzeit (min)** verwendet wird, den Ablauf zu ermitteln. Die Standardeinstellung ist **Typ Ablauf** **absoluten**, was bedeutet, dass der Countdown für ein Element den Ablauf beginnt, wenn das Element in den Cache platziert wird. Nachdem die angegebene Zeitspanne für ein Element abgelaufen ist, läuft das Element ab. Wenn **gleitende** angegeben ist, wird der Ablauf Countdown für ein Element jedes Mal, wenn das Element im Cache zugegriffen wird zurückgesetzt, und das Element wird nicht ablaufen, bis seit der letzten den Zugriff auf die angegebene Zeitspanne abgelaufen ist. Wenn **nie** angegeben ist, klicken Sie dann auf **0** **Zeit (min)** festgelegt werden müssen, und Elemente nicht ablaufen, und bleiben gültig, solange sie im Cache sind.

Gegebenenfalls ein Timeoutintervall verlängern oder verkürzen als was in den Cacheeigenschaften konfiguriert ist kann eine bestimmte Dauer angegeben werden, wenn ein Element hinzugefügt oder im Cache mithilfe der Überladung der **Hinzufügen** und **setzen** , die einen Parameter **TimeSpan** ausführen aktualisiert wird. Im folgenden Beispiel wird der **Wert** Cache, sortiert nach **Element**mit einem Timeout von 30 Minuten hinzugefügt.

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

Die Session State Provider für Azure Cache sind Out-of-Process-Speicher für ASP.NET Applications. Diese Anbieter können Sie Ihre Sitzungszustand in einem Azure Cache anstatt im Speicher oder in einer SQL Server-Datenbank gespeichert. Um den Zwischenspeichern Sitzung Zustand Anbieter verwenden zu können, zuerst konfigurieren Sie Ihren Cache, und konfigurieren Sie Ihrer Anwendung ASP.NET für den Cache NuGet-Paket verwenden, wie unter [Erste Schritte mit Cachediensts verwaltete][]Cache. Wenn das Cache NuGet-Paket installiert ist, fügt einen auskommentierte Abschnitt in web.config, die die erforderliche Konfiguration für eine Anwendung ASP.NET Session State-Provider für Azure Cache verwendet enthält hinzu.

    <!--Uncomment this section to use Azure Caching for session state caching
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

>Wenn Ihre web.config keinen ist dies kommentiert, Abschnitt nach der Installation von den Cache NuGet-Paket, stellen Sie sicher, dass die neuesten NuGet-Paket-Manager aus [NuGet-Paket-Manager-Installation][]installiert ist, und klicken Sie dann deinstallieren und das Paket.

Kommentieren Sie zum Aktivieren der Session State Provider für Azure Cache angegebenen Abschnitt ein. Der Standard-Cache wird in den bereitgestellten Codeausschnitt angegeben. Wenn Sie einen anderen Cache verwenden möchten, geben Sie den gewünschten Cache in das Attribut **CacheName** aus.

Weitere Informationen zur Verwendung von verwalteten Cache Sitzung Zustand Dienstanbieter finden Sie unter [Session State Provider für Azure Cache][].

<a name="store-page"></a>
## <a name="how-to-store-aspnet-page-output-caching-in-the-cache"></a>Gewusst wie: Speichern ASP.NET Seite Ausgabe Zwischenspeichern im Cache

Die Ausgabe-Cache-Anbieter für Azure Cache sind Out-of-Process-Speicher für den Cache Ausgabedaten. Diese Daten ist speziell für vollständige HTTP-Antworten (Seite Zwischenspeichern der Ausgabe). Der Anbieter schließt an die neue Ausgabe Cache Anbieter Erweiterbarkeitspunkt, der in ASP.NET 4 eingeführt wurde. Den Ausgabe Cacheanbieter verwenden, konfigurieren Sie zuerst den Cachecluster, und klicken Sie dann Konfigurieren Ihrer Anwendung ASP.NET für Zwischenspeichern unter Verwendung des Cache NuGet-Pakets, wie unter [Erste Schritte mit Cachediensts verwaltet][]. Wenn das Zwischenspeichern NuGet-Paket installiert ist, fügt in web.config, die die erforderliche Konfiguration für eine Anwendung ASP.NET mit der Ausgabe Cacheanbieter zum Zwischenspeichern von Azure enthält auskommentierte folgenden Abschnitt hinzu.

    <!--Uncomment this section to use Azure Caching for output caching
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

>Wenn Ihre web.config keinen ist dies kommentiert, Abschnitt nach der Installation von den Cache NuGet-Paket, stellen Sie sicher, dass die neuesten NuGet-Paket-Manager aus [NuGet-Paket-Manager-Installation][]installiert ist, und klicken Sie dann deinstallieren und das Paket.

Kommentieren Sie zum Aktivieren der Ausgabe-Cache-Anbieter für Azure Cache angegebenen Abschnitt ein. Der Standard-Cache wird in den bereitgestellten Codeausschnitt angegeben. Wenn Sie einen anderen Cache verwenden möchten, geben Sie den gewünschten Cache in das Attribut **CacheName** aus.

Hinzufügen einer Richtlinie **OutputCache** auf jeder Seite die Ausgabe zwischengespeichert werden soll.

    <%@ OutputCache Duration="60" VaryByParam="*" %>

Die zwischengespeicherte Seitendaten bleibt in diesem Beispiel wird im Cache für 60 Sekunden, und eine andere Version von der Seite für jede Parameterkombination zwischengespeichert werden soll. Weitere Informationen zu den verfügbaren Optionen finden Sie unter [OutputCache Richtlinie][].

Weitere Informationen zur Verwendung von der Ausgabe-Cache-Anbieter für Azure Cache finden Sie unter [Ausgabe Cache Datenanbieter für Azure Cache][].

<a name="next-steps"></a>
## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Cachediensts verwaltete bearbeitet haben, führen Sie die folgenden Links, um zu erfahren, wie komplexere Zwischenspeichern Aufgaben ausführen.

-   Finden Sie in der MSDN-Referenz: [verwaltete Cachediensts][]
-   Informationen zum Migrieren zu Cachediensts verwaltet: [Migrieren zu verwalteten Cachediensts][]
-   Schauen Sie sich die Beispiele: [Beispielen für verwaltete Cache][]

<!-- INTRA-TOPIC LINKS -->
[Nächste Schritte]: #next-steps
[What is Azure Managed Cache Service?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Erste Schritte mit verwalteten Cachediensts]: #getting-started-cache-service
[Erstellen Sie den cache]: #create-cache
[Konfigurieren des Caches]: #enable-caching
[Konfigurieren der Cache-clients]: #NuGet
[Working with Caches]: #working-with-caches
[Gewusst wie: Erstellen eines Objekts DataCache]: #create-cache-object
[Gewusst wie: Hinzufügen und Abrufen ein Objekts aus dem Cache]: #add-object
[Gewusst wie: Geben Sie den Ablauf eines Objekts im Cache]: #specify-expiration
[Gewusst wie: Speichern ASP.NET Session State im Cache]: #store-session
[Gewusst wie: Speichern ASP.NET Seite Ausgabe Zwischenspeichern im Cache]: #store-page
[Target a Supported .NET Framework Profile]: #prepare-vs-target-net
  
<!-- IMAGES -->
[NewCacheMenu]: ./media/cache-dotnet-how-to-use-service/CacheServiceNewCacheMenu.png

[QuickCreate]: ./media/cache-dotnet-how-to-use-service/CacheServiceQuickCreate.png

[Endpoint]: ./media/cache-dotnet-how-to-use-service/CacheServiceEndpoint.png

[AccessKeys]: ./media/cache-dotnet-how-to-use-service/CacheServiceManageAccessKeys.png

[NuGetPackage]: ./media/cache-dotnet-how-to-use-service/CacheServiceManageNuGetPackage.png

[NuGetPackageMenu]: ./media/cache-dotnet-how-to-use-service/CacheServiceManageNuGetPackagesMenu.png

[NamedCaches]: ./media/cache-dotnet-how-to-use-service/CacheServiceNamedCaches.jpg
  
   
<!-- LINKS -->
[Azure klassischen-Portal]: https://manage.windowsazure.com/
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State-Anbieter für Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Die Ausgabe-Cache-Anbieter für Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Übersicht über Azure verwaltete Cachediensts]: http://go.microsoft.com/fwlink/?LinkId=320830
[Verwaltete Cachediensts]: http://go.microsoft.com/fwlink/?LinkId=320830
[OutputCache Richtlinie]: http://go.microsoft.com/fwlink/?LinkId=251979
[Problembehandlung und Diagnose]: http://go.microsoft.com/fwlink/?LinkId=320839
[NuGet-Paket-Manager-Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Informationen zur Preisgestaltung im Cache]: http://www.windowsazure.com/pricing/details/cache/
[Cache Angebote]: http://go.microsoft.com/fwlink/?LinkId=317277
[Capacity planning]: http://go.microsoft.com/fwlink/?LinkId=320167
[Ablauf und Entfernung]: http://go.microsoft.com/fwlink/?LinkId=317278
[Hohe Verfügbarkeit]: http://go.microsoft.com/fwlink/?LinkId=317329
[Benachrichtigungen]: http://go.microsoft.com/fwlink/?LinkId=317276
[Migrieren Sie zu verwalteten Cachediensts]: http://go.microsoft.com/fwlink/?LinkId=317347
[Verwaltete Cache Dienstbeispiele]: http://go.microsoft.com/fwlink/?LinkId=320840
[Neue AzureManagedCache]: http://go.microsoft.com/fwlink/?LinkId=400495
[Azure Managed Cache Cmdlets]: http://go.microsoft.com/fwlink/?LinkID=398555
[So installieren und Konfigurieren von Azure PowerShell]: http://go.microsoft.com/fwlink/?LinkId=400494
[Hinzufügen von AzureAccount]: http://msdn.microsoft.com/library/dn495128.aspx
[Wählen Sie-AzureSubscription]: http://msdn.microsoft.com/library/dn495203.aspx

[Which Azure Cache offering is right for me?]: cache-faq.md#which-azure-cache-offering-is-right-for-me
 
