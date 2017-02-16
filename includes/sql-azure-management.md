
# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>Verwalten von Azure SQL-Datenbank mit SQL Server Management Studio 

SQL Server Management Studio (SSMS) können Sie die logischen Azure SQL-Datenbank-Server und Datenbanken verwalten. Dieses Thema führt Sie durch häufige Aufgaben mit SSMS. Verfügen Sie bereits eine logische Server und in SQL Azure-Datenbank erstellt wurden, bevor Sie beginnen. Um anzufangen, finden Sie unter [Erstellen Ihrer ersten Azure SQL-Datenbank](sql-database-get-started.md) , und kehren Sie danach.

Es wird empfohlen, dass Sie die neueste Version von SSMS verwenden, wenn Sie Azure SQL-Datenbank arbeiten möchten. Besuchen Sie [Herunterladen von SQL Server Management Studio](https://msdn.microsoft.com/en-us/library/mt238290.aspx) , um es zu erhalten. 


## <a name="connect-to-a-sql-database-logical-server"></a>Verbinden Sie mit einem logischen SQL-Datenbankserver

Herstellen einer Verbindung mit einer SQL-Datenbank müssen Sie den Servernamen auf Azure kennen. Sie müssen möglicherweise auf das Portal anmelden, um diese Informationen zu erhalten.

1.  Melden Sie sich bei der [Azure-Verwaltungsportal](http://manage.windowsazure.com).

2.  Klicken Sie im linken Bereich auf **SQL-Datenbanken**.

3.  Klicken Sie auf der Homepage der SQL-Datenbanken auf **Servern** am oberen Rand der Seite um eine Liste aller Server mit Ihrem Abonnement verknüpft ist. Suchen Sie den Namen des Servers, die Sie verbinden und es in die Zwischenablage zu kopieren möchten.

    Konfigurieren Sie anschließend Ihre Firewall SQL-Datenbank Verbindungen aus Ihrem lokalen Computer zulässt. Dazu müssen Sie Ihrem lokalen Computer IP-Adresse der Ausnahmenliste hinzufügen.

1.  Klicken Sie auf der Startseite der SQL-Datenbanken auf **Server** , und klicken Sie dann auf dem Server, den Sie eine Verbindung herstellen möchten.

2.  Klicken Sie auf **Konfigurieren** am oberen Rand der Seite.

3.  Kopieren Sie in der aktuellen IP-Adresse die IP-Adresse ein.

4.  Auf der Seite konfigurieren enthält **Zugelassene IP-Adressen** drei Felder, in dem Sie einen Regelnamen und einen Bereich von IP-Adressen als Start- und Endwert angeben können. Sie können auch den Namen des Computers für einen Regelnamen eingeben. Fügen Sie für den Bereich Start- und in beiden Feldern in die IP-Adresse des Computers ein, und klicken Sie dann auf das Kontrollkästchen, das angezeigt wird.

    Der Name der Regel muss eindeutig sein. Wenn dies Ihr Entwicklungscomputer ist, können Sie die IP-Adresse in das Feld IP-Bereich ein, und das Ende IP-Bereich eingeben. Andernfalls müssen Sie möglicherweise eine breitere von IP-Adressen Verbindungen aus weiteren Computern in Ihrer Organisation angepasst eingeben.
 
5. Klicken Sie auf **Speichern** , am unteren Rand der Seite.

    **Hinweis:** Es können werden von wie 5 Minuten verzögern für Änderungen an den Firewalleinstellungen wirksam.

    Sie können nun die Verbindung mit SQL-Datenbank mithilfe von Management Studio.

1.  Klicken Sie auf der Taskleiste klicken Sie auf **Start**, zeigen Sie auf **Alle Programme**, zeigen Sie auf **Microsoft SQL Server-2014**, und klicken Sie dann auf **SQL Server Management Studio**.

2.  In **Verbindung mit dem Server herstellen**, geben Sie den vollständigen Servernamen als *ServerName*. database.windows.net. Auf Azure ist der Servername eine automatisch generierte Zeichenfolge erstellter alphanumerische Zeichen.

3.  Wählen Sie **SQL Server-Authentifizierung**aus.

4.  Geben Sie im Feld **Login** Administrator SQL Server-Benutzername, den Sie im Portal angegeben, wenn Sie auf den Server erstellt haben.

5.  Geben Sie im Feld **Kennwort** das Kennwort ein, das Sie im Portal angegeben, wenn Sie auf den Server erstellt haben.

8.  Klicken Sie auf **Verbinden** , um eine Verbindung herzustellen.

SQL Server 2014 SSMS mit den neuesten Updates bietet erweiterten Unterstützung für Aufgaben wie SQL Azure-Datenbanken erstellen und ändern. Darüber hinaus können Sie Transact-SQL-Anweisungen verwenden, um diese Aufgaben ausführen. Geben Sie die folgenden Schritte aus Beispiele für diese Anweisungen aus. Weitere Informationen zur Verwendung von Transact-SQL mit SQL-Datenbank, einschließlich Details darüber, die die welche Befehle unterstützt werden, finden Sie unter [Transact-SQL-Referenz (SQL-Datenbank)](http://msdn.microsoft.com/library/bb510741.aspx).

## <a name="create-and-manage-azure-sql-databases"></a>Erstellen und Verwalten von SQL Azure-Datenbanken

Während Sie mit der **master** -Datenbank verbunden, können Sie neue Datenbanken auf dem Server erstellen und ändern oder vorhandene Datenbanken ablegen. Die folgenden Schritte beschreiben, wie mehrere allgemeine Datenbank über Management Studio durchführen. Um diese Aufgaben ausführen zu können, stellen Sie sicher, dass Sie mit der **master** -Datenbank mit dem Server Ebene Hauptbenutzer Benutzernamen verbunden sind, die Sie beim Einrichten einer Verbindung mit des Servers erstellt haben.

Um ein Abfragefenster in Management Studio zu öffnen, öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **System-Datenbanken** , mit der rechten Maustaste auf **master**, und klicken Sie dann auf **Neue Abfrage**.

-   Verwenden Sie **CREATE DATABASE** -Anweisung, um eine neue Datenbank zu erstellen. Weitere Informationen finden Sie unter [CREATE DATABASE (SQL-Datenbank)](https://msdn.microsoft.com/library/dn268335.aspx). Die folgende Anweisung erstellt eine neue Datenbank mit dem Namen **MyTestDB** und gibt an, dass es ein Standard S0 Edition-Datenbank mit einer Standardeinstellung für die maximale Größe von 250 GB ist.

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.

-   Verwenden Sie die **ALTER DATABASE** -Anweisung zum Ändern einer vorhandenen Datenbank, beispielsweise wenn Sie den Namen und die Edition der Datenbank ändern möchten. Weitere Informationen finden Sie unter [ALTER DATABASE (SQL-Datenbank)](https://msdn.microsoft.com/library/ms174269.aspx). Die folgenden Anweisung ändert die Datenbank, die Sie erstellt haben, so ändern Sie im vorhergehenden Schritt s1 Standard Edition.

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   Verwenden **Der DROP DATABASE** -Anweisung, um eine vorhandene Datenbank zu löschen.
    Weitere Informationen finden Sie unter [DROP DATABASE (SQL-Datenbank)](https://msdn.microsoft.com/library/ms178613.aspx). Die folgenden Anweisung löscht die Datenbank **MyTestDB** , aber nicht legen Sie sie jetzt, da Sie vorgesehenen werden im nächsten Schritt Benutzernamen erstellen.

        DROP DATABASE myTestBase;

-   Die master-Datenbank weist die **sys.databases** -Ansicht, die Sie zum Anzeigen von Details zu allen Datenbanken verwenden können. Wenn Sie alle vorhandene Datenbanken anzeigen möchten, führen Sie folgende Anweisung aus:

        SELECT * FROM sys.databases;

-   In SQL-Datenbank wird die **USE** -Anweisung zum Wechseln zwischen Datenbanken nicht unterstützt. In diesem Fall müssen Sie zum Herstellen einer Verbindung direkt in die Zieldatenbank.

>[AZURE.NOTE] Viele der Transact-SQL-Anweisungen, die erstellen oder ändern eine Datenbank in ihren eigenen Stapel ausgeführt werden müssen und nicht mit anderen Transact-SQL-Anweisungen gruppiert werden. Weitere Informationen finden Sie unter der Anweisung-spezifische Informationen aus den oben aufgeführten Links zur Verfügung.

## <a name="create-and-manage-logins"></a>Erstellen und Verwalten von Benutzernamen

Die **master** -Datenbank verfolgt Benutzernamen und welche Benutzernamen über die Berechtigung zum Erstellen von Datenbanken oder andere Benutzernamen. Verwalten von Benutzernamen durch Herstellen einer Verbindung mit der **master** -Datenbank mit dem Server Ebene Hauptbenutzer Benutzernamen, den Sie beim Einrichten einer Verbindung mit des Servers erstellt haben. Die **CREATE LOGIN**, **ALTER LOGIN**oder **DROP LOGIN** -Anweisung können Sie Abfragen für die master-Datenbank ausführen, die über den gesamten Server Benutzernamen verwalten soll. Weitere Informationen finden Sie unter [Verwalten von Datenbanken und Anmeldedaten in SQL-Datenbank](http://msdn.microsoft.com/library/azure/ee336235.aspx). 


-   Verwenden Sie die **CREATE LOGIN** -Anweisung zum Erstellen einer neuen Server Ebene Anmeldung. Weitere Informationen finden Sie unter [CREATE LOGIN (SQL-Datenbank)](https://msdn.microsoft.com/library/ms189751.aspx). Die folgenden Anweisung erstellt eine neue Anmeldung **login1**bezeichnet. Ersetzen Sie **Kennwort1** mit dem Kennwort Ihrer Wahl.

        CREATE LOGIN login1 WITH password='password1';

-   Verwenden der Anweisung **CREATE USER** Datenbank Ebene Berechtigungen erteilen. Alle Benutzernamen müssen in der **master** -Datenbank erstellt werden, aber für einen Benutzernamen für die Verbindung zu einer anderen Datenbank, Sie müssen diese Datenbank Ebene Berechtigungen erteilen die Anweisung **CREATE USER** auf dieser Datenbank verwenden. Weitere Informationen finden Sie unter [CREATE USER (SQL-Datenbank)](https://msdn.microsoft.com/library/ms173463.aspx). 

-   Login1 Berechtigungen für eine Datenbank namens **MyTestDB**verleihen möchten, gehen Sie folgendermaßen vor:

 1.  Zum Aktualisieren von Objekt-Explorer, um die Datenbank **MyTestDB** anzeigen möchten, die Sie soeben erstellt haben, mit der rechten Maustaste im Objekt-Explorer des Servernamens, und klicken Sie dann auf **Aktualisieren**.  

     Wenn Sie die Verbindung geschlossen haben, können Sie wieder herstellen, indem Sie im Menü Datei **Objekt-Explorer verbinden** auswählen.

 2. Mit der rechten Maustaste **MyTestDB** Datenbank, und wählen Sie **Neue Abfrage**.

    3.  Führen Sie die folgende Anweisung für die Datenbank MyTestDB zum Erstellen eines Datenbankbenutzers mit dem Namen **login1User** , die den Server Ebene Login **login1**entspricht.

            CREATE USER login1User FROM LOGIN login1;

-   Verwenden der **sp\_Addrolemember** gespeicherte Prozedur, um dem Benutzerkonto die entsprechende Zugriffsebene Berechtigungen für die Datenbank zu verleihen. Weitere Informationen finden Sie unter [Sp_addrolemember (Transact-SQL)](http://msdn.microsoft.com/library/ms187750.aspx). Die folgenden Anweisung bietet **login1User** schreibgeschützte Berechtigungen für die Datenbank durch Hinzufügen von **login1User** zu den **Db\_Datareader** Rolle.

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   Verwenden Sie die **ALTER LOGIN** -Anweisung so ändern Sie einen vorhandenen Benutzernamen, beispielsweise wenn Sie das Kennwort für die Anmeldung ändern möchten. Weitere Informationen finden Sie unter [ALTER LOGIN (SQL-Datenbank)](https://msdn.microsoft.com/library/ms189828.aspx). **ALTER LOGIN** -Anweisung sollte für die **master** -Datenbank ausgeführt werden. Wechseln Sie wieder zum Abfragefenster, das in dieser Datenbank verbunden ist. 

    Die folgenden Anweisung ändert die Anmeldung **login1** , um das Kennwort zurückzusetzen.
    Ersetzen Sie **NewPassword** mit dem Kennwort Ihrer Wahl und **OldPassword** für das aktuelle Kennwort für die Anmeldung an.

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   Verwenden Sie **Die DROP LOGIN** -Anweisung, um einen vorhandenen Benutzernamen löschen.
    Löschen einer Login Ebene des Servers löscht auch alle zugeordneten Datenbank-Benutzerkonten. Weitere Informationen finden Sie unter [DROP DATABASE (SQL-Datenbank)](https://msdn.microsoft.com/library/ms178613.aspx). **DROP LOGIN** -Anweisung sollte für die **master** -Datenbank ausgeführt werden. Die folgenden Anweisung löscht das **login1** Login.

        DROP LOGIN login1;

-   Die master-Datenbank weist die **sys.sql\_Benutzernamen** anzuzeigen, die Sie verwenden können, um Benutzernamen anzuzeigen. Um alle vorhandenen Benutzernamen anzeigen möchten, führen Sie folgende Anweisung aus:

        SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-viewsh2"></a>Überwachen der SQL-Datenbank mithilfe des dynamischen Management-Ansichten</h2>

SQL-Datenbank unterstützt mehrere dynamische Management-Ansichten, die Sie verwenden können, um eine einzelne Datenbank zu überwachen. Nachfolgend finden Sie einige Beispiele für den Typ des Monitor-Daten, die Sie über diese Ansichten abrufen können. Vollständige Details und weitere Beispiele für die Verwendung finden Sie unter [Überwachen der SQL-Datenbank mithilfe von dynamischen Management Ansichten](https://msdn.microsoft.com/library/azure/ff394114.aspx).

-   Eine dynamische Ansicht Abfragen erfordert **VIEW DATABASE STATE** Berechtigungen. Um die **VIEW DATABASE STATE** einem bestimmten Datenbankbenutzers erteilt, verbinden Sie mit der Datenbank, die Sie mit Ihrem Server Ebene Prinzip Login verwalten, und führen Sie folgende Anweisung für die Datenbank möchten:

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
