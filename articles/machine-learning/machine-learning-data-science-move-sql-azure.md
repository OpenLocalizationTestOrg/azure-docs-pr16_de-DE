<properties 
    pageTitle="Verschieben von Daten in einer SQL Azure-Datenbank für Azure maschinellen Learning | Azure" 
    description="Erstellen von SQL-Tabelle und Laden von Daten in SQL-Tabelle" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016"
    ms.author="bradsev" /> 

# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Verschieben von Daten in einer SQL Azure-Datenbank für Azure Computer-Schulung

In diesem Thema werden die Optionen zum Verschieben von Daten aus einer flachen Dateien (Formate CSV- oder TSV) oder aus den Daten in einer SQL Server lokal, mit einer SQL Azure-Datenbank gespeichert. Die folgenden Aufgaben zum Verschieben von Daten in der Cloud sind Teil des Prozesses Team Daten Wissenschaft.

Ein Thema, das die Optionen zum Verschieben von Daten in einer lokalen SQL Server für maschinelle Learning werden, finden Sie unter [Verschieben von Daten mit SQL Server auf eine Azure-virtuellen Computern](machine-learning-data-science-move-sql-server-virtual-machine.md).

Die folgenden **Menü** Links zu Themen, die beschreiben, wie Daten in der Ziel-Umgebungen Aufnahme, wo die Daten gespeichert und während der Teamwebsite Daten Wissenschaft Prozess (TDSP) verarbeitet werden können.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Die folgende Tabelle enthält eine Übersicht über die Optionen zum Verschieben von Daten in einer SQL Azure-Datenbank.

<b>DATENQUELLE</b> |<b>Ziel: Azure SQL-Datenbank</b> |
-------------- |--------------------------------|
<b>Flatfile (CSV- oder TSV formatiert)</b> |<a href="#bulk-insert-sql-query">Massen einfügen SQL-Abfrage |
<b>Mit lokalen SQLServer</b> | 1. <a href="#export-flat-file">exportieren in Flatfile<br> 2. <a href="#insert-tables-bcp">SQL-Datenbank-Assistent für die Migration<br> 3. <a href="#db-migration">Datenbank sichern von und Wiederherstellen<br> 4. <a href="#adf">Factory Azure-Daten |


## <a name="a-nameprereqsaprerequisites"></a><a name="prereqs"></a>Erforderliche Komponenten
Die hier beschriebenen Verfahren erfordern, dass Sie haben:

* Ein **Azure-Abonnement**. Wenn Sie nicht über ein Abonnement verfügen, können Sie für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)registrieren.
* Ein **Konto Azure-Speicher**. Verwenden Sie ein Konto Azure-Speicher zum Speichern der Daten in diesem Lernprogramm. Wenn Sie ein Azure-Speicher-Konto besitzen, finden Sie im Artikel [Erstellen eines Speicher-Kontos](storage-create-storage-account.md#create-a-storage-account) . Nachdem Sie das Speicherkonto erstellt haben, müssen Sie den Zugriff auf den Speicher verwendet Konto Key zu erhalten. Finden Sie unter [anzeigen, kopieren und neu generieren Speicher Zugriffstasten](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Zugriff auf eine **SQL Azure-Datenbank**. Wenn Sie eine SQL Azure-Datenbank einrichten müssen, enthält [Erste Schritte mit Microsoft Azure SQL-Datenbank](../sql-database/sql-database-get-started.md) Informationen ein, wie Sie eine neue Instanz von einer SQL Azure-Datenbank bereitstellen.
* Installiert und konfiguriert **Azure PowerShell** lokal. Anweisungen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).

