<properties 
   pageTitle="Erste Schritte mit Azure Daten dem Analytics mithilfe der PowerShell Azure | Azure" 
   description="Erfahren Sie, wie an, erstellen Sie ein Konto Lake Datenspeicher, erstellen Sie einen Daten dem Analytics Auftrag mit U-SQL Azure-PowerShell mit, und senden Sie den Auftrag. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/21/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Lernprogramm: Erste Schritte mit Azure Daten dem Analytics mithilfe der PowerShell Azure

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Erfahren Sie, wie Sie mithilfe der PowerShell Azure Azure Daten dem Analytics-Konten erstellen, Daten dem Analytics Aufträge in [SQL-U](data-lake-analytics-u-sql-get-started.md)definieren und übermitteln Aufträge mit Daten dem analytisches Konten. Weitere Informationen zu den Daten dem Analytics finden Sie unter [Übersicht über die Azure Daten dem Analytics](data-lake-analytics-overview.md).

In diesem Lernprogramm wird einen Auftrag entwickelt, der liest einen Tabulator getrennte (TSV) Wertedatei und wandelt sie in eine Datei mit kommagetrennten Werten (CSV). Um durch die gleichen Lernprogramm mit anderen unterstützte Tools zu wechseln, klicken Sie auf den Registerkarten am oberen Rand der in diesem Abschnitt.

##<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).
- **Eine Arbeitsstationen mit Azure PowerShell**. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).
    
##<a name="create-data-lake-analytics-account"></a>Erstellen Sie die Daten dem Analytics-Konto

Sie müssen ein Daten dem Analytics-Konto verfügen, bevor Sie alle weiteren Einzelvorgänge ausführen können. Zum Erstellen einer Daten dem Analytics-Kontos müssen Sie Folgendes angeben:

- **Ressourcengruppe Azure**: A Daten dem Analytics-Konto muss innerhalb einer Azure Ressourcengruppe erstellt werden. [Ressourcenmanager Azure](../azure-resource-manager/resource-group-overview.md) ermöglicht die Arbeit mit den Ressourcen in der Anwendung als Gruppe. Sie können bereitstellen, aktualisieren oder Löschen aller Ressourcen für eine Anwendung in einem einzigen, koordinierte Vorgang.  

    Die Ressourcengruppen in Ihrem Abonnement aufgelistet:
    
        Get-AzureRmResourceGroup
    
    So erstellen Sie eine neue Ressourcengruppe

        New-AzureRmResourceGroup `
            -Name "<Your resource group name>" `
            -Location "<Azure Data Center>" # For example, "East US 2"

- **Daten Lake Analytics Kontonamen**
- **Standort**: eine der zentriert Azure-Daten, die die Daten dem Analytics unterstützt.
- **Die Daten dem standardmäßigen Konto**: jedes Konto Daten dem Analytics weist eine Daten Lake Standardkonto.

    So erstellen Sie ein neues Daten Lake-Konto

        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName "<Your Azure resource group name>" `
            -Name "<Your Data Lake account name>" `
            -Location "<Azure Data Center>"  # For example, "East US 2"

    > [AZURE.NOTE] Der Daten Lake Kontonamen darf nur Kleinbuchstaben und Zahlen enthalten.



**Erstellen eines Daten dem Analytics-Kontos**

