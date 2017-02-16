<properties
    pageTitle="Schritt 3: Erstellen einer neuen Computer Learning experimentieren | Microsoft Azure"
    description="Schritt 3 von der Entwicklung einer Lösung Vorhersage Exemplarische Vorgehensweise: Erstellen einer neuen Schulung experimentieren in Azure maschinellen Learning Studio."
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


# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Exemplarische Vorgehensweise Schritt 3: Erstellen einer neuen Azure maschinellen Learning experimentieren

Dies ist der dritte Schritt die Anleitung, [Entwicklung einer Lösung Vorhersageanalytik Azure Computer interessante](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Erstellen eines Arbeitsbereichs Computer-Schulung](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Hochladen von vorhandenen Daten](machine-learning-walkthrough-2-upload-data.md)
3.  **Erstellen einer neuen experimentieren.**
4.  [Schulen Sie und ausgewertet werden Sie die Modelle](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Bereitstellen des Webdiensts](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Zugriff auf den Webdienst](machine-learning-walkthrough-6-access-web-service.md)

----------

Im nächsten Schritt in dieser exemplarische Vorgehensweise wird ein Versuch in Computer Learning Studio zu erstellen, die das Dataset verwendet, die, das wir hochgeladen.  

1.  Klicken Sie auf in Studio **+ neu** am unteren Rand des Fensters.
2.  Wählen Sie **EXPERIMENTIEREN**aus, und wählen Sie dann auf "Leere experimentieren". Wählen Sie den Standardnamen experimentieren am oberen Rand des Bereichs, und benennen Sie sie in einen aussagekräftigeren Namen

    > [AZURE.TIP] Es empfiehlt sich **Zusammenfassung** und **eine Beschreibung** für den Versuch im Bereich **Eigenschaften** ausfüllen. Diese Eigenschaften bieten Ihnen die Möglichkeit, den Versuch Dokument, damit alle Personen, die später darauf sieht aus Ihrer Ziele und Methoden verstehen werden.

3.  In der Palette Modul, das links neben den Versuch Zeichnungsbereich, erweitern Sie **Datasets gespeichert**.
4.  Suchen nach dem unter **Meine Datasets** erstellten Dataset aus, und ziehen Sie ihn auf den Zeichenbereich. Finden Sie das Dataset auch durch Eingeben von den Namen in **das Suchfeld oben der Palette aus** .  

##<a name="prepare-the-data"></a>Vorbereiten der Daten
Sie können die ersten 100 Zeilen der Daten und einige statistische Daten für das gesamte Dataset anzeigen, indem Sie auf den Ausgang des Datasets (den kleinen Kreis unten) und **visualisieren**.  

Da die Datendatei mit Spaltenüberschriften stammen haben, hat Studio generische Überschriften *(SP1, SP2 usw.)*angegeben. Gute Überschriften nicht wichtig für die Erstellung eines Modells, allerdings werden diese vereinfachen das Arbeiten mit den Daten in den Versuch. Auch wenn wir die Daten dieses Modell in einem Webdienst veröffentlichen, die Überschriften können die Spalten für den Benutzer, der den Dienst identifiziert werden.  

Wir können die [Metadaten bearbeiten] mit Spaltenüberschriften hinzufügen[ edit-metadata] Modul.
Sie verwenden die [Metadaten bearbeiten] [ edit-metadata] Modul zum Ändern von Metadaten ein Dataset zugeordnet. In diesem Fall können sie weitere Anzeigenamen für Spaltenüberschriften bereitstellen. 

Verwendung von [Metadaten bearbeiten][edit-metadata], Sie zuerst angeben, welche Spalten zu ändern (in diesem Fall aller Folien.) Als Nächstes, geben Sie die Aktion ausgeführt werden, klicken Sie auf diese Spalten (in diesem Fall Spaltenüberschriften geändert.)

