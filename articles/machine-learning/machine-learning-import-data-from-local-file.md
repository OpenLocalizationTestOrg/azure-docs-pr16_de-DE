<properties
    pageTitle="Importieren von Daten in Computer Learning Studio aus einer lokalen Datei | Microsoft Azure"
    description="Wie um Azure maschinellen Learning Studio Schulung Daten aus einer lokalen Datei zu importieren."
    keywords="Importieren von Daten, Datenformat, Datentypen, Datenquellen, Schulungsdaten"
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
    ms.date="09/16/2016"
    ms.author="garye;bradsev" />


# <a name="import-your-training-data-into-azure-machine-learning-studio-from-a-local-file"></a>Importieren Sie Ihre Daten Schulung in Azure maschinellen Learning Studio aus einer lokalen Datei

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]


Wenn Ihre eigenen Daten in Computer Learning Studio verwenden möchten, können Sie von Ihrem lokalen Festplatte ein Dataset Modul im Arbeitsbereich erstellt eine Datendatei im Voraus hochladen. 


## <a name="import-data-from-a-local-file"></a>Importieren von Daten aus einer lokalen Datei

Sie können Daten aus einer lokalen Festplatte importieren, mit den folgenden Schritten:

1. Klicken Sie auf **+ neue** am Fuß des Fensters maschinellen Learning Studio.
2. Wählen Sie **DATASET** und **lokale Dateien von**.
3. Klicken Sie im Dialogfeld **Hochladen ein neues Datasets** navigieren Sie zu der Datei, die Sie hochladen möchten.
4. Geben Sie einen Namen, identifizieren Sie den Datentyp zu, und geben Sie optional eine Beschreibung. Eine Beschreibung wird empfohlen: sie können Sie alle Merkmale über die Daten zu erfassen, die Sie bei Denken Sie daran, wenn Sie die Daten in der Zukunft verwenden möchten.
5. Das Kontrollkästchen **Dies ist die neue Version eines vorhandenen Datasets** können Sie ein vorhandenes Dataset mit neuen Daten aktualisieren. Klicken Sie einfach auf dieses Kontrollkästchen, und geben Sie den Namen eines vorhandenen Datasets.

Während des Uploads sehen Sie eine Meldung, dass die Datei hochgeladen wird. Hochladen Zeit hängt von der Größe Ihrer Daten und der Geschwindigkeit der Verbindung zum Dienst.
Wenn Sie, dass die Datei sehr lange dauern wird wissen, können Sie andere Elemente maschinellen Learning Studio tun, während Sie warten. Schließen des Browsers verursacht jedoch die Daten hochladen fehlschlägt.

Nachdem Sie Ihre Daten hochgeladen werden, wird in einem Dataset Modul gespeichert und steht für alle experimentieren im Arbeitsbereich.
Wenn Sie einem Versuch bearbeiten, können Sie die Datasets finden, die Sie in der Liste **Meine Datasets** unter der Liste **Datasets gespeichert** , in der Palette Modul erstellt haben. Sie können Drag & drop der Datasets in den Zeichenbereich experimentieren Wenn Datenmenge für die weiteren Analytics und Computer-Schulung verwendet werden soll.



