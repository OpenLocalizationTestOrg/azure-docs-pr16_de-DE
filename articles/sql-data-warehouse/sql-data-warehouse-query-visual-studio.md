<properties
   pageTitle="Abfrage SQL Azure Datawarehouse (Visual Studio) | Microsoft Azure"
   description="Abfrage SQL Datawarehouse mit Visual Studio."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="query-azure-sql-data-warehouse-visual-studio"></a>Abfrage SQL Azure Datawarehouse (Visual Studio)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Learning Azure-Computern](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Verwenden Sie Visual Studio Abfrage Azure SQL-Data Warehouse in wenigen Minuten. Diese Methode verwendet die Erweiterung SQL Server Data Tools (SSDT) in Visual Studio. 

## <a name="prerequisites"></a>Erforderliche Komponenten

Wenn Sie in diesem Lernprogramm verwenden zu können, müssen Sie folgende Aktionen ausführen:

+ Ein vorhandenes SQL Datawarehouse. Um eine zu erstellen, finden Sie unter [Erstellen einer SQL Data Warehouse][].
+ SSDT für Visual Studio. Wenn Sie Visual Studio haben, haben Sie wahrscheinlich bereits folgt. Installations- und Optionen finden Sie unter [Visual Studio installieren und SSDT][].
+ Den vollqualifizierten Namen für SQL Server. Um dies zu finden, finden Sie unter [Verbinden mit SQL Data Warehouse][].

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. Herstellen einer Verbindung mit Ihrem SQL Datawarehouse

1. Öffnen Sie Visual Studio 2013 oder 2015.
2. SQL Server-Objekt-Explorer zu öffnen. Wählen Sie hierzu die **Ansicht** > **SQL Server-Objekt-Explorer**.

    ![SQL Server-Objekt-Explorer][1]

3. Klicken Sie auf das Symbol **SQL Server hinzufügen** .

    ![Hinzufügen von SQLServer][2]

4. Füllen Sie die Felder mit im Fenster Server aus.

    ![Verbinden mit Server][3]

    - **Servername angezeigt**. Geben Sie den **Servernamen** , die zuvor angegeben haben.
    - **Authentifizierung**. Wählen Sie **SQL Server-Authentifizierung** oder **integrierte Active Directory-Authentifizierung**.
    - **Benutzernamen** und **Ihr Kennwort ein**. Geben Sie Benutzernamen und Ihr Kennwort ein, wenn SQL Server-Authentifizierung über ausgewählt wurde.
    - Klicken Sie auf **Verbinden**.

5. Zum untersuchen, erweitern Sie Ihren SQL Azure-Server aus. Sie können die mit dem Server verbundenen Datenbanken anzeigen. Erweitern Sie AdventureWorksDW, um die Tabellen in der Beispieldatenbank anzuzeigen.

    ![Untersuchen AdventureWorksDW][4]

## <a name="2-run-a-sample-query"></a>2. Führen Sie eine Abfrage für die Stichprobe

Nachdem Sie nun eine Verbindung zu Ihrer Datenbank hergestellt wurde, Schreiben wir nun eine Abfrage aus.

1. Mit der rechten Maustaste in der Datenbank in SQL Server-Objekt-Explorer.

2. Wählen Sie auf **neue Abfrage**. Ein neues Abfragefenster wird geöffnet.

    ![Neue Abfrage][5]

3. Kopieren Sie diese TSQL Abfrage in das Abfragefenster ein:

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. Führen Sie die Abfrage ein. Hierzu klicken Sie auf den grünen Pfeil, oder verwenden Sie die folgende Verknüpfung: `CTRL` + `SHIFT` + `E`.

    ![Ausführen der Abfrage][6]

5. Schauen Sie sich die Ergebnisse der Abfrage. In diesem Beispiel enthält die Tabelle FactInternetSales 60398 Zeilen an.

    ![Abfrageergebnisse][7]

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie eine Verbindung herstellen und Abfragen können, versuchen Sie, [die Daten mit PowerBI visualisieren][].

Zum Konfigurieren Ihrer Umgebung für Azure-Active Directory-Authentifizierung finden Sie unter [authentifizieren in SQL Data Warehouse][].

<!--Arcticles-->
[Verbinden Sie mit SQL Datawarehouse]: sql-data-warehouse-connect-overview.md
[Erstellen einer SQL Datawarehouse]: sql-data-warehouse-get-started-provision.md
[Installieren von Visual Studio und SSDT]: sql-data-warehouse-install-visual-studio.md
[Authentifizieren Sie in SQL Datawarehouse]: sql-data-warehouse-authentication.md
[Visualisieren von Daten mit PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