1.  Geben Sie in der Palette Modul "Metadaten" in **das Suchfeld** ein. Sehen Sie [Metadaten bearbeiten] [ edit-metadata] in der Liste der Module angezeigt werden.
2.  Klicken Sie auf, und ziehen Sie die [Metadaten bearbeiten] [ edit-metadata] Modul auf den Zeichenbereich, und legen Sie es unterhalb der zuvor hinzugefügten Dataset ab.
3.  Herstellen einer Verbindung die [Metadaten bearbeiten]mit Dataset[edit-metadata]: Klicken Sie auf den Ausgang des Datasets (den kleinen Kreis am unteren Rand der Dataset.) Ziehen Sie als Nächstes in der Eingang von [Metadaten bearbeiten] [ edit-metadata] (den kleinen Kreis am oberen Rand des Moduls), lassen Sie die Maustaste los. Das Dataset und das Modul bleiben verbunden, auch wenn Sie entweder auf den Zeichenbereich navigieren.

    Der Versuch sollte nun wie folgt aussehen:  

    ![Hinzufügen von Metadaten bearbeiten][2]
    
    Das rote Ausrufezeichen zeigt an, dass wir die Eigenschaften für dieses Modul noch eingerichtet haben. Wir werden, um das Dialogfeld Ausführen.
    
    > [AZURE.TIP] Sie können einen Kommentar zu einem Modul hinzufügen, indem Sie doppelt auf das Modul und Eingeben von Text. Dadurch können Sie auf einen Blick sehen, was in Ihrem Versuch das Modul macht. Doppelklicken Sie in diesem Fall auf die [Metadaten bearbeiten] [ edit-metadata] Modul und geben Sie den Kommentar "Spaltenüberschriften hinzufügen". Klicken Sie auf den Zeichenbereich, um das Textfeld zu schließen irgendeine andere Stelle. Um den Kommentar anzuzeigen, klicken Sie auf den Pfeil nach unten auf dem Modul.

4.  Wählen Sie [Metadaten bearbeiten][edit-metadata], klicken Sie dann im Bereich **Eigenschaften** rechts neben den Zeichenbereich auf **Spalte Ansichtsauswahl starten**.
5.  Klicken Sie im Dialogfeld **Wählen Sie Spalten** wählen Sie unter **Verfügbare Spalten** aller Zeilen aus, und klicken Sie auf >, die sie in der **Ausgewählten Spalten**zu verschieben.
Das Dialogfeld sollte wie folgt aussehen: ![Spalte Ansichtsauswahl für alle Spalten aktiviert][4]
7.  Klicken Sie auf das Häkchen **OK** .
8.  Klicken Sie im **Eigenschaftenbereich** suchen Sie nach den **Namen der neuen** Parameter. Geben Sie in diesem Feld eine Liste von Namen für die 21 Spalten im Dataset, getrennt durch Kommas getrennt ein und in der Spaltenreihenfolge ein. Sie können die Spaltennamen aus der Dataset-Dokumentation auf der Website UCI abrufen, oder zur Vereinfachung können kopieren und Einfügen in der folgenden Liste:  

          Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  

    Im Bereich Eigenschaften sieht wie folgt aus:

    ![Eigenschaften für Metadaten bearbeiten][1]

> [AZURE.TIP] Wenn Sie die Spaltenüberschriften überprüfen möchten, führen Sie die experimentieren (klicken Sie unter den Zeichenbereich experimentieren **Ausführen** ). Wann er endet Ausführung (ein grünes Häkchen wird angezeigt, klicken Sie auf [Bearbeiten der Metadaten][edit-metadata]), klicken Sie auf den Ausgang der [Metadaten bearbeiten] [ edit-metadata] aktivieren, **visualisieren**und Modul. Sie können die Ausgabe eines beliebigen Moduls anzeigen, auf die gleiche Weise, den Fortschritt der Daten durch den Versuch anzuzeigen.

##<a name="create-training-and-test-datasets"></a>Erstellen Sie Schulung zu und Testen Sie datasets

Im nächsten Schritt von der Versuch wird in separaten Datasets zu erzeugen, die wir für Schulung und Testen unser Modell verwenden möchten.

Dazu verwenden wir die [Aufgeteilten Daten] [ split] Modul.  

