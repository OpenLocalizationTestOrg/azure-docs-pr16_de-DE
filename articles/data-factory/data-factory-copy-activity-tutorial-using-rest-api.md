<properties 
    pageTitle="Lernprogramm: Erstellen einer Verkaufspipeline mit Kopieren Aktivität mit REST-API | Microsoft Azure" 
    description="In diesem Lernprogramm erstellen Sie eine Verkaufspipeline Azure Data Factory mit einer Kopie Aktivität mithilfe der REST-API ein." 
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
    ms.topic="get-started-article" 
    ms.date="09/16/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-rest-api"></a>Lernprogramm: Erstellen einer Verkaufspipeline mit Kopieren Aktivität mit REST-API
> [AZURE.SELECTOR]
- [Übersicht und erforderliche Komponenten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Assistent zum Kopieren von](data-factory-copy-data-wizard-tutorial.md)
- [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure Ressourcenmanager Vorlage](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


In diesem Lernprogramm erfahren Sie, wie erstellen und Überwachen einer Azure Daten Factory die REST-API verwenden. Der Verkaufspipeline in den Daten Factory verwendet eine Kopie Aktivität zum Kopieren von Daten aus Azure BLOB-Speicher mit Azure SQL-Datenbank.

> [AZURE.NOTE] 
> In diesem Artikel befasst sich nicht auf alle die Daten Factory REST-API aus. Umfassende Dokumentation auf Daten Factory-Cmdlets finden Sie unter [Daten Factory REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn906738.aspx) .
  

## <a name="prerequisites"></a>Erforderliche Komponenten

- Aufzurufen Sie [Lernprogramm Übersicht](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) , und führen Sie die Schritte **Voraussetzung** aus.
- Installieren Sie [Aufrollen](https://curl.haxx.se/dlwiz/) auf Ihrem Computer aus. Sie können das Tool Curl mit weiteren Befehlen verwenden, um eine Factory Daten zu erstellen. 
- Führen Sie die Schritte in [diesem Artikel](../resource-group-create-service-principal-portal.md) , um: 
    1. Erstellen einer Webanwendung mit dem Namen **ADFCopyTutorialApp** in Azure Active Directory.
    2. Abrufen von **Client-ID** und **geheimen Schlüssel**. 
    3. Erste **Mandanten-ID**an. 
    4. Weisen Sie die Anwendung **ADFCopyTutorialApp** der **Daten Factory** Teilnehmerrolle.  
- Installieren der [Azure PowerShell](../powershell-install-configure.md).  
- Starten Sie **PowerShell** , und führen Sie den folgenden Befehl aus. Lassen Sie Azure PowerShell bis zum Ende des Lernprogramms geöffnet. Wenn Sie schließen und erneut öffnen, müssen Sie die Befehle erneut ausführen.
    1. Führen Sie den folgenden Befehl aus, und geben Sie den Benutzernamen und das Kennwort für die Anmeldung bei der Azure-Portal.
    
            Login-AzureRmAccount   
    2. Führen Sie den folgenden Befehl zum Anzeigen aller Abonnements für dieses Konto ein.

            Get-AzureRmSubscription 
    3. Führen Sie den folgenden Befehl aus, um das Abonnement auszuwählen, dem Sie mit arbeiten möchten. Ersetzen Sie ** &lt;NameOfAzureSubscription** &gt; durch den Namen Ihres Abonnements Azure. 

            Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    1. Erstellen einer Azure Ressourcengruppe mit dem Namen **ADFTutorialResourceGroup** , indem Sie den folgenden Befehl ausführen, in der PowerShell.  

            New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

        Wenn die Ressourcengruppe bereits vorhanden ist, geben Sie, ob es (Y) aktualisieren oder als (N) beibehalten. 

        Einige der Schritte in diesem Lernprogramm wird davon ausgegangen, dass Sie die Ressourcengruppe mit dem Namen ADFTutorialResourceGroup verwenden. Wenn Sie eine andere Ressourcengruppe verwenden, müssen Sie den Namen der Ressourcengruppe anstelle von ADFTutorialResourceGroup in diesem Lernprogramm verwenden.
  
## <a name="create-json-definitions"></a>Erstellen von JSON-Definitionen
Erstellen Sie die folgenden JSON-Dateien in den Ordner, in dem curl.exe befindet. 

### <a name="datafactoryjson"></a>DataFactory.JSON 
> [AZURE.IMPORTANT] Name muss global eindeutig sein, damit Sie möglicherweise Präfix/Suffix ADFCopyTutorialDF ausreicht einen eindeutigen Namen möchten. 

    {  
        "name": "ADFCopyTutorialDF",  
        "location": "WestUS"
    }  

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.JSON
> [AZURE.IMPORTANT] Ersetzen von **Kontoname** und **Accountkey** mit Namen und den Schlüssel Ihres Kontos Azure-Speicher. So erhalten Sie Ihre Zugriffstaste Speicher finden Sie unter [anzeigen, kopieren und neu generieren Speicher Zugriffstasten](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }

### <a name="azuersqllinkedservicejson"></a>azuersqllinkedservice.JSON
> [AZURE.IMPORTANT] Ersetzen Sie **Servername**, **Datenbankname**, **Benutzername**und **Kennwort** mit Namen Ihres SQL Azure-Servers, den Namen der SQL-Datenbank, Benutzerkonto und ein Kennwort für das Konto ein.  

    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "",
            "typeProperties": {
                "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
            }
        }
    }


