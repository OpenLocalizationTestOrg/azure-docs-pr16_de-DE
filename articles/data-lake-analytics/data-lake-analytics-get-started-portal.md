<properties 
   pageTitle="Erste Schritte mit Azure Daten dem Analytics Azure-Portal mit | Azure" 
   description="So verwenden Sie zum Erstellen eines Kontos Daten dem Analytics Erstellen eines Auftrags für Daten dem Analytics mit U-SQL Azure-Portal, und senden Sie den Auftrag. " 
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
   ms.date="10/06/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-portal"></a>Lernprogramm: Erste Schritte mit Azure Daten dem Analytics mit Azure-portal

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Informationen Sie zum Verwenden des Azure-Portals Azure Daten dem Analytics-Konten erstellen, Daten dem Analytics Aufträge in [SQL-U](data-lake-analytics-u-sql-get-started.md)definieren, und senden Aufträge, die die Daten dem Analytics-Dienst. Weitere Informationen zu den Daten dem Analytics finden Sie unter [Übersicht über die Azure Daten dem Analytics](data-lake-analytics-overview.md).

In diesem Lernprogramm entwickeln Sie ein Projekt, das liest einen Tabulator getrennte (TSV) Wertedatei und wandelt sie in eine Datei mit kommagetrennten Werten (CSV). Um durch die gleichen Lernprogramm mit anderen unterstützte Tools zu wechseln, klicken Sie auf den Registerkarten am oberen Rand der in diesem Abschnitt. Nachdem Sie Ihre erste Arbeit erfolgreich ist, können Sie komplexere Datentransformationen mit SQL-U Schreibvorgangs starten.

##<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie die folgenden Elemente:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).

##<a name="create-data-lake-analytics-account"></a>Erstellen Sie die Daten dem Analytics-Konto

Sie müssen ein Daten dem Analytics-Konto verfügen, bevor Sie alle weiteren Einzelvorgänge ausführen können.

Jedes Daten dem Analytics-Konto hat eine [Azure dem Datenspeicher]() Konto Abhängigkeit.  Dieses Konto wird als Standardkonto Lake Datenspeicher bezeichnet.  Sie können das Konto Lake Datenspeicher erstellen, im voraus, oder wenn Sie Ihre Daten dem Analytics-Konto erstellen. In diesem Lernprogramm erstellen Sie das Konto Lake Datenspeicher mit den Daten dem Analytics-Konto.

**Erstellen eines Daten dem Analytics-Kontos**

