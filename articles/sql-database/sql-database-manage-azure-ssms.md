<properties 
    pageTitle="Verwalten eine SQL-Datenbank mit SSMS | Microsoft Azure" 
    description="Erfahren Sie, wie Sie SQL Server Management Studio, um die SQL-Datenbankserver und Datenbanken verwalten." 
    services="sql-database" 
    documentationCenter=".net" 
    authors="stevestein" 
    manager="jhubbard" 
    editor="tysonn"/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sstein"/>


# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>Verwalten von Azure SQL-Datenbank mit SQL Server Management Studio 


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

SQL Server Management Studio (SSMS) können Sie Azure SQL-Datenbankserver und Datenbanken verwalten. Dieses Thema führt Sie durch häufige Aufgaben mit SSMS. Sie sollten bereits einen Server und die Datenbank in SQL Azure-Datenbank erstellt werden, bevor Sie beginnen. Weitere Informationen finden Sie unter [Erstellen Ihrer ersten Azure SQL-Datenbank](sql-database-get-started.md) und [Verbinden und Abfrage mit kürzerer SSMS](sql-database-connect-query-ssms.md) .

Es wird empfohlen, dass Sie die neueste Version von SSMS verwenden, wenn Sie Azure SQL-Datenbank arbeiten möchten. 

> [AZURE.IMPORTANT] Verwenden Sie immer die neueste Version von SSMS auf, da es ständig verbessert wird, um die Arbeit mit den neuesten Updates für Azure und SQL-Datenbank. Um die neueste Version zu erhalten, finden Sie unter [Herunterladen von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).



## <a name="create-and-manage-azure-sql-databases"></a>Erstellen und Verwalten von SQL Azure-Datenbanken

Während Sie auf das **master** -Datenbank verbunden sind, können Sie Datenbanken auf dem Server erstellen und ändern oder vorhandene Datenbanken ablegen. Die folgenden Schritte beschreiben, wie mehrere allgemeine Datenbank Management durch Management Studio Aufgaben. Um diese Aufgaben ausführen zu können, stellen Sie sicher, dass Sie mit der **master** -Datenbank mit dem Server Ebene Hauptbenutzer Benutzernamen verbunden sind, die Sie beim Einrichten einer Verbindung mit des Servers erstellt haben.

Um ein Abfragefenster in Management Studio zu öffnen, öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **System-Datenbanken** , mit der rechten Maustaste auf **master**, und klicken Sie dann auf **Neue Abfrage**.

