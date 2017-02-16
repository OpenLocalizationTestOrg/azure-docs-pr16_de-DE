<properties
    pageTitle="Azure maschinellen Learning-Webdiensten: Bereitstellung und Verbrauch | Microsoft Azure"
    description="Ressourcen für die Bereitstellung und Verwenden von Webdiensten."
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
    ms.date="10/12/2016"
    ms.author="v-donglo"/>

# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Azure maschinellen Learning-Webdiensten: Bereitstellung und Verbrauch

Azure maschinellen Learning können Sie den Computer-Learning-Workflows und Modelle als Webdienste bereitstellen. Diese Webdienste können dann verwendet werden, die Computer-Learning-Modellen von Applications aufrufen, über das Internet Vorhersagen in Echtzeit oder im Stapelverarbeitungsmodus ausführen. Da die Webdienste RESTful haben, können Sie sie aus verschiedenen Programmierung Sprachen und Plattformen wie .NET und Java, und Anwendungen wie Excel aufrufen.

Die nächsten Abschnitte enthalten Links zu Vorgehensweisen, Code und Dokumentation, die Ihnen beim Einstieg helfen.

## <a name="deploy-a-web-service"></a>Bereitstellen eines Webdiensts

### <a name="with-azure-machine-learning-studio"></a>Mit Azure-Computern Learning Studio

Maschinelle Learning Studio und Portal Microsoft Azure maschinellen Learning-Webdienste-Hilfe, die beim Bereitstellen und Verwalten von einem Webdienst ohne Code zu schreiben.

Die folgenden Links bieten allgemeine Informationen dazu, wie Sie einen neuen Webdienst bereitstellen:

* Eine Übersicht dazu, wie Sie einen neuen Webdienst bereitstellen, der auf Azure Ressourcenmanager basiert, finden Sie unter [Bereitstellen eines neuen Webdienst](machine-learning-webservice-deploy-a-web-service.md).
* Informationen zum Bereitstellen von eines Webdiensts eine exemplarische Vorgehensweise finden Sie unter [Bereitstellen eines Webdiensts Azure maschinellen Schulung](machine-learning-publish-a-machine-learning-web-service.md).
* Informationen zum Erstellen und Bereitstellen eines Webdiensts vollständige eine exemplarische Vorgehensweise, finden Sie unter [Exemplarische Vorgehensweise Schritt 1: Erstellen eines Arbeitsbereichs maschinellen Learning](machine-learning-walkthrough-1-create-ml-workspace.md).
* Bestimmte Beispiele, bei die einen Webdienst bereitgestellt werden, finden Sie unter:

    * [Exemplarische Vorgehensweise Schritt 5: Bereitstellen von den Webdienst Azure maschinellen Schulung](machine-learning-walkthrough-5-publish-web-service.md)
    * [Gewusst wie: Bereitstellen von einem Webdienst auf mehrere Bereiche](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Web Services Ressource Anbieter APIs (Azure Ressourcenmanager APIs)

Der Azure maschinellen Learning-Ressourcen-Anbieter für Webdienste ermöglicht Bereitstellung und Verwaltung von Webdiensten mithilfe von REST-API Anrufe an. Weitere Details finden Sie unter den [Computer Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) Bezug auf MSDN.

### <a name="with-powershell-cmdlets"></a>Mit PowerShell-cmdlets

Azure maschinellen Learning Ressource-Anbieter für Webdienste ermöglicht Bereitstellung und Verwaltung von Webdiensten mithilfe der PowerShell-Cmdlets.

Um die Cmdlets verwenden zu können, müssen Sie zuerst bei Ihrem Azure-Konto innerhalb der PowerShell-Umgebung melden Sie sich mithilfe des Cmdlets [AzureRmAccount hinzufügen](https://msdn.microsoft.com/library/mt619267.aspx) . Wenn Sie mit der Verwendung von PowerShell-Befehle aufrufen, die auf Ressourcenmanager basieren nicht vertraut sind, finden Sie unter [Verwenden von Azure PowerShell Azure Ressourcenmanager](../powershell-azure-resource-manager.md#login-to-your-azure-account).

Verwenden Sie zum Exportieren der Vorhersage experimentieren [Dieses Beispiel-Code](https://github.com/ritwik20/AzureML-WebServices)ein. Nachdem Sie die .exe-Datei aus dem Code erstellen, können Sie eingeben:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Ausführen der Anwendung erstellt eine Web Service-JSON-Vorlage. Wenn die Vorlage für die Bereitstellung von eines Webdiensts verwenden möchten, müssen Sie die folgende Informationen hinzufügen:

* Speicher Kontonamen und Schlüssel

    Sie können die Kontonamen Speicher und die Taste aus der [Azure-Portal](https://portal.azure.com/) oder im [Azure klassischen Portal](http://manage.windowsazure.com/)erhalten.
* Zusicherung Plan-ID

    Die Plan-ID können aus dem Portal [Azure maschinellen Learning-Webdiensten](https://services.azureml.net) erhalten Sie, sich anzumelden und durch Klicken auf den Namen eines Plans.

Hinzufügen der JSON-Vorlage als untergeordnetes Element des Knoten *Eigenschaften* auf der gleichen Ebene wie der *MachineLearningWorkspace* -Knoten.

Hier ist ein Beispiel:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Finden Sie in den folgenden Artikeln und Beispiel-Code für weitere Details:

* [Azure maschinellen Learning Cmdlets]( https://msdn.microsoft.com/library/azure/mt767952.aspx) -Referenz auf MSDN
* Beispiel für [Exemplarische Vorgehensweise](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) auf GitHub

## <a name="consume-the-web-services"></a>Nutzen Sie die Webdienste

### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Aus den Azure-Computern Learning-Webdiensten (Test) UI

Sie können Ihre Webdienst vom Azure maschinellen Learning-Webdienste-Portal zu testen. Dies umfasst den Anforderung / Antwort-Dienst (RRS) testen und Stapel Ausführung-Dienst (l)-Schnittstellen.

* [Bereitstellen eines neuen Webdiensts](machine-learning-webservice-deploy-a-web-service.md)
* [Bereitstellen eines Webdiensts Azure Computer-Schulung](machine-learning-publish-a-machine-learning-web-service.md)
* [Exemplarische Vorgehensweise Schritt 5: Bereitstellen von den Webdienst Azure maschinellen Schulung](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Aus Excel

Sie können eine Excel-Vorlage herunterladen, die den Webdienst verwendet:

* [Verarbeitung von einem Webdienst Azure maschinellen Learning aus Excel](machine-learning-consuming-from-excel.md)
* [Excel-add-in für Azure maschinellen Learning-Webdiensten](machine-learning-excel-add-in-for-web-services.md)


### <a name="from-a-rest-based-client"></a>Von einem REST-basierten client

Azure maschinellen Learning-Webdiensten gibt Rest APIs. Sie können diese APIs aus verschiedenen Plattformen, wie etwa .NET, Python, R, Java usw. nutzen. Die **verbrauchen** für Ihren Webdienst im [Portal von Microsoft Azure maschinellen Learning-Webdiensten](https://services.azureml.net) verfügt über Stichprobe Code, die Ihnen beim Einstieg helfen können. Weitere Informationen finden Sie unter [so einen Webdienst Azure maschinellen Learning nutzen, der von einem Computer Learning experimentieren bereitgestellt wurde](machine-learning-consume-web-services.md).

