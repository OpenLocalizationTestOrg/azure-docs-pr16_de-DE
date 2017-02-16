<properties
    pageTitle="Neu trainieren maschinellen Learning Modelle programmgesteuert | Microsoft Azure"
    description="Informationen Sie zum programmgesteuerten neu Trainieren eines Modells und aktualisieren den Webdienst, um das Modell neu ausgebildeten Azure Computer interessante verwenden."
    services="machine-learning"
    documentationCenter=""
    authors="raymondlaghaeian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="raymondl;garye;v-donglo"/>


# <a name="retrain-machine-learning-models-programmatically"></a>Neu Trainieren Sie maschinellen Learning Modelle programmgesteuert  

In dieser Anleitung erfahren Sie, wie eine Azure maschinellen Learning-Webdienst mit c# und den Computer Learning Stapel Ausführung Dienst programmgesteuert neu trainieren.

Nachdem Sie das Modell einarbeiten haben, anzeigen die folgenden Vorgehensweisen zum Aktualisieren des Modells in Ihrem Vorhersage Webdienst aus:

- Wenn Sie einen klassischen Webdienst im Portal maschinellen Learning-Webdiensten bereitgestellt haben, finden Sie unter [neu Trainieren von einem Webdienst klassischen](machine-learning-retrain-a-classic-web-service.md). 
- Wenn Sie einen neuen Webdienst bereitgestellt haben, finden Sie unter [neu trainieren einen neuen Webdienst den Computer Learning Management-Cmdlets verwenden](machine-learning-retrain-new-web-service-using-powershell.md).

Übersicht über die retraining Prozess finden Sie unter [neu trainieren ein Modell für maschinelle lernen](machine-learning-retrain-machine-learning-model.md).

