<properties
   pageTitle="Analysieren von Website-Protokolle mit Azure Daten dem Analytics | Azure"
   description="Informationen Sie zum Analysieren von Website-Protokolle, die Daten dem Analytics verwenden. "
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

# <a name="tutorial-analyze-website-logs-using-azure-data-lake-analytics"></a>Lernprogramm: Analysieren Sie Website Protokolle mit Azure Daten dem Analytics

Informationen Sie zum Analysieren von Website-Protokolle, die Daten dem Analytics, besonders auf herausfinden, welche Quellen in Fehler ausgeführt wurde, als sie versucht haben, besuchen die Website verwenden.

>[AZURE.NOTE] Wenn Sie der Anwendung arbeiten sehen möchten, es sinnvoll, [interaktive Lernprogramme verwenden Azure Daten dem Analytics](data-lake-analytics-use-interactive-tutorials.md)durchlaufen. In diesem Lernprogramm basiert auf dem gleichen Szenario und denselben Code. Der Zweck dieses Lernprogramms ist Entwickler die Leistung von erstellen und Ausführen einer Anwendungs Daten dem Analytics von Anfang bis Ende zu bieten.

## <a name="prerequisites"></a>Voraussetzungen für:

- **Aktualisieren von visual Studio 2015, Visual Studio 2013 4, oder Visual Studio 2012 mit Visual C++ installiert**.
- **Microsoft Azure SDK für .NET Version 2,5 oder höher**.  Installieren Sie es über das [Web Plattform Installer](http://www.microsoft.com/web/downloads/platform.aspx)aus.
- Die **[Daten dem Tools für Visual Studio](http://aka.ms/adltoolsvs)**.

    Sobald die Daten dem Tools für Visual Studio installiert ist, wird ein Menü **Daten dem** in Visual Studio angezeigt:

    ![Visual Studio U-SQL-Menü](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)

- **Grundlegende Kenntnisse Daten dem Analytics und die Daten dem Tools für Visual Studio**. Um anzufangen, finden Sie unter:

    - [Erste Schritte mit Azure Daten dem Analytics Azure-Portal verwenden](data-lake-analytics-get-started-portal.md).
    - [Entwickeln U-SQL-Skript, mit dem Daten-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

- **Ein Daten dem Analytics-Konto.**  Finden Sie unter [Erstellen eines Azure Daten dem Analytics-Kontos](data-lake-analytics-get-started-portal.md#create_adl_analytics_account).

    Die Daten dem Tools unterstützt keine Daten dem Analytics-Konten erstellen.  Daher müssen Sie es mithilfe der Azure-Portal, Azure PowerShell, .NET SDK oder Azure CLI erstellen.
- **Hochladen der Beispieldaten mit dem Daten dem Analytics-Konto an.** Finden Sie unter [SearchLog.tsv zu dem Datenspeicher Standardkonto hochladen](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account).

    Wenn Sie ein Projekt Daten dem Analytics ausgeführt, benötigen Sie einige Daten. Obwohl die Daten dem Tools Hochladen von Daten unterstützt, werden Sie im Portal verwenden, um die Beispieldaten, um dieses Lernprogramms folgen erleichtern hochzuladen.

## <a name="connect-to-azure"></a>Herstellen einer Verbindung Azure mit

Bevor Sie erstellen und Testen Sie sämtliche U-SQL-Skripts können, müssen Sie zuerst mit Azure verbinden.

**Verbindung zu Daten dem Analytics**

1. Öffnen Sie Visual Studio.
2. Klicken Sie auf **Optionen und Einstellungen**, wählen Sie im Menü **Daten dem** .
4. Klicken Sie auf **Anmelden**, oder **Benutzer ändern** , wenn jemand angemeldet hat, und folgen Sie den Anweisungen.
5. Klicken Sie auf **OK** , um das Dialogfeld Optionen und Einstellungen zu schließen.

**Durchsuchen Sie Ihre Daten dem Analytics-Konten**

1. Öffnen Sie in Visual Studio **Server-Explorer** durch Drücken von **STRG + ALT + S**aus.
2. **Server-Explorer**erweitern Sie **Azure**und erweitern Sie dann die **Daten dem Analytics**. Eine Liste Ihrer Daten dem Analytics-Konten gilt angezeigt werden, wenn eine vorhanden sind. Sie können keine Daten dem Analytics-Konten aus dem Studio erstellen. Zum Erstellen eines Kontos, finden Sie unter [Erste Schritte mit Azure Daten dem Analytics mithilfe von Azure-Portal](data-lake-analytics-get-started-portal.md) oder [Erste Schritte mit Azure Daten dem Analytics Azure PowerShell verwenden](data-lake-analytics-get-started-powershell.md).

## <a name="develop-u-sql-application"></a>Entwickeln Sie U-SQL-Anwendung

Eine U-SQL-Anwendung ist hauptsächlich ein U-SQL-Skript. Weitere Informationen zum U-SQL finden Sie unter [Erste Schritte mit SQL-U](data-lake-analytics-u-sql-get-started.md).

Sie können die Erweiterung benutzerdefinierte Operatoren zur Anwendung hinzufügen.  Weitere Informationen finden Sie unter [Operatoren für Daten dem Analytics Aufträge für benutzerdefinierte U-SQL entwickeln](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**Erstellen und Senden eines Auftrags Daten dem Analytics**

1. Im Menü **Datei** klicken Sie auf **neu**, und klicken Sie dann auf **Projekt**.
2. Wählen Sie den U-SQL-Projekttyp aus.

    ![neue U SQL Visual Studio-Projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Klicken Sie auf **OK**. Visual Studio erstellt eine Lösung mit einer Script.usql-Datei.
4. Geben Sie das folgende Skript in der Script.usql-Datei ein:

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            PARTITIONED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    Um den U-SQL-Code zu verstehen, finden Sie unter [Erste Schritte mit Daten dem Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md).    

5. Hinzufügen eines neuen U-SQL-Skripts zu einem Projekt, und geben Sie Folgendes ein:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();

6. Wechseln Sie wieder zu dem ersten U-SQL-Skript und neben der Schaltfläche **Senden** , geben Sie Ihre Analytics-Konto.
7. Klicken Sie mit der rechten Maustaste auf **Script.usql** **Lösung-Explorer**und klicken Sie dann auf **Skript erstellen**. Überprüfen Sie die Ergebnisse im Ausgabebereich aus.
8. **Lösung-Explorer**klicken Sie mit der rechten Maustaste auf **Script.usql**und klicken Sie dann auf **Skript übermitteln**.
9. Stellen Sie sicher, dass das **Konto Analytics** ist der möchten Sie den Auftrag ausführen, und klicken Sie dann auf **Senden**. Einreichung Ergebnisse und Position Link sind aus den Daten dem Tools für Visual Studio führt Fenster, wenn die Übermittlung abgeschlossen ist.
10. Warten Sie, bis der Auftrag erfolgreich abgeschlossen ist.  Der Auftrag fehlgeschlagen ist, ist es wahrscheinlich die Quelldatei fehlt.  Finden Sie im Abschnitt erforderliche dieses Lernprogramms. Weitere Informationen zur Problembehandlung finden Sie unter [Überwachen und Behandeln von Problemen mit Azure Daten dem Analytics Aufträge](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    Wenn das Projekt abgeschlossen ist, werden Sie im folgenden Bildschirm finden Sie unter:

    ![Analysieren von Daten Lake Analytics Blogs Website von Protokollen](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)

11. Jetzt wiederholen Sie die Schritte 7 bis 10 für **Script1.usql**ein.

>[AZURE.NOTE]Sie können nicht aus lesen oder Schreiben einer U-SQL-Tabelle, die im gleichen Skript geändert oder erstellt wurde.  Darum verwenden zwei Skripts verwenden Sie für dieses Beispiel.

**Um die Ausgabe der Position anzuzeigen.**

1. Aus dem **Server-Explorer** **Azure**erweitern, erweitern Sie die **Daten dem Analytics**, erweitern Sie Ihre Daten dem Analytics-Konto, **Speicherkonten**erweitern, mit der rechten Maustaste in dem Datenspeicher Standardkonto und klicken Sie dann auf **Explorer**.
2.  Doppelklicken Sie auf **Beispiele** , um den Ordner zu öffnen, und doppelklicken Sie dann auf **Ausgaben**.
3.  Doppelklicken Sie auf **UnsuccessfulResponsees.log**.
4.  Sie können auch die Ausgabedatei in der Diagrammansicht des Projekts doppelklicken, um direkt in die Ausgabe zu navigieren.

## <a name="see-also"></a>Siehe auch

Um mit Daten dem Analytics verschiedene Tools verwenden anzufangen, finden Sie unter:

- [Erste Schritte mit Daten dem Analytics mithilfe von Azure-Portal](data-lake-analytics-get-started-portal.md)
- [Erste Schritte mit Daten dem Analytics mithilfe der PowerShell Azure](data-lake-analytics-get-started-powershell.md)
- [Erste Schritte mit Daten dem Analytics mit .NET SDK](data-lake-analytics-get-started-net-sdk.md)

Um weitere Themen zur Entwicklung finden Sie unter:

- [Entwickeln Sie U-SQL-Skripts mit dem Datentools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Erste Schritte mit Azure Daten dem Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md)
- [Entwickeln Sie U-SQL benutzerdefinierte Operatoren für Daten dem Analytics Aufträge](data-lake-analytics-u-sql-develop-user-defined-operators.md)