-   Verwenden Sie die **CREATE DATABASE** -Anweisung zum Erstellen einer Datenbank ein. Weitere Informationen finden Sie unter [CREATE DATABASE (SQL-Datenbank)](https://msdn.microsoft.com/library/dn268335.aspx). Die folgende Anweisung erstellt eine Datenbank namens **MyTestDB** und gibt an, dass es ein Standard S0 Edition-Datenbank mit einer Standardeinstellung für die maximale Größe von 250 GB ist.

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.

-   Verwenden Sie die **ALTER DATABASE** -Anweisung zum Ändern einer vorhandenen Datenbank, beispielsweise wenn Sie den Namen und die Edition der Datenbank ändern möchten. Weitere Informationen finden Sie unter [ALTER DATABASE (SQL-Datenbank)](https://msdn.microsoft.com/library/ms174269.aspx). Die folgende Anweisung ändert die Datenbank, die Sie erstellt haben, so ändern Sie im vorhergehenden Schritt s1 Standard Edition.

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   Verwenden **Der DROP DATABASE** -Anweisung, um eine vorhandene Datenbank zu löschen. Weitere Informationen finden Sie unter [DROP DATABASE (SQL-Datenbank)](https://msdn.microsoft.com/library/ms178613.aspx). Die folgende Anweisung löscht die Datenbank **MyTestDB** , aber nicht legen Sie sie jetzt, da Sie ihn verwenden möchten, Benutzernamen im nächsten Schritt erstellen.

        DROP DATABASE myTestBase;

-   Die master-Datenbank weist die **sys.databases** -Ansicht, die Sie zum Anzeigen von Details zu allen Datenbanken verwenden können. Wenn Sie alle vorhandene Datenbanken anzeigen möchten, führen Sie folgende Anweisung aus:

        SELECT * FROM sys.databases;

-   In SQL-Datenbank wird die **USE** -Anweisung zum Wechseln zwischen Datenbanken nicht unterstützt. In diesem Fall müssen Sie zum Herstellen einer Verbindung direkt in die Zieldatenbank.

>[AZURE.NOTE] Viele der Transact-SQL-Anweisungen, die erstellen oder ändern eine Datenbank in ihren eigenen Stapel ausgeführt werden müssen und nicht mit anderen Transact-SQL-Anweisungen gruppiert werden. Weitere Informationen finden Sie unter der Anweisung-spezifische Informationen.

## <a name="create-and-manage-logins"></a>Erstellen und Verwalten von Benutzernamen

Die **master** -Datenbank enthält Benutzernamen und welche Benutzernamen über die Berechtigung zum Erstellen von Datenbanken oder andere Benutzernamen. Verwalten von Benutzernamen durch Herstellen einer Verbindung mit der **master** -Datenbank mit dem Server Ebene Hauptbenutzer Benutzernamen, den Sie beim Einrichten einer Verbindung mit des Servers erstellt haben. Die **CREATE LOGIN**, **ALTER LOGIN**oder **DROP LOGIN** -Anweisung können Sie die Ausführung von Abfragen für die master-Datenbank, die über den gesamten Server Benutzernamen verwaltet werden. Weitere Informationen finden Sie unter [Verwalten von Datenbanken und Anmeldedaten in SQL-Datenbank](http://msdn.microsoft.com/library/azure/ee336235.aspx). 


-   Verwenden Sie die **CREATE LOGIN** -Anweisung, um eine Anmeldung Server Ebene zu erstellen. Weitere Informationen finden Sie unter [CREATE LOGIN (SQL-Datenbank)](https://msdn.microsoft.com/library/ms189751.aspx). Die folgende Anweisung erstellt einen Benutzernamen **login1**bezeichnet. Ersetzen Sie **Kennwort1** mit dem Kennwort Ihrer Wahl.

        CREATE LOGIN login1 WITH password='password1';

-   Verwenden der Anweisung **CREATE USER** Datenbank Ebene Berechtigungen erteilen. Alle Benutzernamen müssen in der **master** -Datenbank erstellt werden. Für einen Benutzernamen für die Verbindung zu einer anderen Datenbank müssen Sie es Datenbank Ebene mit der Anweisung **CREATE USER** auf dieser Datenbank erteilen. Weitere Informationen finden Sie unter [CREATE USER (SQL-Datenbank)](https://msdn.microsoft.com/library/ms173463.aspx). 

-   Login1 Berechtigungen für eine Datenbank namens **MyTestDB**verleihen möchten, gehen Sie folgendermaßen vor:

 1.  Zum Aktualisieren von Objekt-Explorer, um die Datenbank **MyTestDB** anzeigen möchten, die Sie erstellt haben, mit der rechten Maustaste im Objekt-Explorer des Servernamens, und klicken Sie dann auf **Aktualisieren**.  

     Wenn Sie die Verbindung geschlossen haben, können Sie wieder herstellen, indem Sie im Menü Datei **Objekt-Explorer verbinden** auswählen.

 2. Mit der rechten Maustaste **MyTestDB** Datenbank, und wählen Sie **Neue Abfrage**.

    3.  Führen Sie die folgende Anweisung für die Datenbank MyTestDB zum Erstellen eines Datenbankbenutzers mit dem Namen **login1User** , die den Server Ebene Login **login1**entspricht.

            CREATE USER login1User FROM LOGIN login1;

-   Verwenden der **sp\_Addrolemember** gespeicherte Prozedur, um dem Benutzerkonto die entsprechende Zugriffsebene Berechtigungen für die Datenbank zu verleihen. Weitere Informationen finden Sie unter [Sp_addrolemember (Transact-SQL)](http://msdn.microsoft.com/library/ms187750.aspx). Die folgende Anweisung liefert **login1User** schreibgeschützte Berechtigungen für die Datenbank durch Hinzufügen von **login1User** zu den **Db\_Datareader** Rolle.

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   Verwenden Sie die **ALTER LOGIN** -Anweisung so ändern Sie einen vorhandenen Benutzernamen, beispielsweise wenn Sie das Kennwort für die Anmeldung ändern möchten. Weitere Informationen finden Sie unter [ALTER LOGIN (SQL-Datenbank)](https://msdn.microsoft.com/library/ms189828.aspx). **ALTER LOGIN** -Anweisung sollte für die **master** -Datenbank ausgeführt werden. Wechseln Sie wieder zum Abfragefenster, das in dieser Datenbank verbunden ist. Die folgende Anweisung ändert die Anmeldung **login1** , um das Kennwort zurückzusetzen. Ersetzen Sie **NewPassword** mit dem Kennwort Ihrer Wahl und **OldPassword** für das aktuelle Kennwort für die Anmeldung an.

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   Verwenden Sie **Die DROP LOGIN** -Anweisung, um einen vorhandenen Benutzernamen löschen. Löschen einer Login Ebene des Servers löscht auch alle zugeordneten Datenbank-Benutzerkonten. Weitere Informationen finden Sie unter [DROP DATABASE (SQL-Datenbank)](https://msdn.microsoft.com/library/ms178613.aspx). **DROP LOGIN** -Anweisung sollte für die **master** -Datenbank ausgeführt werden. Die Anweisung löscht das **login1** Login.

        DROP LOGIN login1;

-   Die master-Datenbank weist die **sys.sql\_Benutzernamen** anzuzeigen, die Sie verwenden können, um Benutzernamen anzuzeigen. Um alle vorhandenen Benutzernamen anzeigen möchten, führen Sie folgende Anweisung aus:

        SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-views"></a>Überwachen der SQL-Datenbank mithilfe des dynamischen Management-Ansichten

SQL-Datenbank unterstützt mehrere dynamische Management-Ansichten, die Sie verwenden können, um eine einzelne Datenbank überwachen. Es folgen ein paar Beispiele für die Art von Monitor-Daten, die Sie über diese Ansichten abrufen können. Vollständige Details und weitere Beispiele für die Verwendung finden Sie unter [Überwachen der SQL-Datenbank mithilfe von dynamischen Management Ansichten](https://msdn.microsoft.com/library/azure/ff394114.aspx).

-   Eine dynamische Ansicht Abfragen erfordert **VIEW DATABASE STATE** Berechtigungen. Um die **VIEW DATABASE STATE** einem bestimmten Datenbankbenutzers erteilt, Verbinden mit der Datenbank, und führen Sie folgende Anweisung für die Datenbank:

        GRANT VIEW DATABASE STATE TO login1User;

-   Berechnung Datenbank mit der Größe der **sys.dm\_Db\_Partition\_Stats** anzeigen. Die **sys.dm\_Db\_Partition\_Stats** Ansicht gibt Informationen zur Seite und Zeilenanzahl für jede Partition in der Datenbank, die Sie verwenden können, um die Größe der Datenbank zu berechnen. Die folgende Abfrage gibt die Größe der Datenbank, in Megabyte:

        SELECT SUM(reserved_page_count)*8.0/1024
        FROM sys.dm_db_partition_stats;   

-   Verwenden der **sys.dm\_Mitarbeiter\_Verbindungen** und **sys.dm\_Mitarbeiter\_Sitzungen** Ansichten zum Abrufen von Informationen zum aktuellen Benutzer Verbindungen und internen Aufgaben im Zusammenhang mit der Datenbank. Die folgende Abfrage gibt Informationen über die aktuelle Verbindung zurück:

        SELECT
            e.connection_id,
            s.session_id,
            s.login_name,
            s.last_request_end_time,
            s.cpu_time
        FROM
            sys.dm_exec_sessions s
            INNER JOIN sys.dm_exec_connections e
              ON s.session_id = e.session_id;

-   Verwenden der **sys.dm\_Mitarbeiter\_Abfrage\_Stats** Ansicht zum Abrufen von aggregierte Leistungsstatistiken für Cache Abfrage-Pläne. Die folgende Abfrage gibt Informationen zu den oberen fünf Abfragen, die durchschnittliche CPU-Zeit Rangfolge.

        SELECT TOP 5 query_stats.query_hash AS "Query Hash",
            SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
            MIN(query_stats.statement_text) AS "Statement Text"
        FROM
            (SELECT QS.*,
            SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
            ((CASE statement_end_offset
                WHEN -1 THEN DATALENGTH(ST.text)
                ELSE QS.statement_end_offset END
                    - QS.statement_start_offset)/2) + 1) AS statement_text
             FROM sys.dm_exec_query_stats AS QS
             CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
        GROUP BY query_stats.query_hash
        ORDER BY 2 DESC;
 
 
