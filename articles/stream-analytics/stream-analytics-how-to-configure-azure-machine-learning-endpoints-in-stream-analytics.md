<properties 
    pageTitle="Konfigurieren von Azure maschinellen Learning Endpunkte in Stream Analytics | Microsoft Azure" 
    description="Sprache für benutzerdefinierte Funktionen in Stream Analytics"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"
/>

# <a name="machine-learning-integration-in-stream-analytics"></a>Learning-Integration in Stream Analytics Computer

Stream Analytics unterstützt benutzerdefinierte Funktionen, die Endpunkte Azure maschinellen Learning aufrufen. REST-API-Unterstützung für dieses Feature ist in der [Bibliothek Stream Analytics REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)detaillierte. Dieser Artikel enthält ergänzende Informationen für die erfolgreiche Implementierung dieser Funktion Stream Analytics erforderlich. Ein Lernprogramm auch gebucht wurde und steht [hier](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Übersicht: Azure maschinellen Learning Terminologie

Microsoft Azure maschinellen Learning bietet ein Zusammenarbeit, Drag & Drop-Tools, die, das Sie zum Erstellen, testen und Bereitstellen von Vorhersageanalytik Lösungen für Ihre Daten verwenden können. Dieses Tool heißt die *Azure maschinellen Learning Studio*. Die Studio wird verwendet, um die Computer Lernressourcen interagieren und einfach erstellen, testen und den Entwurf bewerten. Diese Ressourcen und die entsprechenden Definitionen sind unter.

- **Arbeitsbereich**: der *Arbeitsbereich* ist ein Container, alle anderen Computer Lernressourcen zusammen in einem Container für die Verwaltung und Steuerelement enthält.
- **Experimentieren**: *Versuche* Daten Wissenschaftler Datasets nutzen und Schulen von einem Computer Learning Modell erstellt werden.
- **Endpunkt**: *Endpunkte* sind die Azure maschinellen Learning Objekt zum Akzeptieren von Features als Eingabe, ein Modell für die angegebene Computer lernen anwenden und bewertete Ausgabe zurückgegeben.
- **Webdienst bewerten**: ein *Webdienst bewerten* ist eine Zusammenstellung von Endpunkten erwähnt.

Jeder Endpunkt verfügt über apis für Stapel Ausführung und synchroner Ausführung. Stream Analytics verwendet synchroner Ausführung. Der bestimmte Dienst heißt AzureML Studio eine [Anforderung/Antwort-Dienst](../machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs) .

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Computer-Learning-Ressourcen für Projekte Stream Analytics erforderlich

Im Sinne des Streams Analytics sind Verarbeitung von Aufträgen, einen Endpunkt Anforderung/Antwort, eine [Apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md#get-an-azure-machine-learning-authorization-key)und Swagger Definition alle notwendigen zur erfolgreichen Ausführung. Stream Analytics weist einen zusätzlichen Endpunkt, der die Url für Swagger Endpunkt erstellt, sucht auf der Benutzeroberfläche und Definition UDFs standardmäßig an den Benutzer zurückgegeben.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Konfigurieren eines Stream Analytics und Computer Learning UDFs über REST-API

Mithilfe der REST-APIs können Sie Ihre Arbeit, um die Sprache für Azure Funktionen aufrufen konfigurieren. Die Schritte sind wie folgt aus:

1. Erstellen eines Auftrags für Stream Analytics
2. Definieren einer Eingabesprache
3. Definieren einer Ausgabe
4. Erstellen Sie eine benutzerdefinierte Funktion (UDFs)
5. Schreiben Sie eine Stream Analytics-Transformation, die UDFs aufgeführt Telefonanrufe.
6. Starten Sie den Auftrag

## <a name="creating-a-udf-with-basic-properties"></a>Erstellen einer UDFs mit grundlegenden Eigenschaften

Beispielsweise erstellt der folgende Code skalaren UDFs mit dem Namen *Newudf* , die an einen Endpunkt Azure maschinellen Learning bindet. Notiz, die den *Endpunkt* (Dienst-URI) kann, klicken Sie auf der Hilfeseite API für den ausgewählten Dienst und der *ApiKey gefunden werden* finden Sie auf der Hauptseite von Services.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Beispiel für Anforderungstexts:  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Rufen Sie RetrieveDefaultDefinition Endpunkt für standardmäßig UDFs

Nachdem das Gerüst UDFs erstellt wurde, wird die vollständige Definition der UDFs benötigt. Der Endpunkt RetreiveDefaultDefinition hilft Ihnen die Default-Definition für eine Funktion skalaren erhalten, die an einen Endpunkt Azure maschinellen Learning gebunden ist. Die folgenden Nutzlast müssen Sie über die standardmäßigen UDFs Definition für eine Funktion skalaren erhalten, die an einen Endpunkt Azure maschinellen Learning gebunden ist. Dies angeben nicht den tatsächlichen Endpunkt, wie es bereits während sich Anforderung bereitgestellt wurde. Stream Analytics Ruft den Endpunkt in der Besprechungsanfrage bereitgestellt werden, wenn es explizit bereitgestellt wird. Andernfalls wird das Schema auf den ursprünglich verwiesen verwendet. Hier Zeichenfolge der UDFs Grundlage einer einzigen Zeichenfolge Parameter (Satzanfang) und gibt eine einzelne vom Typ ausgeben: Gibt die Bezeichnung "Grüße" für diesen Satz an.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Beispiel für Anforderungstexts:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

Eine Stichprobe ausgeben diesen suchen etwas unterhalb möchten.  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-the-response"></a>Patch UDFs mit der Antwort 

Nachdem Sie nun mit der vorherigen Antwort, Patch UDFs angewendet werden muss, wie unten dargestellt.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

Anforderungstexts (Ausgabe von RetrieveDefaultDefinition):

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>Implementieren der Stream Analytics Transformation UDFs anrufen

Jetzt UDFs (hier ScoreTweet genannt) für jedes Ereignis einer Abfrage, und Schreiben Sie eine Antwort für dieses Ereignis in eine Ausgabe.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>Anfordern von Hilfe
Für weitere Unterstützung zu erhalten versuchen Sie es unsere [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren Sie Azure Stream Analytics Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Bezug](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
