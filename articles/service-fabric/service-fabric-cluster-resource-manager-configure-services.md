<properties
   pageTitle="Konfigurieren von Diensten mit Dienst Fabric Cluster Ressourcenmanager | Microsoft Azure"
   description="Eine Fabric-Dienst durch Angabe von Kennzahlen, Platzierung Einschränkungen und andere Platzierungsrichtlinien beschrieben."
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


# <a name="configuring-cluster-resource-manager-settings-for-service-fabric-services"></a>Konfigurieren von Cluster Resource Manager-Einstellungen für den Dienst Fabric services
Der Dienst Fabric Cluster Ressourcenmanager kann sehr detaillierte Kontrolle die Regeln für die Arbeit mit jeder einzelnen benannten Dienst an. Jede Instanz der benannten Service angeben wie es im Cluster zugewiesen werden soll, und kann definieren den Satz von Kriterien, die sie melden möchten möchte, einschließlich wie wichtig er für diesen Dienst sind Regeln. Im Allgemeinen Konfigurieren von Diensten Umbrüche nach unten in drei unterschiedliche Aufgaben:

1. Konfigurieren von Platzierung Einschränkungen
2. Konfigurieren von Kriterien
3. Konfigurieren von erweiterten Platzierungsrichtlinien (weniger häufige)

Wissenswertes zu den einzelnen wiederum:

## <a name="placement-constraints"></a>Platzierung Einschränkungen
Zum Steuern der Knoten im Cluster einen Dienst tatsächlich ausgeführt werden können, werden Platzierung Einschränkungen verwendet. In der Regel wird eine Instanz von bestimmter benannte Dienst angezeigt oder alle Dienste eines bestimmten Typs beschränkt, auf einen bestimmten Typ von Knoten auszuführen sind. Andererseits Platzierung Einschränkungen sind erweiterbar – Sie können eine Reihe von Eigenschaften auf Basis Typ Knoten definieren, und wählen Sie für diese mit Einschränkungen beim Dienst erstellt wird. Platzierung Einschränkungen sind auch über die Gültigkeitsdauer des Diensts, so dass Sie Änderungen im Cluster reagiert dynamisch aktualisiert werden kann. Die Eigenschaften eines bestimmten Knotens können auch im Cluster dynamisch aktualisiert werden. Platzierung Einschränkungen und so konfigurieren sie weitere Informationen finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-cluster-description.md#placement-constraints-and-node-properties)

## <a name="metrics"></a>Kennzahlen
Kennzahlen gibt den Satz von Ressourcen, die eine Serviceinstanz der angegebenen benannten, einschließlich Informationen wie viel dieser Ressource muss, die jeder dynamische Replikat oder statusfreie Instanz von diesem Dienst standardmäßig verwendet. Die Kriterien umfassen auch eine Stärke, die festlegt, wie wichtig die Metrik Lastenausgleich zu diesem Dienst ist, im Fall Kompromisse erforderlich sind.

## <a name="other-placement-rules"></a>Andere Platzierungsregeln
Es gibt andere Arten von Platzierungsregeln, die hauptsächlich hilfreich sind in Cluster die geografischen verteilt sind oder in anderen weniger häufige Szenarien aus. Diese werden über Korrelationen oder Richtlinien konfiguriert. Während sie nicht in einer Vielzahl von Szenarien verwendet, werden diese auf Vollständigkeit beschrieben.

## <a name="next-steps"></a>Nächste Schritte
- Kennzahlen gibt an, wie der Dienst Fabric Cluster Ressource-Manager Verbrauch und Kapazität im Cluster verwaltet. Erfahren Sie, lesen Sie weitere Informationen zu diesen und so konfigurieren sie [in diesem Artikel](service-fabric-cluster-resource-manager-metrics.md)
- Zugehörigkeit ist ein Modus, die Sie für Ihre Dienstleistungen konfigurieren können. Es ist nicht üblich, aber bei Bedarf weitere dagegen [hier](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
- Es gibt viele verschiedene Platzierungsregeln, die auf dem Dienst verarbeitet zusätzliche Szenarien konfiguriert werden können. Sie können erfahren Sie mehr über diese anderen Platzierungsrichtlinien [hier](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
- Beginnen Sie am Anfang und [erhalten Sie eine Einführung in die Ressourcenmanager Dienst Fabric Cluster](service-fabric-cluster-resource-manager-introduction.md)
- Um herauszufinden, darüber, wie der Ressourcenmanager Cluster verwaltet und verteilt Last im Cluster, schauen Sie sich im Artikel auf [Last](service-fabric-cluster-resource-manager-balancing.md)
- Ressourcenmanager Cluster verfügt über zahlreiche Optionen für die Beschreibung des Clusters. Um herauszufinden Auschecken Weitere Informationen zu diesen Artikels zu [einen Cluster Service Fabric beschreiben](service-fabric-cluster-resource-manager-cluster-description.md)
