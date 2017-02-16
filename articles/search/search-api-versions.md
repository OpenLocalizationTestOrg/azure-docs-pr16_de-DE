<properties
   pageTitle="Versionen der Azure-API | Microsoft Azure | Search-API"
   description="Versionsrichtlinie für Azure Suche REST-APIs und der Clientbibliothek im .NET SDK."
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/16/2016"
   ms.author="brjohnst"/>

# <a name="api-versions-in-azure-search"></a>API-Versionen in Azure-Suche

Azure Suche setzt sich Feature Aktualisierungen in regelmäßigen Abständen. Manchmal aber nicht immer, erfordern diese Updates uns eine neue Version von unserem API veröffentlichen können, um die Abwärtskompatibilität sicherzustellen. Eine neue Version veröffentlichen, können Sie steuern, wann und wie Sie Dienstupdates suchen im Code integrieren.

In der Regel Versuch neuere Versionen nur bei Bedarf veröffentlicht werden, da es einige leistungsgesteuert zum Aktualisieren Ihres Codes, um eine neue Version der API verwendet werden kann. Wir werden nur eine neue Version veröffentlichen, wenn wir einige Aspekt der API auf eine Weise ändern, die Abwärtskompatibilität Umbrüche müssen. Dies kann geschehen, da Updates an den vorhandenen Features oder aufgrund von neuen Features, die vorhandene API-Oberfläche ändern.

Wir führen Sie die gleiche Regel SDK Updates. Das Azure SDK folgt die Regeln [semantische Versioning](http://semver.org/) , d. h., dass deren Version drei Teilen: Haupt- und Nebenversion build-Nummer (z. B. 1.1.0). Es wird eine neue Hauptversion des SDK nur bei einer Änderung freigeben, die Abwärtskompatibilität unterbrechen. Geschütztes Feature Updates erhöhen wir die Nebenversion und zur Behebung von Problemen werden wir nur die Buildversion vergrößern.

##<a name="snapshot-of-current-versions"></a>Momentaufnahme der aktuellen Versionen 

Unter ist eine Momentaufnahme der aktuellen Versionen aller Schnittstellen zu Azure programming. Leitfäden und andere Details können in den folgenden Abschnitten dieses Dokuments gefunden werden.

Schnittstellen|Neueste Hauptversion|Status
----------|-------------------------|------
[.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)|1.1|In der Regel verfügbar, Februar 2016 freigegeben
[.NET SDK-Vorschau](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx)|2.0-Vorschau|Vorschau August 2016 freigegeben
[Dienst REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx)|2015-02-28|In der Regel verfügbar
[Service REST-API Preview](search-api-2015-02-28-preview.md)|2015-02-28-Vorschau|Vorschau
[Management REST-API](https://msdn.microsoft.com/library/azure/dn832684.aspx)|2015-08-19|In der Regel verfügbar

Für die restlichen-APIs, einschließlich der `api-version` auf jede ist erforderlich. Dies erleichtert das auf eine bestimmte Version, beispielsweise eine Vorschau API ausgerichtet. Im folgenden Beispiel wird veranschaulicht, wie die `api-version` angegeben wurde:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2015-02-28

> [AZURE.NOTE] Obwohl jede Anforderung hat eine `api-version`, es empfiehlt sich, dass Sie dieselbe Version für alle API Anfragen verwenden. Dies gilt besonders, wenn neuere Versionen von API vorstellen, Attribute oder Vorgänge, die von früheren Versionen nicht erkannt werden. Kombinieren von API-Versionen haben kann unbeabsichtigte Folgen und sollte vermieden werden.
> 
Der Dienst REST-API und Management REST-API bilden unabhängig voneinander. Alle Ähnlichkeit in Versionsnummern ist, gemeinsame Nebenkosten.

In der Regel verfügbar (oder GA) APIs Herstellung verwendet werden können und Azure Service Servicelevel werden. Vorschauversionen haben Versuche Features, die nicht immer an GA Version migriert werden. **Wir raten dringend Vorschau APIs in Clientanwendungen Herstellung verwenden.**

##<a name="sdk-version-roadmap"></a>SDK Version Wegweiser

Jede-Version .NET SDK verwendet eine bestimmte Version der Dienst REST-API. Funktionen sind in die REST-API zuerst Variationswebsites, und klicken Sie dann im SDK implementiert.

.NET SDK ist jetzt in der Regel verfügbar und derzeit wird die bereits auf die nächste Version. In der folgenden Tabelle wird in zukünftigen Versionen von SDK anstehen gesucht, damit stehen Ihnen eine Vorstellung davon, was als Nächstes fällig wird.

.NET SDK-version|REST-API version|Features|ETA
----------------|----------------|--------|---
1.1|2015-02-28|Lucene Abfragesyntax|Februar 2016
2.0-Vorschau|2015-02-28-Vorschau|Benutzerdefinierte Analyzern, Azure Blob und Tabelle Indexern, Feld Zuordnungen, ETags|August 2016
2.x|Neue Version von GA-API|Identisch 2.0-Vorschau|Frühestes F4 2016

##<a name="about-preview-and-generally-available-versions"></a>Informationen zu Seitenansicht und in der Regel verfügbar Versionen

Azure Suche frei vorab immer Versuche Features über die REST-API zuerst, dann durch Beta-Version von .NET SDK.

Vorschau-Features werden nicht unbedingt migriert werden auf ein GA-Release. Features, die in einer Version GA unveränderliche und wahrscheinlich nicht ändern, mit Ausnahme von kleinen abwärtskompatibel korrigiert und erweitert betrachtet werden, stehen Preview-Features zum Testen und experimentieren, mit dem Ziel Sammeln von Feedback auf Feature Planung und Implementierung. 

Da Features Preview Ankündigung geändert haben, empfehlen wir jedoch gegen Schreiben von Code Herstellung, der eine Abhängigkeit Preview-Versionen annimmt. Wenn Sie eine ältere Preview-Version verwenden, wird empfohlen, auf die in der Regel verfügbar (GA) Version migrieren. 

Für .NET SDK: Leitfaden für die Codemigration von finden Sie unter [Upgrade von .NET SDK](search-dotnet-sdk-migration.md).

Allgemeiner Verfügbarkeit bedeutet, dass Azure Suche jetzt unter Servicelevel (Vereinbarung zum SERVICELEVEL). Der Vereinbarung zum SERVICELEVEL finden Sie unter [Azure Suche Service Level Agreements](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

