<properties
    pageTitle="Verwalten von experimentieren Iterationen in Computer Learning Studio | Microsoft Azure"
    description="So experimentieren Iterationen in Azure maschinellen Learning Studio verwalten"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="garye"/>

# <a name="manage-experiment-iterations-in-azure-machine-learning-studio"></a>Verwalten von experimentieren Iterationen in Azure maschinellen Learning Studio

Entwickeln eines Analysemodells Vorhersage ist eine iterative Prozess - während Sie die verschiedenen Funktionen und Parameter der experimentieren ändern, Zusammenführung der Ergebnisse vor, bis Sie zufrieden sind, dass Sie ein Modell ausgebildetem, effektiven haben. -Taste, um dieses Verfahren wird die verschiedenen Iterationen experimentieren Parameter und Konfigurationen nachverfolgen.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Überprüfen Sie vorherige Strecken von Ihrer Versuche zu einem beliebigen Zeitpunkt um Herausforderung, wieder besuchen, und schließlich bestätigen oder vorherige Annahmen optimieren. Beim Ausführen einer experimentieren zeichnet maschinellen Learning Studio einem Verlauf ausführen, einschließlich Dataset, Modul und Port Verbindungen und Parameter aus. Verlauf erfasst werden auch Ergebnisse zur Laufzeit wie starten und beenden Zeiten, Nachrichten protokollieren und Ausführungsstatus. Sie können sich eine der folgenden ausgeführt wieder zu einem beliebigen Zeitpunkt überprüfen Sie die Informationen experimentieren und Zwischenergebnisse ansehen. In eine frühere Abfolge von Ihrem experimentieren können auch auf Ihrem Pfad zum Erstellen von einfachen, komplexen oder sogar Ensemble Modellierung Lösungen in eine neue Phase der Abfrage und Suche starten.

> [AZURE.NOTE] Wenn Sie eine frühere Ausführung ein Versuch anzeigen, die Version der Versuch ist gesperrt, und kann nicht bearbeitet werden. Sie können jedoch eine Kopie davon speichern, indem Sie auf **Speichern unter** , und einen neuen Namen für die Kopie bereitstellen. Maschinelle Learning Studio öffnet die neue Kopie, die Sie dann bearbeiten und ausführen können. Diese Kopie Ihrer experimentieren steht in der Liste **Versuche** sowie alle anderen Versuche.

## <a name="viewing-the-prior-run"></a>Anzeigen der vorherigen ausführen

Bei einem Versuch geöffnet, in dem Sie bereits mindestens einmal ausgeführt haben können Sie die vorherige Abfolge von den Versuch anzeigen, indem Sie im Eigenschaftenbereich auf **Vorherige ausführen** .

Angenommen, Sie einem Versuch erstellen und Ausführen von Versionen davon am 11:23, 11:42 und 11:55. Wenn Sie die letzte Ausführung von den Versuch (11:55) öffnen und auf **Vorherige ausführen**, wird die Version, die Sie bei 11:42 ausgeführt haben geöffnet.

## <a name="viewing-the-run-history"></a>Anzeigen des Verlaufs ausführen

Sie können alle vorherigen ausgeführt ein Versuch, indem Sie in einem geöffneten Versuch **Ausführen Historie anzeigen** auf anzeigen.

Angenommen, Sie erstellen eine Experimentieren mit der [Lineare Regression] [ linear-regression] Modul und beobachten Sie das Verhalten beim Ändern des Werts **Learning Zins** auf Ihrer Ergebnissen experimentieren möchten. Sie führen den Versuch mehrmals mit anderen Werten für diesen Parameter, wie folgt aus:

| Learning Zins-Wert | Führen Sie die Startzeit |
| ------------------- | -------------- |
| 0,1 auf. | 9/11/2014 4:18:58 Uhr
| 0,2 | 9/11/2014 4:24:33 pm
| 0.4 | 9/11/2014 4:28:36 pm
| 0,5 | 9/11/2014 4:33:31 pm

