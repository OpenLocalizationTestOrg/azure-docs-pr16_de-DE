<properties 
   pageTitle="Benachrichtigen der Benutzer über die Daten von Sensoren oder anderen Systemen empfangen | Microsoft Azure"
   description="Beschreibt das Ereignis Hubs zu verwenden, damit Benutzer Sensordaten benachrichtigt."
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor="" />
<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="notify-users-of-data-received-from-sensors-or-other-systems"></a>Benachrichtigen der Benutzer über die Daten empfangen von Sensoren oder andere Betriebssysteme

Nehmen Sie an, dass Sie eine Anwendung haben, die Daten in Echtzeit überwacht oder erzeugt Berichte nach einem Zeitplan. Wenn Sie die Website Grundlage dieser in Echtzeit Diagramme oder Berichte angezeigt werden betrachten, wird möglicherweise etwas angezeigt, die Aktion erfordert. Was geschieht, wenn Sie auf diese Situationen aufmerksam gemacht, anstatt sich auf erinnern, überprüfen Sie die Website verlassen sein müssen? Stellen Sie sich, dass Ihre ein Licht vergrößern enthält, und Sie müssen sofort wissen, ob die Light abgeschaltet. Eine Möglichkeit besteht darin, erledigen wäre mit einer Lichtsensors in der Gewächshaus, ordnen eine e-Mail-Nachricht gesendet werden, wenn die Light deaktiviert ist.

![][1]

In einem anderen Szenario stellen Sie sich, dass Sie eine Haustiere Einstiegshilfe Einrichtung ausführen, und Sie zu niedrig Inventory Lieferung Ebenen benachrichtigt werden müssen. Beispielsweise können Sie Textnachricht von Ihrem ERP-System gesendet werden, wenn Ihre Lagerbestandsliste der Hundefutter auf eine Ebene kritische gefallen ist anordnen. 

![][2]

Das Problem ist so wichtigen Informationen zu erhalten, wenn Sie bestimmte Bedingungen erfüllt sind, nicht, wenn Sie zum Auschecken eines statischen Berichts navigieren, an. Wenn Sie ein [Ereignis-Hub Azure][] oder [Azure IoT Hub][] für die Daten von Geräten oder Enterprise-Anwendung, z. B. [ANGEHÖREN][]erhalten verwenden, haben Sie mehrere Optionen wie Leads zu bearbeiten. Sie können sie auf einer Website anzeigen, können Sie diese analysieren, können Sie sie speichern und können sie etwas tun Befehle ausgelöst. Um dies zu tun, können Sie leistungsfähige Tools wie [Websites Azure][], [SQL Azure][], [HDInsight][], [Cortana Intelligence Suite][], [IoT Suite][], [Logik Apps][]oder [Azure Benachrichtigung Hubs][]verwenden. Doch manchmal möchten Sie tun lediglich die Daten mit mindestens Verwaltungsaufwand versenden. Damit so, dass Sie mit nur ein wenig Code gehen Sie wie folgt angezeigt werden, haben wir eine neue Stichprobe, [AppToNotifyUsers][]bereitgestellt. Optionen sind e-Mail (SMTP), SMS und Telefon.

## <a name="application-structure"></a>Anwendungsstruktur

Die Anwendung in c# geschrieben ist, und die Infodatei in der Stichprobe enthält sämtliche Informationen, die, den Sie ändern, erstellen und veröffentlichen Sie die Anwendung müssen. Die folgenden Abschnitte enthalten einen detaillierten Überblick über die Funktionsweise der Anwendungs.

Zunächst unter der Voraussetzung, dass Sie wichtige Ereignisse mit einer Azure Ereignis Hub oder IoT Hub gedrückt wird haben. Alle Hub wird führen, solange Sie die Verbindungszeichenfolge kennen und darauf zugreifen.

