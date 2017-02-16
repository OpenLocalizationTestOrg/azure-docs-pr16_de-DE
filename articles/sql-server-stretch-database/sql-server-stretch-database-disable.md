<properties
    pageTitle="Gedehnt Datenbank deaktivieren, und fügen Sie die wieder remote Daten | Microsoft Azure"
    description="Informationen Sie zum Deaktivieren von gedehnt Datenbank für eine Tabelle und optional fügen Sie wieder remote-Daten."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="disable-stretch-database-and-bring-back-remote-data"></a>Deaktivieren Sie gedehnt Datenbank, und fügen Sie die wieder remote-Daten

Wählen Sie zum Deaktivieren von gedehnt Datenbank für eine Tabelle **gedehnt** für eine Tabelle in SQL Server Management Studio aus. Wählen Sie dann eine der folgenden Optionen aus.

-   Deaktivieren der **| Fügen Sie die Daten wieder aus Azure**. Kopieren, die die remote-Daten für die Tabelle aus Azure mit SQL Server, wieder deaktivieren dann gedehnt Datenbank für die Tabelle. Dieser Vorgang führt zu Daten durchstellen Kosten und kann nicht abgebrochen werden.

-   Deaktivieren der **| Lassen Sie die Daten in Azure**. Deaktivieren Sie gedehnt Datenbank für die Tabelle ein.  Abbrechen der remote-Daten für die Tabelle in Azure.

Sie können auch die Transact\-SQL gedehnt Datenbank für eine Tabelle oder einer Datenbank deaktivieren.

Nachdem Sie für eine Tabelle gedehnt Datenbank deaktivieren, gehören Daten Migration Stopps und Abfrageergebnisse nicht mehr Ergebnisse aus der remote-Tabelle.

Wenn Sie einfach Datenmigration anhalten möchten, finden Sie unter [Anhalten und fortsetzen gedehnt Datenbank](sql-server-stretch-database-pause.md).

