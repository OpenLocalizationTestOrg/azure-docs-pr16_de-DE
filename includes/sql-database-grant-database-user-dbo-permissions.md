

## <a name="grant-new-database-user-dbowner-permissions"></a>Neue Datenbank Db_owner Benutzerberechtigungen gewähren

Gehen Sie folgendermaßen vor, eine vorhandene Datenbank Db_owner Erteilen von Benutzerberechtigungen

Thesenpapiere-Schritten wird vorausgesetzt, dass Sie mit der SQL-Datenbank im Objekt-Explorer in SSMS verbunden sind und als Server Ebene Hauptbenutzer-Administrator oder mit einem Benutzerkonto mit Berechtigungen erteilen von Berechtigungen für einen Benutzer mit dem logischen SQL-Datenbankserver verbunden sind. 

1. Im Objekt-Explorer erweitern Sie den Knoten Datenbanken, und wählen Sie die Datenbank mit dem Benutzer, mit dem Sie den Dbo-Berechtigungen erteilen möchten.

     ![SQL Server Management Studio: Herstellen einer Verbindung SQL-Datenbankserver mit](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-1.png)

2. Mit der rechten Maustaste in der ausgewählten Datenbank aus, und klicken Sie dann auf **Abfrage**.

     ![SQL Server Management Studio: Herstellen einer Verbindung SQL-Datenbankserver mit](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-2.png)

3. Im Fenster Abfrage bearbeiten Sie, und verwenden Sie die folgende Transact-SQL-Anweisung zu einem angegebenen Benutzer Dbo-Berechtigungen erteilen. 

    '''ALTER Rolle Db_owner Mitglied hinzufügen Benutzer1;
    ```

     ![SQL Server Management Studio: Connect to SQL Database server](./media/sql-database-grant-database-user-dbo-permissions/sql-database-grant-database-user-dbo-permissions-1.png)


