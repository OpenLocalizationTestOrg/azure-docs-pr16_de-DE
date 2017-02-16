<properties
    pageTitle="Hinzufügen den Serie Trigger Logik apps | Microsoft Azure"
    description="Übersicht über die Serie Trigger und Einsatzbreite mit einer app Azure Logik."
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-recurrence-trigger"></a>Erste Schritte mit der Serie trigger

Mithilfe des Serie Triggers können Sie leistungsfähige Workflows in der Cloud erstellen.

Beispielsweise können Sie:

- Planen eines Workflows, eine gespeicherte SQL-Prozedur jeden Tag auszuführen.
- Per e-Mail eine Zusammenfassung aller Tweets innerhalb der letzten Woche zu einer bestimmten hashtags ein.

Um anzufangen den Serie Trigger in einer app Logik verwenden, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-a-recurrence-trigger"></a>Verwenden eines Triggers Serie

Ein Trigger ist ein Ereignis, die verwendet werden kann, um den Workflow zu starten, der in einer app Logik definiert ist. [Erfahren Sie mehr über Trigger](connectors-overview.md).

Hier ist eine Beispiel-Abfolge von Informationen zum Einrichten eines Triggers Serie in einer app Logik:

1. Fügen Sie der **Serie** Trigger als der erste Schritt in einer app Logik hinzu.
2. Füllen Sie die Parameter für das Serienintervall aus.

Die app Logik wird jetzt ein Testlaufs nach jedem Intervall Zeit gestartet.

![HTTP-trigger](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Auslösen von details

Der Serie Trigger weist die folgenden Eigenschaften, die Sie konfigurieren können.

Es wird eine app Logik nach dem ein angegebenes Zeitintervall ausgelöst.
A * bedeutet, dass es ein Feld erforderlich ist.

|Anzeigename|Eigenschaftsname|Beschreibung|
|---|---|---|
|Häufigkeit *|Häufigkeit|The unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.|
|Intervall *|Intervall|Das Zeitintervall die angegebenen Häufigkeit für die Häufigkeit.|
|Zeitzone|Zeitzone|Wenn Sie eine Anfangszeit ohne einen UTC-Offset bereitgestellt wird, wird dieser Zeitzone verwendet werden.|
|Startzeit|Startzeit|Die Startzeit im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).|
<br>


## <a name="next-steps"></a>Nächste Schritte

Probieren Sie die Plattform und [Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md). Sie können der verfügbaren Connectors Logik Apps vertraut machen, indem Sie die [Liste der APIs](apis-list.md).