Wenn Sie mit Ihrer vorhandenen neue Azure Ressourcenmanager Grundlage Webdienst beginnen möchten, finden Sie unter [neu trainieren einen vorhandenen Vorhersage Webdienst](machine-learning-retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Erstellen einer Schulung experimentieren
 
In diesem Beispiel verwenden Sie "Beispiel 5: Zug, Tests auswerten für binäre Klassifizierung: Erwachsenen Dataset" aus der Microsoft Azure maschinellen Learning von Beispielen. 
    
Um den Versuch zu erstellen:

1.  Melden Sie sich bei Microsoft Azure Computer Learning Studio. 
2.  Klicken Sie auf der unteren rechten Ecke des Dashboards klicken Sie auf **neu**.
3.  Wählen Sie die Microsoft-Samples Beispiel 5 aus.
4.  Wählen Sie zum Umbenennen der experimentieren, am oberen Rand des Zeichenbereichs experimentieren den experimentieren Namen "Beispiel 5: Zug, Tests auswerten für binäre Klassifizierung: Erwachsenen Dataset".
5.  Typ Erhebung Modell.
6.  Klicken Sie am unteren Rand der experimentieren Zeichnungsbereich, klicken Sie auf **Ausführen**.
7.  Klicken Sie auf **Einrichten-Webdienst** , und wählen Sie **Retraining-Webdienst**. 

    ![Ursprüngliche experimentieren.][2]

Diagramm 2: Initiale experimentieren.

## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Erstellen Sie einer Vorhersage experimentieren und als Webdienst veröffentlichen  

Als Nächstes erstellen Sie eine Predicative experimentieren.

1.  Klicken Sie am unteren Rand des Zeichenbereichs experimentieren klicken Sie auf **Web-Dienst** , und wählen Sie **Vorhersagen Webdienst**. Dies speichert das Modell als ausgebildeten Modell und addiert Web Service Eingabe- und Module. 
2.  Klicken Sie auf **Ausführen**. 
3.  Nachdem der Versuch ausgeführt wurde, klicken Sie auf **Bereitstellen Webdienst [klassischen]** oder **[New] Webdienst bereitstellen**.

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>Bereitstellen der Schulung experimentieren als Webdienst Schulung

Um das Modell ausgebildeten neu trainieren, müssen Sie die Schulung experimentieren bereitstellen, die Sie als Webdienst Retraining erstellt. Dieser Webdienst benötigt ein *Web Service Ausgabe* Modul bei einer Verbindung zu den * [Zug Modell] [ train-model] * Modul, neue ausgebildeten Modelle werden können.

1. Um Schulung zukommen zurückzukehren, klicken Sie auf das Symbol Versuche im linken Bereich, und klicken Sie auf den Versuch, mit dem Namen Erhebung Model.  
2. Geben Sie in das Suchfeld der Suche experimentieren Elemente Webdienst aus. 
3. Ziehen Sie ein Modul *Web Service Eingabe* auf den Zeichenbereich experimentieren und verbunden Sie deren Ausgabe mit dem *Säubern fehlende Daten* Modul.  Dadurch wird sichergestellt, dass Ihre Daten retraining die gleiche Weise wie die ursprüngliche Schulung Daten verarbeitet werden.
4. Ziehen Sie die zwei *Webdienst Ausgabe* Module auf den Zeichenbereich experimentieren. Herstellen einer Verbindung eine und deren Ausgabe des Moduls in das andere *Modell ausgewertet werden* mit der Ausgabe des Moduls *Zug Modell* . Die Ausgabe der Web-Dienst für **Zug-Modell** , erhalten Sie das neue ausgebildete Modell. Die Ausgabe angefügter **Auswerten Modell** sieht des Moduls, also die Leistungsergebnisse.
5. Klicken Sie auf **Ausführen**. 

Als Nächstes müssen Sie die Schulung experimentieren als Webdienst bereitstellen, die ein ausgebildeten Modell und Modell Auswertung Ergebnisse erzeugt. Um dies zu erreichen, werden der nächsten Satz von Aktionen abhängig davon, ob Sie mit einem Webdienst Classic oder einen neuen Webdienst arbeiten.  
  
**Klassische Webdienst**

Klicken Sie am unteren Rand des Zeichenbereichs experimentieren klicken Sie auf **Web-Dienst** , und wählen Sie **Webdienst bereitstellen [klassischen]**. Die Webdienst- **Dashboard** wird mit der API-Schlüssel und der Hilfeseite API für die Ausführung des Stapels angezeigt. Nur die Stapel Execution-Methode kann zum Erstellen von ausgebildeten Modelle verwendet werden.

**Neuen Webdienst**

Klicken Sie am unteren Rand des Zeichenbereichs experimentieren klicken Sie auf **Web-Dienst** , und wählen Sie **[New] Webdienst bereitstellen**. Im Web Service Azure maschinellen Learning-Webdienste-Portal wird geöffnet, zu der Webseite Dienst bereitstellen. Geben Sie einen Namen für den Webdienst, wählen Sie einen Zahlungsplan aus, und klicken Sie auf **Bereitstellen**. Nur die Stapel Execution-Methode kann verwendet werden, für das Erstellen ausgebildeten Modelle

Nachdem experimentieren Ausführung abgeschlossen ist, sollte der resultierende Workflow in beiden Fällen wie folgt aussehen:

![Resultierende Workflow nach dem Ausführen.][4]

Diagramm 3: Resultierender Workflow nach dem Ausführen.

## <a name="retrain-the-model-with-new-data-using-bes"></a>Neu Trainieren des Modells mit neuen Daten mit l

In diesem Beispiel verwenden c# Sie zum Erstellen der Anwendung retraining. Den Code Python oder R können Sie auch diese Aufgabe auszuführen.

Um die APIs Umschulung anzurufen:

1. Erstellen Sie eine c# in Visual Studio (-> neue Project -> Windows Desktop-Anwendung Console >).
2.  Melden Sie sich am Computer Learning Webdienst-Portal aus.
3.  Wenn Sie mit einem Webdienst klassischen arbeiten, klicken Sie auf **Klassische Webdienste**.
    1.  Klicken Sie auf den Webdienst, mit dem Sie arbeiten.
    2.  Klicken Sie auf den Standardendpunkt.
    3.  Klicken Sie auf **nutzen**.
    4.  Klicken Sie am unteren Rand der Seite **verbrauchen** im Abschnitt **Beispiel-Code** auf **Stapel**.
    5.  Fahren Sie mit Schritt 5 fort.
4.  Wenn Sie mit einem neuen Webdienst arbeiten, klicken Sie auf **Webdienste**.
    1.  Klicken Sie auf den Webdienst, mit dem Sie arbeiten.
    2.  Klicken Sie auf **nutzen**.
    3.  Klicken Sie am unteren Rand der Seite verbrauchen im Abschnitt **Beispiel-Code** auf **Stapel**.
5.  Kopieren Sie den C#-Code für die Ausführung des Stapels, und fügen Sie ihn in die Program.cs-Datei, und stellen Sie sicher, dass der Namespace intakt bleibt.

Fügen Sie die Nuget-Paket Microsoft.AspNet.WebApi.Client gemäß Angabe in den Kommentaren hinzu. Wenn Sie den Bezug auf Microsoft.WindowsAzure.Storage.dll hinzufügen möchten, müssen Sie zuerst die Clientbibliothek für Microsoft Azure Storage Services installieren. Weitere Informationen finden Sie unter [Windows Storage Services](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-the-apikey-declaration"></a>Aktualisieren Sie die Deklaration apikey

Suchen Sie nach der Deklaration **Apikey** .

    const string apiKey = "abc123"; // Replace this with the API key for the web service

Suchen Sie im Abschnitt **Verbrauch grundlegende Informationen** von der Seite **verbrauchen** den Primärschlüssel und Kopieren in die **Apikey** Deklaration.

### <a name="update-the-azure-storage-information"></a>Aktualisieren Sie die Informationen Azure-Speicher

Der Code l eine Datei aus einem lokalen Laufwerk (beispielsweise "C:\temp\CensusIpnput.csv") mit Azure-Speicher hochgeladen, verarbeitet und schreibt die Ergebnisse in Azure-Speicher zurück.  

Um diese Aufgabe auszuführen, müssen Sie die Speicher-Kontoinformationen an Name, Schlüssel und Container für Ihr Konto Speicher aus dem klassischen Azure-Portal und die entsprechenden Werte aktualisieren in den Code abrufen können. 

1. Melden Sie sich zum klassischen Azure-Portal aus.
1. Klicken Sie in der Spalte linken Navigationsbereich auf **Speicher**.
1. Wählen Sie aus der Liste der Speicherkonten eine retrained Modell gespeichert.
1. Klicken Sie am unteren Rand der Seite auf **Zugriffstasten verwalten**.
1. Kopieren Sie und speichern Sie den **Primärschlüssel Access** , und schließen Sie das Dialogfeld. 
1. Klicken Sie am oberen Rand der Seite auf **Container**.
1. Wählen Sie einen vorhandenen Container oder erstellen Sie einen neuen und speichern Sie den Namen.

Suchen Sie die Deklarationen *StorageAccountName*, *StorageAccountKey*und *StorageContainerName* , und aktualisieren Sie die Werte, die Sie vom Azure-Portal gespeichert haben.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name
            
Sie müssen außerdem sicherstellen, dass die Datei an der Position im Code angegebenen verfügbar ist. 

### <a name="specify-the-output-location"></a>Geben Sie den Ausgabespeicherort

Bei der Angabe der Ausgabeort in den Nutzdaten anfordern, muss die Erweiterung in *RelativeLocation* angegebene Datei als Ilearner angegeben werden. 

Im folgende Beispiel wird angezeigt:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

>[AZURE.NOTE] Die Namen der Ausgabe stellen möglicherweise von den Preisen in diese Anleitung basierend auf der Reihenfolge, in der Sie die Web-Dienst Ausgabemodule hinzugefügt. Da Sie diese Schulung Experimentieren mit zwei Ausgaben eingerichtet haben, enthalten die Ergebnisse Standortinformationen für beide Speicher aus.  

![Umschulung Ausgabe][6]

Diagramm 4: Umschulung Ausgabe an.

## <a name="evaluate-the-retraining-results"></a>Die Retraining Ergebnisse auswerten
 
Wenn Sie die Anwendung ausführen, enthält die Ausgabe das URL und SAS Token erforderlich sind, um die Ergebnisse der Auswertung zugreifen.

Sie können die Ergebnisse des Modells retrained durch Kombinieren der *BaseLocation*, *RelativeLocation*und *SasBlobToken* aus den Ausgabeergebnissen für *output2* (wie in der obigen retraining Ausgabe Abbildung gezeigt) und Einfügen die vollständige URL in der Adressleiste des Browsers anzeigen.  

Untersuchen Sie die Ergebnisse, um festzustellen, ob das Modell neu ausgebildeten ausreichend gut ausführt, ersetzen Sie den vorhandenen ein.

Kopieren Sie die *BaseLocation*, *RelativeLocation*und *SasBlobToken* aus den Ausgabeergebnissen, werden während des retraining Prozess verwenden können.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie den Vorhersage Webdienst bereitgestellt, indem Sie auf **Bereitstellen Webdienst [klassischen]**, finden Sie unter [neu Trainieren von einem Webdienst klassischen](machine-learning-retrain-a-classic-web-service.md).

Wenn Sie den Vorhersage Webdienst bereitgestellt, indem Sie auf **[New] Webdienst bereitstellen**, finden Sie unter [neu trainieren einen neuen Webdienst den Computer Learning Management-Cmdlets verwenden](machine-learning-retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/