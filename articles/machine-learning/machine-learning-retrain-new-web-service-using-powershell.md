<properties
    pageTitle="Neu trainieren einen neuen Webdienst mithilfe der maschinellen Learning Management PowerShell-Cmdlets | Microsoft Azure"
    description="Informationen Sie zum programmgesteuerten neu Trainieren eines Modells und aktualisieren den Webdienst, um das Modell neu ausgebildeten Azure Computer interessante mithilfe der maschinellen Learning Management PowerShell-Cmdlets verwenden."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-new-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Neu Trainieren Sie einen neuen Webdienst mithilfe der maschinellen Learning Management PowerShell-cmdlets

Wenn Sie einen neuen Webdienst neu trainieren, aktualisieren Sie die Vorhersage Web Service Definition das neue ausgebildete Modell verweisen.  

## <a name="prerequisites"></a>Erforderliche Komponenten

Sie müssen eine Schulung experimentieren und eine Vorhersage experimentieren eingerichtet haben Siehe [neu trainieren maschinellen Learning Modelle programmgesteuert](machine-learning-retrain-models-programmatically.md). 

>[AZURE.IMPORTANT] Die Vorhersage experimentieren muss als eine Azure Ressourcenmanager (neu) basierte Computer learning Webdienst bereitgestellt werden. 
 
Weitere Informationen zum Bereitstellen von Web Services finden Sie unter [Bereitstellen eines Webdiensts Azure maschinellen Schulung](machine-learning-publish-a-machine-learning-web-service.md).

Dieses Verfahren erfordert, dass Sie die Azure maschinellen Learning Cmdlets installiert haben. Installieren den Computer Learning-Cmdlets Informationen finden Sie unter [Azure maschinellen Learning Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) Bezug auf MSDN.

Kopiert die folgende Informationen aus der retraining Ausgabe an:

* BaseLocation
* RelativeLocation

Die Schritte, die Sie ergreifen sind:

1.  Melden Sie sich bei Ihrem Konto Azure Ressourcenmanager.
2.  Abrufen der Definition der Web-Dienst
3.  Exportieren der Web Service Definition als JSON
4.  Aktualisieren Sie den Bezug auf die Ilearner Blob in das JSON.
5.  Importieren Sie die JSON in Web Service Definition
6.  Aktualisieren Sie den Webdienst mit neuen Web Service Definition

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Melden Sie sich bei Ihrem Konto Azure Ressourcenmanager

Sie müssen sich zuerst bei Ihrem Azure-Konto innerhalb der PowerShell-Umgebung mithilfe des Cmdlets [Hinzufügen-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) anmelden.

## <a name="get-the-web-service-definition"></a>Abrufen der Definition der Web-Dienst

Als Nächstes rufen Sie den Webdienst, indem Sie das Cmdlet " [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) ". Die Definition der Web-Dienst ist eine interne Darstellung des Modells des Webdiensts ausgebildeten und kann nicht direkt geändert werden. Stellen Sie sicher, dass Sie die Web-Service-Definition für Ihre Vorhersagen experimentieren und nicht Ihre Schulung experimentieren abrufen möchten.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Wenn Sie um den Namen der Gruppe Ressource von einem vorhandenen Webdienst zu ermitteln, führen Sie das Cmdlet "Get-AzureRmMlWebService" ohne Parameter der Webdienste in Ihrem Abonnement angezeigt werden. Suchen Sie nach den Webdienst, und sehen Sie sich deren Web-Dienst-ID. Der Name der Ressourcengruppe ist das vierte Element im Feld-ID, einfach nach dem *ResourceGroups* Element. Im folgenden Beispiel wird die Gruppe Ressourcenname Standard-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Alternativ für die Ermittlung der Gruppe Ressourcenname von einem vorhandenen Webdienst melden Sie sich bei Microsoft Azure maschinellen Learning-Webdienste-Portal an. Wählen Sie den Webdienst aus. Der Namen der Ressource Gruppe ist das fünfte Element neben der URL des Webdiensts, einfach nach dem *ResourceGroups* Element. Im folgenden Beispiel wird die Gruppe Ressourcenname Standard-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Exportieren der Web Service Definition als JSON

Zum Ändern der Definition zum ausgebildeten Modell mit der ausgebildeten Modell, Sie müssen zuerst mithilfe das [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) -Cmdlet ihn in eine Datei im Format JSON exportieren.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>Aktualisieren Sie den Bezug auf die Ilearner Blob in das JSON.

In der Posten [ausgebildete Modell] suchen, *Uri* -Werts in der *LocationInfo* -Knoten mit dem URI des Ilearner Blob aktualisieren. Der URI wird durch Kombinieren der *BaseLocation* und der *RelativeLocation* aus der Ausgabe der Anruf Umschulung l generiert.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
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

## <a name="import-the-json-into-a-web-service-definition"></a>Importieren Sie die JSON in Web Service Definition

Sie müssen mit dem [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) -Cmdlet verwenden, um die geänderte JSON-Datei wieder in eine Web Service Definition zu konvertieren, die Sie verwenden können, aktualisieren Sie die Predicative experimentieren.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>Aktualisieren Sie den Webdienst mit neuen Web Service Definition

Verwenden Sie schließlich [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) -Cmdlet der Vorhersage experimentieren aktualisieren aus.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Zusammenfassung

Die Cmdlets für die Verwaltung von maschinellen Learning PowerShell können Sie ausgebildete Modell eines Webdiensts Vorhersage UFI-Szenarien, z. B. aktualisieren:

* Periodische Modell Umschulung mit neuen Daten.
* Verteilung eines Modells für Kunden mit dem Ziel ganz können sie das Modell verwenden ihre eigenen Daten neu trainieren.
