<properties
   pageTitle="Einführung in die Service Fabric Cluster Ressourcenmanager | Microsoft Azure"
   description="Eine Einführung in die Ressourcenmanager Dienst Fabric Cluster."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Einführung in den Dienst Fabric Cluster Ressourcenmanager
Traditionell Verwaltung von IT-Systemen oder eine Reihe von Diensten auffällt erste wenigen physischen oder virtuellen Computern, die diese bestimmte Dienste oder Systeme zugeordnet. Viele Bereiche Services wurden in eine Stufe "Web" und "Daten" oder "Speicher"-Kategorie und vielleicht mit ein paar andere spezialisierten Komponenten, wie ein Cache unterteilt. Andere Arten von Applications müssten eine messaging-Schicht, wo Anfragen vergrößern und verkleinern, Umwandeln von Texten, mit einer Ebene Arbeit Analyse oder als Teil der messaging erforderlichen Transformation verbunden ist. Jede Art von Arbeitsbelastung haben Sie eine bestimmte Computer dafür: haben Sie einige Autos dafür, den Webservern ein Paar die Datenbank. Wenn Sie ein bestimmter Typ von Arbeitsbelastung auf die Computern verursacht war auch wichtiges ausführen und dann Sie mehrere Maschinen bei dieser Art von Arbeitsbelastung so konfiguriert ist, führen Sie dort hinzugefügt oder nur einige der Computer mit größeren Computern ersetzt. Einfache. Wenn Fehler bei ein Computer, ist diesen Teil der gesamten Anwendung am unteren Kapazität, bis der Computer wiederhergestellt werden konnte. Immer noch recht einfach (sofern nicht unbedingt Spaß).

Jetzt jedoch uns sagen Sie müssen gefunden haben skalieren und den Container und/oder Microservice Eintauchfräsen getroffen haben. Plötzlich finden Sie sich mit Dutzende, Hunderte oder sogar Tausende von Maschinen, Dutzende verschiedene Typen von Diensten, vielleicht hundert verschiedene Instanzen der Dienste, jede mit einer oder mehreren Instanzen oder Replikate für High Availability (HA).

Plötzlich Verwalten Ihrer Umgebung ist nicht so einfach wie das wenige Computern zu einzelnen Arten von Auslastung dedizierter verwalten. Ihre Server werden virtuelle und nicht mehr Namen haben (Sie *haben* Einstellung von [Tiere zu Rinder](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) nach allen aktiviert). Konfiguration ist kleiner zu den Computern und mehr über die Dienste selbst. Dedizierter Hardware ist eine der Vergangenheit und Services sind kleine verteilte Systeme, mehrere kleinere Textstellen gängige Hardware Tag geworden.

Als Folge unterbrechen Ihre app früher monolithische, gestufte in separate Dienste für gängige Hardware ausgeführt, müssen Sie nun viele weitere Kombinationen behandelt. Wer feststellen, welche Arten von Auslastung auf welche Hardware ausgeführt werden können oder wie viele? Welche Auslastung auch auf der gleichen Hardware arbeiten, und der einen Konflikt? Bei ein Computer fällt aus... Was es auch ausgeführt wurde? Wer ist für die Arbeitsbelastung sicherzustellen beginnt erneut ausführen? Warten Sie für den Computer (virtuelle?), bis der-oder diejenige oder Ihrer Auslastung automatisch fallen über auf anderen Computern und dabei weiterhin ausgeführt? Ist personenbezogenen Eingriff erforderlich? Was ist mit Upgrades in dieser Art von Umgebung?

Zum Verwalten von dieser Komplexität Hilfe benötigen wir werden als Entwickler und Operatoren in dieser Art von Welt leben und d. h., ein Einstellungsprozess Binge und versuchen, über die Komplexität für Personen das Papier ist nicht die richtige Antwort erhalten.

Was zu tun ist?

## <a name="introducing-orchestrators"></a>Einführung in orchestrators
Eine "Orchestrator" wird als allgemeiner Begriff für einen Teil der Software, mit dem Administratoren, die folgenden Arten von Umgebungen verwalten kann. Orchestrators sind die Komponenten, mit denen in Anfragen wie "Ich 5 Exemplare von diesem Dienst in meiner Umgebung ausführen möchten", machen Sie es wahr ist, und klicken Sie dann (versuchen) aus, um weiterhin auf diese Weise.

Orchestrators (nicht Menschen) gibt an, was Läuten Aktion beim schlägt, ein Computer fehl oder einer Arbeitsbelastung unerwartete Gründen beendet. Die meisten Orchestrators nicht einfach nur nachdenken Fehler aufgetreten, z. B. hilft Ihnen bei der Bereitstellung neuer, Behandeln von Updates und Ressourcen Ernährung schreibgeschützte jedoch alle grundlegend zum Verwalten von, dass einige Status Konfiguration in der Umgebung gewünscht sind. Sie möchten können eine Orchestrator mitteilen, was Sie möchten, und führen Sie die Aufgaben haben. Chronos oder Marathon auf Mesos, Fleet, Punktschwarms, Kubernetes und Dienst Fabric sind alle Beispiele Orchestrators (oder haben sie sich). Weitere erstellt werden immer als die Komplexität der Verwaltung von realen Bereitstellungen in verschiedenen Arten von Umgebungen und Konditionen wächst und ändern.