1. Öffnen Sie von Ihrem Windows-Computer PowerShell ISE.
2. Führen Sie das folgende Skript ein:

        $resourceGroupName = "<ResourceGroupName>"
        $dataLakeStoreName = "<DataLakeAccountName>"
        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $location = "East US 2"
        
        Write-Host "Create a resource group ..." -ForegroundColor Green
        New-AzureRmResourceGroup `
            -Name  $resourceGroupName `
            -Location $location
        
        Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeStoreName `
            -Location $location 
        
        Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
        New-AzureRmDataLakeAnalyticsAccount `
            -Name $dataLakeAnalyticsName `
            -ResourceGroupName $resourceGroupName `
            -Location $location `
            -DefaultDataLake $dataLakeStoreName
        
        Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
        Get-AzureRmDataLakeAnalyticsAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeAnalyticsName  

##<a name="upload-data-to-data-lake"></a>Hochladen von Daten auf Daten Lake

In diesem Lernprogramm verarbeitet Sie einige Protokolle suchen.  Das Protokoll suchen kann entweder Daten Lake Store oder Azure Blob Storage gespeichert werden. 

Eine Beispiel einer Protokolldatei suchen wurde zu einem öffentlichen Azure Blob Container kopiert. Verwenden Sie das folgende PowerShell-Skript, um die Datei auf Ihre Arbeitsstationen herunterzuladen, und Laden Sie die Datei in das Lake Datenspeicher Standardkonto Ihres Kontos Daten dem Analytics.

    $dataLakeStoreName = "<The default Data Lake Store account name>"
    
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file. 
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force 

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

Das folgende PowerShell-Skript wird gezeigt, wie den Standardnamen Lake Datenspeicher für ein Konto Daten dem Analytics erhalten:


    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticsName).Properties.DefaultDataLakeAccount
    echo $dataLakeStoreName

>[AZURE.NOTE] Das Azure-Portal stellt eine Benutzeroberfläche, um den Beispieldatendateien in das Lake Datenspeicher Standardkonto zu kopieren. Anweisungen finden Sie unter [Erste Schritte mit Azure Daten dem Analytics Azure-Portal verwenden](data-lake-analytics-get-started-portal.md#upload-data-to-the-default-data-lake-store-account).

Außerdem können Daten Lake Analytics Azure Blob-Speicher zugreifen.  Hochladen von Daten in Azure Blob-Speicher, finden Sie unter [Verwenden von Azure PowerShell mit Azure-Speicher](../storage/storage-powershell-guide-full.md).

##<a name="submit-data-lake-analytics-jobs"></a>Senden Sie die Daten dem Analytics Aufträge

Die Daten dem Analytics Aufträge werden in die Sprache U-SQL geschrieben. Weitere Informationen zu U-SQL finden Sie unter [Erste Schritte mit U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md) und [U-SQL-Sprache-Referenz](http://go.microsoft.com/fwlink/?LinkId=691348).

**Erstellen eines Daten dem Analytics Auftrag Skripts**

- Erstellen Sie eine Textdatei mit folgenden U-SQL-Skript, und speichern Sie die Textdatei auf Ihre Arbeitsstationen:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Dieses U-SQL-Skript liest die Quelldatei mit **Extractors.Tsv()**und erstellt dann eine CSV-Datei mit **Outputters.Csv()**. 
    
    Ändern Sie die beiden Pfade nicht, es sei denn, Sie die Quelldatei in einem anderen Speicherort zu kopieren.  Daten Lake Analytics erstellt den Ausgabeordner aus, wenn es nicht vorhanden ist.
    
    Es ist einfacher Verwendung relativer Pfade für Dateien im Standardmodus Daten Lake Konten gespeichert. Sie können auch absolute Pfade verwenden.  Beispielsweise 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Sie müssen absolute Pfade Zugriff auf Dateien verknüpfte Speicherkonten verwenden.  Die Syntax für Dateien, die in verknüpften Speicher Azure-Konto gespeichert ist:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob-Container mit öffentlichen Blobs oder öffentlichen Container Zugriffsberechtigungen werden derzeit nicht unterstützt.    
    
    
**Übermitteln Sie den Auftrag**

1. Öffnen Sie von Ihrem Windows-Computer PowerShell ISE.
2. Führen Sie das folgende Skript ein:

        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $usqlScript = "c:\tutorials\data-lake-analytics\copyFile.usql"
        
        $job = Submit-AzureRmDataLakeAnalyticsJob -Name "convertTSVtoCSV" -AccountName $dataLakeAnalyticsName –ScriptPath $usqlScript 

        Wait-AdlJob -Account $dataLakeAnalyticsName -JobId $job.JobId

        Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId

    U-SQL-Skriptdatei wird im Skript am c:\tutorials\data-lake-analytics\copyFile.usql gespeichert. Aktualisieren Sie den Dateipfad entsprechend.
 
Nachdem Sie der Auftrag abgeschlossen ist, können Sie verwenden die folgenden Cmdlets, um die Datei nicht aufgeführt, und die Datei nicht herunterladen:
    
    $resourceGroupName = "<Resource Group Name>"
    $dataLakeAnalyticName = "<Data Lake Analytic Account Name>"
    $destFile = "C:\tutorials\data-lake-analytics\SearchLog-from-Data-Lake.csv"
    
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount
    
    Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -path "/Output"
    
    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "/Output/SearchLog-from-Data-Lake.csv" -Destination $destFile

## <a name="see-also"></a>Siehe auch

- Um die gleichen Lernprogramm mit anderen Tools angezeigt wird, klicken Sie auf die Registerkarte Selektoren am oberen Rand der Seite.
- Eine komplexe Abfrage finden Sie unter [Analysieren Website Protokolle Azure Daten dem Analytics verwenden](data-lake-analytics-analyze-weblogs.md).
- Um anzufangen U SQL Anwendungen entwickeln, finden Sie unter [entwickeln U-SQL-Skripts, die mit dem Datentools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL finden Sie unter [Erste Schritte mit Azure Daten dem Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md).
- Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Daten dem Analytics Azure-Portal verwenden](data-lake-analytics-manage-use-portal.md).
- Um einen Überblick über die Daten dem Analytics zu gelangen, finden Sie unter [Übersicht über die Azure Daten dem Analytics](data-lake-analytics-overview.md).

