<properties 
    pageTitle="Sitzungszustand mit Azure Redis Cache in Azure-App-Verwaltungsdienst" 
    description="Erfahren Sie, wie in der Azure-Cache-Dienst verwenden, um ASP.NET Sitzung Zustand Zwischenspeichern unterstützen." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    manager="wpickett" 
    editor="none"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="06/27/2016" 
    ms.author="riande"/>


# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Sitzungszustand mit Azure Redis Cache in Azure-App-Verwaltungsdienst


In diesem Thema wird erläutert, wie der Azure Redis Cachediensts für Sitzungszustand verwendet wird.

Wenn ASP.NET Web app Sitzungszustand verwendet, müssen Sie einen externen Sitzung Zustand Provider (des Cachediensts Redis oder eines Anbieters für SQL Server-Sitzung) konfigurieren. Wenn Sie Sitzungszustand verwenden und einen externen Provider nicht verwenden, können Sie eine Instanz des Web app stehen. Die Cachediensts Redis ist die schnellste und einfachste zu aktivieren.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

##<a name="a-idcreatecacheacreate-the-cache"></a><a id="createcache"></a>Erstellen Sie den Cache
Führen Sie [diese Anweisungen](../cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) , um den Cache zu erstellen.

##<a name="a-idconfigureprojectaadd-the-redissessionstateprovider-nuget-package-to-your-web-app"></a><a id="configureproject"></a>Hinzufügen des RedisSessionStateProvider NuGet-Pakets zu Ihrer Web-app
Installieren der NuGet `RedisSessionStateProvider` Paket.  Verwenden Sie den folgenden Befehl aus der Paket-Manager-Konsole installieren (**Tools** > **NuGet Package Manager** > **Paket-Manager-Konsole**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`
  
Installieren von **Tools** > **NuGet Package Manager** > **NugGet Pakete für Lösung verwalten**, suchen Sie nach `RedisSessionStateProvider`.

Weitere Informationen finden Sie auf der [Seite NuGet RedisSessionStateProvider](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/ ) und [Konfigurieren der Cacheclient](../cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

##<a name="a-idconfigurewebconfigamodify-the-webconfig-file"></a><a id="configurewebconfig"></a>Ändern Sie die Datei Web.Config
Nicht nur Verweise auf Assemblys für Cache, fügt das NuGet-Paket Stub-Einträge in der Datei *web.config* . 

1. Öffnen Sie die *web.config* und suchen Sie die **SessionState** Element.

1. Geben Sie die Werte für `host`, `accessKey`, `port` (SSL-Anschluss sollten 6380), und legen Sie `SSL` auf `true`. Diese Werte können aus dem Blade [Azure-Portal](http://go.microsoft.com/fwlink/?LinkId=529715) für Ihre Cacheinstanz abgerufen werden. Weitere Informationen finden Sie unter [Verbinden mit dem Cache](../cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). Beachten Sie, dass der Port nicht SSL standardmäßig für neue Caches deaktiviert ist. Weitere Informationen zu den Port nicht SSL aktivieren finden Sie unter dem Abschnitt [Access-Ports](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) im Thema [Konfigurieren eines Cache in Azure Redis Cache](https://msdn.microsoft.com/library/azure/dn793612.aspx) . Das folgende Markup zeigt die Änderungen an der Datei *web.config* , insbesondere der Änderungen an den *Port*, *Host*, AccessKey*, und *Ssl *.

          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;


##<a name="a-idusesessionobjecta-use-the-session-object-in-code"></a><a id="usesessionobject"></a>Verwenden Sie das Sitzungsobjekt Code
Der letzte Schritt ist mit dem Sitzungsobjekt in Ihrer ASP.NET-Code beginnen. Objekte hinzufügen in Sitzungszustand mithilfe der Methode **Session.Add** . Diese Methode wird zum Speichern von Elementen im Cache Zustand Sitzung Schlüssel-Wert-Paare verwendet.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

Im folgende Code ruft diesen Wert von Sitzungszustand.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue; 

Sie können auch der Cache Redis Cacheobjekte in Ihrer Web app verwenden. Weitere Informationen finden Sie unter [MVC Film app mit Azure Redis Cache in 15 Minuten](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
Ausführliche Informationen zum Verwenden von ASP.NET Session State finden Sie unter [ASP.NET Session State Overview][].

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="whats-changed"></a>Was hat sich geändert
* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

  *Indem Sie [Mit der rechten Maustaste Schmidt](https://twitter.com/RickAndMSFT)*
  
  [installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
  [ASP.NET Session State (Übersicht)]: http://msdn.microsoft.com/library/ms178581.aspx

  [NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
  [NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
  [CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
  [NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
  [OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
  [CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
  [EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
  [ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png
 