**Daten**: die Migrationsprozesse werden mit dem [NYC Taxi Dataset](http://chriswhong.com/open-data/foil_nyc_taxi/)veranschaulicht. Das Dataset NYC Taxi enthält Informationen zu Geschäftsreise Daten und messen und verfügbar ist, wie dieser Beitrag, klicken Sie auf Azure Blob-Speicher erwähnt: [NYC Taxi Daten](http://www.andresmh.com/nyctaxitrips/). Eine Stichprobe und eine Beschreibung der diese Dateien werden in [NYC Taxi Schleifen Datasetbeschreibung](machine-learning-data-science-process-sql-walkthrough.md#dataset)bereitgestellt.
 
Sie können anpassen, die einem Satz von Ihre eigenen Daten hier beschriebenen Verfahren oder führen Sie die Schritte aus, wie mithilfe des NYC Taxi Datasets beschrieben. Um das Dataset NYC Taxi in Ihre mit lokalen SQL Server-Datenbank hochzuladen, folgen Sie dem Verfahren in [Massen Importieren von Daten in SQL Server-Datenbank](machine-learning-data-science-process-sql-walkthrough.md#dbload)aus. Diese Anweisungen für einen SQL Server auf eine Azure-virtuellen Computern sind, aber die Vorgehensweise für das Hochladen in der SQL Server lokal entspricht.


## <a name="a-namefile-to-azure-sql-databasea-moving-data-from-a-flat-file-source-to-an-azure-sql-database"></a><a name="file-to-azure-sql-database"></a>Verschieben von Daten aus einer Quelle Flatfile mit einer SQL Azure-Datenbank

Daten in einer flachen Dateien (CSV- oder TSV formatiert) können mit einer SQL Azure-Datenbank mithilfe einer Massen einfügen SQL-Abfrage verschoben werden.

### <a name="a-namebulk-insert-sql-querya-bulk-insert-sql-query"></a><a name="bulk-insert-sql-query"></a>Massen einfügen SQL-Abfrage

Die Schritte für die Prozedur mithilfe der Massen einfügen SQL-Abfrage sind ähnlich wie die in den Abschnitten zum Verschieben von Daten aus einer Quelle Flatfile mit SQL Server ein Azure-virtuellen Computers behandelt. Weitere Informationen finden Sie unter [Massen einfügen SQL-Abfrage](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery).


##<a name="a-namesql-on-prem-to-sazure-sql-databasea-moving-data-from-on-premise-sql-server-to-an-azure-sql-database"></a><a name="sql-on-prem-to-sazure-sql-database"></a>Verschieben von Daten aus lokalen SQL Server mit einer SQL Azure-Datenbank

Wenn Sie die Quelldaten in einer lokalen SQL Server gespeichert ist, gibt es verschiedene mögliche Werte für die Daten in einer SQL Azure-Datenbank verschieben:

1. [Exportieren in Flatfile](#export-flat-file) 
2. [SQL-Datenbank-Assistent für die Migration](#insert-tables-bcp)
3. [Sichern von Database- und Wiederherstellen](#db-migration)
4. [Factory Azure-Daten](#adf)

Die Schritte für die ersten drei sind sehr ähnlich diese Abschnitte in [Verschieben von Daten mit SQL Server auf eine Azure-virtuellen Computern](machine-learning-data-science-move-sql-server-virtual-machine.md) , die denselben Verfahren zu behandeln. Links zu den entsprechenden Abschnitten in diesem Thema werden in den folgenden Anweisungen bereitgestellt.

###<a name="a-nameexport-flat-fileaexport-to-flat-file"></a><a name="export-flat-file"></a>Exportieren in Flatfile

Die Schritte für diese exportieren nach einer Flatfile ähneln denen in [Exportieren in Flatfile](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file)behandelt.

###<a name="a-nameinsert-tables-bcpasql-database-migration-wizard"></a><a name="insert-tables-bcp"></a>SQL-Datenbank-Assistent für die Migration

Die für die Verwendung der SQL-Datenbank Migrations-Assistent beschriebenen Schritte im [Assistenten zum Migrieren von SQL-Datenbank](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration)erteilt.

###<a name="a-namedb-migrationadatabase-back-up-and-restore"></a><a name="db-migration"></a>Sichern von Database- und Wiederherstellen

Die Schritte zum Verwenden der Datenbank sichern und Wiederherstellen ähneln denen in der [Datenbank sichern und Wiederherstellen von](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup)behandelt.

###<a name="a-nameadfaazure-data-factory"></a><a name="adf"></a>Factory Azure-Daten

Verfahren zum Verschieben von Daten mit einer SQL Azure-Datenbank mit Azure Daten Factory (ADF) werden im Thema [Verschieben von Daten aus einer mit lokalen SQLServer in SQL Azure mit Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md)bereitgestellt. In diesem Thema veranschaulicht, wie Sie zum Verschieben von Daten aus einer mit lokalen SQL Server-Datenbank mit einer SQL Azure-Datenbank über Azure BLOB-Speicher ADF verwenden. 

Erwägen Sie ADF, wenn Daten in einem Szenario Hybrid ständig migriert werden, die sowohl lokal als auch Cloud Ressourcen greift auf müssen und die Daten Transaktionen oder werden oder geänderten Geschäftslogik hinzugefügt, wenn migriert werden müssen. ADF kann für die Planung und Überwachung der Aufträge mithilfe von einfachen JSON-Skripts, die zur Verwaltung von der Verlagerung von Daten in regelmäßigen Abständen. ADF verfügt auch über andere Funktionen wie z. B. Unterstützung für komplexe Vorgänge.




