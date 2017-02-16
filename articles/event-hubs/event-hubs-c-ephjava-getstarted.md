<properties
    pageTitle="Erste Schritte mit Ereignis Hubs in C | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm den Einstieg in Azure Ereignis Hubs; C Ereignisse senden und Empfangen von in Java mit dem Ereignis Prozessor Host."
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
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Erste Schritte mit Ereignis Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Einführung

Ereignis Hubs ist ein hochgradig skalierbare Aufnahme-System, die Aufnahme Millionen können von Ereignissen pro Sekunde, aktivieren die Anwendung zum Verarbeiten und Analysieren der großer Datenmengen von Ihrer verbundenen Geräte und Clientanwendungen gefertigt. Nachdem in Ereignis Hubs erfasst werden, können Sie transformieren und Speichern von Daten mithilfe eines beliebigen in Echtzeit Analytics-Anbieter oder Speicher Cluster.

Weitere Informationen finden Sie unter dem [Ereignis Hubs Übersicht][].

In diesem Lernprogramm erfahren Sie, wie Nachrichten an ein Ereignis-Hub verwenden eine Console-Anwendung in C Aufnahme, und wie diese sich parallel mit der Bibliothek C#- [Ereignis Prozessor Host][] abgerufen werden.

Für die Durchführung dieses Lernprogramms benötigen Sie Folgendes:

+ Ein Entwicklungsumgebung C. In diesem Lernprogramm wird den Stapel Gcc auf einer [Azure Linux virtueller Computer](../virtual-machines/virtual-machines-linux-quick-create-cli.md) mit Ubuntu 14.04 angenommen. Anweisungen für andere Umgebungen werden in externen Links bereitgestellt werden.

+ Microsoft Visual Studio Express für Windows

+ Ein aktives Azure-Konto. <br/>Wenn Sie kein Konto haben, können Sie ein kostenloses Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure kostenlose Testversion</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Jetzt sind Sie bereit sind, der die Anwendung läuft.

1.  Führen Sie das **Empfänger** Projekt.

    ![][21]

2.  Führen Sie die **Absender** aus, und beachten Sie die Ereignisse im Fenster Empfänger angezeigt werden.

    ![][24]

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie eine funktionsfähige Anwendung erstellt haben, die ein Ereignis Hub erstellt und sendet und empfängt Daten, können Sie in den folgenden Szenarien verschieben, klicken Sie auf:

- Eine vollständige [Beispiel-Anwendung, die Ereignis Hubs verwendet][].
- Das Beispiel [Skalierung Ereignis mit Ereignis Hubs verarbeiten][] .
- [Ereignis Hubs (Übersicht)][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephjava-getstarted/ephjava.png
[24]: ./media/event-hubs-c-ephjava-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Ereignis Prozessor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Ereignis Hubs (Übersicht)]: event-hubs-overview.md
[Beispiel-Anwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Ereignis mit Ereignis Hubs Verarbeitung skalieren]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
