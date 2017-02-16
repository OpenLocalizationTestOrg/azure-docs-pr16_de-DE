<properties
    pageTitle="Excel-add-in für maschinelle Learning-Webdiensten | Microsoft Azure"
    description="So Azure maschinellen Learning-Webdiensten direkt in Excel verwenden können, ohne Code."
    services="machine-learning"
    documentationCenter=""
    authors="tedway"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/05/2016"
    ms.author="tedway;garye" />

# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Excel-Add-in für Azure maschinellen Learning-Webdiensten

Excel erleichtert die anzurufende Webdienste direkt ohne Code schreiben zu müssen.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Schritte zum Verwenden eines vorhandenen Webdiensts in der Arbeitsmappe

1. Öffnen Sie die [Excel-Beispieldatei](http://aka.ms/amlexcel-sample-2), das Excel-add-Ins und Daten zu Personen auf die Titanic enthält.
2. Wählen Sie den Webdienst, indem Sie darauf-"Titanic Hinterbliebenenversorgung Vorhersage (Excel-Add-in Beispiel) [Ergebnis]" in diesem Beispiel.

    ![Wählen Sie Webdienst][01]

3. Dadurch gelangen Sie zum Abschnitt **Predict** .  Diese Arbeitsmappe enthält bereits Beispieldaten, aber für eine leere Arbeitsmappe können Sie eine Zelle in Excel auswählen und auf **Beispieldaten verwenden**.
4. Markieren Sie die Daten mit Überschriften, und klicken Sie auf das Symbol der Eingabedaten Bereich.  Stellen Sie sicher, dass das Feld "Meine Daten haben Überschriften" aktiviert ist.
5. **Ausgabe**Geben Sie unter der Zelle, der Sie die Ausgabe sein, beispielsweise "H1" Hier möchten.
6. Klicken Sie auf **Vorhersagen**.

    ![Vorhersagen Abschnitt][02]

## <a name="steps-to-add-a-new-web-service"></a>Schritte zum Hinzufügen eines neuen Webdiensts

Bereitstellen eines Webdiensts, oder verwenden Sie einen vorhandenen Webdienst. Weitere Informationen zum Bereitstellen von einem Webdienst finden Sie unter [Exemplarische Vorgehensweise Schritt 5: Bereitstellen von Azure maschinellen Learning-Webdienst](machine-learning-walkthrough-5-publish-web-service.md).

Abrufen des API-Schlüssels für den Webdienst. Stelle, an der Sie dafür abhängig, ob Sie einen klassische Computer Learning Webdienst oder einen neuen Computer Learning-Webdienst veröffentlicht.

**Mithilfe eines Webdiensts klassischen** 

1. Klicken Sie in Computer Learning Studio im Abschnitt **Webdienste** im linken Bereich auf, und wählen Sie dann auf den Webdienst.

    ![Wählen Sie einen Webdienst Studio][04]

2. Kopieren Sie die Taste API für den Webdienst.

    ![Studio-API-Taste][05]

3. Klicken Sie auf der Registerkarte **DASHBOARD** für den Webdienst auf den Link **Anforderung/Antwort** .
4. Suchen Sie im Abschnitt **URI anfordern** .  Kopieren Sie und speichern Sie die URL.

>[AZURE.NOTE] Es ist jetzt möglich, melden Sie sich am Portal [Azure maschinellen Learning-Webdiensten](https://services.azureml.net) API für einen klassischen maschinellen Learning-Webdienst abrufen.

**Verwenden Sie einen neuen Webdienst**

1. Im Portal [Azure maschinellen Learning-Webdiensten](https://services.azureml.net) klicken Sie auf **Webdienste**, und wählen Sie dann Ihren Webdienst. 
2. Klicken Sie auf **nutzen**.
3. Suchen Sie im Abschnitt **grundlegende Verbrauch Info** . Kopieren Sie und speichern Sie den **Primärschlüssel** und die **Anforderung / Antwort-** URL.


## <a name="steps-to-add-a-new-web-service"></a>Schritte zum Hinzufügen eines neuen Webdiensts

1. Bereitstellen eines Webdiensts, oder verwenden Sie einen vorhandenen Webdienst. Weitere Informationen zum Bereitstellen von einem Webdienst finden Sie unter [Exemplarische Vorgehensweise Schritt 5: Bereitstellen von Azure maschinellen Learning-Webdienst](machine-learning-walkthrough-5-publish-web-service.md).
2. Klicken Sie auf **nutzen**.
3. Suchen Sie im Abschnitt **grundlegende Verbrauch Info** . Kopieren Sie und speichern Sie den **Primärschlüssel** und die **Anforderung / Antwort-** URL.
2. In Excel, wechseln Sie zum Abschnitt **-Webdiensten** (Wenn Sie im Abschnitt **Predict** sind, klicken Sie auf den Pfeil, um zur Liste der Webdienste wechseln).

    ![Wechseln Sie zur Auswahl der Web-Dienst][03]
    
3. Klicken Sie auf **Webdienst hinzufügen**.
4. Fügen Sie die URL in das Excel-add-ins Textfeld **URL**ein.
5. Fügen Sie die Taste API/primär in das Textfeld **API-Schlüssel**ein.
6. Klicken Sie auf **Hinzufügen**.

    ![URL und API Schlüssel für einen klassischen Webdienst.][06]

10. Wenn Sie den Web-Dienst verwenden, führen Sie die vorherigen Anweisungen, "Schritte zum Verwenden einer vorhandenen Webdiensts".

## <a name="sharing-your-workbook"></a>Freigeben einer Arbeitsmappe

Wenn Sie die Arbeitsmappe speichern, wird die Taste API/primär für die Webdienste, die Sie hinzugefügt haben ebenfalls gespeichert. Dies bedeutet, dass die Arbeitsmappe nur mit Personen freigeben sollte, denen Sie vertrauen.

Bitten Sie alle Fragen in den folgenden Kommentarbereich oder auf unsere [Forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
