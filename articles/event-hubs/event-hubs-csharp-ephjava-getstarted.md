<properties
    pageTitle="Erste Schritte mit Ereignis Hubs in c# | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm den Einstieg in Azure Ereignis Hubs; Ereignisse in c# senden und Empfangen von in Java mit der EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Erste Schritte mit Ereignis Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Einführung

Ereignis Hubs ist ein Dienst, der große Datenmengen Ereignis (telemetrieprotokoll) von verbundenen Geräte und Programme verarbeitet werden. Nach dem Sammeln von Daten in Ereignis Hubs können Sie die Daten mit einem Cluster Speicher speichern oder mit einem in Echtzeit Analytics-Anbieter transformieren. Diese Ereignis umfangreiche Sammlung und Verarbeitung-Funktion ist eine wichtige Komponente der modernen Architekturen, einschließlich der im Internet der Dinge (IoT).

In diesem Lernprogramm erfahren, wie das Azure klassische Portal verwenden, um ein Ereignis Hub zu erstellen. Es werden auch zum Sammeln von Nachrichten an ein Ereignis Hub mit einer in c# geschriebenen Console-Anwendung, und wie sie sich parallel mit der Bibliothek Java Ereignis Prozessor Host abgerufen.

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Ein aktives Azure-Konto. <br/>Wenn Sie eine besitzen, können Sie ein kostenloses Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Jetzt sind Sie bereit sind, der die Anwendung läuft.

1.  Führen Sie das **Empfänger** Projekt.

    ![][21]

2.  Führen Sie das **Absender** Projekt.

    ![][22]

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie eine funktionsfähige Anwendung erstellt haben, die ein Ereignis Hub erstellt und sendet und empfängt Daten, können Sie in den folgenden Szenarien verschieben, klicken Sie auf:

- Eine vollständige [Beispiel-Anwendung, die Ereignis Hubs verwendet][].
- Das Beispiel [Skalierung Ereignis mit Ereignis Hubs verarbeiten][] .
- [Ereignis Hubs (Übersicht)][]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Ereignis Hubs (Übersicht)]: event-hubs-overview.md
[Beispiel-Anwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Ereignis mit Ereignis Hubs Verarbeitung skalieren]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
