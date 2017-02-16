
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-the-connection-string-from-the-azure-portal"></a>Die Verbindungszeichenfolge aus der Azure-Portal zu erhalten.


Verwenden Sie der [Azure-Portal](https://portal.azure.com/) , um die Verbindungszeichenfolge notwendigen für Ihr Clientprogramm Interaktion mit Azure SQL-Datenbank zu erhalten: 


1. Klicken Sie auf **Durchsuchen** > **SQL-Datenbanken**.

2. Geben Sie den Namen der Datenbank in das Textfeld "Filter" in der Nähe der linken oberen des **SQL-Datenbanken** Blades aus.

3. Klicken Sie auf die Zeile für die Datenbank.

4. Nachdem das Blade für die Datenbank angezeigt wird, können visual Komfort Sie klicken, um die standard minimieren-Steuerelementen, um die Blades reduzieren, die Sie für das Durchsuchen und Filtern von Datenbank verwendet haben. 
 
    ![Der Filters zum Isolieren der Datenbank][10-FilterDatabase]

5. Klicken Sie auf das Blade für die Datenbank auf die **Datenbank Verbindungszeichenfolgen anzeigen**.

6. Wenn Sie die Bibliothek ADO.NET Verbindung verwenden möchten, kopieren Sie die Zeichenfolge mit der Bezeichnung **ADO**. 
 
    ![Kopieren Sie die ADO-Verbindungszeichenfolge für die Datenbank][20-CopyAdoConnectionString]
 
7. Fügen Sie in einem Format oder einer anderen Informationen für die Verbindungszeichenfolge in Ihrer Client-Programm-Code ein.



Weitere Informationen finden Sie unter:<br/>[Verbindungszeichenfolgen und Konfigurationsdateien](http://msdn.microsoft.com/library/ms254494.aspx).



<!-- Image references. -->

[10-FilterDatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png

[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
