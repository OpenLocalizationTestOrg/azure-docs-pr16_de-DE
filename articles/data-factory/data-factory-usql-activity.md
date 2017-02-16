<properties 
    pageTitle="U-SQL-Skript Azure Daten dem Analytics aus Azure Data Factory ausgeführt" 
    description="Erfahren Sie, wie Daten zu verarbeiten, indem U-SQL-Skripts auf Azure Daten dem Analytics Computing-Service ausführen." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="spelluru"/>

# <a name="run-u-sql-script-on-azure-data-lake-analytics-from-azure-data-factory"></a>U-SQL-Skript Azure Daten dem Analytics aus Azure Data Factory ausgeführt
> [AZURE.SELECTOR]
[Struktur](data-factory-hive-activity.md)  
[Schwein](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Computer Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
[Daten dem Analytics U SQL](data-factory-usql-activity.md)
[benutzerdefinierten .NET](data-factory-use-custom-activities.md)
 
Eine Verkaufspipeline in einer Factory Azure-Daten verarbeitet Daten in verknüpften Speicherservices unter Verwendung von verknüpften berechnen Services. Sie enthält eine Abfolge von Aktivitäten, wobei jede Aktivität eine bestimmte Verarbeitung ausführt. In diesem Artikel werden die **Daten dem Analytics U SQL Aktivitäten** , die ein Skript **U-SQL** **Azure Daten dem Analytics** berechnen verknüpft Dienst ausgeführt wird. 

> [AZURE.NOTE] 
> Erstellen Sie ein Konto Azure Daten dem Analytics, bevor Sie eine Verkaufspipeline mit einem Daten dem Analytics U-SQL-Aktivität erstellen. Weitere Informationen zu Azure Daten dem Analytics finden Sie unter [Erste Schritte mit Azure Daten dem Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).
>  
> Überprüfen Sie die [Erstellen Ihrer ersten Verkaufspipeline Lernprogramm](data-factory-build-your-first-pipeline.md) die detaillierten Schritte zum Erstellen einer Factory Daten, verknüpften Diensten, Datasets und eine Verkaufspipeline. Verwenden Sie JSON-Codeausschnitte mit Daten Factory-Editor oder Visual Studio oder Azure PowerShell, um Daten Factory-Elemente zu erstellen.

## <a name="azure-data-lake-analytics-linked-service"></a>Azure-Daten Lake Analytics verknüpft Service
Erstellen Sie einen Dienst **Azure Daten dem Analytics** verknüpft ein Azure Daten dem Analytics Computing-Service eine Verknüpfung zu einer Factory Azure-Daten aus. Die Daten dem Analytics U-SQL-Aktivität in der Verkaufspipeline bezieht sich auf diesen Dienst verknüpften. 

Im folgende Beispiel stellt JSON-Definition für einen Dienst Azure Daten dem Analytics verknüpft. 

    {
        "name": "AzureDataLakeAnalyticsLinkedService",
        "properties": {
            "type": "AzureDataLakeAnalytics",
            "typeProperties": {
                "accountName": "adftestaccount",
                "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
                "authorization": "<authcode>",
                "sessionId": "<session ID>", 
                "subscriptionId": "<subscription id>",
                "resourceGroupName": "<resource group name>"
            }
        }
    }


Die folgende Tabelle enthält eine Beschreibung für die Eigenschaften in der JSON-Definition verwendet werden. 

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Die Typeigenschaft auf festgelegt sein sollte: **AzureDataLakeAnalytics**. | Ja
Kontoname | Azure-Daten Lake Analytics-Kontoname. | Ja
dataLakeAnalyticsUri | Lake Analytics-URI Azure-Daten. |  Nein 
Autorisierung | Autorisierungscode wird automatisch abgerufen, nach dem Klicken auf die Schaltfläche **Autorisieren** im Factory-Editor und das OAuth Login durchführen.  | Ja 
subscriptionId | Azure-Abonnement-id | Nein (wenn nicht angegeben, wird der Daten Factory Abonnement verwendet). 
resourceGroupName | Gruppennamen Azure Ressource |  Nein (wenn nicht angegeben, wird der Daten Factory Ressourcengruppe verwendet).
sessionId | Sitzung-Id aus der OAuth Autorisierung Sitzung. Jede Id für eine Sitzung ist eindeutig und darf nur einmal verwendet werden. Die Sitzung ist-Id in der Factory-Editor automatisch generiert. | Ja

Der Autorisierungscode, den Sie mithilfe der Schaltfläche **Autorisieren** generiert läuft ab nach einer Weile. In der folgenden Tabelle finden Sie die Ablaufzeiten für verschiedene Typen von Benutzerkonten. Wird den folgenden Fehler angezeigt Meldung an, wenn die Authentifizierung **Sicherheitstoken abläuft**: Vorgang Anmeldefehler: Invalid_grant - AADSTS70002: Fehler beim Überprüfen der Anmeldeinformationen. AADSTS70008: Die bereitgestellten Zugriff gewähren abgelaufen ist oder widerrufen. Spur-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelations-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Zeitstempel: 2015-12-15 21:09:31Z

 
| Benutzertyp | Läuft ab nach |
| :-------- | :----------- | 
| Benutzerkonten, die nicht von Azure Active Directory verwaltet (@hotmail.com, @live.com, usw..) | 12 Stunden |
| Konten für Benutzer von Azure Active Directory (AAD) verwaltet | 14 Tage nach dem letzten Segment ausführen zu können. <br/><br/>90 Tage, wenn ein Segment basierend auf Verknüpfte Dienst OAuth-basierten alle 14 Tage mindestens einmal ausgeführt wird. |

Um zu vermeiden, / dieser Fehler beheben, autorisieren möchten, verwenden die **Autorisieren** Schaltfläche, wenn die **Sicherheitstoken abläuft** und die erneute Bereitstellung verknüpfter Dienst. Sie können auch die Werte für **SessionId** und **Autorisierung** Eigenschaften programmgesteuert im folgenden Abschnitt mit Code generieren. 

  
### <a name="to-programmatically-generate-sessionid-and-authorization-values"></a>Programmgesteuert SessionId und Autorisierung Werte generieren 

    if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
        linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
    {
        AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

        WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
        string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

        AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
        if (azureDataLakeStoreProperties != null)
        {
            azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeStoreProperties.Authorization = authorization;
        }

        AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
        if (azureDataLakeAnalyticsProperties != null)
        {
            azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeAnalyticsProperties.Authorization = authorization;
        }
    }

Finden Sie Details zu den Daten Factory-Klassen im Code verwendeten [AzureDataLakeStoreLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)und [AuthorizationSessionGetResponse Klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) Themen. Fügen Sie einen Verweis zu: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll für die WindowsFormsWebAuthenticationDialog-Klasse. 
 
 
## <a name="data-lake-analytics-u-sql-activity"></a>Daten Lake Analytics U-SQL-Aktivität 

Im folgende JSON-Codeausschnitt definiert eine Verkaufspipeline mit einem Daten dem Analytics U-SQL-Aktivität. Die Aktivitätsdefinition weist einen Verweis auf den Dienst Azure Daten dem Analytics verknüpft, die Sie zuvor erstellt haben.   
  

    {
        "name": "ComputeEventsByRegionPipeline",
        "properties": {
            "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
            "activities": 
            [
                {
                    "type": "DataLakeAnalyticsU-SQL",
                    "typeProperties": {
                        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                        "scriptLinkedService": "StorageLinkedService",
                        "degreeOfParallelism": 3,
                        "priority": 100,
                        "parameters": {
                            "in": "/datalake/input/SearchLog.tsv",
                            "out": "/datalake/output/Result.tsv"
                        }
                    },
                    "inputs": [
                        {
                            "name": "DataLakeTable"
                        }
                    ],
                    "outputs": 
                    [
                        {
                            "name": "EventsByRegionTable"
                        }
                    ],
                    "policy": {
                        "timeout": "06:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "EventsByRegion",
                    "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
                }
            ],
            "start": "2015-08-08T00:00:00Z",
            "end": "2015-08-08T01:00:00Z",
            "isPaused": false
        }
    }


Die folgende Tabelle beschreibt die Namen und eine Beschreibung der Eigenschaften, die für diese Aktivität spezifisch sind. 

Eigenschaft | Beschreibung | Erforderlich
:-------- | :----------- | :--------
Typ | Die Eigenschaft muss **DataLakeAnalyticsU-SQL**festgelegt werden. | Ja
scriptPath | Pfad zum Ordner, das U-SQL-Skript enthält. Name der Datei beachtet werden. | Nein (falls Sie Skript verwenden)
scriptLinkedService | Verknüpfte Dienst, der die Speicherung links, die das Skript Fabrik Daten enthält. | Nein (falls Sie Skript verwenden)
Skript | Geben Sie Inlineskript anstatt ScriptPath und ScriptLinkedService an. Beispiel: "Skript": "Test-Datenbank erstellen". | Nein (Wenn Sie ScriptPath und ScriptLinkedService verwenden)
degreeOfParallelism | Die maximale Anzahl von Knoten gleichzeitig verwendet, um die Aufgabe durchzuführen. | Nein
Priorität | Bestimmt, welche Einzelvorgänge aus allen, die sich in der Warteschlange ausgewählt werden sollte, um zuerst ausgeführt werden. Je niedriger der Wert, desto höher die Priorität. | Nein 
Parameter | Parameter für das U-SQL-Skript | Nein 

Die Skriptdefinition finden Sie unter [SearchLogProcessing.txt Skriptdefinition](#script-definition) . 

## <a name="sample-input-and-output-datasets"></a>Beispiele für Eingabe und Datasets ausgeben

### <a name="input-dataset"></a>Eingabe-dataset
In diesem Beispiel befindet sich die Eingabedaten in einer Azure-Sees Datenspeicher (SearchLog.tsv-Datei im Ordner Datalake/Eingabemethoden) ein. 

    {
        "name": "DataLakeTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/input/",
                "fileName": "SearchLog.tsv",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }   

### <a name="output-dataset"></a>Die Ausgabe dataset
In diesem Beispiel werden die Ausgabedaten von der U-SQL-Skript erzeugte in einer Azure-Sees Datenspeicher (Datalake/Ausgabe Ordner) gespeichert. 

    {
        "name": "EventsByRegionTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/output/"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="sample-data-lake-store-linked-service"></a>Beispiel für Lake Datenspeicher verknüpft Service
Hier ist die Definition der Stichprobe, die Azure Lake Datenspeicher Dienst verwendet, die für die Eingabe/Ausgabe Datasets verknüpft. 

    {
        "name": "AzureDataLakeStoreLinkedService",
        "properties": {
            "type": "AzureDataLakeStore",
            "typeProperties": {
                "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
                "sessionId": "<session ID>",
                "authorization": "<authorization URL>"
            }
        }
    }

[Verschieben von Daten zwischen Azure dem Datenspeicher](data-factory-azure-datalake-connector.md) finden Sie im Artikel eine Beschreibung der JSON-Eigenschaften. 

## <a name="sample-u-sql-script"></a>Beispiel für U-SQL-Skript 

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv(nullEscape:"#NULL#");
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start <= DateTime.Parse("2012/02/19");
    
    OUTPUT @rs1   
        TO @out
          USING Outputters.Tsv(quoting:false, dateTimeFormat:null);

Die Werte für **@in** und **@out** Parameter im U-SQL-Skript sind übergebener dynamisch ADF mithilfe des Abschnitts "Parameters". Siehe Abschnitt "Parameters" in der Verkaufspipeline Definition.

Sie können andere Eigenschaften wie DegreeOfParallelism und Priorität als auch in der Definition Ihrer Verkaufspipeline für die Einzelvorgänge angeben, die Azure Daten dem Analytics-Dienst ausgeführt werden.

## <a name="dynamic-parameters"></a>Dynamische Parameter
Parameter werden ein-und der Definition der Stichprobe Verkaufspipeline mit hartcodierte Werte zugewiesen. 

    "parameters": {
        "in": "/datalake/input/SearchLog.tsv",
        "out": "/datalake/output/Result.tsv"
    }

Es ist möglich, können Sie stattdessen dynamische Parameter verwenden. Beispiel: 

    "parameters": {
        "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
        "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
    }

In diesem Fall Eingabewerte Dateien werden weiterhin im Ordner /datalake/input übernommen und Ausgabedateien werden im Ordner /datalake/output generiert. Die Dateinamen sind dynamische ausgehend von der Enduhrzeit Segments.  