1.  Suchen nach die [Aufgeteilten Daten] [ split] Modul, ziehen Sie ihn auf den Zeichenbereich, und verbinden Sie es mit der letzten [Metadaten bearbeiten] [ edit-metadata] Modul.
2.  Standardmäßig das Verhältnis geteilten 0,5 und der Parameter **Randomized Teilen** festgelegt ist. Dies bedeutet, dass der Daten eine zufällige halber Ausgabe über einen Anschluss [Aufgeteilten Daten] ist[ split] Modul und Hälfte über einen anderen. Sie können diese als auch die Parameter **verteilte Startwert** so ändern Sie die Teilung zwischen Schulung und Testen der Daten anpassen. In diesem Beispiel werden wir lassen Sie sie als-ist.
    
    > [AZURE.TIP] Die Eigenschaft **Teiler von Zeilen in der ersten Ausgabe Dataset** bestimmt, wie viele Daten Ausgabe über den linken Ausgabe Port ist. Wenn Sie das Verhältnis auf 0.7 festgelegt haben, ist beispielsweise dann 70 % der Daten Ausgabe über den linken Port und 30 % über den richtigen Anschluss.  
    
3. Doppelklicken Sie auf die [Aufgeteilten Daten] [ split] Modul, und geben Sie den Kommentar, "Schulung/testen Daten Teilen 50 %". 

Wir können die Ausgaben [Aufgeteilten Daten] [ split] Modul wir wie jedoch entscheiden wir die Ausgabe die Links zu verwenden, wie Schulungsdaten und rechts als testen Daten ausgeben.  

Wie auf der Website UCI erwähnt, sind die Kosten für eine hohe Kreditkarte Risiko als niedrig sicherzustellen fünf Mal größer als die Kosten ein Risikos niedrig Kreditkarte als hohe sicherzustellen. Um dies zu berücksichtigen, generieren wir ein neues Dataset, das diese Kostenfunktion wiedergibt. In das neue Dataset wird jede hohem Risiko Beispiel fünf Mal repliziert, während jedes Beispiel niedrig Risiko nicht repliziert wird.   

Wir können diese Replikation kann mithilfe von R Code:  

1.  Suchen nach, und ziehen Sie das [Ausführen R Skript] [ execute-r-script] Modul auf den Versuch Zeichnungsbereich, und schließen Sie den linken Ausgabe Port [Aufgeteilten Daten] [ split] zu der ersten Eingang ("Dataset1") des [R Skript ausführen] [ execute-r-script] Modul.
2. Doppelklicken Sie auf das [Ausführen R Skript] [ execute-r-script] Modul, und geben Sie den Kommentar, "Satz Anpassung Kosten".
2.  Klicken Sie im Bereich **Eigenschaften** löschen Sie den Standardtext im **R Skript** Parameter, und geben Sie dieses Skript:

          dataset1 <- maml.mapInputPort(1)
          data.set<-dataset1[dataset1[,21]==1,]
          pos<-dataset1[dataset1[,21]==2,]
          for (i in 1:5) data.set<-rbind(data.set,pos)
          maml.mapOutputPort("data.set")


Führen Sie die gleichen Replikationsvorgang für jedes Ausgabe [Aufgeteilten Daten] müssen wir[ split] Modul, damit die Daten Schulung und testen die gleichen Kosten Anpassung haben.

1.  Mit der rechten Maustaste in das [Ausführen R Skript] [ execute-r-script] Modul und select **Kopieren**.
2.  Mit der rechten Maustaste in des Zeichenbereichs experimentieren, und wählen Sie **Einfügen**aus.
3.  Verbinden der ersten Eingang dieses [Ausführen R Skript] [ execute-r-script] Modul rechts ausgeben Port [Aufgeteilten Daten] [ split] Modul. 
4.  Klicken Sie am unteren Rand des Bereichs auf **Ausführen**. 

> [AZURE.TIP] Die Kopie des Moduls ausführen R Skript enthält dasselbe Skript wie das ursprüngliche Modul. Beim Kopieren und ein Moduls in den Zeichenbereich einfügen, behält die Kopie des ursprünglichen alle Eigenschaften aus.  

Unsere experimentieren sieht jetzt ungefähr wie folgt aus:

![Hinzufügen der geteilten Modul und R Skripts][3]

Weitere Informationen zur Verwendung von R Skripts in Ihre Versuche finden Sie unter [Erweitern Ihrer Experimentieren mit R](machine-learning-extend-your-experiment-with-r.md).

**Als Nächstes: [Zug die Modelle auswerten](machine-learning-walkthrough-4-train-and-evaluate-models.md)**


[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/create1.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/create2.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/create3.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/columnselector.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
