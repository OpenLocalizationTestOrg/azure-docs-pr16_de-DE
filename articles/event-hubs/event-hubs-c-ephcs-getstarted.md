<properties
    pageTitle="Erste Schritte mit Ereignis Hubs in C und c# | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm den Einstieg in Azure Ereignis Hubs; C Ereignisse senden und Empfangen von Umschlag in c# mithilfe der EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="csharp"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Erste Schritte mit Ereignis Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Einführung

Ereignis Hubs ist ein hochgradig skalierbare Aufnahme-System, die Aufnahme Millionen können von Ereignissen pro Sekunde, aktivieren die Anwendung zum Verarbeiten und Analysieren der großer Datenmengen von Ihrer verbundenen Geräte und Clientanwendungen gefertigt. Nachdem in Ereignis Hubs erfasst werden, können Sie transformieren und Speichern von Daten mithilfe eines beliebigen in Echtzeit Analytics-Anbieter oder Speicher Cluster.

Weitere Informationen finden Sie unter [Ereignis Hubs Übersicht][].

In diesem Lernprogramm erfahren Sie, wie Nachrichten an ein Ereignis-Hub verwenden eine Console-Anwendung in C Aufnahme, und wie diese sich parallel mit der Bibliothek C#- [Ereignis Prozessor Host][] abgerufen werden.

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

+ Ein Entwicklungsumgebung C. In diesem Lernprogramm wird den Stapel Gcc auf einer [Azure Linux virtueller Computer](../virtual-machines/virtual-machines-linux-quick-create-cli.md) mit Ubuntu 14.04 angenommen. Anweisungen für andere Umgebungen werden in externen Links bereitgestellt werden.

+ Microsoft Visual Studio Express für Windows

+ Ein aktives Azure-Konto. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Jetzt sind Sie bereit sind, der die Anwendung läuft.

1.  Führen Sie das Projekt **Empfänger** aus in Visual Studio, und warten Sie, um den Empfängern für alle Partitionen zu starten.

    ![][21]

2.  Führen Sie die **Absender** aus, und prüfen Sie die Ereignisse im Fenster Empfänger angezeigt werden.

    ![][24]

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie eine funktionsfähige Anwendung erstellt haben, die ein Ereignis Hub erstellt und sendet und empfängt Daten, können Sie in den folgenden Szenarien verschieben, klicken Sie auf:

- Eine vollständige [Beispiel-Anwendung, die Ereignis Hubs verwendet][].
- Das Beispiel [Skalierung Ereignis mit Ereignis Hubs verarbeiten][] .
- [Ereignis Hubs (Übersicht)][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Ereignis Prozessor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Ereignis Hubs (Übersicht)]: event-hubs-overview.md
[Beispiel-Anwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Ereignis mit Ereignis Hubs Verarbeitung skalieren]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