1. Melden Sie sich auf der [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **neu**, klicken Sie auf **Intelligence + Analytics**, und klicken Sie dann auf **Daten dem Analytics**.
3. Geben Sie ein, oder wählen Sie die folgenden Werte aus:

    ![Azure Daten Lake Analytics Portal blade](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **Name**: Benennen Sie das Daten dem Analytics-Konto.
    - **Abonnements**: Wählen Sie das Azure-Abonnement für das Konto Analytics verwendet.
    - **Ressourcengruppe**. Wählen Sie eine vorhandene Azure Ressourcengruppe oder erstellen Sie einen neuen. Azure Ressourcenmanager können Sie für die Arbeit mit den Ressourcen in der Anwendung als Gruppe. Weitere Informationen finden Sie unter [Azure Ressourcenmanager Übersicht](resource-group-overview.md). 
    - **Speicherort**. Wählen Sie eine Azure Data Center für die Daten dem Analytics-Konto an. 
    - **Dem Datenspeicher**: jeder Daten dem Analytics Konto verfügt ein abhängige Lake Datenspeicher-Konto. Die Daten dem Analytics-Konto und das abhängige Lake Datenspeicher Konto müssen sich in derselben Azure Data Center befinden. Führen Sie die Anweisung zum Erstellen eines neuen Kontos mit Lake Datenspeicher oder wählen Sie ein vorhandenes Layout aus.

8. Klicken Sie auf **Erstellen**. Es wird auf der Startseite des Portals. Eine neue Kachel wird mit der Bezeichnung "Bereitstellen von Azure Daten dem Analytics" mit der StartBoard hinzugefügt. Es dauert einen Moment, ein Konto Daten dem Analytics erstellen. Wenn Sie das Konto erstellt wurde, wird im Portal des Kontos auf einem neuen Blade geöffnet.

Nachdem ein Daten dem Analytics-Konto erstellt wurde, können Sie zusätzliche Lake Datenspeicher-Konten und Azure-Speicher-Konten hinzufügen. Anweisungen finden Sie unter [Verwalten von Daten dem Analytics Datenquellen zu berücksichtigen](data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

##<a name="prepare-source-data"></a>Vorbereiten der Quelldaten

In diesem Lernprogramm haben Sie einige Suche Protokolle verarbeiten.  Das Protokoll suchen kann entweder dData Lake Store oder Azure Blob Storage gespeichert werden. 

Azure-Portal stellt eine Benutzeroberfläche für einige Beispieldatendateien dem standardmäßigen Lake Datenspeicher Benutzerkonto, das eine Protokolldatei suchen einschließen kopieren.

**So kopieren Sie Daten von Beispieldateien**

1. Öffnen Sie aus dem [Azure-Portal](https://portal.azure.com)Ihre Daten dem Analytics-Konto ein.  Finden Sie unter [Verwalten von Daten dem Analytics-Konten](data-lake-analytics-get-started-portal.md#manage-accounts) zu erstellen, und öffnen Sie das Konto im Portal.
3. Erweitern Sie im Bereich **Essentials** , und klicken Sie dann auf **Durchsuchen Stichprobe Skripts**. Es wird ein anderes Blade aufgerufen **Stichprobe Skripts**geöffnet.

    ![Azure Daten Lake Analytics Portal Beispiel-Skript](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-sample-scripts.png)

4. Klicken Sie auf die **Stichprobe Daten fehlen** , um den Beispieldatendateien zu kopieren. Wenn es fertig ist, zeigt das Portal **Beispieldaten wurde erfolgreich aktualisiert**.
7. Klicken Sie aus dem Daten Lake Analytics Konto Blade oben auf **Daten-Explorer** . 

    ![Schaltfläche für Azure Daten Lake Analytics Daten-explorer](./media/data-lake-analytics-get-started-portal/data-lake-analytics-data-explorer-button.png)

    Zwei Blades geöffnet wird. Eine ist **Daten-Explorer**und anderen Lake Datenspeicher Standardkonto.
8. Klicken Sie in der standardmäßigen Lake Datenspeicher Konto Blade auf **Beispiele** zum Erweitern Sie den Ordner und dann auf **Daten** , um den Ordner zu erweitern. Sie müssen die folgenden Dateien und Ordnern finden Sie unter:

    - AmbulanceData /
    - AdsLog.tsv
    - SearchLog.tsv
    - Version.txt
    - WebLog.log
    
    In diesem Lernprogramm verwenden Sie SearchLog.tsv.

In der Praxis Programm Sie entweder Ihre Programme zum Schreiben von Daten in einer verknüpften Speicherkonten oder Hochladen von Daten aus. Hochladen von Dateien, finden Sie unter [Hochladen von Daten in dem Datenspeicher](data-lake-analytics-manage-use-portal.md#upload-data-to-adls) oder [Hochladen von Daten auf Blob-Speicher](data-lake-analytics-manage-use-portal.md#upload-data-to-wasb).

##<a name="create-and-submit-data-lake-analytics-jobs"></a>Erstellen Sie und senden Sie die Daten dem Analytics Aufträge

Nachdem Sie die Quelldaten vorbereitet haben, können Sie die Entwicklung einer U-SQL-Skript starten.  

**Übermitteln Sie ein Projekt**

1. Klicken Sie auf **Neues Projekt**, aus dem Daten Lake Analytics Konto Blade im Portal. 

    ![Schaltfläche für neue Projekt-Azure Daten dem Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job-button.png)

    Wenn Sie das Blade angezeigt werden, finden Sie unter [eröffnen eines Kontos Daten dem Analytics Portal](data-lake-analytics-manage-use-portal.md#access-adla-account).
2. Geben Sie **Projektname**und die folgenden U-SQL-Skript ein:

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

    ![Erstellen von Azure Daten dem Analytics U-SQL-Aufträgen](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job.png)

    Dieses U-SQL-Skript liest die Quelldatei mit **Extractors.Tsv()**und erstellt dann eine CSV-Datei mit **Outputters.Csv()**. 
    
    Ändern Sie die beiden Pfade nicht, es sei denn, Sie die Quelldatei in einem anderen Speicherort zu kopieren.  Daten Lake Analytics erstellt den Ausgabeordner aus, wenn es nicht vorhanden ist.  In diesem Fall verwenden wir die einfachen, relative Pfade.  
    
    Es ist einfacher, relative Pfade für Dateien, die in Standard Daten Lake Konten zu verwenden. Sie können auch absolute Pfade verwenden.  Beispielsweise 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
      

    Weitere Informationen zu U-SQL finden Sie unter [Erste Schritte mit Azure Daten dem Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md) und [U-SQL-Sprache-Referenz](http://go.microsoft.com/fwlink/?LinkId=691348).
     
3. Klicken Sie oben auf **Auftrag senden** .   
4. Warten Sie, bis der Status in **erfolgreich**geändert wird. Sie können sehen, dass der Auftrag zum Abschließen ungefähr einer Minute benötigt wurde.
    
    Falls der Auftrag fehlgeschlagen ist, finden Sie unter [Überwachen und Behandeln von Problemen mit Daten dem Analytics Aufträge](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

5. Am unteren Rand der Blade klicken Sie auf der Registerkarte **Ausgabe** , und klicken Sie dann auf **SearchLog aus Daten Lake.csv**. Sie können eine Vorschau, herunterladen, umbenennen und löschen Sie die Ausgabedatei.

    ![Azure Daten Lake Analytics Auftrag ausgeben Dateieigenschaften](./media/data-lake-analytics-get-started-portal/data-lake-analytics-output-file-properties.png)


##<a name="see-also"></a>Siehe auch

- Eine komplexe Abfrage finden Sie unter [Analysieren Website Protokolle Azure Daten dem Analytics verwenden](data-lake-analytics-analyze-weblogs.md).
- Um anzufangen U SQL Anwendungen entwickeln, finden Sie unter [entwickeln U-SQL-Skripts, die mit dem Datentools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL finden Sie unter [Erste Schritte mit Azure Daten dem Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md).
- Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Daten dem Analytics verwenden Azure-Portal](data-lake-analytics-manage-use-portal.md).
- Um einen Überblick über die Daten dem Analytics zu gelangen, finden Sie unter [Übersicht über die Azure Daten dem Analytics](data-lake-analytics-overview.md).
- Um die gleichen Lernprogramm mit anderen Tools angezeigt wird, klicken Sie auf die Registerkarte Selektoren am oberen Rand der Seite.
- Zum Melden von Diagnoseinformationen finden Sie unter [Zugreifen auf Diagnose Protokolle für Azure Daten dem Analytics](data-lake-analytics-diagnostic-logs.md)
