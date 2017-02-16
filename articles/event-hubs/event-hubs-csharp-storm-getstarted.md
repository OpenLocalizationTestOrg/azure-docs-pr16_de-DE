<properties
    pageTitle="Erste Schritte mit Ereignis Hubs in c# mit Apache Storm | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm den Einstieg in Azure Ereignis Hubs; Senden von Ereignissen in c# und diese in einem Cluster Apache Storm empfangen."
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
    ms.topic="article" 
    ms.date="09/06/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Erste Schritte mit Ereignis Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Einführung

Ereignis Hubs ist ein hochgradig skalierbare Aufnahme-System, die Aufnahme Millionen können von Ereignissen pro Sekunde, aktivieren die Anwendung zum Verarbeiten und Analysieren der großer Datenmengen von Ihrer verbundenen Geräte und Clientanwendungen gefertigt. Nachdem in Ereignis Hubs erfasst werden, können Sie transformieren und Speichern von Daten mithilfe eines beliebigen in Echtzeit Analytics-Anbieter oder Speicher Cluster.

Weitere Informationen finden Sie unter dem [Ereignis Hubs Übersicht].

In diesem Lernprogramm erfahren Sie, wie Nachrichten an ein Ereignis-Hub verwenden eine Console-Anwendung in C#-Aufnahme, und wie diese parallel mit Apache Storm abgerufen werden.

Für die Durchführung dieses Lernprogramms benötigen Sie Folgendes:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Eine Java-Entwicklungsumgebung so konfiguriert, dass [Maven](http://maven.apache.org/)ausführen. In diesem Lernprogramm wird davon ["Ellipse"](https://www.eclipse.org/) ausgegangen.

+ Ein aktives Azure-Konto. <br/>Wenn Sie kein Konto haben, können Sie ein kostenloses Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure kostenlose Testversion</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Jetzt sind Sie bereit sind, der die Anwendung läuft.

1.  Führen Sie die **LogTopology** -Klasse aus "Ellipse" und dann auf die Empfängern für alle Partitionen starten warten.

2.  Führen Sie das Projekt **Absender** , und drücken Sie die **EINGABETASTE** im Fenster Konsole finden Sie unter den Ereignissen im Fenster Empfänger angezeigt werden.

    ![][22]

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie eine funktionsfähige Anwendung erstellt haben, die ein Ereignis Hub erstellt und sendet und empfängt Daten, können Sie in den folgenden Szenarien verschieben, klicken Sie auf:

- Eine vollständige [Beispiel-Anwendung, die Ereignis Hubs verwendet][].
- Das Beispiel [Skalierung Ereignis mit Ereignis Hubs verarbeiten][] .

<!-- Images. -->
[22]: ./media/event-hubs-csharp-storm-getstarted/receive-storm1.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Ereignis Hubs (Übersicht)]: event-hubs-overview.md
[Beispiel-Anwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Ereignis mit Ereignis Hubs Verarbeitung skalieren]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 