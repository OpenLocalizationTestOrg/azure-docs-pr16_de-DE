<properties
    pageTitle="Erste Schritte mit Ereignis Hubs mit C und Apache Storm | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm den Einstieg in Azure Ereignis Hubs; Senden von Ereignissen in C und diese in einem Cluster Apache Storm empfangen."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="java"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Erste Schritte mit Ereignis Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Einführung

Ereignis Hubs ist ein hochgradig skalierbare Aufnahme-System, die Aufnahme Millionen können von Ereignissen pro Sekunde, aktivieren die Anwendung zum Verarbeiten und Analysieren der großer Datenmengen von Ihrer verbundenen Geräte und Clientanwendungen gefertigt. Nachdem in Ereignis Hubs erfasst werden, können Sie transformieren und Speichern von Daten mithilfe eines beliebigen in Echtzeit Analytics-Anbieter oder Speicher Cluster.

Weitere Informationen finden Sie unter [Übersicht Ereignis Hubs].

In diesem Lernprogramm erfahren Sie, wie Nachrichten an ein Ereignis-Hub verwenden eine Console-Anwendung in C Aufnahme, und wie diese parallel mit Apache Storm abgerufen werden.

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

+ Ein Entwicklungsumgebung C. In diesem Lernprogramm wird den Stapel Gcc auf einer [Azure Linux virtueller Computer](../virtual-machines/virtual-machines-linux-quick-create-cli.md) mit Ubuntu 14.04 angenommen. Anweisungen für andere Umgebungen werden in externen Links bereitgestellt werden.

+ Eine Java-Entwicklungsumgebung so konfiguriert, dass [Maven](http://maven.apache.org/)ausführen. In diesem Lernprogramm wird ["Ellipse"](https://www.eclipse.org/)angenommen.

+ Ein aktives Azure-Konto. Wenn Sie kein Konto haben, können Sie ein kostenloses Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Jetzt sind Sie bereit sind, der die Anwendung läuft.

1.  Führen Sie die **LogTopology** -Klasse aus "Ellipse" und dann auf die Empfängern für alle Partitionen starten warten.

2.  Führen Sie die **Absender** aus, und prüfen Sie die Ereignisse im Fenster Empfänger angezeigt werden.

    ![][23]

> [AZURE.NOTE] Verwenden Sie in diesem Lernprogramm nur Storm im lokalen Modus hinsichtlich der Entwicklung ein. Schlagen Sie in der [HDInsight Storm Übersicht] und der offiziellen [Apache Storm] Dokumentation Storm Bereitstellungen und Muster für Weitere Informationen.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Ressourcen stehen für die Entwicklung von Applications Integration von Ereignis Hubs und Storm.

- [Analysieren von Sensordaten mit Storm und HDInsight][] ist eine vollständige Szenario-Lernprogramm von Ereignis Hubs, Storm und HBase zum Sensordaten in einem Cluster Hadoop Aufnahme.
- [Entwicklung streaming Datenverarbeitung Applikationen mit SCP.NET und c# Storm und HDInsight][] ist ein Lernprogramm zum Schreiben Storm Pipelines mit c#.

<!-- Images. -->
[23]: ./media/event-hubs-c-storm-getstarted/receive-storm3.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Ereignis Hubs (Übersicht)]: event-hubs-overview.md

[Apache Storm]: https://storm.incubator.apache.org
[HDInsight Storm (Übersicht)]: ../hdinsight/hdinsight-storm-overview.md/
[Analysieren von Sensordaten mit Storm und HDInsight]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[Entwickeln Sie streaming Datenverarbeitung Applikationen mit SCP.NET und c# Storm und HDInsight]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