>   [AZURE.NOTE] Dehnen Datenbank für eine Tabelle oder für eine Datenbank zu deaktivieren, wird das remote-Objekt nicht gelöscht. Wenn Sie die entfernte Tabelle oder die Remotedatenbank löschen möchten, müssen Sie es mithilfe der Azure-Verwaltungsportal ablegen. Der remote-Objekte weiterhin Azure Kosten entstehen, bis Sie gelöscht werden. Weitere Informationen finden Sie unter [SQL Server gedehnt Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-table"></a>Deaktivieren Sie gedehnt Datenbank für eine Tabelle

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-table"></a>Verwenden von SQL Server Management Studio gedehnt Datenbank für eine Tabelle deaktivieren

1.  Wählen Sie in SQL Server Management Studio im Objekt-Explorer die Tabelle, für die Sie gedehnt Datenbank deaktivieren möchten.

2.  Rechts\-klicken Sie auf **gedehnt**wählen Sie aus, und wählen Sie dann eine der folgenden Optionen aus.

    -   Deaktivieren der **| Fügen Sie die Daten wieder aus Azure**. Kopieren, die die remote-Daten für die Tabelle aus Azure mit SQL Server, wieder deaktivieren dann gedehnt Datenbank für die Tabelle. Dieser Befehl kann nicht abgebrochen werden.

        >   [AZURE.NOTE] Kopieren der remote-Daten für die Tabelle aus Azure budgetgerecht zurück, um den SQL Server Data Transferkosten. Weitere Informationen finden Sie unter [Daten Übertragung Preise Details](https://azure.microsoft.com/pricing/details/data-transfers/).

        Nachdem alle remote-Daten aus Azure kopiert wurde mit SQL Server zu sichern, wird gedehnt für die Tabelle deaktiviert.

    -   Deaktivieren der **| Lassen Sie die Daten in Azure**. Deaktivieren Sie gedehnt Datenbank für die Tabelle ein.  Abbrechen der remote-Daten für die Tabelle in Azure.

    >   [AZURE.NOTE] Deaktivieren von gedehnt Datenbank für eine Tabelle wird die remote Daten oder die entfernte Tabelle nicht gelöscht. Wenn Sie die entfernte Tabelle löschen möchten, müssen Sie es mithilfe der Azure-Verwaltungsportal ablegen. Die entfernte Tabelle befindet sich Azure Kosten entstehen, bis Sie es löschen. Weitere Informationen finden Sie unter [SQL Server gedehnt Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-table"></a>Verwenden Sie Transact\-SQL So deaktivieren Sie gedehnt Datenbank für eine Tabelle

-   Führen Sie den folgenden Befehl aus, um gedehnt für eine Tabelle und kopieren, die die remote-Daten für die Tabelle aus Azure mit SQL Server wieder deaktivieren. Nachdem alle remote-Daten aus Azure kopiert wurde mit SQL Server zu sichern, wird gedehnt für die Tabelle deaktiviert.

    Dieser Befehl kann nicht abgebrochen werden.

    ```tsql
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    GO
    ```
    >   [AZURE.NOTE] Kopieren der remote-Daten für die Tabelle aus Azure budgetgerecht zurück, um den SQL Server Data Transferkosten. Weitere Informationen finden Sie unter [Daten Übertragung Preise Details](https://azure.microsoft.com/pricing/details/data-transfers/).

-   Führen Sie zum Deaktivieren einer Tabelle gedehnt und aufgeben remote-Daten, den folgenden Befehl ein.

    ```tsql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

>   [AZURE.NOTE] Deaktivieren von gedehnt Datenbank für eine Tabelle wird die remote Daten oder die entfernte Tabelle nicht gelöscht. Wenn Sie die entfernte Tabelle löschen möchten, müssen Sie es mithilfe der Azure-Verwaltungsportal ablegen. Die entfernte Tabelle befindet sich Azure Kosten entstehen, bis Sie es löschen. Weitere Informationen finden Sie unter [SQL Server gedehnt Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-database"></a>Deaktivieren Sie gedehnt Datenbank für eine Datenbank
Bevor Sie gedehnt Datenbank für eine Datenbank deaktivieren können, müssen Sie gedehnt Datenbank auf die einzelnen gedehnt deaktivieren\-Tabellen in der Datenbank aktiviert.

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-database"></a>Verwenden von SQL Server Management Studio gedehnt Datenbank für eine Datenbank deaktivieren

1.  Wählen Sie die Datenbank aus, für die Sie gedehnt Datenbank deaktivieren möchten, in SQL Server Management Studio im Objekt-Explorer.

2.  Rechts\-klicken Sie auf und wählen Sie **Aufgaben**, und wählen Sie dann auf **gedehnt**, und wählen Sie dann auf **Deaktivieren**.

>   [AZURE.NOTE] Deaktivieren von gedehnt Datenbank für die Datenbank wird die Remotedatenbank nicht gelöscht. Wenn Sie die Remotedatenbank löschen möchten, müssen Sie es mithilfe der Azure-Verwaltungsportal ablegen. Die Remotedatenbank weiterhin Azure Kosten entstehen, bis Sie es löschen. Weitere Informationen finden Sie unter [SQL Server gedehnt Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-database"></a>Verwenden Sie Transact\-SQL gedehnt Datenbank für eine Datenbank deaktivieren
Führen Sie den folgenden Befehl ein.

```tsql
ALTER DATABASE <database name>
    SET REMOTE_DATA_ARCHIVE = OFF ;
```

>   [AZURE.NOTE] Deaktivieren von gedehnt Datenbank für die Datenbank wird die Remotedatenbank nicht gelöscht. Wenn Sie die Remotedatenbank löschen möchten, müssen Sie es mithilfe der Azure-Verwaltungsportal ablegen. Die Remotedatenbank weiterhin Azure Kosten entstehen, bis Sie es löschen. Weitere Informationen finden Sie unter [SQL Server gedehnt Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="see-also"></a>Siehe auch

[ALTER Datenbank Festlegen von Optionen (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[Anhalten Sie und fortsetzen Sie gedehnt Datenbank](sql-server-stretch-database-pause.md)
