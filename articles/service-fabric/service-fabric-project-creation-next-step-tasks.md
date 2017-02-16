<properties
   pageTitle="Dienst Fabric Project Creation weitere Schritte | Microsoft Azure"
   description="Dieser Artikel enthält Links zu einer Reihe von Core Entwicklungsaufgaben für Fabric Service"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="your-service-fabric-application-and-next-steps"></a>Der Dienst Fabric-Anwendung, und weitere Schritte
Die Anwendung Azure Service Fabric wurde erstellt. Dieser Artikel beschreibt den Aufbau Ihres Projekts und einige weitere mögliche Schritte.

## <a name="your-application"></a>Ihrer Anwendung
Jeder neue Anwendung enthält eine Anwendungsprojekt. Möglicherweise gibt es eine oder zwei zusätzliche Projekte, je nach Typ des Diensts ausgewählt.

### <a name="the-application-project"></a>Projekt für die Anwendung
Projekt für die Anwendung besteht aus:

- Eine Reihe von Verweisen auf die Dienste, die die Anwendung bilden.

- Zwei veröffentlichen Profile (lokale und Cloud), die Sie zum Verwalten von Einstellungen für das Arbeiten mit verschiedenen Umgebungen – beispielsweise Einstellungen im Zusammenhang mit einen Endpunkt Cluster und, ob ein Upgrade Bereitstellungen erstellt standardmäßig verwenden können.

- Zwei Anwendung Parameterdateien (lokale und Cloud), die Sie zum Verwalten von Konfigurationen Umgebung-Anwendung, beispielsweise die Anzahl der zu erstellenden für einen Dienst Partitionen verwenden können.

- Ein Bereitstellungsskript, mit denen Sie die Anwendung über die Befehlszeile oder als Teil einer automatisierten fortlaufender Integration und Bereitstellung Verkaufspipeline bereitstellen.

- Das Anwendungsmanifest, das die Anwendung beschreibt. Das Manifest finden Sie unter dem Ordner ApplicationPackageRoot.

### <a name="stateless-service"></a>Statusfreie service
Wenn Sie einen neuen statusfreien Dienst hinzufügen, fügt Visual Studio ein Projekts Dienst zu Ihrer Lösung, die einen Typ von Nachfolger enthält `StatelessService`. Der Dienst erhöht eine lokale Variable in einen Indikator.

### <a name="stateful-service"></a>Dynamische service
Wenn Sie eine neue dynamische Dienstleistung hinzufügen, fügt Visual Studio ein Dienst-Projekt zu Ihrer Lösung, die einen Typ von Nachfolger umfasst `StatefulService`. Der Dienst erhöht einen Zähler in deren `RunAsync` Methode und speichert das Ergebnis in einem `ReliableDictionary`.

### <a name="actor-service"></a>Akteur-Dienst
Wenn Sie einen neuen zuverlässigen Akteur hinzufügen, fügt Visual Studio zwei Projekte zu Ihrer Lösung: ein Akteur-Projekt und ein Benutzeroberflächen-Projekt.

Das Projekt Akteur stellt Methoden für die Einstellung und Abrufen des Werts der Zähler, die innerhalb der Akteur des Status zuverlässig beibehalten wird. Das Benutzeroberflächen-Projekt bietet eine Benutzeroberfläche, die andere Dienste verwenden können, um den Akteur aufzurufen.

### <a name="stateless-web-api"></a>Statusfreie Web-API
Die statusfreie Project Web-API bietet einen einfachen Webdienst, den Sie verwenden können, um eine Anwendung für externe Clients zu öffnen. Weitere Informationen zur Verwendung des Projekts strukturiert, finden Sie unter [Service Fabric Web-API Services mit OWIN Self-hosting](service-fabric-reliable-services-communication-webapi.md).

### <a name="aspnet-core"></a>ASP.NET core

Der Dienst Fabric SDK bietet demselben Satz von ASP.NET Core verfügbaren Vorlagen für eigenständige ASP.NET Core Projekte: leer ist, wird [Web API][aspnet-webapi], und [Webanwendung][aspnet-webapp].

## <a name="next-steps"></a>Nächste Schritte
### <a name="create-an-azure-cluster"></a>Erstellen eines Azure Clusters
Der Dienst Fabric SDK stellt einen lokalen Cluster zum Entwickeln und testen. Erstellen einen Cluster in Azure finden Sie unter [Einrichten von einem Dienst Fabric Cluster vom Azure-Portal][create-cluster-in-portal].

### <a name="try-deploying-to-azure-for-free-with-party-clusters"></a>Versuchen Sie es in Azure kostenlos mit Partei Cluster bereitstellen

Wenn Sie, ohne das Einrichten Ihrer eigenen Cluster bereitstellen und Verwalten von Applications in Azure zu testen möchten, können Sie dem kostenlosen [Partei Cluster-Dienst](http://aka.ms/tryservicefabric)verwenden.

### <a name="publish-your-application-to-azure"></a>Veröffentlichen Sie die Anwendung in Azure
Sie können eine Anwendung direkt in Visual Studio zu einem Azure Cluster veröffentlichen. Informationen hierzu finden Sie unter [Veröffentlichen der Anwendung in Azure][publish-app-to-azure].

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Mit dem Dienst Fabric Explorer Ihren Cluster visualisieren
Dienst Fabric Explorer bietet eine einfache Möglichkeit zum Darstellen Ihrer Clusters, einschließlich bereitgestellten Applikationen und physische Layout. Weitere Informationen finden Sie unter [Ihren Cluster mit Service Fabric Explorer visualisieren][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Version und Durchführen eines Upgrades Ihrer Dienste
Dienst Fabric ermöglicht unabhängigen Versioning und Aktualisierung von unabhängigen Diensten in einer Anwendung. Weitere Informationen finden Sie unter [Versioning und Aktualisieren Ihrer Dienste][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Fortlaufenden Integration mit Visual Studio Team Services konfigurieren
Um zu erfahren, wie Sie eine fortlaufende Integrationsprozess für eine Anwendung Dienst Fabric einrichten können, finden Sie unter [Konfigurieren fortlaufender Integration in Visual Studio Team Services][ci-with-vso].


<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
