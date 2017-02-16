<properties
   pageTitle="Definieren und Verwalten von Bundesstaat | Microsoft Azure"
   description="So definieren und Verwalten von Dienststatus Dienst Struktur"
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

# <a name="service-state"></a>Dienststatus
**Dienststatus** bezieht sich auf die Daten, die der Dienst benötigt, um die Funktion. Sie enthält die Datenstrukturen und Variablen, die der Dienst liest und schreibt Arbeit zugewiesen wurde.

Betrachten Sie einen einfachen Rechner-Dienst, beispielsweise ein. Dieser Dienst übernimmt zwei Zahlen und gibt die Summe. Dies ist ein rein statusfreie Dienst, der keine Daten zugeordnet hat.

Betrachten Sie nun auf den gleichen Rechner, aber zusätzlich computing Summe, weist ebenfalls eine Methode zur Rückgabe der letzten Summe, die sie berechnet wurde. Dieser Dienst ist jetzt Stateful – es einige Status, die sie enthält in schreibt (Wenn sie eine neue Summe berechnet) und liest aus (Wenn sie die letzte berechnete Summe zurückgibt).

Azure Service Struktur heißt der erste Dienst statusfreien Dienst. Der zweite Dienst wird eine dynamische Service bezeichnet.

## <a name="storing-service-state"></a>Speichern von-Dienststatus
Bundesland kann entweder externalisiert oder gemeinsam mit den Code, der den Zustand manipuliert gespeichert werden. Externalization des Status ist in der Regel mithilfe einer externen Datenbank oder Store abgeschlossen. In diesem Beispiel Taschenrechner könnte dies einer SQL-Datenbank sein, in der das aktuelle Ergebnis in einer Tabelle gespeichert ist. Jeder Anforderung zum Berechnen der Summe führt eine Aktualisierung in dieser Zeile.

Bundesstaat kann auch mit der Code gemeinsame gefunden werden, die diesen Code bearbeitet. Dynamische Services Dienst Struktur werden unter Verwendung dieses Modell erstellt. Fabric-Dienst bietet die Infrastruktur, um sicherzustellen, dass dieser Status hoch verfügbar ist und die Fehlertoleranz im Fall eines Fehlers.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Dienst Fabric Konzepte finden Sie unter den folgenden:

- [Verfügbarkeit von Diensten Fabric Service](service-fabric-availability-services.md)

- [Der Dienst Fabric Services Skalierbarkeit](service-fabric-concepts-scalability.md)

- [Vorherigen Dienst Fabric services](service-fabric-concepts-partitioning.md)
