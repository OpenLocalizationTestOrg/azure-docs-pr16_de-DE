<properties
    pageTitle="Alle Themen Daten Factory-Dienst | Microsoft Azure"
    description="Tabelle aller Themen für den Dienst Azure benannte Factory Daten, die auf http://azure.microsoft.com/documentation/articles/, Titel und Beschreibung vorhanden sind."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="data-factory"
    ms.workload="data-factory"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="spelluru"/>


# <a name="all-topics-for-azure-data-factory-service"></a>Alle Themen Azure Data Factory-Dienst

Dieses Thema enthält eine Liste jeder direkt an den **Daten Factory** -Dienst von Azure zutreffende Thema. Sie können diese Webseite nach Schlüsselwörtern suchen, mithilfe von **STRG + F**, um den aktuellen relevante Themen zu finden.




## <a name="new"></a>Neu

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 1 | [Verschieben von Daten aus Amazon Redshift Azure Data Factory verwenden](data-factory-amazon-redshift-connector.md) | Informationen Sie zum Verschieben von Daten aus Amazon Redshift Azure Data Factory verwenden. |
| 2 | [Verschieben von Daten aus Amazon einfache Speicherdienst Azure Data Factory verwenden](data-factory-amazon-simple-storage-service-connector.md) | Informationen Sie zum Verschieben von Daten aus einer einfachen Amazon-Speicherdienst (S3) Azure Data Factory verwenden. |
| 3 | [Azure-Daten Factory - Assistenten zum Kopieren von](data-factory-azure-copy-wizard.md) | Informationen Sie dazu, wie Sie den Assistenten zum Kopieren von Daten Factory Azure verwenden Sie zum Kopieren von Daten aus unterstützten Datenquellen zu senken. |
| 4 | [Lernprogramm: Erstellen Sie Ihrer erste Azure-Daten Factory mit Daten Factory REST-API](data-factory-build-your-first-pipeline-using-rest-api.md) | In diesem Lernprogramm erstellen Sie eine Stichprobe Azure Data Factory Verkaufspipeline Data Factory REST-API verwenden. |
| 5 | [Lernprogramm: Erstellen einer Verkaufspipeline mit Kopieren Aktivität mit .NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md) | In diesem Lernprogramm erstellen Sie eine Verkaufspipeline Azure Data Factory mit einer Kopie Aktivität mithilfe von .NET-API ein. |
| 6 | [Lernprogramm: Erstellen einer Verkaufspipeline mit Kopieren Aktivität mit REST-API](data-factory-copy-activity-tutorial-using-rest-api.md) | In diesem Lernprogramm erstellen Sie eine Verkaufspipeline Azure Data Factory mit einer Kopie Aktivität mithilfe der REST-API ein. |
| 7 | [Assistent zum Kopieren von Factory](data-factory-copy-wizard.md) | Informationen Sie zum Verwenden Sie den Assistenten zum Kopieren von Factory zum Kopieren von Daten aus unterstützten Datenquellen zu senken. |
| 8 | [Datenverwaltungsgateway](data-factory-data-management-gateway.md) | Richten Sie ein datenverwaltungsgateway zum Verschieben von Daten zwischen lokal und in der Cloud. Verwenden Sie Datenverwaltungsgateway in Azure Data Factory, um Ihre Daten zu wechseln. |
| 9 | [Verschieben von Daten aus einer lokalen Cassandra-Datenbank mit Azure Data Factory](data-factory-onprem-cassandra-connector.md) | Informationen Sie zum Verschieben von Daten aus einer lokalen Cassandra Datenbank Azure Data Factory verwenden. |
| 10 | [Verschieben von Daten aus MongoDB Azure Data Factory verwenden](data-factory-on-premises-mongodb-connector.md) | Informationen Sie zum Verschieben von Daten aus Azure Data Factory mit MongoDB-Datenbank. |
| 11 | [Verschieben von Daten aus Vertrieb mithilfe von Azure Data Factory](data-factory-salesforce-connector.md) | Informationen Sie zum Verschieben von Daten mithilfe von Azure Data Factory aus Vertrieb. |


## <a name="updated-articles-data-factory"></a>Aktualisierte Artikel, Daten Factory