Wenn Sie **Ausführen Historie anzeigen**klicken, wird eine Liste der alle diese ausgeführt:

![Beispiel für Verlauf ausführen][runhistory]

Klicken Sie auf eine der folgenden führt eine Momentaufnahme der den Versuch gleichzeitig anzeigen, die Sie es ausgeführt haben. Die Konfiguration, Parameterwerte, Kommentare und Ergebnisse werden alle beibehalten, um eine vollständige Aufzeichnung dieser Abfolge von Ihrem experimentieren zu erhalten.

> [AZURE.TIP] Um den Versuch der Iterationen zu dokumentieren, Sie können den Titel jedes Mal, wenn Sie es ausführen ändern, Sie können die **Zusammenfassung** der Versuch im Bereich Eigenschaften aktualisieren und können Sie hinzufügen oder Aktualisieren von Kommentaren auf einzelne Module Ihre Änderungen aufzeichnen. Die Titel, Zusammenfassung und Modul Kommentare werden bei jedem Ausführen von den Versuch gespeichert.

Die Liste der Versuche auf der Registerkarte **Versuche** in Computer Learning Studio zeigt immer die neueste Version von einem Versuch. Wenn Sie eine vorherige öffnen können von den Versuch (mit **Früheren ausführen** oder **Ausführen Historie anzeigen**) führen, Sie zurück zum die Entwurfsversion indem **Ausführen Historie anzeigen** und Auswählen der Iteration mit dem **Status** der **bearbeitbar**.

## <a name="iterating-on-a-previous-run"></a>Bei einer vorherigen Ausführung durchlaufen

Wenn Sie klicken Sie auf **Vorherige ausführen** oder **Ausführen Historie anzeigen** , und öffnen eine vorherige ausführen, können Sie eine fertig experimentieren im schreibgeschützten Modus anzeigen.

Wenn Sie eine Iteration des Ihrer experimentieren, wie Sie es für einer früheren Ausführung konfiguriert angefangen beginnen möchten, können Sie hierzu ausführen öffnen, und dann auf **Speichern unter**. Dies erstellt eine neue experimentieren, mit einem neuen Titel, eine leere Verlauf, ausführen, und führen Sie die Komponenten und Parameterwerte des vorhergehenden. Diesem neuen Versuch der Computer Learning Studio-Startseite die Registerkarte **WEINBAUVERSUCHE** aufgeführt ist, und Sie ändern und ausführen, einen neuen ausführen Verlauf für diese Iteration von Ihrem experimentieren initiieren können. 

Nehmen Sie beispielsweise an, dass Sie den Versuch, führen Sie im vorherigen Abschnitt dargestellt Verlauf verfügen. Beachten Sie, was passiert, wenn Sie den **Zins Learning** -Parameter auf 0.4 festlegen, und versuchen Sie unterschiedliche Werte für die **Anzahl der Schulung Epochs** Parameter werden soll.


1. Klicken Sie auf **Ansicht ausführen Verlauf** , und öffnen Sie die Iteration des den Versuch, den Sie bei 4:28:36 pm ausgeführt haben (in der Sie den Parameterwert auf 0.4 festgelegt).

2. Klicken Sie auf **Speichern unter**.

3. Geben Sie einen neuen Titel ein, und klicken Sie auf das Häkchen **OK** . Eine neue Instanz von den Versuch wird erstellt.

4. Ändern des **Anzahl der Schulung Epochs** Parameters.

5. Klicken Sie auf **Ausführen**.

Sie können jetzt weiterhin ändern, und führen Sie diese Version von Ihrer experimentieren, erstellen einen neuen ausführen Verlauf, um Ihre Arbeit aufzuzeichnen.


<!-- Images -->
[runhistory]:./media/machine-learning-manage-experiment-iterations/viewrunhistory.jpg


<!-- Module References -->
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
