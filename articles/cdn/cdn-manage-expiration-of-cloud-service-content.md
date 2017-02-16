<properties
 pageTitle="Zum Verwalten von Azure Web Apps/Cloud Services, ASP.NET und IIS-Inhalten in Azure CDN Ablauf | Microsoft Azure"
 description="Beschreibt, wie Sie den Ablauf von Cloud-Service-Inhalten in Azure CDN verwalten"
 services="cdn"
 documentationCenter=".NET"
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="09/19/2016"
 ms.author="casoper"/>

# <a name="how-to-manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Zum Verwalten von Apps/Cloud-Webdienste Azure, ASP.NET oder IIS-Inhalten in Azure CDN Ablauf

> [AZURE.SELECTOR]
- [Azure-Apps/Cloud-Webdiensten, ASP.NET oder IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure Blob-Speicherdienst](cdn-manage-expiration-of-blob-content.md)

Dateien aus einer beliebigen öffentlich zugängliche ursprünglichen Webserver können in Azure CDN zwischengespeichert werden, bis zum Ablauf der jeweilige Time to live (TTL).  Die Gültigkeitsdauer wird durch den [ *Cache-Control* -Header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in der HTTP-Antwort aus dem Ausgangsserver bestimmt.  Dieser Artikel beschreibt, wie Sie `Cache-Control` Kopfzeilen für Azure Web Apps, Azure Cloud Services, ASP.NET Applications und Internet Information Services-Websites, die entsprechend konfiguriert.

>[AZURE.TIP] Sie entscheiden möglicherweise in einer Datei keine TTL nicht festgelegt.  In diesem Fall wendet Azure CDN automatisch einen Standardwert von sieben Tagen TTL.
>
>Weitere Informationen zur Funktionsweise von Azure CDN zum Beschleunigen der Zugriff auf Dateien und anderen Ressourcen finden Sie unter der [Azure CDN Übersicht](./cdn-overview.md).

## <a name="setting-cache-control-headers-in-configuration"></a>Festlegen von Cache-Control-Header in Konfiguration

Für statische Inhalte wie Bilder und Stylesheets können Sie die Häufigkeit aktualisieren durch Ändern der **applicationHost.config** oder **web.config** -Dateien für eine Webanwendung steuern.  Das **system.webServer\staticContent\clientCache** -Element in der Konfigurationsdatei festgelegt wird die `Cache-Control` Kopfzeile für den Inhalt. Für **web.config**wirkt die Konfiguration Einstellungen alles in den Ordner und alle Unterordner, sich außer Ebene der Unterordner außer Kraft gesetzt.  Angenommen, können Sie festlegen eines Time-to-live im Stammverzeichnis alle statischen Inhalt Cache für 3 Tage haben, haben aber einen Unterordner aus, der Inhalt mit Einstellung Cache 6 Stunden mehr Variable ist.  Für **applicationHost.config**alle Programme, klicken Sie auf der Website sind davon betroffen, aber in **web.config** -Dateien, die in der Anwendung überschrieben werden können.

Die folgenden XML-zeigt und Beispiele für die Einstellung **ClientCache** ein maximale Alter von 3 Tagen angeben:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Angeben von **UseMaxAge** Fügt eine `Cache-Control: max-age=<nnn>` Kopfzeile, um die Antwort auf Grundlage der Wert in das Attribut **CacheControlMaxAge** angegeben. Das Format der erstellten ist für das Attribut **CacheControlMaxAge** ist <days>. <hours>:<min>:<sec>. Weitere Informationen zu den Knoten **ClientCache** , finden Sie unter [Client Cache <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Festlegen von Cache-Control-Header im Code

Für ASP können Sie das Verhalten programmgesteuert durch Festlegen der Eigenschaft **HttpResponse.Cache** Zwischenspeichern CDN festlegen. Weitere Informationen für die Eigenschaft **HttpResponse.Cache** finden Sie unter [HttpResponse.Cache Eigenschaft](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) und [HttpCachePolicy Class](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Wenn Sie programmgesteuert zwischenspeichern Anwendungsinhalte in ASP.NET, stellen Sie sicher, dass der Inhalt als zwischengespeichert werden markiert ist, indem Sie HttpCacheability fest auf *Öffentliche*möchten. Darüber hinaus stellen Sie sicher, dass eine Bestätigung Cache festgelegt ist. Die Bestätigung Cache kann die letzte Änderung Zeitstempel festlegen, indem Sie SetLastModified oder einen Etag-Wert festlegen, indem Sie SetETag. Optional können Sie können auch nacheinander Ablauf Cache angeben, indem Sie SetExpires aufrufen, oder Sie können auf die zuvor in diesem Dokument beschriebenen Cache Standardheuristik verlassen.  

Cache für eine Stunde Inhalt, fügen Sie beispielsweise Folgendes ein:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Details zu dem Element **clientCache**](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
- [Lesen Sie die Dokumentation für die Eigenschaft **HttpResponse.Cache**](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
- [Lesen Sie die Dokumentation zur **HttpCachePolicy Class**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  
