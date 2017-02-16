<properties
    pageTitle="Azure-App-Dienst und deren Einfluss auf die vorhandenen Azure services"
    description="Wird erläutert, wie die neuen Azure-App-Verwaltungsdienst und der zugehörigen Funktionen vorhandenen Services in Azure auswirken."
    services="app-service"
    documentationCenter=""
    authors="yochay"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="yochayk"/>


# <a name="azure-app-service-and-existing-azure-services"></a>Azure-App-Dienst und vorhandenen Azure services

In diesem Artikel werden die Änderungen zu vorhandenen Azure Services als Teil der Änderung an mehrere Azure Dienste in [Azure-App-Dienst](https://azure.microsoft.com/services/app-service/)eine neue integriertes Angebot zu kombinieren.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>(Übersicht)

[App-Verwaltungsdienst Azure](https://azure.microsoft.com/services/app-service/) ist ein neues vorübergehend und einmalig Clouddienst, der ermöglicht Entwicklern das Web und mobile apps für jede Plattform und jedem Gerät zu erstellen. App-Dienst ist eine integrierte Lösung wiederholte Codierung Funktionen optimieren, Enterprise und SaaS Systeme integrieren und Automatisieren von Geschäftsprozessen während einer Besprechung Ihren Anforderungen für Sicherheit, Zuverlässigkeit und Skalierbarkeit soll.

App-Dienst vereint die folgenden vorhandenen Azure Services - [Websites](https://azure.microsoft.com/services/websites/), [Mobile Dienste](https://azure.microsoft.com/services/mobile-services/)und [Biztalk-Dienste](https://azure.microsoft.com/services/biztalk-services/) in einen einzelnen kombinierten Dienst beim Hinzufügen von leistungsfähige neue Funktionen.  App-Dienst können Sie die folgenden Arten von app hosten:

-   Web Apps
-   Mobile-Apps
-   API-Apps
-   Logik Apps

In der folgenden Tabelle wird erläutert, wie vorhandene Azure Services-Karte App-Dienst und die app-Typen darin verfügbar.

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">Vorhandene Azure Service</th>
<th align="left", style="width:10%">Azure App-Verwaltungsdienst</th>
<th align="left", style="width:80%">Was hat sich geändert</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure-Websites</td>
<td align="left">Web Apps</td>
<td align="left"><li>Bei Azure-Websites beträgt App Dienst grundsätzlich Namensänderung Websites bei Web Apps.
<p><li>Alle vorhandenen Instanzen von Websites sind jetzt Web Apps in der App-Dienst.</p>
<p><li>Hier Sie alle vorhandenen Websites unter <em>Web Apps finden</em>können Sie Ihren vorhandenen Websites über das <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure-Portal</a>zugreifen.</p>
<p><li><em>Web Hostinganbieter planen</em> ist jetzt <em>App Dienst planen</em>. Eine <em>App Dienst planen</em> können eine beliebige app-Typ des App-Verwaltungsdienst, z. B. Web, mobil, Logik oder API apps hosten.</p>
<p><li>Azure App Dienst Web Apps ist im allgemeinen Verfügbarkeit.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">Erfahren Sie mehr über das Web Apps</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure Mobile Dienste</td>
<td align="left">Mobile-Apps</td>
<td align="left"><p><li>Mobile-Dienste weiterhin als eigenständigen Dienst zur Verfügung und bleiben vollständig unterstützt.</p>
<p><li>Mobile-Apps ist, geben Sie eine app App-Verwaltungsdienst, welche alle Funktionen von Mobile Dienste und vieles mehr integriert.</p>
<p><li>Es ist einfach zu <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">Migrieren von Mobile-Apps Mobile-Dienste</a>.</p>
<p><li>Als Bestandteil der App-Dienst erhalten Mobile-Apps neue Funktionen jenseits Mobile-Dienste, wie z. B. Integration mit lokal und SaaS Systeme, das staging Steckplätze, WebJobs, besser Skalierungsoptionen und mehr.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">Weitere Informationen zu Mobile-Apps</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API-Apps</td>
<td align="left">
<p><li>Apps-API ist ein neuer app-Typ in der App-Verwaltungsdienst, mit dem Sie einfach erstellen und APIs in der Cloud zu nutzen.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/api/">Weitere Informationen über Apps API</a>.</p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">Logik Apps</td>
<td align="left">
<p><li>Logik Apps ist ein neuer app-Typ in der App-Dienst, den Sie einfach Geschäftsprozess automatisieren können.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/logic/">Weitere Informationen über Apps Logik</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">Azure BizTalk-Dienste</td>
<td align="left">BizTalk API Apps</td>
<td align="left">
<li><p>BizTalk-Dienste weiterhin als eigenständigen Dienst zur Verfügung und bleiben vollständig unterstützt.</p>
<li><p>Alle Funktionen der BizTalk-Dienste sind in der App-Dienst als API Apps es Benutzern integriert und B2B Integration Szenarien mit der app Typen im App-Dienst ausführen integriert.</p>
<li><p>Mit Logik Apps können Sie jetzt eine visuelle Design-Benutzeroberfläche zum Erstellen von Workflows mit Geschäftsprozesse automatisieren.</p></td>
</tr>
</tbody>
</table>

Weitere Informationen finden Sie auf [servicedokumentation App](https://azure.microsoft.com/documentation/services/app-service/).
