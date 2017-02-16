<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-the-connection-string-from-the-azure-portal"></a>Die Verbindungszeichenfolge aus der Azure-Portal zu erhalten.


Verwenden Sie der [Azure-Portal](https://portal.azure.com/) , um die Verbindungszeichenfolge notwendigen für Ihr Clientprogramm Interaktion mit Azure SQL-Datenbank zu erhalten:


1. Klicken Sie auf **Durchsuchen** > **SQL-Datenbanken**.

    ![Wählen Sie SQL aus.][1-select-sql]

2. Geben Sie den Namen der Datenbank in das Textfeld "Filter" in der Nähe der linken oberen des **SQL-Datenbanken** Blades aus.

    ![Wählen Sie die Datenbank][2-select-database]]

3. Klicken Sie auf die Zeile für die Datenbank.

4. Nachdem das Blade für die Datenbank angezeigt wird, können visual Komfort Sie klicken, um die standard minimieren-Steuerelementen, um die Blades reduzieren, die Sie für das Durchsuchen und Filtern von Datenbank verwendet haben.

5. Klicken Sie auf das Blade für die Datenbank auf die **Datenbank Verbindungszeichenfolgen anzeigen**.

6. Wenn Sie die Bibliothek JDBC Verbindung verwenden möchten, kopieren Sie die Zeichenfolge mit der Bezeichnung **JDBC**.

    ![Kopieren Sie die JDBC Verbindungszeichenfolge für die Datenbank][3-get-connection-string]

7. Fügen Sie Informationen für die Verbindungszeichenfolge in Ihrer Client-Programm-Code ein.  Sie benötigen die {Your_password_here} durch real Kennwort ersetzen.



Weitere Informationen finden Sie unter:<br/>[Verbindungszeichenfolgen und Konfigurationsdateien](https://msdn.microsoft.com/library/ms378428.aspx).



<!-- Image references. -->

[1-select-sql]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-sql.png


[2-select-database]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-database.PNG

[3-get-connection-string]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-jdbc.PNG


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
