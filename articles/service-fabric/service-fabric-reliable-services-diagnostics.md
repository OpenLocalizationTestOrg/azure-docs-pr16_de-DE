<properties
   pageTitle="Dynamische zuverlässigen Services Diagnose | Microsoft Azure"
   description="Diagnose Funktionen für Stateful zuverlässigen Services"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="alanwar"/>

# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnose Funktionalität für zuverlässigen Stateful-Dienste
Die Stateful zuverlässigen Services StatefulServiceBase Klasse gibt aus [Quelle](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) Ereignisse, die den Dienst debuggen, ermöglichen Einblicke in wie die Laufzeit arbeitet, und Unterstützung bei der Problembehandlung verwendet werden können.

## <a name="eventsource-events"></a>Quelle Ereignisse
Der Namen der Quelle für die Stateful zuverlässigen Services StatefulServiceBase Klasse ist "Microsoft ServiceFabric Services". Beim Dienst [Debuggen in Visual Studio](service-fabric-debugging-your-application.md)wird, werden Ereignisse aus dieser Ereignisquelle im Fenster [Diagnose Ereignisse](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) angezeigt.

Beispiele für Tools und Technologien, mit deren Hilfe in sammeln und/oder anzeigen Quelle Ereignisse sind [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Microsoft Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics.md)und der [Microsoft TraceEvent Bibliothek](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Ereignisse

|Name des Ereignisses|Ereignis-ID|Ebene|Beschreibung des Ereignisses|
|----------|--------|-----|-----------------|
|StatefulRunAsyncInvocation|1|Informations-|Ausgegeben, wenn der Dienst RunAsync Aufgabe gestartet wird|
|StatefulRunAsyncCancellation|2|Informations-|Ausgegeben, wenn der Dienst RunAsync Aufgabe abgebrochen wird|
|StatefulRunAsyncCompletion|3|Informations-|Ausgegeben, wenn der Dienst RunAsync Vorgang abgeschlossen ist|
|StatefulRunAsyncSlowCancellation|4|Warnung|Ausgegeben, wenn der Dienst RunAsync Vorgang abschließen Absage zu lange dauert|
|StatefulRunAsyncFailure|5|Fehler|Ausgegeben, wenn der Dienst RunAsync Aufgabe eine Ausnahme auslöst|

## <a name="interpret-events"></a>Interpretieren von Ereignissen

Ereignisse StatefulRunAsyncInvocation, StatefulRunAsyncCompletion und StatefulRunAsyncCancellation eignen sich in den Dienst-Generator zu verstehen des Lebenszyklus von einem Dienst – als auch die Anzeigedauer, wenn ein Dienst gestartet, abgebrochen oder abgeschlossen ist. Dies kann nützlich sein, wenn für das Debuggen Dienstprobleme oder Grundlegendes zu den Dienst Lebenszyklus.

Dienst Autoren sollten schließen auf StatefulRunAsyncSlowCancellation und StatefulRunAsyncFailure Ereignisse achten, da sie Probleme mit dem Dienst anzugeben.

StatefulRunAsyncFailure wird ausgegeben, wenn die Dienst RunAsync() Aufgabe eine Ausnahme auslöst. In der Regel zeigt an eine Ausnahme ausgelöst wird, einen Fehler oder Fehler im Dienst. Darüber hinaus bewirkt, dass die Ausnahme den Dienst fehlschlägt, damit es in einen anderen Knoten verschoben wird. Dies kann ein teurer Vorgang und eingehende Anfragen verzögern kann, während der Dienst verschoben wird. Dienst Autoren sollte die Ursache der Ausnahme ermitteln und minimieren sie, falls möglich.

StatefulRunAsyncSlowCancellation wird ausgegeben, wenn eine Absage für den Vorgang RunAsync länger als vier Sekunden dauert. Wenn Sie ein Dienst Absage abzuschließen zu lange dauert, wirkt sich auf die sie die Möglichkeit für den Dienst schnell auf einem anderen Knoten neu gestartet werden. Dies kann die allgemeine Verfügbarkeit des Diensts auswirken.
