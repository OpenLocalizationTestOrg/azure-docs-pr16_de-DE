<properties
    pageTitle="Neu trainieren einen vorhandenen Vorhersage Webdienst | Microsoft Azure"
    description="Informationen Sie zum neu Trainieren eines Modells und aktualisieren den Webdienst, um das Modell neu ausgebildeten Azure Computer interessante verwenden."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="v-donglo"/>


# <a name="retrain-an-existing-predictive-web-service"></a>Neu Trainieren Sie einen vorhandenen Vorhersage Webdienst

Dieses Dokument beschreibt die retraining für das folgende Szenario:

* Sie haben eine Schulung experimentieren und eine Vorhersage experimentieren, die Sie als operationalized Webdienst bereitgestellt haben.
* Sie haben die neue Daten gewünschte Ihrer Vorhersage Webdienst zu verwenden, um deren bewerten auszuführen.

Beginnen mit Ihrem vorhandenen Webdienst und Versuche, müssen Sie die folgenden Schritte:

1. Aktualisieren Sie das Modell ein.
    1. Ändern Sie Ihrer Schulung experimentieren zu Web Service Eingaben und Ausgaben zulässig ist.
    2. Bereitstellen des Schulung Versuchs als retraining Webdienst.
    3. Verwenden Sie die Schulung-experimentieren Stapel Ausführung Service (l), um das Modell neu trainieren.
2. Verwenden Sie die Azure maschinellen Learning PowerShell-Cmdlets, um die Vorhersage experimentieren zu aktualisieren.
    1.  Melden Sie sich bei Ihrem Konto Azure Ressourcenmanager.
    2.  Abrufen der Web Service Definition.
    3.  Exportieren der Web Service Definition als JSON.
    4.  Aktualisieren Sie den Bezug auf die Ilearner Blob in das JSON.
    5.  Importieren Sie die JSON in Web Service Definition.
    6.  Aktualisieren Sie den Webdienst mit einer neuen Web Service Definition.

## <a name="deploy-the-training-experiment"></a>Bereitstellen des Versuchs Schulung

Um den Versuch Schulung als retraining Webdienst bereitstellen zu können, müssen Sie Web Service ein- und Ausgaben in das Datenmodell hinzufügen. Durch das Verbinden eines Moduls *Web Service Ausgabe* des zukommen * [Zug Modell] [ train-model] * Modul, aktivieren Sie die Schulung experimentieren, um ein neues ausgebildeten Modell zu erzeugen, die Sie in Ihrem Vorhersage experimentieren verwenden können. Wenn Sie ein *Modell ausgewertet werden* -Modul verfügen, können Sie auch Web Service Ausgabe, um die Ergebnisse der Bewertung in der Ausgabe erhalten anfügen.

So aktualisieren Sie Ihre experimentieren Schulung:

1. Herstellen einer Verbindung der Dateneingabe (beispielsweise ein Modul *Säubern fehlende Daten* ) mit einer *Web Service Eingabewerte* Modul. Sie möchten in der Regel, stellen Sie sicher, dass die Eingabedaten in die gleiche Weise wie die ursprüngliche Schulung Daten verarbeitet werden.
2. Herstellen einer Verbindung die Ausgabe Ihres Moduls *Zug Modell* mit einer *Web Service Ausgabe* Modul.
3. Wenn Sie ein Modul *Modell ausgewertet werden soll haben* , und Sie die Ergebnisse der Auswertung ausgeben möchten, schließen Sie ein *Web Service Ausgabe* -Modul in die Ausgabe Ihres Moduls *Modell ausgewertet werden soll* .

Führen Sie Ihre experimentieren.

Als Nächstes müssen Sie die Schulung experimentieren als Webdienst bereitstellen, die ein ausgebildeten Modell und Modell Auswertung Ergebnisse erzeugt.  

Klicken Sie am unteren Rand des Zeichenbereichs experimentieren klicken Sie auf **Web-Dienst**, und wählen Sie dann **[New] Webdienst bereitstellen**. Im Portal Azure maschinellen Learning-Webdiensten wird geöffnet, auf der Seite **Webdienst bereitstellen** . Geben Sie einen Namen für den Webdienst, wählen Sie einen Zahlungsplan aus, und klicken Sie auf **Bereitstellen**. Die Stapel Execution-Methode können nur für das Erstellen ausgebildeten Modelle.


## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Neu Trainieren Sie das Modell mit neuen Daten mithilfe von l

In diesem Beispiel haben wir c# verwendet, um die retraining Anwendung zu erstellen. Sie können auch Python oder R Stichprobe Code verwenden, um diese Aufgabe auszuführen.

Um die retraining APIs anzurufen:

1. Erstellen Sie eine c# in Visual Studio (**neu** > **Project** > **Windows-Desktop** > **Console-Anwendung**).
2.  Melden Sie sich am Computer Learning-Webdienste-Portal aus.
3.  Klicken Sie auf der Webdienst, dem Sie beim Arbeiten mit.
2.  Klicken Sie auf **nutzen**.
3.  Klicken Sie am unteren Rand der Seite **verbrauchen** im Abschnitt **Beispiel-Code** auf **Stapel**.
5.  Kopieren Sie den C#-Code für die Ausführung des Stapels, und fügen Sie ihn in die Program.cs-Datei. Stellen Sie sicher, dass der Namespace intakt bleibt.

Fügen Sie die NuGet-Paket Microsoft.AspNet.WebApi.Client, gemäß Angabe in den Kommentaren hinzu. Um den Bezug auf Microsoft.WindowsAzure.Storage.dll hinzufügen zu können, müssen Sie zuerst die [Clientbibliothek für Azure Storage Services](https://www.nuget.org/packages/WindowsAzure.Storage)installieren.

Das folgende Bildschirmabbild zeigt die Seite **verbrauchen** im Portal Azure maschinellen Learning-Webdiensten.

![Nutzen Sie die Seite][1]

### <a name="update-the-apikey-declaration"></a>Aktualisieren Sie die Deklaration apikey

Suchen Sie nach der Deklaration **Apikey** :

    const string apiKey = "abc123"; // Replace this with the API key for the web service

Suchen Sie im Abschnitt **Verbrauch grundlegende Informationen** von der Seite **verbrauchen** den Primärschlüssel und Kopieren in die **Apikey** Deklaration.

### <a name="update-the-azure-storage-information"></a>Aktualisieren Sie die Informationen Azure-Speicher

Der Code l eine Datei aus einem lokalen Laufwerk (beispielsweise "C:\temp\CensusIpnput.csv") mit Azure-Speicher hochgeladen, verarbeitet und schreibt die Ergebnisse in Azure-Speicher zurück.  

Um den Azure-Speicher zu aktualisieren, müssen Sie den Kontonamen Speicher, Schlüssel und Containerinformationen für Ihr Speicherkonto vom klassischen Azure-Portal abrufen sollte, und klicken Sie dann Aktualisieren der Correspondi nach dem Ausführen der experimentieren, das sich daraus ergebende Workflow ähnlich wie die folgende:

![Resultierende Workflow nach dem Ausführen][4]NG Werte in den Code.

1. Melden Sie sich zum klassischen Azure-Portal aus.
1. Klicken Sie in der Spalte linken Navigationsbereich auf **Speicher**.
1. Wählen Sie aus der Liste der Speicherkonten eine retrained Modell gespeichert.
1. Klicken Sie am unteren Rand der Seite auf **Zugriffstasten verwalten**.
1. Kopieren Sie und speichern Sie den **Primärschlüssel Access** , und schließen Sie das Dialogfeld.
1. Klicken Sie am oberen Rand der Seite auf **Container**.
1. Wählen Sie einen vorhandenen Container, oder erstellen Sie einen neuen und speichern Sie den Namen.

Suchen Sie nach der *StorageAccountName*, *StorageAccountKey*und *StorageContainerName* Deklarationen, und aktualisieren Sie die Werte, die Sie aus dem klassischen Portal gespeichert.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Sie müssen außerdem sicherstellen, dass die Eingabe Datei am Speicherort, den Sie, in den Code angeben verfügbar ist.

### <a name="specify-the-output-location"></a>Geben Sie den Ausgabespeicherort

Wenn Sie in den Nutzdaten Anfordern der Ausgabeort angeben, muss die Erweiterung der Datei, die in *RelativeLocation* angegeben wird als angegeben `ilearner`. Im folgende Beispiel wird angezeigt:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Im folgenden ist ein Beispiel für die Ausgabe Umschulung: ![Umschulung Ausgabe][6]

## <a name="evaluate-the-retraining-results"></a>Die retraining Ergebnisse auswerten

Wenn Sie die Anwendung ausführen, enthält die Ausgabe der URL und dem gemeinsamen Zugriff Signaturen Token, die zum Zugreifen auf die Ergebnisse der Auswertung erforderlich sind.

Sie können die Ergebnisse des Modells retrained anzeigen, indem Sie kombinieren der *BaseLocation*, *RelativeLocation*und *SasBlobToken* aus den Ausgabeergebnissen für *output2* (wie in der obigen retraining Ausgabe Abbildung gezeigt) und die vollständige URL in der Adressleiste des Browsers einfügen.  

Untersuchen Sie die Ergebnisse, um festzustellen, ob das Modell neu ausgebildeten ausreichend gut ausführt, ersetzen Sie den vorhandenen ein.

Kopieren Sie die *BaseLocation*, *RelativeLocation*und *SasBlobToken* aus den Ausgabeergebnissen an.

## <a name="retrain-the-web-service"></a>Neu Trainieren Sie den Webdienst

Wenn Sie einen neuen Webdienst neu trainieren, aktualisieren Sie die Vorhersage Web Service Definition das neue ausgebildete Modell verweisen. Die Definition der Web-Dienst ist eine interne Darstellung des Modells des Webdiensts ausgebildeten und kann nicht direkt geändert werden. Stellen Sie sicher, dass Sie die Web-Service-Definition für Ihre Vorhersagen experimentieren und nicht Ihre Schulung experimentieren abrufen möchten.

## <a name="sign-in-to-azure-resource-manager"></a>Melden Sie sich zum Azure Ressourcenmanager

Sie müssen zuerst bei Ihrem Azure-Konto innerhalb der PowerShell-Umgebung melden Sie sich mithilfe des Cmdlets [AzureRmAccount hinzufügen](https://msdn.microsoft.com/library/mt619267.aspx) .

## <a name="get-the-web-service-definition-object"></a>Rufen Sie das Objekt Definition der Web-Diensts

Als Nächstes rufen Sie das Objekt Web Service Definition durch das Cmdlet " [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) " aufrufen.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Wenn Sie um den Namen der Gruppe Ressource von einem vorhandenen Webdienst zu ermitteln, führen Sie das Cmdlet "Get-AzureRmMlWebService" ohne Parameter der Webdienste in Ihrem Abonnement angezeigt werden. Suchen Sie nach den Webdienst, und sehen Sie sich deren Web-Dienst-ID. Der Name der Ressourcengruppe ist das vierte Element im Feld-ID, einfach nach dem *ResourceGroups* Element. Im folgenden Beispiel wird die Gruppe Ressourcenname Standard-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Alternativ für die Ermittlung der Gruppe Ressourcenname von einem vorhandenen Webdienst melden Sie sich mit dem Portal Azure maschinellen Learning-Webdiensten. Wählen Sie den Webdienst aus. Der Namen der Ressource Gruppe ist das fünfte Element neben der URL des Webdiensts, einfach nach dem *ResourceGroups* Element. Im folgenden Beispiel wird die Gruppe Ressourcenname Standard-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>Exportieren Sie das Objekt Web Service Definition als JSON

Zum Ändern der Definition ausgebildeten Modell neu ausgebildeten Modell verwenden, müssen Sie so exportieren Sie es in eine Datei im JSON-Format des [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) -Cmdlets verwenden.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>Aktualisieren Sie den Bezug auf die Ilearner blob

In der Posten [ausgebildete Modell] suchen, *Uri* -Werts in der *LocationInfo* -Knoten mit dem URI des Ilearner Blob aktualisieren. Der URI wird durch Kombinieren der *BaseLocation* und der *RelativeLocation* aus der Ausgabe der Anruf Umschulung l generiert.

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition-object"></a>Importieren Sie die JSON in ein Web Service Definition-Objekt

Sie müssen mit dem [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) -Cmdlet verwenden, um die geänderte JSON-Datei wieder in eine Web Service Definition Objekt zu konvertieren, die Sie verwenden können, aktualisieren Sie die predicative experimentieren.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>Aktualisieren Sie den Webdienst

Verwenden Sie das [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) -Cmdlet schließlich die Vorhersage experimentieren aktualisieren.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
