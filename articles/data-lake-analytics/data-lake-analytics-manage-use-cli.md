<properties 
   pageTitle="Verwalten von Azure Daten dem Analytics Azure Line Schnittstelle | Azure" 
   description="Erfahren Sie, wie Daten dem Analytics-Konten, Datenquellen, Aufträge und Benutzer per Azure CLI verwalten" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="manage-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Verwalten von Azure Daten dem Analytics mit Azure Line Interface (CLI)

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Informationen Sie zum Verwalten von Azure Daten dem Analytics-Konten, Datenquellen, Benutzern und Aufträge mithilfe der Azure. Um Management Thema mit anderen Tools angezeigt wird, klicken Sie auf der Registerkarte wählen Sie oben.

**Erforderliche Komponenten**

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).
- **Azure CLI**. Finden Sie unter [Installieren und Konfigurieren von Azure CLI](../xplat-cli-install.md).
    - Herunterladen Sie und installieren Sie die **Vorabversion** [CLI Azure-Tools](https://github.com/MicrosoftBigData/AzureDataLake/releases) für die Durchführung dieser Demo.
- **Authentifizierung**, verwenden den folgenden Befehl aus:

        azure login
    Weitere Informationen zum Authentifizieren über ein Konto geschäftlichen oder schulnotizbücher finden Sie unter [Verbinden zu einem Azure Abonnement über die Befehlszeile Azure](../xplat-cli-connect.md).
- **Die Ressourcenmanager Azure-Modus wechseln**, verwenden den folgenden Befehl aus:

        azure config mode arm

**Listen Sie die Befehle Lake Datenspeicher und Daten dem Analytics:**

    azure datalake store
    azure datalake analytics

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-accounts"></a>Verwalten von Konten

Vor dem Ausführen von Daten dem Analytics Aufträge, müssen Sie ein Daten dem Analytics-Konto verfügen. Im Gegensatz zu Azure-HDInsight bezahlen nicht Sie für ein Konto Analytics, wenn sie ein Projekt nicht ausgeführt wird.  Sie Zahlen nur für die Zeit an, wenn sie ein Projekt ausgeführt wird.  Weitere Informationen finden Sie unter [Azure Daten dem Analytics Übersicht](data-lake-analytics-overview.md).  

###<a name="create-accounts"></a>Erstellen von Konten

    azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"


###<a name="update-accounts"></a>Aktualisieren von Konten

Mit dem folgende Befehl aktualisiert, dass Sie die Eigenschaften eines vorhandenen Daten dem Analytics-Kontos
    
    azure datalake analytics account set "<Data Lake Analytics Account Name>"


###<a name="list-accounts"></a>Liste Konten

Liste Daten Lake Analytics-Konten 

    azure datalake analytics account list

Liste Daten Lake Analytics Konten innerhalb einer bestimmten Ressourcengruppe

    azure datalake analytics account list -g "<Azure Resource Group Name>"

Anzeigen von Details eines bestimmten Daten dem Analytics-Kontos

    azure datalake analytics account show -g "<Azure Resource Group Name>" -n "<Data Lake Analytics Account Name>"

###<a name="delete-data-lake-analytics-accounts"></a>Löschen Sie die Daten dem Analytics-Konten

    azure datalake analytics account delete "<Data Lake Analytics Account Name>"


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Verwalten von Datenquellen Konto

Daten Lake Analytics unterstützt derzeit die folgenden Datenquellen:

- [Azure Lake Datenspeicher](../data-lake-store/data-lake-store-overview.md)
- [Azure-Speicher](../storage/storage-introduction.md)

Wenn Sie ein Konto Analytics erstellen, müssen Sie ein Azure Daten dem Speicher Konto werden im Speicher Standardkonto festlegen. Das ADL Speicher Standardkonto dient zum Auftrag Metadaten und Position Überwachungsprotokolle speichern. Nachdem Sie ein Konto Analytics erstellt haben, können Sie zusätzliche dem Datenspeicher Konten und/oder Speicher Azure-Konto hinzufügen. 

### <a name="find-the-default-adl-storage-account"></a>Suchen Sie das standardmäßige ADL Speicher-Konto

    azure datalake analytics account show "<Data Lake Analytics Account Name>"

Der Wert wird unter Eigenschaften: DatalakeStoreAccount:name aufgeführt.

### <a name="add-additional-azure-blob-storage-accounts"></a>Fügen Sie weitere Azure Blob-Speicher-Konten hinzu

    azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -b "<Azure Blob Storage Account Short Name>" -k "<Azure Storage Account Key>"

>[AZURE.NOTE] Nur BLOB-Speicher kurze Namen werden unterstützt.  Verwenden Sie keine vollqualifizierten Domänennamen, beispielsweise "myblob.blob.core.windows.net".

### <a name="add-additional-data-lake-store-accounts"></a>Fügen Sie weitere Lake Datenspeicher-Konten hinzu

    azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -l "<Data Lake Store Account Name>" [-d]

[-d] ist ein Optionaler Schalter, anzugeben, ob die Daten Lake hinzugefügt werden die Daten Lake Standardkonto ist. 

### <a name="update-existing-data-source"></a>Aktualisieren von vorhandenen Datenquelle

So richten Sie ein vorhandenes Lake Datenspeicher-Konto als Standard

    azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -l "<Azure Data Lake Store Account Name>" -d
      
So aktualisieren Sie eine vorhandene BLOB-Speicher kontoschlüssel:

    azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -b "<Blob Storage Account Name>" -k "<New Blob Storage Account Key>"

### <a name="list-data-sources"></a>Liste der Datenquellen:

    azure datalake analytics account show "<Data Lake Analytics Account Name>"
    
![Daten Lake Analytics Listendatenquelle](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a>Löschen von Datenquellen:

So löschen Sie ein Konto Lake Datenspeicher

    azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Azure Data Lake Store Account Name>"

So löschen Sie ein Blob-Speicher-Konto

    azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Blob Storage Account Name>"

## <a name="manage-jobs"></a>Verwalten von Projekten

Sie müssen ein Daten dem Analytics-Konto verfügen, bevor Sie ein Projekt erstellen können.  Weitere Informationen finden Sie unter [Verwalten von Daten dem Analytics-Konten](#manage-accounts).

### <a name="list-jobs"></a>Liste Aufträge

    azure datalake analytics job list -n "<Data Lake Analytics Account Name>"

![Daten Lake Analytics Listendatenquelle](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a>Abrufen von Job-details

    azure datalake analytics job show -n "<Data Lake Analytics Account Name>" -j "<Job ID>"
    
### <a name="submit-jobs"></a>Aufträge senden

> [AZURE.NOTE] Die standardmäßige Priorität eines Auftrags ist 1000 und der Standard-Grad der Parallelität für ein Projekt 1.

    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"

### <a name="cancel-jobs"></a>Aufträge abbrechen

Mithilfe des Befehls Liste Abbrechen zum Abbrechen der Auftrags-Id, und verwenden Sie dann den Auftrag suchen.

    azure datalake analytics job list -n "<Data Lake Analytics Account Name>"
    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job ID>"

## <a name="manage-catalog"></a>Katalog verwalten

U-SQL-Katalogs dient zum Strukturieren von Daten und Code, damit nach U-SQL-Skripts gemeinsam genutzt werden können. Der Katalog ermöglicht die höchste Performance mit Daten in Azure Daten Lake möglich. Weitere Informationen finden Sie unter [verwenden U-SQL-Katalog](data-lake-analytics-use-u-sql-catalog.md).
 
###<a name="list-catalog-items"></a>Katalog von Listenelementen

    #List databases
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t database

    #List tables
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t table
    
Zu den Typen gehören die Datenbank, Schema, Assembly, externe Datenquelle, Tabelle, Tabellenwertfunktion oder Tabellenstatistik.

###<a name="create-catalog-secret"></a>Katalog geheim erstellen

    azure datalake analytics catalog secret create -n "<Data Lake Analytics Account Name>" <databaseName> <hostUri> <secretName>

### <a name="modify-catalog-secret"></a>Ändern der Katalog geheim

    azure datalake analytics catalog secret set -n "<Data Lake Analytics Account Name>" <databaseName> <hostUri> <secretName>

###<a name="delete-catalog-secret"></a>Katalog geheim löschen

    azure datalake analytics catalog secrete delete -n "<Data Lake Analytics Account Name>" <databaseName> <hostUri> <secretName>

<!-- ################################ -->
<!-- ################################ -->
## <a name="use-arm-groups"></a>Verwenden der Cloud-Gruppen

Applikationen bestehen normalerweise aus vielen Komponenten, beispielsweise eine Web app, Datenbank, Datenbankserver, Speicher und 3rd Party-Dienste. Azure Ressource-Manager (Cloud) ermöglicht es Ihnen für die Arbeit mit den Ressourcen in der Anwendung als Gruppe, als eine Ressourcengruppe Azure bezeichnet. Bereitstellen können, aktualisieren, überwachen oder alle Ressourcen für eine Anwendung in einem einzigen, koordinierte Vorgang löschen. Verwenden Sie eine Vorlage für Bereitstellung und dieser Vorlage für die verschiedenen Umgebungen z. B. Tests, Staging und Herstellung arbeiten kann. Sie können die Abrechnung für Ihre Organisation verdeutlichen möchten, indem Sie die Rollup-Kosten für die gesamte Gruppe anzeigen. Weitere Informationen finden Sie unter [Azure Ressourcenmanager Übersicht](../azure-resource-manager/resource-group-overview.md). 

Ein Daten dem Analytics-Dienst kann die folgenden Komponenten umfassen:

- Azure Daten Lake Analytics-Konto
- Erforderliche Azure Daten dem Speicher Standardkonto
- Zusätzliche Azure Daten Lake Speicher-Konten
- Zusätzlicher Speicher Azure-Konten

Sie können alle diese Komponenten im Rahmen einer Gruppe von Cloud leichter verwalten erstellen.

![Azure Daten Lake Analytics-Konto und Speicher](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Ein Daten dem Analytics-Konto und die Speicherkonten abhängige müssen in derselben Azure Data Center platziert werden.
Die Cloud Gruppe kann jedoch in einem anderen Data Center befinden.  


##<a name="see-also"></a>Siehe auch 

- [Übersicht über Microsoft Azure-Daten Lake Analytics](data-lake-analytics-overview.md)
- [Erste Schritte mit Daten dem Analytics mithilfe von Azure-Portal](data-lake-analytics-get-started-portal.md)
- [Verwalten von Azure Daten dem Analytics mithilfe von Azure-Portal](data-lake-analytics-manage-use-portal.md)
- [Überwachen Sie und Behandeln von Problemen mit Azure Daten dem Analytics Aufträge mithilfe von Azure-Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