### <a name="inputdatasetjson"></a>inputdataset.JSON

    {
      "name": "AzureBlobInput",
      "properties": {
        "structure": [
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
          "folderPath": "adftutorial/",
          "fileName": "emp.txt",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Die JSON-Definition definiert ein Dataset namens **AzureBlobInput**, die Eingabedaten für eine Aktivität in der Verkaufspipeline darstellt. Darüber hinaus gibt es, dass die Eingabedaten in der Datei **emp.txt** befindet, die Blob Container **Adftutorial**ist. 

 Beachten Sie die folgenden Punkte: 

- DataSet **Typ** wird auf **AzureBlob**festgelegt.
- **LinkedServiceName** wird auf **AzureStorageLinkedService**festgelegt. 
- **Ordnerpfad** ist in den Container **Adftutorial** und **FileName** auf **emp.txt**festgelegt ist.  
- **Art** der Formatierung wird auf **TextFormat** festgelegt.
- Es gibt zwei Felder in der Textdatei – **FirstName** und **LastName** – getrennt durch ein Komma (**ColumnDelimiter**) 
- Die **Verfügbarkeit** wird festgelegt, um **stündlich** (Häufigkeit auf Stunde festgelegt ist und Intervall auf 1 festgelegt ist). Daher sucht Daten Factory Eingabedaten stündlich im Stammordner des angegebenen Blob Containers (**Adftutorial**). 

Wenn Sie einen **Dateinamen** für eine Eingabe-Dataset nicht angeben, werden alle Dateien/Blobs im Ordner "Eingabe" (**Ordnerpfad**) als Eingaben angesehen. Wenn Sie einen Dateinamen in das JSON angeben, wird nur das angegebene Datei/Blob als Eingabe angesehen.

Wenn Sie einen **Dateinamen** für eine **Ausgabetabelle**nicht angeben, werden die generierten Dateien in den **Ordnerpfad** im folgenden Format benannt: Daten. &lt;Guid&gt;txt (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

Um **Ordnerpfad** und **Dateiname** dynamisch basierend auf der Uhrzeit **SliceStart** festzulegen, verwenden Sie die Eigenschaft **PartitionedBy** ein. Im folgenden Beispiel Ordnerpfad verwendet Jahr, Monat und Tag aus der SliceStart (Startzeit für das Segment des verarbeiteten) und FileName verwendet Stunde aus der SliceStart. Wenn Sie ein Segment für 2014 gefertigt wird beispielsweise-10-20T08:00:00, der Ordnername auf Wikidatagateway/Wikisampledataout/2014/10/20 festgelegt ist und der Dateinamen auf 08.csv festgelegt ist. 

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
    [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],


### <a name="outputdatasetjson"></a>outputdataset.JSON
    
    {
      "name": "AzureSqlOutput",
      "properties": {
        "structure": [
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "emp"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


Die JSON-Definition definiert ein Dataset namens **AzureSqlOutput**, die Ausgabedaten für eine Aktivität in der Verkaufspipeline darstellt. Darüber hinaus gibt es an, dass die Ergebnisse in der Tabelle gespeichert werden: **emp** in der Datenbank, die durch die AzureSqlLinkedService dargestellt werden. Im Abschnitt **Verfügbarkeit** der gibt an, dass das Ausgabe Dataset stündlich erzeugt wird (Häufigkeit: Stunde und jedes Intervall: 1) Basis.

Beachten Sie die folgenden Punkte: 

- DataSet **Typ** wird auf **AzureSQLTable**festgelegt.
- **LinkedServiceName** wird auf **AzureSqlLinkedService**festgelegt.
- **Tabellenname** ist **emp**festgelegt.
- Es gibt drei Spalten – **ID**, **FirstName**und **LastName** – in der Tabelle emp in der Datenbank ein. ID ist eine Identitätsspalte, daher Sie nur **FirstName** und **LastName** hier angeben müssen.
- Die **Verfügbarkeit** wird **stündlich** (**Häufigkeit** auf **Hour** festgelegten und **Intervall** auf **1**festgelegt) festgelegt.  Der Dienst Daten Factory generiert ein Ausgabe Daten Segments stündlich in der Tabelle **emp** der SQL Azure-Datenbank.

### <a name="pipelinejson"></a>Pipeline.JSON

    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "description": "Push Regional Effectiveness Campaign data to Azure SQL database",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureSqlOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2016-08-12T00:00:00Z",
        "end": "2016-08-13T00:00:00Z"
      }
    }


Beachten Sie die folgenden Punkte:

- Im Abschnitt Aktivitäten gibt es nur eine Aktivität, deren **Typ** auf **CopyActivity**festgelegt ist.
- Eingabe für die Aktivität auf **AzureBlobInput** festgelegt ist, und die Ausgabe für die Aktivität auf **AzureSqlOutput**festgelegt ist.
- Im Abschnitt **Transformation** **BlobSource** als den Quelltyp angegeben wird, und **SqlSink** als den Typ der Empfänger angegeben.

Ersetzen Sie den Wert der Eigenschaft **beginnen Sie** mit dem aktuellen Tag und die **Endzeit** der Wert mit dem nächsten Tag. Können Sie nur den Datumsteil angeben und den Zeitteil des Datums Uhrzeit überspringen. Beispielsweise "2015-02-03" entspricht "2015-02-03T00:00:00Z"

Beide starten und beenden Zeiteingabe muss im [ISO-Format](http://en.wikipedia.org/wiki/ISO_8601). Beispiel: 2014-10-14T16:32:41Z. **Die Endzeit** ist optional, aber wir in diesem Lernprogramm verwenden. 

Wenn Sie keinen Wert für die Eigenschaft **Ende** angeben, wird es als "**Start + 48 Stunden**" berechnet. Wenn der Verkaufspipeline endlos ausführen möchten, geben Sie als Wert für die Eigenschaft **Ende** **9999-09-09** an.

Im Beispiel werden 24 Datensegmente wie jedes Segment Daten stündlich erstellt wird.
    
> [AZURE.NOTE] [Aufbau einer Verkaufspipeline](data-factory-create-pipelines.md#anatomy-of-a-pipeline) finden Sie Details JSON-Eigenschaften, die im vorhergehenden Beispiel verwendet.

## <a name="set-global-variables"></a>Festlegen globaler Variablen

Führen Sie die folgenden Befehle in Azure PowerShell Nachdem Sie die Werte durch ein eigenes ersetzen:

> [AZURE.IMPORTANT] Finden Sie unter [Voraussetzungen für](#prerequisites) Anweisungen im Abschnitt erhalten geheim Client, Client-ID-Mandanten-ID und Abonnement-ID.   

    $client_id = "<client ID of application in AAD>"
    $client_secret = "<client key of application in AAD>"
    $tenant = "<Azure tenant ID>";
    $subscription_id="<Azure subscription ID>";

    $rg = "ADFTutorialResourceGroup"
    $adf = "ADFCopyTutorialDF"

## <a name="authenticate-with-aad"></a>Mit AAD authentifizieren 
Führen Sie den folgenden Befehl zum Authentifizieren mit Azure Active Directory (AAD). 

    $cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
    $responseToken = Invoke-Command -scriptblock $cmd;
    $accessToken = (ConvertFrom-Json $responseToken).access_token;
    
    (ConvertFrom-Json $responseToken) 

## <a name="create-data-factory"></a>Erstellen von Daten factory

In diesem Schritt erstellen Sie eine Azure Data Factory mit dem Namen **ADFCopyTutorialDF**. Eine Factory Daten kann eine oder mehrere Rohrleitungen haben. Eine Verkaufspipeline kann eine oder mehrere Aktivitäten enthalten. Angenommen, eine Kopie Aktivität zum Kopieren von Daten aus einer Quelle zu einem Ziel Datenspeicher. Eine HDInsight Struktur Aktivität Struktur Skript aus, um die Eingabedaten auf Ausgabe Produktdaten transformieren ausführen. Führen Sie die folgenden Befehle zum Erstellen der Factory Daten: 

1. Weisen Sie den Befehl mit dem Namen **Cmd**Variablen aus. 

    Bestätigen, dass der Name der Factory Daten, die Sie hier angeben (ADFCopyTutorialDF) entspricht der Name in der **datafactory.json**angegeben. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/ADFCopyTutorialDF?api-version=2015-10-01};
2. Führen Sie den Befehl **Aufrufen-Befehl**verwenden.

        $results = Invoke-Command -scriptblock $cmd;
3. Anzeigen der Ergebnisse an. Wenn die Daten Factory erfolgreich erstellt wurde, wird die JSON für die Factory Daten in die **Ergebnisse**; Andernfalls wird eine Fehlermeldung angezeigt.  

        Write-Host $results

Beachten Sie die folgenden Punkte:
 
- Der Name der Factory Daten Azure muss global eindeutig sein. Wenn Sie den Fehler in Suchergebnissen angezeigt: **Factory Data Source Name "ADFCopyTutorialDF" ist nicht verfügbar**, führen Sie die folgenden Schritte aus:  
    1. Ändern Sie den Namen (beispielsweise YournameADFCopyTutorialDF) in der Datei **datafactory.json** .
    2. Ersetzen Sie in den ersten Befehl, wo die Variable **$cmd** einen Wert zugewiesen ist ADFCopyTutorialDF durch den neuen Namen, und führen Sie den Befehl. 
    3. Führen Sie die folgenden beiden Befehle zum Aufrufen der REST-API, um die Daten Factory erstellen und Drucken der Resultate des Vorgangs. 
    
    Finden Sie unter [Data Factory - Regeln zur Benennung von](data-factory-naming-rules.md) Thema Benennungskonventionen für Daten Factory Elemente.
- Um Daten Factory-Instanzen erstellen zu können, müssen Sie ein Mitwirkender/Administrator des Abonnements Azure sein
- Der Name der Factory Daten möglicherweise als DNS-Namen in der Zukunft und somit werden öffentlich sichtbar registriert werden.
- Wenn Sie die Fehlermeldung: "**Abonnement ist nicht registriert, um den Namespace Microsoft.DataFactory verwenden**", führen Sie eine der folgenden Aktionen, und versuchen Sie erneut veröffentlichen: 

    - Führen Sie in Azure PowerShell zum Registrieren des Daten Factory-Anbieters den folgenden Befehl ein. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Sie können dem folgenden Befehl aus, um zu bestätigen, dass die Daten Factory ausführen, wenn Anbieter registriert ist. 
    
            Get-AzureRmResourceProvider
    - Melden Sie sich mit dem Azure Abonnement in der [Azure-Portal](https://portal.azure.com) und navigieren Sie zu einer Blade Daten Factory (oder) erstellen Sie eine Factory Daten im Azure-Portal. Diese Aktion registriert automatisch den Anbieter für Sie.

Vor dem Erstellen einer Verkaufspipeline, müssen Sie zuerst ein paar Daten Factory-Elemente zu erstellen. Zuerst erstellen Sie ein verknüpfte Diensten Quell- und Zielfelder Datenspeicher eine Verknüpfung zu Ihrem Datenspeicher. Klicken Sie dann definieren Sie Eingabe- und Datasets, um Daten in einer verknüpften Datenspeicher darzustellen. Erstellen Sie schließlich die Verkaufspipeline mit einer Aktivität, die diese Datasets verwendet.

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten
Verknüpfte Services Datenspeicher verknüpfen oder Dienste für eine Fabrik Azure-Daten zu berechnen. Ein Datenspeicher möglich-Speicher Azure, SQL Azure-Datenbank oder einer lokalen SQL Server-Datenbank, die Eingabedaten enthält oder Ausgabedaten für eine Fabrik Daten Verkaufspipeline speichert. Computing-Service ist der Dienst, der Eingabedaten verarbeitet und Ausgabedaten erzeugt. 

In diesem Schritt erstellen Sie zwei verknüpfte Diensten: **AzureStorageLinkedService** und **AzureSqlLinkedService**. AzureStorageLinkedService Dienst Links verknüpft ein Azure-Speicher-Konto und AzureSqlLinkedService verknüpft Azure SQL-Datenbank mit den Daten Factory: **ADFCopyTutorialDF**. Erstellen Sie eine Verkaufspipeline später in diesem Lernprogramm, die Daten aus einem Container Blob in AzuretStorageLinkedService zu einer SQL-Tabelle in AzureSqlLinkedService kopiert.

### <a name="create-azure-storage-linked-service"></a>Erstellen von Azure verknüpft Speicherdienst
In diesem Schritt verknüpfen Sie Ihr Konto Azure-Speicher mit Ihrer Daten Factory an. Mit diesem Lernprogramm verwenden Sie das Konto Azure-Speicher, um die eingegebenen Daten speichern. 

1. Weisen Sie den Befehl mit dem Namen **Cmd**Variablen aus. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@azurestoragelinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. Führen Sie den Befehl **Aufrufen-Befehl**verwenden.
 
        $results = Invoke-Command -scriptblock $cmd;
3. Anzeigen der Ergebnisse an. Wenn der verknüpfte Dienst erfolgreich erstellt wurde, wird die JSON für den verknüpften Dienst in die **Ergebnisse**; Andernfalls wird eine Fehlermeldung angezeigt.
  
        Write-Host $results

### <a name="create-azure-sql-linked-service"></a>Erstellen von SQL Azure-Verknüpfte Dienst
In diesem Schritt verknüpfen Sie die SQL Azure-Datenbank mit Ihrer Daten Factory an. Mit diesem Lernprogramm verwenden Sie die gleiche SQL Azure-Datenbank zum Speichern der Ausgabedaten aus.

1. Weisen Sie den Befehl mit dem Namen **Cmd**Variablen aus. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azuresqllinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureSqlLinkedService?api-version=2015-10-01};
2. Führen Sie den Befehl **Aufrufen-Befehl**verwenden.
 
        $results = Invoke-Command -scriptblock $cmd;
3. Anzeigen der Ergebnisse an. Wenn der verknüpfte Dienst erfolgreich erstellt wurde, wird die JSON für den verknüpften Dienst in die **Ergebnisse**; Andernfalls wird eine Fehlermeldung angezeigt.
  
        Write-Host $results

## <a name="create-datasets"></a>Datasets erstellen

Im vorherigen Schritt erstellt Sie verknüpfte Diensten **AzureStorageLinkedService** und **AzureSqlLinkedService** ein Speicher Azure-Konto und SQL Azure-Datenbank Fabrik Daten verknüpfen: **ADFCopyTutorialDF**. In diesem Schritt erstellen Sie Datasets, die die Eingabe- und Daten für die Aktivität kopieren in der Verkaufspipeline darstellen, die Sie im nächsten Schritt erstellen. 

Des Eingabe-Dataset in diesem Lernprogramm bezieht sich auf einen Container Blob in den Azure-Speicher die AzureStorageLinkedService für verweist auf die. Das Ausgabe Dataset bezieht sich auf einer SQL-Tabelle in der SQL Azure-Datenbank, die auf AzureSqlLinkedService zeigt.  

### <a name="prepare-azure-blob-storage-and-azure-sql-database-for-the-tutorial"></a>Vorbereitung des Lernprogramms Azure BLOB-Speicher und Azure SQL-Datenbank
Führen Sie die folgenden Schritte aus, um die Azure Blob-Speicher und SQL Azure-Datenbank in diesem Lernprogramm vorzubereiten. 
 
* Erstellen Sie einen Blob-Container mit dem Namen **Adftutorial** in der Azure Blob-Speicher, dem auf **AzureStorageLinkedService** verweist. 
* Erstellen und Hochladen einer Textdatei, **emp.txt**, als Blob in den Container **Adftutorial** . 
* Erstellen einer Tabelle mit dem Namen **emp** in der Azure SQL-Datenbank in der SQL Azure-Datenbank, der auf **AzureSqlLinkedService** verweist.


1. Starten Sie den Editor, fügen Sie den folgenden Text, und speichern Sie es als **emp.txt** **C:\ADFGetStartedPSH** Ordner auf Ihrer Festplatte. 

        John, Doe
        Jane, Doe
                
2. Verwenden Sie Tools wie [Azure-Speicher-Explorer](https://azurestorageexplorer.codeplex.com/) aus, um den Container **Adftutorial** erstellen und die **emp.txt** -Datei auf den Container hochzuladen.

    ![Azure-Speicher-Explorer](media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png)
3. Verwenden Sie das folgende SQL-Skript, um die Tabelle **emp** in Ihrem SQL Azure-Datenbank zu erstellen.  


        CREATE TABLE dbo.emp 
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID); 

    Wenn Sie SQL Server 2014 auf Ihrem Computer installiert haben: Anweisungen aus [Schritt2: Herstellen einer Verbindung mit der Verwaltung von Azure SQL-Datenbank mit SQL Server Management Studio SQL-Datenbank mit] [ sql-management-studio] Artikel, um eine Verbindung mit dem SQL Azure-Server, und führen Sie das Skript SQL.

    Wenn Sie Ihren Kunden Zugriff auf den SQL Azure-Server nicht zulässig ist, müssen Sie zum Konfigurieren der Firewall für Ihre SQL Azure-Server für den Zugriff von Ihrem Computer (IP-Adresse). Finden Sie [in diesem Artikel](../sql-database/sql-database-configure-firewall-settings.md) Schritte zum Konfigurieren der Firewall für Ihre SQL Azure-Server aus.
        
### <a name="create-input-dataset"></a>Erstellen von dataset 
In diesem Schritt erstellen Sie ein Dataset namens **AzureBlobInput** , die zu einem Container Blob in den Azure-Speicher dargestellt werden vom Dienst **AzureStorageLinkedService** verknüpft verweist. Diese Container Blob (**Adftutorial**) enthält die Eingabedaten in der Datei: **emp.txt**. 

1. Weisen Sie den Befehl mit dem Namen **Cmd**Variablen aus. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. Führen Sie den Befehl **Aufrufen-Befehl**verwenden.

        $results = Invoke-Command -scriptblock $cmd;
3. Anzeigen der Ergebnisse an. Wenn das Dataset erfolgreich erstellt wurde, wird die JSON für das Dataset in die **Ergebnisse**; Andernfalls wird eine Fehlermeldung angezeigt.
  
        Write-Host $results

### <a name="create-output-dataset"></a>Die Ausgabe Dataset erstellen
In diesem Schritt erstellen Sie eine Ausgabetabelle mit dem Namen **AzureSqlOutput**. Dieses Dataset verweist auf eine SQL-Tabelle (**emp**) in der SQL Azure-Datenbank, die durch **AzureSqlLinkedService**dargestellt werden. Der Verkaufspipeline werden Daten aus dem Eingabe-Blob in der Tabelle **emp** kopiert. 

1. Weisen Sie den Befehl mit dem Namen **Cmd**Variablen aus.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureSqlOutput?api-version=2015-10-01};
2. Führen Sie den Befehl **Aufrufen-Befehl**verwenden.

        $results = Invoke-Command -scriptblock $cmd;
3. Anzeigen der Ergebnisse an. Wenn das Dataset erfolgreich erstellt wurde, wird die JSON für das Dataset in die **Ergebnisse**; Andernfalls wird eine Fehlermeldung angezeigt.
  
        Write-Host $results 

## <a name="create-pipeline"></a>Erstellen der Verkaufspipeline
In diesem Schritt erstellen Sie eine Verkaufspipeline mit einer **Kopie Aktivitäten** , die **AzureBlobInput** als Eingabe verwendet und **AzureSqlOutput** in der Ausgabe.

1. Weisen Sie den Befehl mit dem Namen **Cmd**Variablen aus.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. Führen Sie den Befehl **Aufrufen-Befehl**verwenden.

        $results = Invoke-Command -scriptblock $cmd;
3. Anzeigen der Ergebnisse an. Wenn das Dataset erfolgreich erstellt wurde, wird die JSON für das Dataset in die **Ergebnisse**; Andernfalls wird eine Fehlermeldung angezeigt.  

        Write-Host $results

**Herzlichen Glückwunsch!** Sie haben eine Fabrik Azure-Daten mit einer Verkaufspipeline erstellt, die Daten aus Azure BLOB-Speicher mit SQL Azure-Datenbank kopiert.

## <a name="monitor-pipeline"></a>Monitor Verkaufspipeline
In diesem Schritt verwenden Sie Data Factory REST-API zum Überwachen der Verkaufspipeline adressierte Segmente aus.

    $ds ="AzureSqlOutput"

    $cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

    $results2 = Invoke-Command -scriptblock $cmd;


    IF ((ConvertFrom-Json $results2).value -ne $NULL) {
        ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
    } else {
            (convertFrom-Json $results2).RemoteException
    }

Führen Sie diese Befehle, bis ein Segment im **bereit** oder **fehlgeschlagen** Zustand angezeigt wird. Wenn das Segment eingeschaltet ist, überprüfen Sie die **emp** -Tabelle in der SQL Azure-Datenbank für die Ausgabedaten. 

Für jedes Segment werden zwei Zeilen mit Daten aus der Quelldatei in der Tabelle emp der SQL Azure-Datenbank kopiert. Daher sehen Sie 24 neue Datensätze in der Tabelle emp Wenn alle Segmente (in den Zustand bereit) erfolgreich verarbeitet werden. 


## <a name="summary"></a>Zusammenfassung
In diesem Lernprogramm verwendet Sie REST-API zum Erstellen einer Factory Azure-Daten so kopieren Sie Daten aus einer Azure BLOB-mit einer SQL Azure-Datenbank. Hier sind die allgemeinen Schritte, die Sie in diesem Lernprogramm ausgeführt:  

1.  Erstellt eine Azure- **Daten Factory**.
2.  Erstellt von **verknüpften Diensten**:
    1. Ein Azure verknüpft Speicherdienst auf Ihr Konto Azure-Speicher zu verknüpfen, die Eingabedaten enthält.    
    2. Einer SQL Azure verknüpft Dienst Ihrer SQL Azure-Datenbank zu verknüpfen, die die Ausgabedaten enthält. 
3.  **Datasets**, erstellt, die Eingabe von Daten und Ausgabedaten für Pipelines zu beschreiben.
4.  Erstellt eine **Verkaufspipeline** mit einer Kopie Aktivität mit BlobSource als Quell- und SqlSink als Empfänger. 

## <a name="see-also"></a>Siehe auch
| Thema | Beschreibung |
| :---- | :---- |
| [Aktivitäten zum Verschieben von Daten](data-factory-data-movement-activities.md) | Dieser Artikel enthält ausführliche Informationen zur Aktivität kopieren Sie im Lernprogramm verwendet. |
| [Planung und Ausführung](data-factory-scheduling-and-execution.md) | In diesem Artikel wird erläutert, die Planung und Ausführung Aspekte des Modells für Azure Data Factory-Anwendung. |
| [Pipelines](data-factory-create-pipelines.md) | In diesem Artikel können Sie die Pipelines und Aktivitäten in Azure Data Factory und deren Verwendung zum Erstellen von End-to-End-Daten basierende Workflows für Ihre Szenario oder Ihr Unternehmen zu verstehen. |
| [Datasets](data-factory-create-datasets.md) | In diesem Artikel können Sie die Datasets in Azure Data Factory zu verstehen.
| [Überwachen und Verwalten von Pipelines mit App für die Überwachung](data-factory-monitor-manage-app.md) | Dieser Artikel beschreibt, wie überwachen, verwalten und Debuggen Pipelines die Überwachung und Verwaltung App verwenden. 



[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908

[cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234.aspx
[old-cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234(v=azure.98).aspx
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[azure-portal]: http://portal.azure.com
[download-azure-powershell]: ../powershell-install-configure.md
[data-factory-introduction]: data-factory-introduction.md

[image-data-factory-get-started-storage-explorer]: ./media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png

[sql-management-studio]: ../sql-database/sql-database-manage-azure-ssms.md
 