<properties 
   pageTitle="Erste Schritte mit Azure Daten dem Analytics Azure Line Schnittstelle | Microsoft Azure" 
   description="Informationen Sie zum Verwenden der Azure Line-Benutzeroberfläche zum Erstellen eines Kontos Lake Datenspeicher mit SQL-U Daten dem Analytics Auftrag erstellen, und senden Sie den Auftrag. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Lernprogramm: Erste Schritte mit Azure Daten dem Analytics mit Azure Line Interface (CLI)

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Informationen Sie zum Verwenden von Azure CLI Azure Daten dem Analytics-Konten erstellen, Daten dem Analytics Aufträge in [SQL-U](data-lake-analytics-u-sql-get-started.md)definieren, und senden Aufträge mit Daten dem Analytics-Konten. Weitere Informationen zu den Daten dem Analytics finden Sie unter [Übersicht über die Azure Daten dem Analytics](data-lake-analytics-overview.md).

In diesem Lernprogramm wird einen Auftrag entwickelt, der liest einen Tabulator getrennte (TSV) Wertedatei und wandelt sie in eine Datei mit kommagetrennten Werten (CSV). Um durch die gleichen Lernprogramm mit anderen unterstützte Tools zu wechseln, klicken Sie auf den Registerkarten am oberen Rand der in diesem Abschnitt.

##<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).
- **Azure CLI**. Finden Sie unter [Installieren und Konfigurieren von Azure CLI](../xplat-cli-install.md).
    - Herunterladen Sie und installieren Sie die **Vorabversion** [CLI Azure-Tools](https://github.com/MicrosoftBigData/AzureDataLake/releases) für die Durchführung dieser Demo.
- **Authentifizierung**, verwenden den folgenden Befehl aus:

        azure login
    Weitere Informationen zum Authentifizieren über ein Konto geschäftlichen oder schulnotizbücher finden Sie unter [Verbinden zu einem Azure Abonnement über die Befehlszeile Azure](../xplat-cli-connect.md).
- **Die Ressourcenmanager Azure-Modus wechseln**, verwenden den folgenden Befehl aus:

        azure config mode arm
        
## <a name="create-data-lake-analytics-account"></a>Erstellen Sie die Daten dem Analytics-Konto

Sie müssen ein Daten dem Analytics-Konto verfügen, bevor Sie alle weiteren Einzelvorgänge ausführen können. Zum Erstellen einer Daten dem Analytics-Kontos müssen Sie Folgendes angeben:

- **Ressourcengruppe Azure**: A Daten dem Analytics-Konto muss innerhalb einer Azure Ressourcengruppe erstellt werden. [Ressourcenmanager Azure](../azure-resource-manager/resource-group-overview.md) ermöglicht die Arbeit mit den Ressourcen in der Anwendung als Gruppe. Sie können bereitstellen, aktualisieren oder Löschen aller Ressourcen für eine Anwendung in einem einzigen, koordinierte Vorgang.  

    Die Ressourcengruppen in Ihrem Abonnement aufgelistet:
    
        azure group list 
    
    So erstellen Sie eine neue Ressourcengruppe

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Daten Lake Analytics Kontonamen**
- **Standort**: eine der zentriert Azure-Daten, die die Daten dem Analytics unterstützt.
- **Die Daten dem standardmäßigen Konto**: jedes Konto Daten dem Analytics weist eine Daten Lake Standardkonto.

    Liste der vorhandenen Daten Lake Konto:
    
        azure datalake store account list

    So erstellen Sie ein neues Daten Lake-Konto

        azure datalake store account create "<Data Lake Store Account Name>" "<Azure Location>" "<Resource Group Name>"

    > [AZURE.NOTE] Der Daten Lake Kontonamen darf nur Kleinbuchstaben und Zahlen enthalten.



**Erstellen eines Daten dem Analytics-Kontos**

        azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"

        azure datalake analytics account list
        azure datalake analytics account show "<Data Lake Analytics Account Name>"          

![Anzeigen von Daten Lake Analytics Konto](./media/data-lake-analytics-get-started-cli/data-lake-analytics-show-account-cli.png)

> [AZURE.NOTE] Der Daten dem Analytics Kontonamen darf nur Kleinbuchstaben und Zahlen enthalten.


## <a name="upload-data-to-data-lake-store"></a>Hochladen von Daten in Lake Datenspeicher

In diesem Lernprogramm verarbeitet Sie einige Protokolle suchen.  Das Protokoll suchen kann entweder Daten Lake Store oder Azure Blob Storage gespeichert werden. 

Azure-Portal stellt eine Benutzeroberfläche für das Kopieren von einige Beispieldatendateien in das Standardkonto Lake Daten, die eine Protokolldatei suchen enthalten. Finden Sie unter [Vorbereiten Quelldaten](data-lake-analytics-get-started-portal.md#prepare-source-data) die Daten mit dem standardmäßigen Lake Datenspeicher Konto hochladen.

Verwenden Sie zum Hochladen von Dateien mit Cli den folgenden Befehl aus:

    azure datalake store filesystem import "<Data Lake Store Account Name>" "<Path>" "<Destination>"
    azure datalake store filesystem list "<Data Lake Store Account Name>" "<Path>"

Außerdem können Daten Lake Analytics Azure Blob-Speicher zugreifen.  Hochladen von Daten in Azure Blob-Speicher, finden Sie unter [Verwenden der Azure CLI mit Azure-Speicher](../storage/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Senden Sie die Daten dem Analytics Aufträge

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


    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"
    
    
Die folgenden Befehle können Aufträge auflisten, Job-Details abrufen und Aufträge abbrechen verwendet werden:

    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job Id>"
    azure datalake analytics job list "<Data Lake Analytics Account Name>"
    azure datalake analytics job show "<Data Lake Analytics Account Name>" "<Job Id>"

Nachdem Sie der Auftrag abgeschlossen ist, können Sie verwenden die folgenden Cmdlets, um die Datei nicht aufgeführt, und die Datei nicht herunterladen:
    
    azure datalake store filesystem list "<Data Lake Store Account Name>" "/Output"
    azure datalake store filesystem export "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" "<Destination>"
    azure datalake store filesystem read "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" <Length> <Offset>

## <a name="see-also"></a>Siehe auch

- Um die gleichen Lernprogramm mit anderen Tools angezeigt wird, klicken Sie auf die Registerkarte Selektoren am oberen Rand der Seite.
- Eine komplexe Abfrage finden Sie unter [Analysieren Website Protokolle Azure Daten dem Analytics verwenden](data-lake-analytics-analyze-weblogs.md).
- Um anzufangen U SQL Anwendungen entwickeln, finden Sie unter [entwickeln U-SQL-Skripts, die mit dem Datentools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL finden Sie unter [Erste Schritte mit Azure Daten dem Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md).
- Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Daten dem Analytics Azure-Portal verwenden](data-lake-analytics-manage-use-portal.md).
- Um einen Überblick über die Daten dem Analytics zu gelangen, finden Sie unter [Übersicht über die Azure Daten dem Analytics](data-lake-analytics-overview.md).

