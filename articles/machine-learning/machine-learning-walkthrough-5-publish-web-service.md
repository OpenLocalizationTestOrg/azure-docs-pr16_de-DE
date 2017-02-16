<properties
    pageTitle="Schritt 5: Bereitstellen von den Computer Learning Webdienst | Microsoft Azure"
    description="Schritt 5 für die Entwicklung eine exemplarische Vorgehensweise zur Vorhersage Lösung: Bereitstellen eines Vorhersage experimentieren in Computer Learning Studio als Webdienst."
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
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-web-service"></a>Exemplarische Vorgehensweise Schritt 5: Bereitstellen von Azure maschinellen Learning-Webdienst

Dies ist das fünfte der Anleitung, [Entwicklung einer Lösung Vorhersageanalytik Azure Computer interessante](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Erstellen eines Arbeitsbereichs Computer-Schulung](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Hochladen von vorhandenen Daten](machine-learning-walkthrough-2-upload-data.md)
3.  [Erstellen einer neuen experimentieren.](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Schulen Sie und ausgewertet werden Sie die Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  **Bereitstellen des Webdiensts**
6.  [Zugriff auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

----------

Damit andere Personen können die Möglichkeit, das Modell Vorhersage verwenden, die, das wir in dieser Anleitung erfahren entwickelt haben, stellen wir es als Webdienst auf Azure.

Bis zu diesem Zeitpunkt haben wir Experimentieren mit unserem Modell Schulung. Aber bereitgestellte Dienst ist nicht mehr vertraut Schulung tun – es generiert Vorhersagen, indem Sie die Eingabe des Benutzers basierend auf unser Modell bewerten. Damit wir nicht auf vorbereitende Schritte Plänen zu diesem Versuch aus einer ***Schulung*** Versuch zum Konvertieren einer ***Vorhersage*** experimentieren. 

Dies umfasst zwei Schritte:  

1. Konvertieren der *Schulung experimentieren* , die wir erstellt haben, in einer *Vorhersage experimentieren*
2. Bereitstellen der Vorhersage experimentieren als Webdienst

Jedoch zuerst müssen wir diesem Versuch etwas nach unten zu kürzen. Wir haben derzeit zwei verschiedene Modellen den Versuch, aber wir nur ein Modell soll, wenn wir dies als Webdienst bereitstellen.  

Angenommen, wir entschieden haben, dass das Strukturmodell stärkere besser Modell verwendet wurde. Damit erstes tun ist entfernen die [Zwei Class Support Vektor Computer] [ two-class-support-vector-machine] Modul und Module ab, die für diese Schulung verwendet wurden. Möglicherweise möchten eine Kopie der Versuch zuerst vornehmen, indem Sie auf **Speichern unter** am unteren Rand des Zeichenbereichs experimentieren.

Wir müssen folgende Module zu löschen:  

- [Zwei Unterstützung Vektor Computer][two-class-support-vector-machine]
- [Schulen von Modell] [ train-model] und [Punktzahl Modell] [ score-model] Module, die darauf verbunden wurden
- [Normalisieren von Daten] [ normalize-data] (beide)
- [Auswerten Modells][evaluate-model]

Wählen Sie das Modul, und drücken Sie die ENTF-Taste, oder mit der rechten Maustaste im Moduls, und wählen Sie **Löschen**aus.

Jetzt wir zum Bereitstellen dieses Modell mithilfe der [Beiden-Klasse verstärkt Entscheidungsstruktur]bereit sind[two-class-boosted-decision-tree].

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Konvertieren Sie die Schulung experimentieren in einer Vorhersage experimentieren

Konvertieren in einer Vorhersage experimentieren umfasst drei Schritte:

1. Speichern Sie das Modell, das wir haben gelernt, und Ersetzen Sie unseren Schulungsmodule
2. Kürzen der experimentieren, um Module zu entfernen, die nur für Schulung benötigt wurden
3. Definieren Sie, wo der Webdienst Eingabe akzeptiert und Stelle, an der die Ausgabe generiert wird

Glücklicherweise können alle drei Schritte ausgeführt werden, indem Sie auf **Festlegen von Webdienst** am unteren Rand des Zeichenbereichs experimentieren (auswählen die Option **Vorhersage Webdienst** ).

Wenn Sie **Festlegen von Webdienst**klicken, werden verschiedene Aktionen durchgeführt:

- Geschulte Modell wird in der Palette Modul, das links neben den Zeichenbereich experimentieren als einzelne **Ausgebildeten Modell** Modul gespeichert (Sie können unter **Ausgebildeten Modelle**es Auffinden).
- Module, die für die Schulung verwendet wurden, werden entfernt. Insbesondere:
  - [Zwei-Klasse stärkere Entscheidungsstruktur][two-class-boosted-decision-tree]
  - [Zug Modell][train-model]
  - [Aufteilen von Daten][split]
  - Das zweite [R Skript ausführen] [ execute-r-script] Modul, das zum von Testdaten verwendet wurde
- Das gespeicherte ausgebildete Modell wird wieder in den Versuch hinzugefügt.
- **Web Service Eingabe-** und **Web Service** -Module werden hinzugefügt.

> [AZURE.NOTE] Der Versuch wurde in zwei Teilen unter Registerkarten, die am oberen Rand des Zeichenbereichs experimentieren hinzugefügt wurden gespeichert: der ursprünglichen Schulung Versuch ist unter der Registerkarte **Schulung experimentieren**und der neu erstellte Vorhersage Versuch unter **prädiktive experimentieren**.

Wir müssen einen weiteren Schritt mit diesem bestimmten Versuch durchführen.
Wir hinzugefügten zwei [Ausführen R Skript] [ execute-r-script] Module eine Funktion Gewichtung zu den Daten bereitstellen für Schulung und testen. Wir erforderlich nicht, die im Modell abgeschlossen.

Maschinelle Learning Studio entfernt eine [Ausführen R Skript] [ execute-r-script] Modul, wenn es sich um die [Teilung] entfernt[ split] Modul. Nun können wir jetzt das andere entfernen und [Metadaten Editor] herstellen[ metadata-editor] direkt auf [Punktzahl Modell][score-model].    

Unsere experimentieren sollte nun wie folgt aussehen:  

![Bewerten des Modells ausgebildeten][4]  

> [AZURE.NOTE] Sie vielleicht, warum wir UCI Deutsch Kreditkartendaten Dataset den Vorhersage Versuch nach links. Der Dienst ist zum Verwenden des Benutzers, nicht im ursprünglichen Dataset, also Warum sollten Sie das ursprüngliche Dataset im Modell vertraut?
>
>Es gilt, dass der Dienst die Kreditkarte Originaldaten nicht erforderlich ist. Das Schema für die Daten, die Informationen, wie z. B. wie viele Spalten enthält, es gibt und welche Spalten als numerische benötigt aber. Diese Schemainformationen ist erforderlich, die Daten des Benutzers bei der Interpretation. Wir lassen diese Komponenten verbunden, sodass das Punktzahl Modul Schema des Datasets hat, wenn der Dienst ausgeführt wird. Die Daten werden nicht verwendet, nur das Schema.  

Ausführen der letzten Mal experimentieren (klicken Sie auf **Ausführen**.) Wenn Sie überprüfen, ob das Modell noch funktioniert möchten, klicken Sie auf die Ausgabe der [Punktzahl Modell] [ score-model] Modul und select- **Ergebnisse anzuzeigen**. Sehen Sie, dass die Originaldaten angezeigt werden, zusammen mit der Kreditkarte Risikowert ("bewertet Etiketten") und der Punktzahl Wahrscheinlichkeitswert ("bewertet Wahrscheinlichkeiten".) 

## <a name="deploy-the-web-service"></a>Bereitstellen des Webdiensts

Sie können den Versuch als einen klassischen Webdienst oder einen neuen Webdienst basierend auf Azure Ressourcenmanager bereitstellen.

### <a name="deploy-as-a-classic-web-service"></a>Als klassische Webdienst bereitstellen ###

Um einen klassischen Webdienst abgeleitet von unserem experimentieren bereitstellen zu können, klicken Sie unter den Zeichenbereich **Webdienst bereitstellen** auf, und wählen Sie **Webdienst bereitstellen [klassischen]**. Maschinelle Learning Studio den Versuch als Webdienst bereitstellt und gelangen Sie zum Dashboard für diesen Webdienst. Hier können Sie zurückkehren zukommen (**Momentaufnahme anzeigen** oder **neueste anzeigen**) und führen Sie einen einfachen Test des Webdiensts (finden Sie unter **Testen der Webdienst** nachfolgend dargestellt). Es gibt auch hier Informationen zum Erstellen von Anwendungen, die den Webdienst (mehr dazu im nächsten Schritt diese Anleitung) zugreifen können.

![Web Service-dashboard][6]

Sie können den Dienst konfigurieren, indem Sie auf die Registerkarte **Konfiguration** . Hier können Sie den Namen (es hat den Namen experimentieren standardmäßig erteilt) bearbeiten und probieren Sie es mit eine Beschreibung. Sie können auch weitere geeignet Etiketten für die Eingangs- und Spalten verleihen.  

![Webdienst konfigurieren][5]  

### <a name="deploy-as-a-new-web-service"></a>Als neue Webdienst bereitstellen

Um einen neuen Webdienst abgeleitet von unserem experimentieren bereitstellen möchten, klicken Sie auf **Webdienst bereitstellen** unter dem Zeichenbereich und **Webdienst bereitstellen [New]**. Maschinelle Learning Studio weiterleitet Sie zur Seite Azure maschinellen Learning Web Services experimentieren bereitstellen.

Geben Sie einen Namen für den Webdienst, und wählen Sie einen Plan Preisgestaltung. Wenn Sie einen vorhandenen Preisgestaltung Plan, dass Sie ihn auswählen können haben, müssen andernfalls Sie einen neuen Preisplan für den Dienst erstellen. 

1.  Wählen Sie in der Dropdownliste den **Preisplan** einen vorhandenen Plan aus, oder wählen Sie die Option **neuen Plan auswählen** .
2.  Geben Sie unter **Plan Name**einen Namen, der den Plan auf Ihrer Rechnung identifiziert.
3.  Wählen Sie eine der **Monatlichen planen Ebenen**. Plan Ebenen Standard für die Pläne für Ihre Standardregion und Ihre Webdienst wird die Region bereitgestellt.

Klicken Sie auf **Bereitstellen** und die **Schnellstart** -Seite für Ihre Web-Dienst wird geöffnet.

Sie können den Dienst konfigurieren, indem Sie auf die Menüoption **Konfigurieren** . Hier können Sie den Namen (es hat den Namen experimentieren standardmäßig erteilt) bearbeiten und probieren Sie es mit eine Beschreibung. 

Klicken Sie zum Testen der Web-Dienst auswählen, klicken Sie auf die Menüoption **Testen** (siehe unten **testen den Webdienst** ). Informationen zum Erstellen von Anwendungen, die den Webdienst zugreifen können, klicken Sie auf die Menüoption **verbrauchen** (mehr dazu im nächsten Schritt diese Anleitung).

> [AZURE.TIP] Nachdem Sie es bereitgestellt haben, können Sie den Webdienst aktualisieren. Beispielsweise wenn Sie Ihr Modell ändern möchten, bearbeiten Sie die Schulung experimentieren, gibt die Modellparameter und klicken Sie auf **Webdienst bereitstellen**. Wählen Sie dann **Webdienst bereitstellen [klassischen]** oder **[New] Webdienst bereitstellen**. Wenn Sie den Versuch erneut bereitstellen, wird den Webdienst jetzt mit aktualisierten Modell ersetzt.  

## <a name="test-the-web-service"></a>Testen des Webdiensts

### <a name="test-a-classic-web-service"></a>Einen klassischen Webdienst testen

Sie können den Dienst maschinellen Learning Studio oder im Portal Azure maschinellen Learning-Webdiensten testen. Den Vorteil, dass Sie aktivieren hat im Portal Azure maschinellen Learning-Webdiensten testen 

**Testen Sie in Computer Learning Studio**

Klicken Sie auf der Seite **DASHBOARD** unter **Standard-Endpunkt**auf die Schaltfläche **Testen** . Ein Dialogfeld wird eingeblendet und für die Eingabedaten für den Dienst, bitten Sie. Hierbei handelt es sich um die gleichen Spalten, die in der ursprünglichen Deutsch Kreditkarte Risiko Dataset dargestellt.  

Geben Sie eine Reihe von Daten, und klicken Sie dann auf **OK**. 

**Im Portal Azure maschinellen Learning-Webdiensten testen**

Klicken Sie auf der Seite **DASHBOARD** unter **Standard-Endpunkt**auf die Vorschau **Testen** des Links. Die Testseite im Portal Azure maschinellen Learning-Webdiensten für den Webdienst-Endpunkt wird geöffnet, und Sie gefragt werden, für die Eingabedaten für den Dienst. Hierbei handelt es sich um die gleichen Spalten, die in der ursprünglichen Deutsch Kreditkarte Risiko Dataset dargestellt.

Klicken Sie auf **Anforderung / Antwort-Tests zurück**. 

Im Webdienst, die Daten eingibt, über das **Web Service Eingabesprache** Modul, durch die [Metadaten Editor] [ metadata-editor] Modul, und das [Modell Punktzahl] [ score-model] Modul, in dem es bewertet wird. Die Ergebnisse werden dann aus dem Webdienst in der **Web-Dienst Ausgabe**ausgegeben.

**Testen Sie einen neuen Webdienst**

Klicken Sie im Portal Azure maschinellen Learning Web Services **Test** am oberen Rand der Seite auf. **Die Testseite** wird geöffnet, und können Sie Daten für den Dienst eingeben. Die angezeigten Eingabefelder entsprechen den Spalten, die in der ursprünglichen Deutsch Kreditkarte Risiko Dataset dargestellt. 

Geben Sie eine Reihe von Daten, und klicken Sie dann auf **Anforderung / Antwort-Tests zurück**.

Die Ergebnisse der Prüfung werden auf der rechten Seite der Seite in der Ausgabespalte angezeigt. 

Wenn Sie im Portal Azure maschinellen Learning-Webdiensten zu testen, können Sie Beispieldaten aktivieren, die Sie zum Testen des Anforderung / Antwort-Diensts verwenden können. Wenn Sie den Webdienst in Computer Learning Studio erstellt, die Beispieldaten aus den Daten der verwendeten zum Schulen von Modell stammt.

> [AZURE.TIP] Wir haben die Vorhersage experimentieren, die so konfiguriert ist, wie die gesamte Ergebnisse aus dem [Modell Punktzahl] [ score-model] Modul zurückgegeben werden. Dies umfasst die Eingabedaten plus der Risikowert Kreditkarte und die Wahrscheinlichkeit bewertet. Wenn Sie unterschiedliche - zurückkehren möchten beispielsweise nur die Kreditkarte enttäuschen Value - und dann eine [Project-Spalten] eingefügt werden könnten[ project-columns] Modul zwischen [Punktzahl Modell] [ score-model] und die **Ausgabe der Web-Dienst** um Spalten zu entfernen, nicht Webdienst gewünschten zurück. 

## <a name="manage-the-web-service"></a>Verwalten des Webdiensts

**Verwalten von einem Webdienst klassischen**

Nachdem Sie Ihre klassischen Webdienst bereitgestellt haben, können Sie es aus dem [Azure klassischen Portal](https://manage.windowsazure.com)verwalten.

1. Melden Sie sich zum [Azure klassischen Portal](https://manage.windowsazure.com)aus.
2. Klicken Sie im Bereich Microsoft Azure Services auf **Computer Schulung**.
3. Klicken Sie auf dem Arbeitsbereich.
4. Klicken Sie auf der Registerkarte **Webdienste** .
5. Klicken Sie auf den Webdienst, den wir erstellt haben.
6. Klicken Sie auf den Endpunkt "Standard".

Von hier aus können Sie beispielsweise überwachen, wie der Webdienst schlagen ist ausführen und Tabellenerstellungsabfrage Leistung Optimierungen vor durch ändern, wie viele gleichzeitige Dienst Anrufe verarbeitet werden können.
Sie können auch Ihren Webdienst in der Azure Marketplace veröffentlichen.

Weitere Informationen hierzu finden Sie unter:

- [Erstellen von Endpunkten](machine-learning-create-endpoint.md)
- [Anpassungsbereich für Webdienst](machine-learning-scaling-webservice.md)
- [Veröffentlichen von Azure maschinellen Learning-Webdiensts zu Azure Marketplace](machine-learning-publish-web-service-to-azure-marketplace.md)

**Verwalten von einem Webdienst im Portal Azure maschinellen Learning-Webdiensten**

Nachdem Sie Ihren Webdienst, Classic oder neu bereitgestellt haben, können Sie es aus dem [Azure maschinellen Learning Web Services-Portal](https://servics.azureml.net)verwalten.

Um die Leistung des Web Service zu überwachen:

1. Melden Sie sich zum [Azure maschinellen Learning Web Services-Portal](https://servics.azureml.net)aus.
2. Klicken Sie auf **Webdienste**.
3. Klicken Sie auf den Webdienst.
4. Klicken Sie auf dem **Dashboard**.

----------

**Als Nächstes: [greifen Sie auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)**

[1]: ./media/machine-learning-walkthrough-5-publish-web-service/publish1.png
[2]: ./media/machine-learning-walkthrough-5-publish-web-service/publish2.png
[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/