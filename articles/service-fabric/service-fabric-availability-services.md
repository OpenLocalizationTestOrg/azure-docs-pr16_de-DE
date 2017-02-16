<properties
   pageTitle="Verfügbarkeit von Diensten Dienst Fabric | Microsoft Azure"
   description="Fehlerstrukturanalyse-Erkennung, Failover und Wiederherstellung für Dienste werden"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="availability-of-service-fabric-services"></a>Verfügbarkeit von Diensten Fabric Service
Azure Service Fabric Services können entweder dynamische oder statusfrei sein. Dieser Artikel bietet einen Überblick darüber, wie Dienst Fabric Verfügbarkeit eines Service Ausfall unterhält.

## <a name="availability-of-service-fabric-stateless-services"></a>Verfügbarkeit von Service Fabric zustandsloser Dienste
Ein statusfreier Dienst ist ein Anwendungsdienst, der nicht über einen [lokalen beständigen Status](service-fabric-concepts-state.md)verfügt.

Erstellen eines statusfreien Diensts erfordert Definieren einer Anzahl der Instanzen, also die Anzahl der Instanzen des Diensts statusfreie, die im Cluster ausgeführt werden soll. Dies ist die Anzahl der Exemplare, die Logik der Anwendung, die im Cluster instanziiert werden soll. Erhöhen der Anzahl der Instanzen ist die empfohlene Methode der Skalierung von einem statusfreie Dienst.

Wenn ein Fehler in allen Instanzen von einem statusfreie Dienst gefunden wurde, wird eine neue Instanz auf einem anderen berechtigt Knoten im Cluster erstellt.

## <a name="availability-of-service-fabric-stateful-services"></a>Verfügbarkeit von dynamische Dienst Fabric-Diensten
Ein dynamische Dienst enthält einige Status zugeordnet. Dienst Struktur ist ein dynamische Dienst als Satz von Replikaten erstellt. Jedes Replikat ist eine Instanz des Codes, der der Dienst, der eine Kopie des Status. Lese- und Schreibberechtigungen Vorgänge werden in eine Replikation (die primäre genannt) ausgeführt. Änderungen von Aussage schreiben, dass Vorgänge auf mehrere andere Replikate (aktive sekundäre genannt) *repliziert* werden. Die Kombination von Primär- und aktive sekundäre Replikate ist der Replikatgruppe des Diensts.

Es kann nur eine primäres Replikat Wartung lesen und Schreiben von Besprechungsanfragen, aber es können mehrere aktive sekundäre Kopien werden. Die Anzahl der aktive sekundäre Kopien konfiguriert ist, und eine höhere Anzahl von Replikaten tolerieren kann eine größere Anzahl von gleichzeitigen Software- und Hardware-Fehlern.

Dienst Fabric in das Ereignis eines Fehlers (wenn primäre Replikat wechselt nach unten), ist eine der die aktiven sekundären Kopien der neuen primäres Replikat. Diese aktiven sekundäre Kopie hat bereits die aktualisierte Version des Status (mittels *Replikation*), und weiter verarbeitet weiteren Lesen und Schreiben Sie Vorgänge kann.

Dieses Konzept – einer Replikation wird entweder kein primärer oder aktiv sekundären – wie die Rolle des Replikat bekannt ist.

### <a name="replica-roles"></a>Replikat Rollen
Die Rolle einer Replikation wird zum Verwalten des Lebenszyklus des Status von diesem Replikat verwaltet werden. Ein Replikat, dessen Rolle primären Dienste ist, Anfragen zu lesen. Diese services auch Schreibanfragen durch Aktualisieren der Zustand und die Änderungen auf die aktive sekundäre in deren Replikatgruppe repliziert. Die Rolle des einer aktiven sekundären besteht darin empfangen Zustandsänderungen, die primäre Replikat repliziert wurde, und aktualisieren die Ansicht des Status.

>[AZURE.NOTE] Programmierung gelegt wie [zuverlässigen Akteuren Framework](service-fabric-reliable-actors-introduction.md) abstrakte abwesend des Konzepts der Replikat Rolle aus der Entwickler.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Dienst Fabric Konzepte finden Sie unter den folgenden:

- [Skalierbarkeit Dienst Fabric-Dienste](service-fabric-concepts-scalability.md)

- [Vorherigen Dienst Fabric-services](service-fabric-concepts-partitioning.md)

- [Definieren und Verwalten von Zustand](service-fabric-concepts-state.md)