Dieser Abschnitt listet Artikeln, die vor kurzem aktualisiert wurden, in dem die Aktualisierung wurde, große oder erheblichen. Für jede aktualisierte Artikel ist ein grober Ausschnitt des Texts hinzugefügten Abzug angezeigt. Artikel wurden in den Datumsbereich **2016-08-22** zu **2016-10-05**aktualisiert.

| &nbsp; | Artikel | Aktualisierte Text, Codeausschnitt | Wann aktualisiert |
| --: | :-- | :-- | :-- |
| 12 | [Azure Daten Factory - API .NET Änderungsprotokoll](data-factory-api-change-log.md) | Dieser Artikel enthält Informationen zu Azure Data Factory SDK in einer bestimmten Version. Finden Sie das neueste NuGet-Paket für Azure Daten Factory hier (https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) **Version 4.11.0** Leistungsmerkmale: / die folgenden Arten von verknüpften Dienst hinzugefügt wurden: - OnPremisesMongoDbLinkedService (https://msdn.microsoft.com/library/mt765129.aspx) - AmazonRedshiftLinkedService (https://msdn.microsoft.com/library/mt765121.aspx) - AwsAccessKeyLinkedService (https://msdn.microsoft.com/library/mt765144.aspx) / die folgenden Arten von Dataset hinzugefügt wurden: - MongoDbCollectionDataset (https://msdn.microsoft.com/library/mt765145.aspx) - AmazonS3Dataset (https://msdn.microsoft.com/library/mt765112.aspx) / die folgenden Arten von kopieren Quelle hinzugefügt wurden :-MongoDbSource (https://msdn.microsoft.com/en-US/library/mt765123.aspx) **Version 4.10.0** / TextFormat die folgenden optionalen Eigenschaften hinzugefügt wurden:-Ski | 2016-09-22 |
| 13 | [Verschieben von Daten an und von Azure Blob Azure Data Factory verwenden](data-factory-azure-blob-connector.md) |  / CopyBehavior / definiert das Verhalten beim Kopieren aus, wenn die Quelle BlobSource oder Dateisystem ist.  / **PreserveHierarchy:** behält die Hierarchie im Zielordner. Der relative Pfad der Quelldatei in Quellordner ist der relative Pfad der Zieldatei zum Zielordner... Br /... Br /. **FlattenHierarchy:** alle Dateien im Ordner "Datenquelle" in die erste Ebene von Zielordner sind. Die Zieldateien haben Namen automatisch generiert. .ru /... Br /. **MergeFiles: (Standard)** werden alle Dateien aus dem Quellordner zu einer Datei zusammengeführt. Wenn der Name der Datei/Blob angegeben wird, wäre der verbundenen Dateiname den angegebenen Namen; Andernfalls wäre automatisch generierten Dateinamen.  / Nicht / **BlobSource** unterstützt auch diese beiden Eigenschaften für Abwärtskompatibilität. / **TreatEmptyAsNull**: Gibt an, ob null oder eine leere Zeichenfolge als null-Wert zu behandeln. / **SkipHeaderLineCount** - gibt an, wie viele Zeilen übersprungen werden müssen. Es gilt nur beim Eingabe-Dataset TextFormat verwendet wird. Entsprechend unterstützt **BlobSink** ten | 2016-09-28 |
| 14 | [Erstellen von Vorhersagen Pipelines mit Azure maschinellen Learning Aktivitäten](data-factory-azure-ml-batch-execution-activity.md) | **Webdienst erfordert mehrere Eingaben** Wenn der Webdienst mehrere Eingaben akzeptiert, verwenden Sie die Eigenschaft **WebServiceInputs** anstelle von **WebServiceInput**aus. Datasets, die von der **WebServiceInputs** verwiesen wird, muss auch in die Aktivität **Eingaben**enthalten sein. In Ihrem Versuch Azure ML haben Web Service Eingabe und Ausgangsports und globale Parameter Standardnamen ("input1", "input2"), die Sie anpassen können. Die Namen, die Sie für WebServiceInputs, WebServiceOutputs und GlobalParameters Einstellungen verwenden, müssen exakt Namen in die Versuche übereinstimmen. Sie können auf der Seite Stapel Ausführung Hilfe für Ihre Azure ML Endpunkt zur Überprüfung der erwarteten Zuordnung der Stichprobe Anforderungsnutzlast anzeigen.    {"Name": "PredictivePipeline", "Eigenschaften": {"Beschreibung": "verwenden Sie AzureML-Modell", "Aktivitäten": {"Name": "MLActivity", "Typ": "AzureMLBatchExecution", "Beschreibung": "Vorhersage Analyse Blattnamen Eingabemethoden", "Eingaben": {"Name": "inputDataset1"}, {"Name": "InputDatase | 2016-09-13 |
| 15 | [Aktivität Leistung und Videogeräten Führungslinie kopieren](data-factory-copy-activity-performance.md) | 1. **Einrichten eines Basisplans**. Testen Sie während der Entwicklungsphase der Verkaufspipeline anhand einer Stichprobe Vertreter Daten mithilfe von kopieren Aktivität. Die Daten Factory segmentieren Modell (Daten-Factory-Planung-und-execution.md Time-series-datasets-and-data-slices) können Sie um die Menge der Daten zu beschränken, mit denen Sie arbeiten.  Erfassen von Ausführung dauert und Leistungsmerkmale mithilfe der **Überwachung und Verwaltung App**. Wählen Sie auf der Startseite Daten Factory **Überwachen und verwalten** aus. Wählen Sie in der Strukturansicht **Ausgabe Dataset**aus. Wählen Sie in der Liste **Aktivität Windows** die Kopie Aktivität ausführen. **Aktivitäten in Windows** -Listen die Dauer der Aktivität kopieren und die Größe der Daten, die kopiert werden. Der Durchsatz wird im **Windows-Explorer Aktivität**aufgeführt. Weitere Informationen über die app finden Sie unter Überwachen und Verwalten von Azure Data Factory Pipelines mithilfe der Überwachung und Verwaltung App (Daten-Factory-Monitor-verwalten-app.md).  ! Führen Sie die Details (./media/data-factory-copy-activity-performance/mmapp-activity-run-details.pn Aktivität | 2016-09-27 |
| 16 | [Lernprogramm: Erstellen einer Verkaufspipeline mit Kopieren Aktivität mit Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) |   Beachten Sie die folgenden Punkte:-Dataset **Typ** auf **AzureBlob**festgelegt ist.     - **LinkedServiceName** auf **AzureStorageLinkedService**festgelegt ist. Sie erstellt diesen Dienst verknüpfte in Schritt2 aus.     - **Ordnerpfad** auf den Container **Adftutorial** festgelegt ist. Sie können auch den Namen eines Blob innerhalb des Ordners mithilfe der Eigenschaft **FileName** angeben. Da Sie den Namen des Blob nicht angeben, werden Daten aus allen Blobs im Container als eine Eingabedaten angesehen.    -Format **Typ** auf **TextFormat** festgelegt ist – es gibt zwei Felder in der Text Datei ΓÇô **FirstName** und **LastName** ΓÇô getrennt durch ein Komma (**ColumnDelimiter**) – die **Verfügbarkeit** zu **stündlich** festgelegt ist (**Häufigkeit** auf **Stunde** festgelegt ist und **Intervall** auf **1**festgelegt ist). Daher sucht Daten Factory Eingabedaten stündlich in den Stammordner des Blob Container (**Adftutorial**), die Sie angegeben haben.  Wenn Sie einen **Dateinamen** für eine **Eingabe** -Dataset nicht angeben, werden alle Dateien/Blobs im Ordner "Eingabe" (**Ordnerpfad**) consid | 2016-09-29 |
| 17 | [Erstellen, überwachen und Verwalten von Azure Daten Factory mit Daten Factory .NET SDK](data-factory-create-data-factories-programmatically.md) | Notieren Sie die Anwendung-ID und das Kennwort (Client geheim), und verwenden Sie diese in die exemplarische Vorgehensweise zu. **Erste Azure-Abonnement und Mandanten IDs** Wenn Sie nicht neueste Version von PowerShell auf Ihrem Computer installiert haben, führen Sie die Anweisungen zum Installieren und Konfigurieren von Azure PowerShell (... / Powershell-Installation-configure.md) Artikel, um es zu installieren. 1. Starten Sie Azure PowerShell, und führen Sie den folgenden Befehl 2. Führen Sie den folgenden Befehl aus, und geben Sie den Benutzernamen und das Kennwort für die Anmeldung bei der Azure-Portal.         Login-AzureRmAccount, wenn Sie nur ein mit diesem Konto verknüpfte Azure-Abonnement haben, müssen Sie nicht die folgenden beiden Schritte ausführen. 3. Führen Sie den folgenden Befehl zum Anzeigen aller Abonnements für dieses Konto ein.       Get-AzureRmSubscription 4. Führen Sie den folgenden Befehl aus, um das Abonnement auszuwählen, dem Sie mit arbeiten möchten. Ersetzen Sie **NameOfAzureSubscription** durch den Namen Ihres Abonnements Azure ein.       Get-AzureRmSubscription - SubscriptionName NameOfAzureSubscription / Set-AzureRmCo | 2016-09-14 |
| 18 | [Pipelines und Aktivitäten in Factory Azure-Daten](data-factory-create-pipelines.md) |       "start": "2016-07-12T00:00:00Z", "Ende": "2016-07-13T00:00:00Z"}} Beachten Sie die folgenden Punkte: / im Abschnitt Aktivitäten vorhanden ist nur eine Aktivität zu **Kopieren**, deren **Typ** festgelegt ist. / Eingabemethoden für die Aktivität auf **InputDataset** festgelegt ist, und die Ausgabe für die Aktivität auf **OutputDataset**festgelegt ist. / Im Abschnitt **TypeProperties** **BlobSource** als den Quelltyp angegeben und den Typ der Empfänger **SqlSink** angegeben ist. Umfassende Informationen zum Erstellen dieses Verkaufspipeline von, finden Sie unter Lernprogramm: Kopieren von Daten aus dem Blob-Speicher mit SQL-Datenbank (Data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). **Beispiel für Transformation Verkaufspipeline** In der folgenden Beispiel Verkaufspipeline gibt es eine Aktivität vom Typ **HDInsightHive** im Abschnitt **Aktivitäten** . In diesem Beispiel transformiert die Aktivität HDInsight Struktur (Daten-Factory-Struktur-activity.md) Daten aus einer Azure Blob-Speicher auf einem Cluster Azure HDInsight Hadoop eine Struktur Skript-Datei ausführen.  {"Name": "TransformPipeline", "p | 2016-09-27 |
| 19 | [Daten in Azure Data Factory transformieren](data-factory-data-transformation-activities.md) | Daten Factory unterstützt die folgenden Daten Transformationsaktivitäten, die hinzugefügt werden können Sie Rohrleitungen (Daten-Factory-erstellen-pipelines.md) entweder einzeln oder mit einer anderen Aktivität verkettet. .  AZURE. Hinweis eine exemplarische Vorgehensweise mit einer schrittweisen Anleitung finden Sie unter Erstellen einer Verkaufspipeline mit Struktur Transformation (Data-factory-build-your-first-pipeline.md) Artikel. **HDInsight Struktur Aktivität** Die Struktur HDInsight Aktivität in einer Daten Factory Verkaufspipeline führt Struktur Abfragen auf Ihrer eigenen oder bei Bedarf Windows, Linux-basierten HDInsight Cluster aus. Finden Sie im Artikel (Daten-Factory-Struktur-activity.md) Struktur Aktivität Details dieser Aktivität. **HDInsight Schwein Aktivität** Die Aktivität HDInsight Schwein in einer Daten Factory Verkaufspipeline führt Schwein Abfragen auf Ihrer eigenen oder bei Bedarf Windows, Linux-basierten HDInsight Cluster aus. Finden Sie im Artikel Schwein Aktivität (Daten-Factory-Schwein-activity.md) Details dieser Aktivität. **HDInsight MapReduce Aktivität** Die Aktivität HDInsight MapReduce in einer Daten Factory Verkaufspipeline führt MapReduce Programme für y | 2016-09-26 |
| 20 | [Daten Factory Planung und Ausführung](data-factory-scheduling-and-execution.md) | CopyActivity2 würde nur ausgeführt, wenn der CopyActivity1 erfolgreich ausgeführt wurde und Datensatz2 zur Verfügung steht. Hier die Stichprobe Verkaufspipeline JSON: {"Name": "ChainActivities", "Eigenschaften": {"Beschreibung": "Ausführen Aktivitäten nacheinander", "Aktivitäten": {"Typ": "Kopieren", "TypeProperties": {"Quelle": {"Typ": "BlobSource"}, "Empfänger": {"Typ": "BlobSink", "CopyBehavior": "PreserveHierarchy", "WriteBatchSize": 0 "WriteBatchTimeout": "00: 00:00"}}, "Eingaben": {"Name": "Dataset1"}, "gibt": {"Name": "Datensatz2"}, "Richtlinie": {"Timeout": "01: 00:00"}, "Zeitplan": {"Häufigkeit": "Stunde", "Intervall": 1}, "Name": "CopyFromBlob1ToBlob2", "Beschreibung": "Kopieren von Daten von einem Blob in eine andere"}, {"Typ": "Kopieren", "TypeProperties": {"Quelle": {"Typ": "BlobSource"}, "Empfänger" : {"Typ": "BlobSink", "WriteBatchSize": 0 "WriteBatchTimeout": "00: 00:00"}}, "in | 2016-09-28 |





## <a name="tutorials"></a>Lernprogramme

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 21 | [Lernprogramm: Erstellen Sie Ihrer ersten Verkaufspipeline zum Verarbeiten von Daten mithilfe von Hadoop cluster](data-factory-build-your-first-pipeline.md) | In diesem Lernprogramm Azure Data Factory wird gezeigt, wie erstellen und planen eine Factory von Daten, die Daten mit Struktur Skript in einem Cluster Hadoop verarbeitet werden können. |
| 22 | [Lernprogramm: Erstellen Sie Ihrer erste Azure-Daten Factory mit Ressourcenmanager Azure-Vorlage](data-factory-build-your-first-pipeline-using-arm.md) | In diesem Lernprogramm erstellen Sie eine Stichprobe Azure Data Factory Verkaufspipeline mithilfe einer Vorlage Azure Ressourcenmanager. |
| 23 | [Lernprogramm: Erstellen Sie Ihrer erste Azure-Daten Factory mit Azure-portal](data-factory-build-your-first-pipeline-using-editor.md) | In diesem Lernprogramm erstellen Sie eine Stichprobe Azure Data Factory Verkaufspipeline mit Daten Factory-Editor in der Azure-Portal an. |
| 24 | [Lernprogramm: Erstellen Sie Ihrer erste Azure-Daten Factory mithilfe der PowerShell Azure](data-factory-build-your-first-pipeline-using-powershell.md) | In diesem Lernprogramm erstellen Sie eine Stichprobe Azure Data Factory Verkaufspipeline Azure PowerShell verwenden. |
| 25 | [Lernprogramm: Erstellen Ihrer ersten Azure Daten Factory mithilfe von Microsoft Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) | In diesem Lernprogramm erstellen Sie eine Stichprobe Azure Data Factory Verkaufspipeline mit Visual Studio. |
| 26 | [Lernprogramm: Erstellen einer Verkaufspipeline mit Kopieren Aktivität mit Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) | In diesem Lernprogramm erstellen Sie eine Verkaufspipeline Azure Data Factory mit einer Kopie Aktivität mithilfe der Daten Factory-Editor im Azure-Portal. |
| 27 | [Lernprogramm: Erstellen einer Verkaufspipeline mit Kopieren Aktivität mithilfe der PowerShell Azure](data-factory-copy-activity-tutorial-using-powershell.md) | In diesem Lernprogramm erstellen Sie eine Verkaufspipeline Azure Data Factory mithilfe der PowerShell Azure mit einer Aktivität kopieren. |
| 28 | [Lernprogramm: Erstellen einer Verkaufspipeline mit Kopieren Aktivität mit Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) | In diesem Lernprogramm erstellen Sie eine Verkaufspipeline Azure Data Factory mit einer Kopie Aktivität mit Visual Studio. |
| 29 | [Kopieren Sie Daten aus Blob-Speicher mit SQL-Datenbank mit Daten Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | In diesem Lernprogramm erfahren Sie, wie in einer Azure Data Factory Verkaufspipeline Aktivität kopieren verwendet, zum Kopieren von Daten aus dem Blob-Speicher mit SQL-Datenbank. |
| 30 | [Lernprogramm: Erstellen einer Verkaufspipeline mit Kopieren Aktivität mithilfe des Assistenten zum Kopieren von Factory](data-factory-copy-data-wizard-tutorial.md) | In diesem Lernprogramm erstellen Sie eine Verkaufspipeline Azure Data Factory mit einer Kopie Aktivität mithilfe des Assistenten zum Kopieren von Daten Factory unterstützt |



## <a name="data-movement"></a>Verschieben von Daten

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 31 | [Verschieben von Daten an und von Azure Blob Azure Data Factory verwenden](data-factory-azure-blob-connector.md) | Informationen Sie zum Kopieren von BLOB-Daten in Azure Data Factory. Verwenden Sie unseren Beispiel: So kopieren Sie Daten an und von Azure BLOB-Speicher und Azure SQL-Datenbank. |
| 32 | [Verschieben von Daten an und von Azure Lake Datenspeicher Azure Data Factory verwenden](data-factory-azure-datalake-connector.md) | Informationen Sie zum Verschieben von Daten zu/aus Azure Lake Datenspeicher Azure Data Factory verwenden |
| 33 | [Verschieben von Daten an und von DocumentDB mit Azure Data Factory](data-factory-azure-documentdb-connector.md) | Erfahren Sie, wie Verschieben von Daten zu/aus Azure DocumentDB Sammlung mit Azure Data Factory |
| 34 | [Verschieben von Daten an und von Azure SQL-Datenbank mit Azure Data Factory](data-factory-azure-sql-connector.md) | Informationen Sie zum Verschieben von Daten aus/mit Azure Data Factory Azure SQL-Datenbank. |
| 35 | [Verschieben von Daten an und von Azure SQL-Data Warehouse Azure Data Factory verwenden](data-factory-azure-sql-data-warehouse-connector.md) | Informationen Sie zum Verschieben von Daten aus/mit Azure Data Factory Data Warehouse mit SQL Azure |
| 36 | [Verschieben von Daten an und von Azure-Tabelle mit Azure Data Factory](data-factory-azure-table-connector.md) | Informationen Sie zum Verschieben von Daten zu/aus Azure Table Storage Azure Data Factory verwenden. |
| 37 | [Aktivität Leistung und Videogeräten Führungslinie kopieren](data-factory-copy-activity-performance.md) | Lernen Sie wichtige Faktoren, die Einfluss auf die Leistung von Verschieben von Daten in Azure Data Factory bei der Verwendung von Aktivität kopieren. |
| 38 | [Verschieben von Daten mithilfe von kopieren Aktivität](data-factory-data-movement-activities.md) | Erfahren Sie mehr über das Verschieben von Daten in Daten Factory Pipelines: Migration von Daten zwischen Cloud speichern und zwischen einer lokalen Store und einem Cloud-Speicher. Verwenden Sie Aktivität kopieren. |
| 39 | [Versionsinformationen für das Datenverwaltungsgateway](data-factory-gateway-release-notes.md) | Daten Management Gateway Tory – Versionsinformationen |
| 40 | [Verschieben von Daten aus lokalen HDFS Azure Data Factory verwenden](data-factory-hdfs-connector.md) | Informationen Sie zum Verschieben von Daten aus lokalen HDFS Azure Data Factory verwenden. |
| 41 | [Überwachen und Verwalten von Azure Data Factory Pipelines mit neuen Überwachung und Verwaltung-App](data-factory-monitor-manage-app.md) | Informationen Sie zum Verwenden von Überwachung und Verwaltung App zum Überwachen und Verwalten von Azure Daten Factory und Pipelines. |
| 42 | [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md) | Richten Sie ein datenverwaltungsgateway zum Verschieben von Daten zwischen lokal und in der Cloud. Verwenden Sie Datenverwaltungsgateway in Azure Data Factory, um Ihre Daten zu wechseln. |
| 43 | [Verschieben von Daten aus einem OData-Quelle Azure Data Factory verwenden](data-factory-odata-connector.md) | Informationen Sie zum Verschieben von Daten aus Azure Data Factory mit OData-Quellen. |
| 44 | [Verschieben von Daten aus ODBC-Datenspeicher Azure Data Factory verwenden](data-factory-odbc-connector.md) | Informationen Sie zum Verschieben von Daten aus ODBC-Datenspeicher Azure Data Factory verwenden. |
| 45 | [Verschieben von Daten aus DB2 Azure Data Factory verwenden](data-factory-onprem-db2-connector.md) | Weitere Informationen über das Verschieben von Daten aus Azure Data Factory mit DB2-Datenbank |
| 46 | [Verschieben von Daten mithilfe von Azure Data Factory an und von einem lokalen Dateisystem](data-factory-onprem-file-system-connector.md) | Erfahren Sie, wie Daten mithilfe von Azure Data Factory an und von einem lokalen Dateisystem verschieben. |
| 47 | [Verschieben von Daten aus MySQL Azure Data Factory verwenden](data-factory-onprem-mysql-connector.md) | Informationen Sie zum Verschieben von Daten aus Azure Data Factory mit MySQL-Datenbank. |
| 48 | [Verschieben von Daten zu/aus lokalen Oracle Azure Data Factory verwenden](data-factory-onprem-oracle-connector.md) | Informationen Sie zum Verschieben von Daten zu/aus der Oracle-Datenbank, die lokal Azure Data Factory verwenden. |
| 49 | [Verschieben von Daten aus PostgreSQL Azure Data Factory verwenden](data-factory-onprem-postgresql-connector.md) | Informationen Sie zum Verschieben von Daten aus Azure Data Factory mit PostgreSQL-Datenbank. |
| 50 | [Verschieben von Daten aus Azure Data Factory mit Sybase](data-factory-onprem-sybase-connector.md) | Informationen Sie zum Verschieben von Daten aus Azure Data Factory mit Sybase-Datenbank. |
| 51 | [Verschieben von Daten aus Azure Data Factory mit Teradata](data-factory-onprem-teradata-connector.md) | Informationen Sie zu Teradata-Connector für den Daten Factory-Dienst, in der Sie die Daten aus Teradata-Datenbank verschieben können |
| 52 | [Verschieben von Daten zu und aus SQL Server lokal oder auf IaaS (Azure virtueller Computer) mit Azure Data Factory](data-factory-sqlserver-connector.md) | Informationen Sie zum Verschieben von Daten in SQL Server-Datenbank, die lokal ist oder in einer Azure virtueller Computer Azure Data Factory verwenden. |
| 53 | [Verschieben von Daten aus einer Web Tabellenquelle Azure Data Factory verwenden](data-factory-web-table-connector.md) | Informationen Sie zum Verschieben von Daten aus lokalen einer Tabelle in eine Webseite Azure Data Factory verwenden. |



## <a name="data-transformation"></a>Datentransformation

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 54 | [Erstellen von Vorhersagen Pipelines mit Azure maschinellen Learning Aktivitäten](data-factory-azure-ml-batch-execution-activity.md) | Beschreibt das Erstellen von erstellen Vorhersage Pipelines mit Azure Data Factory und Azure-Computer-Schulung |
| 55 | [Berechnen von verknüpften Diensten](data-factory-compute-linked-services.md) | Erfahren Sie, dass Sie in Azure Data Factory Pipelines verwenden können, um Transformation-Prozess Daten Enviornments berechnen. |
| 56 | [Prozess umfangreichen Datasets mit Daten Factory und Stapel](data-factory-data-processing-using-batch.md) | Beschrieben, wie Sie große Datenmengen in einer Azure Data Factory Verkaufspipeline mithilfe der parallele Verarbeitung Videofunktionen von Azure Stapel verarbeiten. |
| 57 | [Daten in Azure Data Factory transformieren](data-factory-data-transformation-activities.md) | Erfahren Sie, wie Daten oder Verarbeiten von Daten in Azure Data Factory von Hadoop, Computer Learning oder Azure Daten dem Analytics transformieren. |
| 58 | [Hadoop Streaming Aktivität](data-factory-hadoop-streaming-activity.md) | Erfahren Sie, wie Sie die Hadoop Streaming Aktivität in einer Factory Azure-Daten verwenden können, Programme Hadoop Streaming auf einem auf – Nachfrage/Ihre eigenen HDInsight Cluster ausführen. |
| 59 | [Struktur Aktivität](data-factory-hive-activity.md) | Erfahren Sie, wie Sie die Struktur Aktivität in einer Factory Azure-Daten verwenden können, Struktur Abfragen für eine auf – Nachfrage/Ihre eigenen HDInsight Cluster ausgeführt. |
| 60 | [Rufen Sie MapReduce Programme von Daten Factory](data-factory-map-reduce.md) | Erfahren Sie, wie Daten zu verarbeiten, indem MapReduce Programme aus einer Factory Azure-Daten auf einem Cluster Azure HDInsight ausgeführt. |
| 61 | [Schwein Aktivität](data-factory-pig-activity.md) | Erfahren Sie, wie Sie die Aktivität Schwein in einer Factory Azure-Daten verwenden können, eine auf – Nachfrage/Ihre eigenen HDInsight Cluster Schwein Skripts auszuführen. |
| 62 | [Rufen Sie Spark Programme von Daten Factory](data-factory-spark.md) | Informationen Sie zum Aufrufen Spark Programme von einer Azure Daten Factory mithilfe der MapReduce Aktivität. |
| 63 | [SQLServer gespeicherte Prozedur für die Aktivität](data-factory-stored-proc-activity.md) | Erfahren Sie, wie Sie die SQL Server gespeicherte Prozedur Aktivität verwenden können, um eine gespeicherte Prozedur in einer SQL Azure-Datenbank oder Azure SQL-Data Warehouse aus einer Data Factory Verkaufspipeline aufzurufen. |
| 64 | [Verwenden Sie benutzerdefinierte Aktivitäten in einer Azure Data Factory Verkaufspipeline](data-factory-use-custom-activities.md) | Informationen Sie zum Erstellen von benutzerdefinierten Aktivitäten und deren Verwendung in einer Azure Data Factory Verkaufspipeline. |
| 65 | [U-SQL-Skript Azure Daten dem Analytics aus Azure Data Factory ausgeführt](data-factory-usql-activity.md) | Erfahren Sie, wie Daten zu verarbeiten, indem U-SQL-Skripts auf Azure Daten dem Analytics Computing-Service ausführen. |



## <a name="samples"></a>Beispiele

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 66 | [Azure-Daten Factory - Beispiele](data-factory-samples.md) | Finden Informationen zu Beispielen, in denen liefern mit dem Azure Data Factory-Dienst an. |



## <a name="use-cases"></a>Verwenden von Fällen

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 67 | [Kunden-Fallstudien](data-factory-customer-case-studies.md) | Erfahren Sie, wie unsere Kunden einige Azure Data Factory verwendet haben. |
| 68 | [Verwenden von Case - Kunden ein Profil erstellen](data-factory-customer-profiling-usecase.md) | Erfahren Sie, wie Azure Data Factory zum Erstellen eines Workflows Daten leistungsgesteuert (Verkaufspipeline), wenn Sie ein Profil Spiele Kunden verwendet wird. |
| 69 | [Verwenden von Case - Produkt Empfehlungen](data-factory-product-reco-usecase.md) | Informationen Sie zu einer Anwendungsfall-mithilfe von Azure Data Factory zusammen mit anderen Diensten implementiert. |



## <a name="monitor-and-manage"></a>Überwachen und verwalten

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 70 | [Überwachen und Verwalten von Azure Data Factory pipelines](data-factory-monitor-manage-pipelines.md) | Erfahren Sie, wie Sie Azure-Portal und Azure PowerShell zum Überwachen und Verwalten von Azure Daten Factory und Pipelines, die Sie erstellt haben. |



## <a name="sdk"></a>SDK

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 71 | [Azure Daten Factory - API .NET Änderungsprotokoll](data-factory-api-change-log.md) | Beschreibt grundlegende Änderungen, die zusätzlichen Features beschrieben, die Updates usw.... in einer bestimmten Version von .NET API für die Azure-Daten Factory. |
| 72 | [Erstellen, überwachen und Verwalten von Azure Daten Factory mit Daten Factory .NET SDK](data-factory-create-data-factories-programmatically.md) | Erfahren Sie, wie programmgesteuert erstellen, überwachen und Verwalten von Factory Azure-Daten mithilfe von Data Factory SDK. |
| 73 | [Factory-Entwicklerreferenz Azure-Daten](data-factory-sdks.md) | Informationen Sie zu den verschiedenen Methoden zum Erstellen, überwachen und Verwalten von Factory Azure-Daten |



## <a name="miscellaneous"></a>Verschiedene

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 74 | [Azure-Daten Factory - häufig gestellte Fragen](data-factory-faq.md) | Häufig gestellte Fragen zur Azure-Daten Factory ein. |
| 75 | [Azure-Daten Factory - Funktionen und Systemvariablen](data-factory-functions-variables.md) | Enthält eine Liste der Azure-Daten Factory-Funktionen und Systemvariablen |
| 76 | [Azure-Daten Factory - Benennung von Regeln](data-factory-naming-rules.md) | Beschreibt Benennungskonventionen für Daten Factory Personen an. |
| 77 | [Behandeln von Problemen mit Daten Factory](data-factory-troubleshoot.md) | Informationen Sie zum Behandeln von Problemen im Zusammenhang mit Azure Data Factory. |