## <a name="orchestration-as-a-service"></a>Orchestrierung als Dienst
Die Aufgabe, die Orchestrator in einem Dienst Fabric Cluster erfolgt hauptsächlich vom Cluster Ressource-Manager. Der Dienst Fabric Cluster Ressourcenmanager ist einer der Dienste System in Dienst Fabric und wird innerhalb der einzelnen Cluster automatisch gestartet.  Im Allgemeinen wird der Cluster Ressourcenmanager Position in drei Teile unterteilt:

1. Erzwingen von Regeln
2. Optimierung Ihrer Umgebung
3. Unterstützung von in anderen Prozessen

### <a name="what-it-isnt"></a>Was nicht
In herkömmlichen N Ebene Web-apps war immer einige Konzept von ein "Lastenausgleich", in der Regel als ein Netzwerk laden Lastenausgleich Lastenausgleichprotokolls oder einer Anwendung laden Lastenausgleich (ALB), je nachdem, wo es in den Stapel Netzwerke musst bezeichnet. Einige Lastenausgleich wie F5s BigIP Angebot basierende Hardware andere sind, wie etwa Microsoft Software NLB. In anderen Umgebungen möglicherweise etwas wie HAProxy in diese Rolle angezeigt. In dieser Architekturen ist der Auftrag für den Lastenausgleich um sicherzustellen, dass alle von den verschiedenen statusfreien front-End-Computer oder in den anderen Computern im Cluster (ungefähr) der gleichen Menge an Arbeit erhalten. Strategien für dieses variiert, jeder anderen Anruf an einen anderen Server, an die Sitzung anheften/Klebrigkeit, um tatsächliche Abschätzung und Anruf Zuordnung basierend auf der Soll-Kosten und den aktuellen Computer laden zu senden.

Notiz, die diese im besten wurde verteilt das Verfahren für die Sicherstellung, das die Webebene ungefähr geblieben. Strategien für die Datenebene Lastenausgleich wurden vollständig anderen und der Datenspeichermechanismus, um Daten Sharding normalerweise zentrieren, zwischenspeichern, verwaltete Datenbankansichten und gespeicherten Prozeduren usw. Daten abhängen.

Einige dieser Strategien interessant sind, ist der Dienst Fabric Cluster Ressourcenmanager etwas nicht wie einem Lastenausgleich oder einen Cache. Während einer Netzwerk Lastenausgleich stellt sicher, dass der front-End mittig eingestellt werden durch Verschieben den Datenverkehr an die Stelle, an der die Dienste ausgeführt werden, der Dienst Fabric Cluster Ressourcenmanager akzeptiert eine vollständig andere Strategie – grundlegend, Dienst Fabric verschiebt *Dienste* auf die Stelle, an der sie am sinnvollsten (und erwartet Datenverkehr oder laden, denen Sie folgen). Dies kann, beispielsweise Knoten werden die aktuell kalt sind, da die Dienstleistungen, die es sind nicht viel Arbeit sofort auch oder die Elemente gelöscht oder an anderer Stelle verschoben wurden. Ein weiteres Beispiel konnte Ressourcenmanager Cluster ein Diensts nicht an einem Computer an, welche Informationen für ein Upgrade ist oder aufgrund einer Sammlung Energieverbrauchs überladen, anhand der Dienste verschieben, die darauf ausgeführt wurden. Da mithilfe der Cluster Ressourcenmanager zum Verschieben von Services im Zusammenhang mit (nicht übermitteln von Netzwerkdatenverkehr an, in der Dienste bereits sind), enthält einen erheblich anderen Featuresatz im Vergleich zu was es in einem Lastenausgleich zu finden ist, und beschäftigt grundlegend Strategien für die Sicherstellung, dass die Hardware-Ressourcen im Cluster auch verwendet werden.

## <a name="next-steps"></a>Nächste Schritte
- Informationen der Architektur und den Informationsfluss innerhalb der Cluster Ressourcenmanager sehen Sie sich [in diesem Artikel](service-fabric-cluster-resource-manager-architecture.md)
- Ressourcenmanager Cluster verfügt über zahlreiche Optionen für die Beschreibung des Clusters. Um herauszufinden Auschecken Weitere Informationen zu diesen Artikels zu [einen Cluster Service Fabric beschreiben](service-fabric-cluster-resource-manager-cluster-description.md)
- Für Weitere Informationen zu anderen Optionen für das Konfigurieren verfügbar Auschecken des Themas auf die anderen Cluster Ressourcenmanager Konfigurationen services verfügbare [Informationen zum Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md)
- Kennzahlen gibt an, wie der Dienst Fabric Cluster Ressource-Manager Verbrauch und Kapazität im Cluster verwaltet. Erfahren Sie, lesen Sie weitere Informationen zu diesen und so konfigurieren sie [in diesem Artikel](service-fabric-cluster-resource-manager-metrics.md)
- Ressourcenmanager Cluster funktioniert mit Service-Fabric Verwaltungsfunktionen. Lesen Sie weitere Informationen zu dieser Integration finden Sie [in diesem Artikel](service-fabric-cluster-resource-manager-management-integration.md)
- Um herauszufinden, darüber, wie der Ressourcenmanager Cluster verwaltet und verteilt Last im Cluster, schauen Sie sich im Artikel auf [Last](service-fabric-cluster-resource-manager-balancing.md)