Wenn Sie noch nicht über ein Ereignis Hub oder IoT Hub verfügen, können Sie einfach ein Blumenbeet Test mit einem Arduino Schild und einer Brombeere Pi, folgen die Anweisungen im Projekt [Verbinden der Punkte](https://github.com/Azure/connectthedots) einrichten. Die Lichtsensors auf das Arduino Schild sendet light Ebenen über der Pi an einen [Azure Ereignis Hub][] (**Ehdevices**) und eine Position [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) verschiebt den Benachrichtigungen mit einem zweiten Ereignis Hub (**Ehalerts**), wenn empfangen light Ebenen unterhalb einer bestimmten Ebene liegen.

Wenn **AppToNotify** gestartet wird, wird eine Konfigurationsdatei (App.config) zum Abrufen der URL und die Anmeldeinformationen für den Empfang von der Benachrichtigungen Ereignis-Hub gelesen. Es löst dann einen Prozess zum Überwachen der kontinuierlich, dass Ereignis Hub für jede Nachricht, die über – stammen, solange Sie haben die URL für das Ereignis Hub oder IoT Hub und gültiger Anmeldeinformationen zugreifen können, die diesem Ereignis Hubs Reader Code wird kontinuierlich lesen Sie was kommt aus. Beim Start liest die Anwendung auch die URL und die Anmeldeinformationen für die zu verwendende messaging-Dienst (e-Mail, SMS, Telefonhörer), und der Name/Adresse des Absenders und eine Liste der Empfänger.

Nachdem der Ereignis Hub Monitor eine Nachricht erkennt, löst ein Prozesses, das sendet die Nachricht mithilfe der Methode, die in der Konfigurationsdatei angegeben wird. Beachten Sie, dass es sendet jeder Nachricht, die das Programm erkennt. Wenn Sie festlegen, dass den Monitor verweisen auf ein Ereignis Hub, zehn Nachrichten pro Sekunde empfängt, der Absender senden zehn Nachrichten pro Sekunde – zehn e-Mails pro Sekunde, zehn SMS-Nachrichten pro Sekunde, zehn Telefonanrufe pro Sekunde. Aus diesem Grund Vergewissern Sie sich, dass Sie ein Ereignis Hub überwachen, die nur die Benachrichtigungen empfängt, die sich nicht auf ein Ereignis Hub gesendet werden, die alle unformatierten Daten aus Ihrem Sensoren oder Applikationen empfängt benötigen.

## <a name="applicability"></a>Anwendungsmöglichkeiten

Der Code in diesem Beispiel zeigt nur das Ereignis Hubs überwachen und wie externe messaging-Dienste aufgerufen, wenn Sie diese Funktion an Ihrer Anwendung hinzufügen möchten. Beachten Sie, dass diese Lösung eine DIY, nur Entwicklertools konzentrieren Beispiel ist. Es wird nicht Enterprise-Anforderungen behandelt, wie Redundanz, Fail-Over, starten Sie erneut nach Fehler usw.. Probieren Sie für weitere Lösungen für eine umfassende und Fertigung Folgendes ein:

- Verwenden Verbinder oder Pushbenachrichtigungen mithilfe des Diensts für [Azure Logik Apps](../app-service-logic/app-service-logic-connectors-list.md) .
- Verwenden von [Azure Benachrichtigung Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx), wie den Blog [übertragen Pushbenachrichtigungen Millionen von mobilen Geräten mit Azure Benachrichtigung Hubs](http://weblogs.asp.net/scottgu/broadcast-push-notifications-to-millions-of-mobile-devices-using-windows-azure-notification-hubs)beschrieben. 

## <a name="next-steps"></a>Nächste Schritte

Es ist einfach eine einfache Benachrichtigungsdienst zu erstellen, sendet e-Mails oder Textnachrichten an die Empfänger, oder ruft diese Relay Daten von einem Ereignis Hub oder IoT Hub empfangen. Um die Lösung, um Daten erhalten, indem Sie diese Hubs basierende Benutzer benachrichtigen bereitstellen zu können, finden Sie auf [AppToNotifyUsers][].

Weitere Informationen zu diesen Hubs finden Sie unter den folgenden Artikeln:

- [Azure Ereignis Hubs]
- [Azure IoT-Hub]
- Erste Schritte mit einer [Veranstaltung Hubs Lernprogramm].
- Eine vollständige [Beispiel-Anwendung, die Ereignis Hubs verwendet].

Um die Lösung, um Benutzer basierend auf Daten, die vom diese Hubs benachrichtigen bereitstellen zu können, finden Sie auf:

- [AppToNotifyUsers][]

[Ereignis Hubs Lernprogramm]: event-hubs-csharp-ephcs-getstarted.md
[Azure IoT-Hub]: https://azure.microsoft.com/services/iot-hub/
[Azure Ereignis Hubs]: https://azure.microsoft.com/services/event-hubs/
[Azure Ereignis Hub]: https://azure.microsoft.com/services/event-hubs/
[Beispiel-Anwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[AppToNotifyUsers]: https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications
[Dynamics AX]: http://www.microsoft.com/dynamics/erp-ax-overview.aspx
[Azure-Websites]: https://azure.microsoft.com/services/app-service/web/
[SQL Azure]: https://azure.microsoft.com/services/sql-database/
[HDInsight]: https://azure.microsoft.com/services/hdinsight/
[Cortana Intelligence Suite]: http://www.microsoft.com/server-cloud/cortana-analytics-suite/Overview.aspx?WT.srch=1&WT.mc_ID=SEM_lLFwOJm3&bknode=BlueKai
[IoT Suite]: https://azure.microsoft.com/solutions/iot-suite/
[Logik Apps]: https://azure.microsoft.com/services/app-service/logic/
[Azure Benachrichtigung Hubs]: https://azure.microsoft.com/services/notification-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/services/stream-analytics/
 
[1]: ./media/event-hubs-sensors-notify-users/event-hubs-sensor-alert.png
[2]: ./media/event-hubs-sensors-notify-users/event-hubs-erp-alert.png