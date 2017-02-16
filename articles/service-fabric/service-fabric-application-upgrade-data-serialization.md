<properties
   pageTitle="Anwendungsupgrade: Datenserialisierung | Microsoft Azure"
   description="Optimale Methoden für die Datenserialisierung und Einfluss der parallele Anwendung Updates an."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>


# <a name="how-data-serialization-affects-an-application-upgrade"></a>Wie wirkt sich auf Datenserialisierung ein Anwendungsupgrade

Bei einer [Aktualisierung im Wechsel Anwendung](service-fabric-application-upgrade.md)wird das Upgrade auf eine Teilmenge der Knoten, ein Upgrade Domäne nacheinander angewendet. Dabei werden einige Domänen Upgrade auf der neueren Version der Anwendung, und werden einige Upgrade Domänen auf die ältere Version der Anwendung. Während der Installation die neue Version Ihrer Anwendung muss die alte Version Ihrer Daten zu lesen, und die alte Version Ihrer Anwendung muss die neue Version Ihrer Daten zu lesen. Wenn das Datenformat nicht vorwärts und rückwärts kompatibel ist, kann die Aktualisierung fehl oder schlechter, Daten verloren oder beschädigt. In diesem Artikel wird erläutert, was Ihre Datenformat bildet und bietet bewährte Methoden für die Sicherstellung, dass Ihre Daten vorwärts und rückwärts kompatibel.


## <a name="what-makes-up-your-data-format"></a>Wodurch zeichnet sich von Ihrem Datenformat?

Dann kommt Azure Service-Struktur die Daten, die beibehalten und repliziert aus C#-Klassen ein. Für Applikationen, die [Zuverlässigen Websitesammlungen](service-fabric-reliable-services-reliable-collections.md)verwenden, ist, die die Objekte in der zuverlässigen Wörterbücher und Warteschlangen. Für Applikationen, die [Zuverlässigen Akteuren](service-fabric-reliable-actors-introduction.md)verwenden, ist der zugrunde liegende Zustand für den Akteur. Diese C#-Klassen muss serialisierbar beibehalten und repliziert werden. Daher wird das Datenformat definiert die Felder und Eigenschaften, die, ebenfalls serialisiert werden, wie sie serialisiert werden. Angenommen, in einem `IReliableDictionary<int, MyClass>` die Daten ist ein serialisiertes `int` und ein serialisiertes `MyClass`.

### <a name="code-changes-that-result-in-a-data-format-change"></a>Code geändert wird, die als Ergebnis eine Änderung der Daten formatieren

Da das Datenformat von C#-Klassen bestimmt wird, möglicherweise Änderungen an den Klassen eine Änderung der Daten formatieren. Sie müssen sorgfältig um sicherzustellen, dass ein paralleles Update, die Daten Format ändern behandeln kann. Beispiele, bei die Daten Formatänderungen verursachen können:

- Hinzufügen oder Entfernen von Feldern oder Eigenschaften
- Umbenennen von Feldern oder Eigenschaften
- Ändern die Typen von Feldern oder Eigenschaften
- Ändern der Klassennamen oder des namespace

### <a name="data-contract-as-the-default-serializer"></a>Datenvertrag als Standardserialisierer

Das Serialisierungsprogramm ist im Allgemeinen für Lesen der Daten und Deserialisieren es in der aktuellen Version, verantwortlich, auch wenn die Daten in einer älteren oder eine *neuere* Version ist. Standardserialisierer ist [Datenvertragsserialisierer](https://msdn.microsoft.com/library/ms733127.aspx), das gut definierte Versioning Regeln enthält. Zuverlässige Sammlungen gestatten das Serialisierungsprogramm außer Kraft gesetzt werden, doch zuverlässigen Akteuren zurzeit nicht. Das Daten Serialisierungsprogramm spielt eine wichtige Rolle parallele Updates aktivieren. Der Datenvertragsserialisierer ist das Serialisierungsprogramm, das für Applikationen Dienst Fabric empfohlen.


## <a name="how-the-data-format-affects-a-rolling-upgrade"></a>Wie wirkt sich auf das Datenformat ein paralleles Update

Es gibt zwei wichtigste Szenarien, die Stelle, an der das Serialisierungsprogramm eine ältere oder *neuere* Version Ihrer Daten auftreten können, während einer Aktualisierung im Wechsel:

1. Nach einem Knoten aktualisiert wird und Sichern Sie beginnt, wird das neue Serialisierungsprogramm laden die Daten, die beibehalten wurde auf einem Datenträger, indem Sie die alte Version.
2. Während der Aktualisierung im Wechsel wird der Cluster eine Mischung aus der alten und neuen Versionen von Code enthalten. Da Replikate möglicherweise in verschiedenen Upgrade Domänen platziert werden und Replikate Daten miteinander senden, hat möglicherweise die neue und/oder alte Version Ihrer Daten durch die neuen und/oder alte Version des Serialisierungsprogramms.

> [AZURE.NOTE] Die "neue" und "alte Version" finden Sie hier in der Version des Codes, der ausgeführt wird. "Neue Serialisierungsprogramm" bezieht sich auf den Serialisierungsprogramm Code, der in die neue Version der Anwendung ausgeführt wird. "Neuen Daten" verweist auf die serialisierten C#-Klasse, aus der neuen Version der Anwendung.

Die beiden Versionen von Code und Daten Format muss vorwärts und rückwärts kompatibel. Wenn sie nicht kompatibel sind, die Aktualisierung im Wechsel kann ein Fehler auftreten oder Daten gehen verloren. Parallele Update möglicherweise auftreten, da es sich bei dem Code oder der Serialisierungskomponente lösen möglicherweise Ausnahmen oder einen Fehler Wenn die andere Version vorgefunden wird. Wenn beispielsweise eine neue Eigenschaft hinzugefügt wurde, aber das alte Serialisierungsprogramm sie während der Deserialisierung verwirft verloren Daten.

Datenvertrag ist die empfohlene Lösung für die Sicherstellung, dass Ihre Daten kompatibel ist. Es hat klar definierte Versioning Regeln für das Hinzufügen, entfernen und ändern die Felder aus. Sie hat auch Unterstützung für Umgang mit unbekannten Felder, in den Prozess Serialisierung und Deserialisierung einbinden und schreibgeschützte Klasse Vererbung. Weitere Informationen finden Sie unter [Verwenden von Datenvertrag](https://msdn.microsoft.com/library/ms733127.aspx).


## <a name="next-steps"></a>Nächste Schritte

[Umstellen der Anwendung mithilfe von Visual Studio](service-fabric-application-upgrade-tutorial.md) führt Sie durch eine Anwendung Aktualisierung mit Visual Studio.

[Umstellen der Anwendung mithilfe von Powershell](service-fabric-application-upgrade-tutorial-powershell.md) führt Sie durch eine Anwendung Aktualisierung mithilfe der PowerShell.

Steuern Sie, wie eine Anwendung upgrades mithilfe von [Parametern zu aktualisieren](service-fabric-application-upgrade-parameters.md).

Informationen Sie zum erweiterten Funktionen, die während des Upgrades Ihrer Anwendungs von Verweisen auf [Erweiterte Themen](service-fabric-application-upgrade-advanced.md)verwenden.

Beheben von häufig auftretenden Problemen in Upgrades der Anwendung von Verweisen auf die Schritte in der [Anwendungsupgrades Problembehandlung ](service-fabric-application-upgrade-troubleshooting.md).
