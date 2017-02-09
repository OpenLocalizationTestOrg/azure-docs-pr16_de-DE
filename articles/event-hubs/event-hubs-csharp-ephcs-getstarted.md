<properties
    pageTitle="Erste Schritte mit Ereignis Hubs in c# | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm Azure Ereignis Hubs mit c# und die EventProcessorHost Schritte."
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
    ms.date="09/02/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Erste Schritte mit Ereignis Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Einführung

Ereignis Hubs ist ein Dienst, der große Datenmengen Ereignis (telemetrieprotokoll) von verbundenen Geräte und Programme verarbeitet werden. Nach dem Sammeln von Daten in Ereignis Hubs können Sie die Daten mit einem Cluster Speicher speichern oder mit einem in Echtzeit Analytics-Anbieter transformieren. Diese Ereignis umfangreiche Sammlung und Verarbeitung-Funktion ist eine wichtige Komponente der modernen Architekturen einschließlich Internet der Dinge (IoT) an.

In diesem Lernprogramm erfahren, wie das Azure klassische Portal verwenden, um ein Ereignis Hub zu erstellen. Es werden auch zum Sammeln von Nachrichten an ein Ereignis Hub mit einer in c# geschriebenen Console-Anwendung, und wie sie sich parallel mit der Bibliothek C#- [Ereignis Prozessor Host][] abgerufen.

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Ein aktives Azure-Konto. Wenn Sie eine besitzen, können Sie ein kostenloses Konto nur wenigen Minuten erstellen. Details finden Sie [Kostenlose Testversion Azure](https://azure.microsoft.com/free/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Jetzt sind Sie bereit sind, der die Anwendung läuft.

1. Öffnen Sie in Visual Studio Projekt **Empfänger** , die, das Sie zuvor erstellt haben.

2. Mit der rechten Maustaste in der Lösung **Empfänger** , und klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Vorhandenes Projekt**.
 
3. Suchen Sie die vorhandene Sender.csproj Datei, und doppelklicken Sie darauf, um es in der Lösung hinzuzufügen.
 
4. Erneut mit der rechten Maustaste in der Lösung für die **Empfänger** ein, und klicken Sie dann auf **Eigenschaften**. Die Eigenschaftenseite für die **Empfänger** angezeigt wird.

5. Klicken Sie auf **Start-Projekt**, und klicken Sie auf die Schaltfläche **Start projektübergreifende** . Legen Sie im Feld **Action** für Projekte, die die **Empfänger** und den **Absender** zu **Starten**.

    ![][19]

6. Klicken Sie auf **Projekt Abhängigkeiten**. Klicken Sie im Feld **Projekte** auf **Absender**. Klicken Sie im Feld **Depends on** stellen Sie sicher, dass **Empfänger** aktiviert ist.

    ![][20]

7. Klicken Sie auf **OK** , um das Dialogfeld **Eigenschaften** zu schließen.

1.  Drücken Sie F5, um die **Empfänger** -Projekt aus Visual Studio ausführen und dann warten, bis er die Empfängern für alle Partitionen zu starten.

    ![][21]

2.  Das Projekt **Absender** wird automatisch ausgeführt. Drücken Sie die **EINGABETASTE** im Fenster Konsole, und prüfen Sie die Ereignisse im Fenster Empfänger angezeigt werden.

    ![][22]

Drücken Sie **STRG + C** im Fenster **Absender** in der Absender-Anwendung zu beenden, und drücken Sie dann **EINGABETASTE** im Fenster Empfänger die Anwendung beenden.

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie eine funktionsfähige Anwendung erstellt haben, die ein Ereignis Hub erstellt und sendet und empfängt Daten, können Sie in den folgenden Szenarien verschieben, klicken Sie auf:

- Eine vollständige [Beispiel-Anwendung, die Ereignis Hubs verwendet][].
- Das Beispiel [Skalierung Ereignis mit Ereignis Hubs verarbeiten][] .
- [Ereignis Hubs (Übersicht)][]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Ereignis Prozessor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Ereignis Hubs (Übersicht)]: event-hubs-overview.md
[Beispiel-Anwendung, die Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Ereignis mit Ereignis Hubs Verarbeitung skalieren]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
